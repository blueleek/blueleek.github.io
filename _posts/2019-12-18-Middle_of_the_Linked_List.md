---
layout: post
title:  "876. Middle of the Linked List"
categories: 算法
tags: LeetCode Algorithm LinkedList
author: Ella
---

最近在复习链表数据结构，刷了些 LeetCode 的题，很多思路相似，但很有趣的是在过程中遇到很多更优解和数学证明，以博客进行记录和思路梳理。










### Problem Description

>Given a non-empty, singly linked list with head node head, return a middle node of linked list.
>If there are two middle nodes, return the second middle node.
 
>Example 1:
>Input: [1,2,3,4,5]
 Output: Node 3 from this list (Serialization: [3,4,5])
 The returned node has value 3.  (The judge's serialization of this node is [3,4,5]).
 Note that we returned a ListNode object ans, such that:
 ans.val = 3, ans.next.val = 4, ans.next.next.val = 5, and ans.next.next.next = NULL.
 
>Example 2:
 Input: [1,2,3,4,5,6]
 Output: Node 4 from this list (Serialization: [4,5,6])
 Since the list has two middle nodes with values 3 and 4, we return the second one.
 
> Note:
 The number of nodes in the given list will be between 1 and 100.
 If there are two middle nodes, return the second middle node.
 
 
 
 
### 方法一：求链表长度取中点
 
 ```
 let middleNode = function(head) {
     let targetLen = Math.ceil(getLength(head)/2);
     return returnAtPosition(head, targetLen);
 };
 
 let getLength = function(node) {
     let length = 0;
     while(node.next) {
         length += 1;
         node = node.next;
     }
     return length;
 };
 
 let returnAtPosition = function(node, targetLen) {
     for(let i = 0; i < targetLen; i++) {
         node = node.next;
     }
     return node;
 };
 ```
 
### 方法二：快慢指针
 
 可以先使用举例论证法找到规律：
 ```
 1 > 2 > 3 > 4
 1 > 2 > 3 > 4 > 5
 ```
  定义快慢指针，`slow`，`fast`，慢指针每向前移动一步，快指针移动两步，根据链表长度奇偶特性，当偶数长度链表快指针 `fast.next.next
  === null` 时，返回 `slow.next`，当奇数长度链表 `fast.next === null` 时，返回 `slow`

 ```
 var middleNode = function(head) {
   if (head === null) {
       return false; 
   }
     
   var slow = head;
   var fast = head;
   while (true) {
       if (fast.next === null) {
           return slow;
       }
       
       if (fast.next.next === null) {
           return slow.next;
       }
       
       fast = fast.next.next
       slow = slow.next
   }
 };
 ```

