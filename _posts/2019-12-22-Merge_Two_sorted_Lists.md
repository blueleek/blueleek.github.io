---
layout: post
title:  "21. Merge Two Sorted Lists                                            "
categories: 算法
tags: LeetCode Algorithm LinkedList
author: blueleek
---

基本思想：<br/>
归并排序是利用归并的思想实现的排序算法。<br/>
归并算法采用了经典的分治策略，将问题分解称小的问题，然后递归求解，而治的阶段将分的阶段得到的答案拼接在一起。

 


### Problem Description

>Merge two sorted linked lists and return it as a new list. <br/>
>The new list should be made by splicing together the nodes of the first two lists.
>
> Example: <br/>
```
  Input: 1->2->4, 1->3->4
  Output: 1->1->2->3->4->4
```


在白板上手动画了分治中，治的这一步的实现思路图
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019122218475785.jpeg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2hodGh3eA==,size_16,color_FFFFFF,t_70)

代码示例：
```
var mergeTwoLists = function(l1, l2) {
    
  if (l1 === null && l2 === null) {
      return null;
  }  
    
  var dummy = new ListNode();  
  var curr = dummy;  
  while (l1 !== null && l2 !== null) {
      if (l1.val >= l2.val) { 
          curr.next = l2;
          l2 = l2.next;
      } else {
          curr.next = l1;
          l1 = l1.next;
      }
      curr = curr.next;
  }   
  curr.next = l1 || l2;
  return dummy.next;     
};
```
