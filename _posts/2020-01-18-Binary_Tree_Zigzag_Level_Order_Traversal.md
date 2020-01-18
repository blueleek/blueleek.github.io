---
layout: post
title:  "103. Binary Tree Zigzag Level Order Traversal"
categories: 算法
tags: LeetCode Algorithm LinkedList
author: blueleek
---



### 题目描述
> Given a binary tree, return the zigzag level order traversal of its nodes' values. (ie, from left to right, then right to left for the next level and alternate between).
>
>Example:
> Input: [3,9,20,null,null,15,7]
>
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200118171313314.png) <br/>
> return its zigzag level order traversal as
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200118171259558.png)














### 方法一：BFS 广度优先遍历
从跟节点开始，沿着树的宽度一次遍历树的每个节点
我们借助队列的结构来实现树的广度优先遍历。
代码实现：
```
var zigzagLevelOrder = function(root) {
     if (root === null || root === undefined) {
        return [];
    }
    
    var newOrder = [];
    var queue = [];
    queue.push(root);
    var flag = false;
    while(queue.length) {
        var level = [];
        var length = queue.length;
        while(length > 0) {
            var node = queue.shift();
            level.push(node.val);
            if (node.left !== null) { queue.push(node.left); }
            if (node.right !== null) { queue.push(node.right); }   
            length--;
        }
        if (flag) { level.reverse(); }
        flag = !flag;
        newOrder.push(level); 
    }
    return newOrder;
    
};

```

### 方法二： DFS 深度优先遍历
方法二采用深度优先遍历的方法，从树的根节点开始，先遍历左子树，然后遍历右子树
代码实现：
```
var zigzagLevelOrder = function(root) {
    var newOrder = [];
    travel(root, newOrder, 0);
    return newOrder;
};

var travel = function(node, order, level) {
    if (node === null || node === undefined) {
        return;
    }
    
    if (order.length <= level) {
        order.push([]);
    }
    if (level % 2 === 0) {
        order[level].push(node.val)
    } else {
        order[level].unshift(node.val)
    }
    travel(node.left, order, level +1);
    travel(node.right, order, level +1); 
}
```
