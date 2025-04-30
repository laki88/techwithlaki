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

# Add Two Numbers: A Fun Challenge for Linked List Enthusiasts
## Problem Description
Imagine you're a math whiz dealing with two enormous numbers—numbers so big that they can't fit into a regular integer variable in programming. But here's the twist: these numbers are given to you in reverse order, with each digit stored as a node in a linked list. Your mission is to add these two giant numbers together, digit by digit, just like you did in elementary school, but with a catch: since they're backwards, you'll have to be extra careful with how you handle the addition. And don't forget about the carry! Once you've added them up, you need to return the result in the same format—a linked list with digits in reverse order.

Think of it like adding two stacks of digits, where the top of each stack is the ones place, then tens, hundreds, and so on. You'll need to handle everything carefully, making sure to account for any carry that might occur when the sum of two digits exceeds 9. Oh, and one more thing: these numbers don't have leading zeros, except for the number zero itself. So, no funny business with extra zeros at the beginning!

### Examples
<table style="border-collapse: collapse; width: 100%;">
  <thead>
    <tr>
      <th style="border: 1px solid #ddd; padding: 8px; text-align: left;">Input Lists</th>
      <th style="border: 1px solid #ddd; padding: 8px; text-align: left;">Numbers</th>
      <th style="border: 1px solid #ddd; padding: 8px; text-align: left;">Output List</th>
      <th style="border: 1px solid #ddd; padding: 8px; text-align: left;">Sum</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">l1 = [2,4,3], l2 = [5,6,4]</td>
      <td style="border: 1px solid #ddd; padding: 8px;">342 + 465</td>
      <td style="border: 1px solid #ddd; padding: 8px;">[7,0,8]</td>
      <td style="border: 1px solid #ddd; padding: 8px;">807</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">l1 = [0], l2 = [0]</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0 + 0</td>
      <td style="border: 1px solid #ddd; padding: 8px;">[0]</td>
      <td style="border: 1px solid #ddd; padding: 8px;">0</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]</td>
      <td style="border: 1px solid #ddd; padding: 8px;">9999999 + 9999</td>
      <td style="border: 1px solid #ddd; padding: 8px;">[8,9,9,9,0,0,0,1]</td>
      <td style="border: 1px solid #ddd; padding: 8px;">10009998</td>
    </tr>
  </tbody>
</table>

## Solution Description
Alright, let's break this down step by step, as if I'm explaining it to someone who's just starting out with linked lists and addition.

### Step 1: Understand the Problem

- You have two linked lists, let's call them `list1` and `list2`.
- Each node in these lists holds a single digit (from 0 to 9).
- The lists represent numbers in reverse order. For example:
  - If `list1` is `2 -> 4 -> 3`, it actually represents the number 342 (not 243).
  - If `list2` is `5 -> 6 -> 4`, it represents 465.


- Your job is to add these two numbers (342 + 465) and return the result as another linked list in reverse order (e.g., `7 -> 0 -> 8` for 807).

### Step 2: How Do We Add Numbers?

- Normally, when you add two numbers, you start from the rightmost digit (the ones place) and move left, carrying over any extra when the sum exceeds 9.
- Here, since the numbers are already reversed, we start from the head of the lists (the first node) and move forward.
- We need to:
  - Add the digits from both lists at each position.
  - Keep track of any carry from the previous addition.
  - Handle cases where one list might be longer than the other (treat missing digits as 0).
  - If there's a carry left after adding all digits, add it as an extra digit.



### Step 3: Let's Walk Through the Solution

#### Step 3.1: Set Up

- Create a dummy head for the result list. This is like having an extra space on paper to write the sum—it makes things easier because we don't have to worry about creating the first node separately.
- Initialize a pointer (let's call it current) to the dummy head.
Start with a carry of 0.


#### Step 3.2: Loop Through the Lists

- While either list1 or list2 has nodes left (or there's a carry):
  - Get the value of the current node from list1 (if it exists; otherwise, use 0).
  - Get the value of the current node from list2 (if it exists; otherwise, use 0).
  - Add these two values plus the current carry.
  - The last digit of this sum (sum % 10) becomes the next digit in our result list.
  - The carry for the next step is sum / 10.
  - Move to the next nodes in both lists (if they exist).

#### Step 3.3: Handle Remaining Carry

- After the loop, if there's still a carry left (e.g., when adding 999 + 1), add a new node with that carry value.


#### Step 3.4: Return the Result

- Since we used a dummy head, return dummyHead.next as the start of our result list.

### Step 4: Example Walkthrough
Let’s add `list1: 2 -> 4 -> 3` (342) and list2: `5 -> 6 -> 4` (465):

- Start with carry = 0.
- Step 1: Add 2 + 5 + 0 = 7. Carry = 0. Result: 7.
- Step 2: Add 4 + 6 + 0 = 10. Carry = 1. Result: 0 (write 0, carry 1).
- Step 3: Add 3 + 4 + 1 = 8. Carry = 0. Result: `8 -> 0 -> 7`.
- Final result: `7 -> 0 -> 8`, which is 807 (and indeed, 342 + 465 = 807).

Another example: list1: `9 -> 9 -> 9 -> 9 -> 9 -> 9 -> 9` (9999999) and list2: `9 -> 9 -> 9 -> 9` (9999):

- After adding all digits, you’ll get a carry of 1 at the end.
- Result: `8 -> 9 -> 9 -> 9 -> 0 -> 0 -> 0 -> 1`, which is 10009998.

### Solution Code in Java
```java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode dummyHead = new ListNode(0);
        ListNode current = dummyHead;
        int carry = 0;
        while (l1 != null || l2 != null) {
            int x = (l1 != null) ? l1.val : 0;
            int y = (l2 != null) ? l2.val : 0;
            int sum = carry + x + y;
            carry = sum / 10;
            current.next = new ListNode(sum % 10);
            current = current.next;
            if (l1 != null) l1 = l1.next;
            if (l2 != null) l2 = l2.next;
        }
        if (carry > 0) {
            current.next = new ListNode(carry);
        }
        return dummyHead.next;
    }
}
```

### Complexity Analysis

- Time Complexity: O(max(m, n)), where m and n are the lengths of the two input lists. We traverse both lists once, and the number of iterations is determined by the longer list.

- Space Complexity: O(max(m, n)), as we create a new linked list for the result, which can be as long as the longer of the two input lists plus one (for a potential carry).

### Edge Cases Considered

Lists of unequal lengths: Handled by using 0 for missing digits.
Final carry requiring an extra node: Handled by checking for carry after the loop.
Lists with only zero: Works correctly (e.g., `[0] + [0] = [0]`).
Multiple carry operations: Handled naturally by the carry variable.

### Summary
In this problem, we tackled the challenge of adding two large numbers represented as linked lists in reverse order. By simulating the manual addition process—traversing both lists, handling carries, and accounting for lists of different lengths—we produced an efficient solution. The key takeaways are understanding how to work with linked lists, manage carries during addition, and handle edge cases like varying list lengths and remaining carries.

Please let me know if anything unclear in comment.
