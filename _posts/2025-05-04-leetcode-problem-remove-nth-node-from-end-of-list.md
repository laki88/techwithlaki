---
layout: post
title:  "Leetcode problem 19 - Remove Nth Node From End of List | Problem and solution"
author: Laki
categories: [ leetcode, algorithm, interview questions ]
image: assets/images/leetcode-problem-19/1.jpg
beforetoc: "This post shows the problem statement and several solutions for leetcode Remove Nth Node From End of List problem"
toc: false
---

# [Remove Nth Node From End of List](https://leetcode.com/problems/remove-nth-node-from-end-of-list/)

## Description

Given the head of a linked list, remove the nth node from the end of the list and return its head.

Example 1:
```
Input: head = [1,2,3,4,5], n = 2
Output: [1,2,3,5]
```
Example 2:
```
Input: head = [1], n = 1
Output: []
```
Example 3:
```
Input: head = [1,2], n = 1
Output: [1]
```

## Solutions
### Brute-Force Solution (Two Passes)
The simplest idea is to first determine the length of the linked list. Once we know the total number of nodes, we can compute the position to remove from the start of the list (which is length - n). Then we walk the list again to reach that node and adjust pointers.

```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        int length = 0;
        ListNode temp = head;
        // First pass to count the nodes
        while (temp != null) {
            length++;
            temp = temp.next;
        }
        // If we need to remove the first node
        if (n == length) return head.next;

        temp = head;
        // Second pass to reach the node before the one we want to remove
        for (int i = 1; i < length - n; i++) {
            temp = temp.next;
        }
        temp.next = temp.next.next;
        return head;
    }
}
```
Time Complexity: O(2n)
Space Complexity: O(1)
Pros: Easy to understand
Cons: Requires two passes through the list

### One-Pass Solution Using Two Pointers
To improve performance, we can remove the need for two full passes using the two-pointer technique — sometimes also referred to as the fast and slow pointer method.

We start both pointers at a dummy node before the head. We move the fast pointer n steps ahead. Then, we move both pointers one step at a time until the fast pointer reaches the end. The slow pointer will then be right before the node to delete.

```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummy = new ListNode(0);
        dummy.next = head;

        ListNode fast = dummy;
        ListNode slow = dummy;

        // Move fast ahead by n steps
        for (int i = 0; i < n; i++) {
            fast = fast.next;
        }

        // Move both fast and slow until fast reaches the end
        while (fast.next != null) {
            fast = fast.next;
            slow = slow.next;
        }

        // Skip the target node
        slow.next = slow.next.next;

        return dummy.next;
    }
}
```
Time Complexity: O(n)
Space Complexity: O(1)
Pros: Single traversal, efficient
Cons: Slightly more setup with dummy node and edge case handling

### Edge Case: Deleting the First Node
You may run into a case where the node to be deleted is the very first one. The above code already handles it using a dummy node, but if you'd like to write an explicit check:

```java
if (fast.next == null) {
    return head.next;
}
```
This condition checks whether the list size equals n, meaning we need to remove the head.

### Final Thoughts
This is a classic interview question that tests your understanding of linked list traversal and pointer manipulation. While the brute-force approach is fine for learning, the one-pass method is preferred in real-world scenarios due to its efficiency.

