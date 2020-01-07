---
layout: post
title:  "144. Binary Tree Preorder Traversal"
categories: 算法
tags: LeetCode Algorithm LinkedList
author: blueleek
---






二叉查找树融合了链表插入的灵活性和有序数组查找的高效性，是一个经典的数据结构也是计算机科学中最重要的算法之一。<br/>
今天复习一下二叉查找树前序（根节点 -> 左子树 -> 右子树）遍历的实现。

### Problem Description
> Given a binary tree, return the preorder traversal of its nodes' values.
>
>Example:
> Input: [1,null,2,3]
>
> ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200107231133530.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2hodGh3eA==,size_16,color_FFFFFF,t_70)

### 方法一：递归遍历
类似于在二叉树中查找一个键的递归算法：
> 如果树是空的，则查找未命中，如果查找的键和 root 节点的键值相等，查找命中，否则我们就（递归）在适当的子树木中继续查找。

停止递归调用的条件是节点为null<br/>
代码实现：
```
function recursive(root, arr) {
    if (root === null) return;

    arr.push(root.val);
    recursive(root.left, arr)
    recursive(root.right, arr)
}
var preorderTraversal = function(root) {
    var arr = [];
    recursive(root, arr)
    return arr; 
};

```
时间复杂度： O（n）<br/>
空间复杂度： O（logN）

### 方法二： 模拟栈调用操作
也可以不采用递归的方法进行遍历，模拟栈调用实现遍历
代码实现：
```
var preorderTraversal = function(root) {
  if (!root) return [];
  var result = [];
  var stack = [root];
  
  while(stack.length) {
    var node = stack.pop();
    result.push(node.val);
    if (node.right) stack.push(node.right);
    if (node.left) stack.push(node.left);
  }
  return result;
};
```

其实和二叉树有关的算法基本都可以用递归实现，可以先用简单的树形结构帮助确定循环条件。<br/>
递归函数每个二叉树都会执行，大的二叉树是由子树形成的，所以子树达到目标，整棵树自然可以实现目标。当然使用模拟调用栈操作的方式可以帮助更好的理解栈调用的过程。

