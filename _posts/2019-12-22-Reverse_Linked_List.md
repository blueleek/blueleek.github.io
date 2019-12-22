---
layout: post
title:  "206. Reverse Linked List                                            "
categories: 算法
tags: LeetCode Algorithm LinkedList
author: blueLeek
---

 在单链表的插入，删除某节点，将链表逆序都需要遍历链表，所需要的时间和链表的长度成正比，标准的做法是使用双向链表。但在已给条件是单链表的情况下就需要遍历整个链表进行操作。
 





### Problem Description

>Reverse a singly linked list.
>
> Example: <br/>
```
  Input: 1->2->3->4->5->NULL
  Output: 5->4->3->2->1->NULL
```

在这道逆序链表题目中，需要注意的是，我们希望通过改变原有节点的指向进行逆序，但一旦改变节点的指向，就再也无法访问它曾经指向的结点了，所以我们需要保存原有结点的引用。<br/>


代码示例：
```
var reverseList = function(head) {
    if (head === null) {
        return head;
    }
    
    var prev = null;
    while (head !== null) {
        var temp = head.next;
        head.next = prev
        prev = head
        head = temp;
    }
    
    return prev;
};
```

