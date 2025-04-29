---
layout: post
title:  "Leetcode problem 20 - Valid Parentheses | Problem and solution"
author: Laki
categories: [ leetcode, algorithm, interview questions ]
image: assets/images/2023-01-26-leetcode-problem1-with-solutions/leetcode_meme1.png
beforetoc: "This post shows the problem statement and several solutions for leetcode Valid Parentheses problem"
toc: false
---

# [Valid Parentheses](https://leetcode.com/problems/longest-common-prefix)

## Description

Given an integer array nums, return all the triplets `[nums[i], nums[j], nums[k]]` such that `i != j`, `i != k`, and `j != k`, and `nums[i] + nums[j] + nums[k] == 0`.

Notice that the solution set must not contain duplicate triplets.

Example 1:
```
Input: nums = [-1,0,1,2,-1,-4]
Output: [[-1,-1,2],[-1,0,1]]
Explanation: 
nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0.
nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0.
nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0.
The distinct triplets are [-1,0,1] and [-1,-1,2].
Notice that the order of the output and the order of the triplets does not matter.
```
Example 2:
```
Input: nums = [0,1,1]
Output: []
Explanation: The only possible triplet does not sum up to 0.
```
Example 3:
```
Input: nums = [0,0,0]
Output: [[0,0,0]]
Explanation: The only possible triplet sums up to 0.
```

## Solutions
Let’s walk through the problem-solving process in Java, starting from a basic brute-force solution and gradually improving performance and readability.

### ✅ Brute Force (Time Complexity: O(n³), Space: O(1))
This approach checks every possible triplet and filters valid ones. It's not efficient but helps understand the problem structure.

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        Set<List<Integer>> result = new HashSet<>();
        int n = nums.length;

        for (int i = 0; i < n - 2; i++) {
            for (int j = i + 1; j < n - 1; j++) {
                for (int k = j + 1; k < n; k++) {
                    if (nums[i] + nums[j] + nums[k] == 0) {
                        List<Integer> triplet = Arrays.asList(nums[i], nums[j], nums[k]);
                        Collections.sort(triplet);
                        result.add(triplet);
                    }
                }
            }
        }

        return new ArrayList<>(result);
    }
}
```
This solution works but is too slow for large input sizes. Let's do better.

### 🚀 Optimized Using Sorting + Two Pointers (Time: O(n²), Space: O(n) for result)
We sort the array and fix one number, then use two pointers to find pairs that complete the triplet. This is the most commonly accepted optimal approach.

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        Arrays.sort(nums);

        for (int i = 0; i < nums.length - 2; i++) {
            if (i > 0 && nums[i] == nums[i - 1]) continue; // skip duplicates

            int j = i + 1;
            int k = nums.length - 1;

            while (j < k) {
                int total = nums[i] + nums[j] + nums[k];

                if (total < 0) {
                    j++;
                } else if (total > 0) {
                    k--;
                } else {
                    res.add(Arrays.asList(nums[i], nums[j], nums[k]));
                    j++;
                    k--;

                    while (j < k && nums[j] == nums[j - 1]) j++; // skip duplicates
                    while (j < k && nums[k] == nums[k + 1]) k--; // skip duplicates
                }
            }
        }

        return res;
    }
}
```
This solution is efficient and avoids duplicates cleanly using two-pointer logic after sorting.

### 🧩 Alternate with HashSet to Track Triplets (Time: O(n²), Space: O(n²))
We use a Set to track unique triplets and avoid manual duplicate removal.

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        Set<List<Integer>> result = new HashSet<>();
        Arrays.sort(nums);
        int n = nums.length;

        for (int i = 0; i < n - 2; i++) {
            int j = i + 1;
            int k = n - 1;

            while (j < k) {
                int sum = nums[i] + nums[j] + nums[k];

                if (sum == 0) {
                    result.add(Arrays.asList(nums[i], nums[j], nums[k]));
                    j++;
                    k--;
                } else if (sum < 0) {
                    j++;
                } else {
                    k--;
                }
            }
        }

        return new ArrayList<>(result);
    }
}
```
While this version is straightforward, using Set adds a bit of overhead, and you still sort beforehand. It’s a good balance between simplicity and performance.

Summary

<table style="border-collapse: collapse; width: 100%;">
  <thead>
    <tr>
      <th style="border: 1px solid black; padding: 8px;">Approach</th>
      <th style="border: 1px solid black; padding: 8px;">Time Complexity</th>
      <th style="border: 1px solid black; padding: 8px;">Space Complexity</th>
      <th style="border: 1px solid black; padding: 8px;">Notes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border: 1px solid black; padding: 8px;">Brute Force (Backtracking)</td>
      <td style="border: 1px solid black; padding: 8px;">O(n³)</td>
      <td style="border: 1px solid black; padding: 8px;">O(1)</td>
      <td style="border: 1px solid black; padding: 8px;">Simple but slow</td>
    </tr>
    <tr>
      <td style="border: 1px solid black; padding: 8px;">Two Pointers</td>
      <td style="border: 1px solid black; padding: 8px;">O(n)</td>
      <td style="border: 1px solid black; padding: 8px;">O(m * n)</td>
      <td style="border: 1px solid black; padding: 8px;">Best mix of speed and clarity</td>
    </tr>
    <tr>
      <td style="border: 1px solid black; padding: 8px;">HashSet Version</td>
      <td style="border: 1px solid black; padding: 8px;">O(n²)</td>
      <td style="border: 1px solid black; padding: 8px;">O(n²)</td>
      <td style="border: 1px solid black; padding: 8px;">Avoids duplicate logic manually</td>
    </tr>
  </tbody>
</table>