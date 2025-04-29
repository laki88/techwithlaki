---
layout: post
title:  "Leetcode problem 14 - Longest Common Prefix | Problem and solution"
author: Laki
categories: [ leetcode, algorithm, interview questions ]
image: assets/images/2023-01-26-leetcode-problem1-with-solutions/leetcode_meme1.png
beforetoc: "This post shows the problem statement and several solutions for leetcode Longest Common Prefix problem"
toc: false
---

# [Longest Common Prefix](https://leetcode.com/problems/longest-common-prefix)

## Description

Given an integer array nums of length n and an integer target, find three integers in nums such that the sum is closest to target.

Return the sum of the three integers.

You may assume that each input would have exactly one solution.

 

Example 1:
```
Input: nums = [-1,2,1,-4], target = 1
Output: 2
Explanation: The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).
```
Example 2:
```
Input: nums = [0,0,0], target = 1
Output: 0
Explanation: The sum that is closest to the target is 0. (0 + 0 + 0 = 0).
 ```

## Solutions

### 🛠️ Brute Force Approach (Time Complexity: O(n³))
The simplest approach is to consider all possible triplets and calculate their sums. For each triplet, we compute how close it is to the target and track the minimum difference encountered.

Java Code
```java
class Solution {
    public int threeSumClosest(int[] nums, int target) {
        int closestSum = Integer.MAX_VALUE / 2;  // Prevents overflow
        
        for (int i = 0; i < nums.length - 2; ++i) {
            for (int j = i + 1; j < nums.length - 1; ++j) {
                for (int k = j + 1; k < nums.length; ++k) {
                    int currentSum = nums[i] + nums[j] + nums[k];
                    if (Math.abs(currentSum - target) < Math.abs(closestSum - target)) {
                        closestSum = currentSum;
                    }
                }
            }
        }
        
        return closestSum;
    }
}
```
This solution is easy to understand but slow for larger inputs due to its cubic time complexity.

### ⚡ Optimized Two Pointers Approach (Time Complexity: O(n²))
By sorting the array, we can use two pointers to efficiently check pairs that, along with a fixed element, make up the closest sum.

Key Steps:
Sort the array.

Iterate through the array, fixing one element at a time.

Use two pointers (left and right) to find pairs that bring the sum closer to the target.

Java Code
```java
class Solution {
    public int threeSumClosest(int[] nums, int target) {
        Arrays.sort(nums);
        int closestSum = Integer.MAX_VALUE / 2;

        for (int i = 0; i < nums.length - 2; ++i) {
            int left = i + 1;
            int right = nums.length - 1;

            while (left < right) {
                int currentSum = nums[i] + nums[left] + nums[right];
                if (Math.abs(currentSum - target) < Math.abs(closestSum - target)) {
                    closestSum = currentSum;
                }

                if (currentSum < target) {
                    left++;
                } else if (currentSum > target) {
                    right--;
                } else {
                    return currentSum;  // Exact match
                }
            }
        }

        return closestSum;
    }
}
```
This approach significantly improves performance while remaining easy to implement. It's the recommended solution for most real-world scenarios.

### ✅ Summary

<table style="border-collapse: collapse; width: 100%;">
  <thead>
    <tr>
      <th style="border: 1px solid black; padding: 8px;">Approach</th>
      <th style="border: 1px solid black; padding: 8px;">Time Complexity</th>
      <th style="border: 1px solid black; padding: 8px;">Space Complexity</th>
      <th style="border: 1px solid black; padding: 8px;">When to Use</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border: 1px solid black; padding: 8px;">Brute Force (Backtracking)</td>
      <td style="border: 1px solid black; padding: 8px;">O(n³)</td>
      <td style="border: 1px solid black; padding: 8px;">O(1)</td>
      <td style="border: 1px solid black; padding: 8px;">Simple but inefficient</td>
    </tr>
    <tr>
      <td style="border: 1px solid black; padding: 8px;">Two Pointers</td>
      <td style="border: 1px solid black; padding: 8px;">O(n²)</td>
      <td style="border: 1px solid black; padding: 8px;">O(1)</td>
      <td style="border: 1px solid black; padding: 8px;">fficient and clean</td>
    </tr>
    <tr>
      <td style="border: 1px solid black; padding: 8px;">HashSet Version</td>
      <td style="border: 1px solid black; padding: 8px;">O(n²)</td>
      <td style="border: 1px solid black; padding: 8px;">O(n²)</td>
      <td style="border: 1px solid black; padding: 8px;">Avoids duplicate logic manually</td>
    </tr>
  </tbody>
</table>

By applying sorting and two pointers, we reduce unnecessary work and get much better performance with only a few lines of added logic.
