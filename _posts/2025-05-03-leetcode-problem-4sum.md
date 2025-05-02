---
layout: post
title:  "Leetcode problem 18 - 4Sum | Problem and solution"
author: Laki
categories: [ leetcode, algorithm, interview questions ]
image: assets/images/leetcode-problem-18/1.jpg
beforetoc: "This post shows the problem statement and several solutions for leetcode 4Sum problem"
toc: false
---

# [4Sum](https://leetcode.com/problems/3sum/)

## Description

Given an array nums of n integers, return an array of all the unique quadruplets `[nums[a], nums[b], nums[c], nums[d]]` such that:

- `0 <= a, b, c, d < n`
- `a`, `b`, `c`, and `d` are distinct.
- `nums[a] + nums[b] + nums[c] + nums[d] == target`

You may return the answer in any order.

 

Example 1:
```
Input: nums = [1,0,-1,0,-2,2], target = 0
Output: [[-2,-1,1,2],[-2,0,0,2],[-1,0,0,1]]
```
Example 2:
```
Input: nums = [2,2,2,2,2], target = 8
Output: [[2,2,2,2]]
```

## Solutions

### 🧠 Brute Force Solution (Time Limit Exceeded)
A natural starting point is the brute-force solution. We try every combination of 4 elements and check if their sum equals the target. This approach is simple but not efficient.

Java Code
```java
public List<List<Integer>> fourSumBruteForce(int[] nums, int target) {
    List<List<Integer>> result = new ArrayList<>();
    int n = nums.length;
    Set<List<Integer>> set = new HashSet<>();

    for (int i = 0; i < n - 3; i++) {
        for (int j = i + 1; j < n - 2; j++) {
            for (int k = j + 1; k < n - 1; k++) {
                for (int l = k + 1; l < n; l++) {
                    long sum = (long) nums[i] + nums[j] + nums[k] + nums[l];
                    if (sum == target) {
                        List<Integer> quad = Arrays.asList(nums[i], nums[j], nums[k], nums[l]);
                        Collections.sort(quad);
                        set.add(quad);
                    }
                }
            }
        }
    }
    result.addAll(set);
    return result;
}
```
Analysis
Time Complexity: O(n⁴)

Space Complexity: O(n), due to the hash set used to avoid duplicates.

### ⚙️ Two-Pointer Based Optimized Solution
Now, let’s optimize. Just like 3Sum uses two pointers in a sorted array, we extend the idea for 4Sum by fixing two numbers and using two pointers for the rest.

Java Code
```java
public List<List<Integer>> fourSumTwoPointer(int[] nums, int target) {
    List<List<Integer>> result = new ArrayList<>();
    Arrays.sort(nums);
    int n = nums.length;

    for (int i = 0; i < n - 3; i++) {
        if (i > 0 && nums[i] == nums[i - 1]) continue; // Skip duplicates
        for (int j = i + 1; j < n - 2; j++) {
            if (j > i + 1 && nums[j] == nums[j - 1]) continue; // Skip duplicates
            int left = j + 1, right = n - 1;
            while (left < right) {
                long sum = (long) nums[i] + nums[j] + nums[left] + nums[right];
                if (sum == target) {
                    result.add(Arrays.asList(nums[i], nums[j], nums[left], nums[right]));
                    while (left < right && nums[left] == nums[left + 1]) left++; // Skip duplicates
                    while (left < right && nums[right] == nums[right - 1]) right--; // Skip duplicates
                    left++;
                    right--;
                } else if (sum < target) {
                    left++;
                } else {
                    right--;
                }
            }
        }
    }
    return result;
}
```
Analysis
Time Complexity: O(n³)

Space Complexity: O(1) (excluding output list)

### 🚀 Generalized kSum Recursive Solution
The most elegant and flexible approach is to write a generalized kSum function, which works not only for 4Sum but for any k. This shows a deeper understanding of recursion and problem decomposition.

Java Code
```java
public List<List<Integer>> fourSum(int[] nums, int target) {
    Arrays.sort(nums);
    return kSum(nums, 0, 4, target);
}

private List<List<Integer>> kSum(int[] nums, int start, int k, long target) {
    List<List<Integer>> res = new ArrayList<>();
    if (start == nums.length) return res;

    // Pruning
    long average = target / k;
    if (nums[start] > average || nums[nums.length - 1] < average) return res;

    if (k == 2) return twoSum(nums, start, target);

    for (int i = start; i < nums.length; i++) {
        if (i > start && nums[i] == nums[i - 1]) continue;
        for (List<Integer> subset : kSum(nums, i + 1, k - 1, target - nums[i])) {
            subset.add(0, nums[i]);
            res.add(subset);
        }
    }
    return res;
}

private List<List<Integer>> twoSum(int[] nums, int start, long target) {
    List<List<Integer>> res = new ArrayList<>();
    int left = start, right = nums.length - 1;

    while (left < right) {
        long sum = (long) nums[left] + nums[right];
        if (sum == target) {
            res.add(Arrays.asList(nums[left], nums[right]));
            while (left < right && nums[left] == nums[left + 1]) left++;
            while (left < right && nums[right] == nums[right - 1]) right--;
            left++;
            right--;
        } else if (sum < target) {
            left++;
        } else {
            right--;
        }
    }
    return res;
}
```
Analysis
Time Complexity: O(n^(k-1)) = O(n³) for 4Sum

Space Complexity: O(k) for recursion depth

### 🧩 Conclusion
- Start simple: Brute-force helps clarify what the problem asks for.

- Use two pointers: Efficient and clean for fixed-size subsets like 3Sum, 4Sum.

- Generalize: kSum turns your solution into a reusable toolkit for similar problems.

If you found this useful, feel free to bookmark or share. Stay tuned for the next problem walkthrough!