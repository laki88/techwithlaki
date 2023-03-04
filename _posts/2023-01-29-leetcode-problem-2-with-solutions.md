---
layout: post
title:  "Leetcode problem 2 - Add Two Numbers | Problem and solution"
author: Laki
categories: [ leetcode, algorithm, linked list, interview questions ]
image: assets/images/8.jpg
beforetoc: "This post shows the problem statement and the solution for leetcode Add Two Numbers problem"
toc: false
---

# [Add Two Numbers](https://leetcode.com/problems/add-two-numbers/)

## Description

When two numbers are represented in two non-empty linked lists, You need to add those two numbers and return the result linked list. Each node holds a single digits in the numbers and digits are stored in reverse order. It can be assumed that there is no leading zeros, except number zero case.

### Example 1:
![Adding numbers in two linked lists](../assets/images/2023-01-29-leetcode-problem-2-with-solutions/addtwonumber1.jpg)

```
Input: l1 = [2,4,3], l2 = [5,6,4]
Output: [7,0,8]
Explanation: 342 + 465 = 807.
```
### Example 2:
```
Input: l1 = [0], l2 = [0]
Output: [0]
```
### Example 3:
```
Input: l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]
Output: [8,9,9,9,0,0,0,1]
```

## Solution 
```java
public class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode dummyHead = new ListNode(0);
        ListNode curr = dummyHead;
        int carry = 0;
        while (l1 != null || l2 != null || carry != 0) {
            int x = (l1 != null) ? l1.val : 0;
            int y = (l2 != null) ? l2.val : 0;
            int sum = carry + x + y;
            carry = sum / 10;
            curr.next = new ListNode(sum % 10);
            curr = curr.next;
            if (l1 != null)
                l1 = l1.next;
            if (l2 != null)
                l2 = l2.next;
        }
        return dummyHead.next;
    }
}
class ListNode {
      int val;
      ListNode next;
      ListNode() {}
      ListNode(int val) { this.val = val; }
      ListNode(int val, ListNode next) { this.val = val; this.next = next; }
}
```
Please let me know if anything unclear in comment.
