---
layout: post
title:  "203. Remove Linked List Elements                                            "
categories: 算法
tags: LeetCode Algorithm LinkedList
author: blueLeek
---

在实现链表算法中，有时需针对多种情况分类讨论，稍不注意，就会遗漏某种情况。因此可使用一些tricky的做法，如用dummyNode将情况归一，这样就可以避免分类讨论的情况。<br/>
在这道删除链表节点的题目中，博文中针对两种实现思路都给出了相应的解答，在时间和空间复杂度上，两种方法表现一致，但使用dummyNode使得代码更加简洁，优雅，可读性更高。







### Problem Description

>Remove all elements from a linked list of integers that have value val.
>
> Example: <br/>
```
  Input:  1->2->6->3->4->5->6, val = 6
  Output: 1->2->3->4->5
```

### 方法一：分类讨论
思路：对不同情况进行分类处理
* 当头节点等于val，执行重复handle，将 `head = head.next`
* 当前 `current.next` match 到要匹配的 val，将 `current.next = current.next.next`，否则将其置为 `node after` <br/>
代码示例：
```
var removeElements = function(head, val) {
    if (head === null) {
        return null;
    }
    
    while (head) {
        if (head.val === val) {
            head = head.next;
        } else {
            break;
        }
    }
  
    let curr = head;
    while (curr !== null && curr.next !== null) {
        if (curr.next.val === val) {
            curr.next = curr.next.next
        } else {
            curr = curr.next   
        }
    }
    return head;
};
```

### 方法二： 使用伪节点归一情况
思路：声明一个 dummyNode，将其作为头节点，这样判断是否匹配 val 的情况都是在 `current.next` 下进行讨论，无需考虑头节点和 `val` 匹配的情况 <br/>
代码示例：
```
var removeElements = function(head, val) {
    if (head == null) {
        return null;
    }
    
    var dummy = new ListNode(undefined);
    dummy.next = head;
    var curr = dummy;
    while (curr !== null && curr.next !== null) {
        if (curr.next.val === val) {
            curr.next = curr.next.next
        } else {
            curr = curr.next;
        }
    }
    return dummy.next;
};

```

