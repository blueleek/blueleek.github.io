---
layout: post
title:  "161.Intersection of Two Linked Lists"
categories: 算法
tags: LeetCode Algorithm LinkedList
author: blueLeek
---

早起一道算法题，清神洗脑。<br/>
寻找两链表的交叉点，最初的实现思路是计算出两链表的长度，利用链表长度的差值，将两链表的头节点移动到相同距离的起点位置进行移动，直到节点相遇。<br/>
按照这样思路确实可以实现，只是代码写起来很不优雅，在Discuss中看到most votes解决方案，使用了一种很tricky的解决方法，对两个长度不同的链表进行遍历，至尾节点时将头节点指向对方头部节点，最终指针遍历路程相同时，二者即在交叉点相遇.









### Problem Description

>Write a program to find the node at which the intersection of two singly linked lists begins.
>
> For example, the following two linked lists: <br/>
![](https://assets.leetcode.com/uploads/2018/12/13/160_statement.png)
>To represent a cycle in the given linked list, we use an integer pos which represents the position (0-indexed) in the linked list where tail connects to. If pos is -1, then there is no cycle in the linked list.
>begin to intersect at node c1.<br/>
>Example 1:
![](https://assets.leetcode.com/uploads/2018/12/13/160_example_1.png)
>
>Input: intersectVal = 8, listA = [4,1,8,4,5], listB = [5,0,1,8,4,5], skipA = 2, skipB = 3<br/>
>
>Output: Reference of the node with value = 8<br/>
>
>Input Explanation: The intersected node's value is 8 (note that this must not be 0 if the two lists intersect). From the head of A, it reads as [4,1,8,4,5]. From the head of B, it reads as [5,0,1,8,4,5]. There are 2 nodes before the intersected node in A; There are 3 nodes before the intersected node in B.
>
>Example 2: <br/>
>![](https://assets.leetcode.com/uploads/2018/12/13/160_example_2.png)
>
>Input: intersectVal = 2, listA = [0,9,1,2,4], listB = [3,2,4], skipA = 3, skipB = 1 <br/>
>
>Output: Reference of the node with value = 2 <br/>
>
>Input Explanation: The intersected node's value is 2 (note that this must not be 0 if the two lists intersect). From the head of A, it reads as [0,9,1,2,4]. From the head of B, it reads as [3,2,4]. There are 3 nodes before the intersected node in A; There are 1 node before the intersected node in B.<br/>
>
> Example 3:<br/>
> ![](https://assets.leetcode.com/uploads/2018/12/13/160_example_3.png)
>
>Input: intersectVal = 0, listA = [2,6,4], listB = [1,5], skipA = 3, skipB = 2 <br/>
>
>Output: null <br/>
>
>Input Explanation: From the head of A, it reads as [2,6,4]. From the head of B, it reads as [1,5]. Since the two lists do not intersect, intersectVal must be 0, while skipA and skipB can be arbitrary values.
>Explanation: The two lists do not intersect, so return null.
>
>Note:
>* If the two linked lists have no intersection at all, return null.
>* The linked lists must retain their original structure after the function returns.
>* You may assume there are no cycles anywhere in the entire linked structure.
>* Your code should preferably run in O(n) time and use only O(1) memory.


### 方法一：
思路：求出两链表的长度，利用长度差值，将遍历的起始点移动到相等距离位置<br/>
代码示例：
```
function getLen (head) {
    var len = 0;
    while (head !== null) {
        len += 1;
        head = head.next
  }
    return len;
}

var getIntersectionNode = function(headA, headB) {
    if (headA === null || headB === null) {
        return null;
    }
    
    var lenA = getLen(headA);
    var lenB = getLen(headB);
    if (lenA > lenB) {
        while (lenA !== lenB) {
            headA = headA.next;
            lenA = lenA - 1;
        }
    }
   if (lenA < lenB) {
        while (lenA !== lenB) {
            headB = headB.next;
            lenB = lenB - 1;
        }
   }
    
    
   while (true) {
       if (headA === headB) {
           return headA;
       }
       headA = headA.next;
       headB = headB.next;
   }
   return null;  
};

```

### 方法二：
思路：对两链表进行遍历，至尾节点时将指针移至对方头节点，最终指针遍历路程相等时，二者相遇，即为交叉点或者无交叉点<br/>
代码示例：
```
const getIntersectionNode = (headA, headB) => {
    if (!headA || !headB) {
        return null
    }
    
    let p1 = headA
    let p2 = headB
    
    while (p1 !== p2) {
        p1 = p1 ? p1.next : headB
        p2 = p2 ? p2.next : headA
    }
    
    return p1
}
```

