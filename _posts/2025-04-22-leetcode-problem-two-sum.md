---
layout: post
title:  "Leetcode problem 1 - Two Sum | Problem and solution"
author: Laki
categories: [ leetcode, algorithm, interview questions ]
image: assets/images/2023-01-26-leetcode-problem1-with-solutions/nature_sunrise.jpg
beforetoc: "This post shows the problem statement and several solutions for leetcode two sum problem"
toc: false
---

### [Two Sum](https://leetcode.com/problems/two-sum/)


## Problem Description

Picture yourself as a detective with a list of numbers and a mystery to solve: you need to find two numbers that add up to a specific target sum. But instead of just naming the numbers, you must pinpoint their exact positions (indices) in the list. There’s only one correct pair, and you can’t use the same number twice. Ready to crack this case?

For example, if your list is `[3, 1, 5, 6]` and the target is `7`, you’d return `[1, 3]` because `1` (at index `1`) and 6 (at index `3`) add up to `7`. This is the heart of the Two Sum problem—a classic coding challenge perfect for sharpening your problem-solving skills.

## Examples
Let’s break it down with some examples:

### 1. Example 1  

- Input: nums = [3, 1, 5, 6], target = 7  
- Output: [1, 3]  
- Why? Because nums[1] = 1 and nums[3] = 6, and 1 + 6 = 7.


### 2. Example 2  

- Input: nums = [5, 3, 8], target = 11  
- Output: [1, 2]  
- Why? Because nums[1] = 3 and nums[2] = 8, and 3 + 8 = 11.


### 3. Example 3  

- Input: nums = [5, 4], target = 9  
- Output: [0, 1]  
- Why? Because nums[0] = 5 and nums[1] = 4, and 5 + 4 = 9.



## Solutions
There are three main ways to solve this problem, each with its own approach. I’ll explain them in a way that’s easy to follow, even if you’re new to coding, and include Java code that’s been reviewed for correctness.
### 1. Brute Force
#### What’s the idea?
This is the simplest approach: check every possible pair of numbers in the list to see if they add up to the target. It’s like trying every combination on a lock until one works.

#### How it works:  

- Loop through the list, picking each number one by one.
- For each number, check it against every other number that comes after it.
- If their sum equals the target, return their indices.
- If no pair works, return an empty array (per LeetCode’s requirements).

Code:
```java  
class Solution {
    public int[] twoSum(int[] nums, int target) {
        int n = nums.length;
        for (int i = 0; i < n - 1; i++) {
            for (int j = i + 1; j < n; j++) {
                if (nums[i] + nums[j] == target) {
                    return new int[] {i, j};
                }
            }
        }
        return new int[] {}; // No solution found
    }
}
```

#### Code Review:  

- **Correctness**: The code checks all pairs and returns the correct indices when a match is found. It handles the “no solution” case by returning an empty array, as LeetCode assumes a solution exists.
- **Edge Cases**: Handles lists with at least two elements. If the input is invalid (e.g., fewer than two elements), LeetCode guarantees valid input, so no additional checks are needed.

**Time Complexity**: O(n²) – You’re checking up to `n*(n-1)/2` pairs for a list of size n.

**Space Complexity**: O(1) – Only a constant amount of extra space is used.  

**When to use**: Great for learning or tiny lists (e.g., 2–3 numbers). For larger lists, it’s too slow.

### 2. Two Pointers
#### What’s the idea?
This method sorts the list first, then uses two pointers to find the pair. It’s faster than brute force but requires extra work to track the original indices.
#### How it works:  

- Create a new array of pairs `(number, index)` to keep track of the original positions.
- Sort this array by the numbers (not the indices).
- Use two pointers: one at the start (left) and one at the end (right).
  - If `nums[left] + nums[right] == target`, return their original indices.
  - If the sum is too small, move left right (to get a larger number).
  - If the sum is too large, move right left (to get a smaller number).


Continue until the pointers meet or a solution is found.

Code:
```java  
class Solution {
    public int[] twoSum(int[] nums, int target) {
        int n = nums.length;
        // Create array of [number, index]
        int[][] numsWithIndex = new int[n][2];
        for (int i = 0; i < n; i++) {
            numsWithIndex[i] = new int[] {nums[i], i};
        }
        // Sort by number
        Arrays.sort(numsWithIndex, (a, b) -> Integer.compare(a[0], b[0]));
        
        int left = 0, right = n - 1;
        while (left < right) {
            int sum = numsWithIndex[left][0] + numsWithIndex[right][0];
            if (sum == target) {
                return new int[] {numsWithIndex[left][1], numsWithIndex[right][1]};
            } else if (sum < target) {
                left++;
            } else {
                right--;
            }
        }
        return new int[] {}; // No solution found
    }
}
```
#### Code Review:  

- **Correctness**: The code sorts the numbers and uses two pointers to find the pair, correctly mapping back to original indices.
- **Edge Cases**: Handles valid inputs as per LeetCode’s constraints. The sorting ensures the pointers work correctly.


**Time Complexity**: O(n log n) – Sorting takes O(n log n), and the two-pointer traversal is O(n).

**Space Complexity**: O(n) – Extra space is needed for the array of pairs.  

**When to use**:Use when sorting is okay and you’re comfortable with extra space. It’s faster than brute force but not as efficient as the next method.

### 3. Hash Map
#### What’s the idea?
This is the most efficient approach. It uses a hash map (like a dictionary) to store numbers and their indices, making it super fast to find pairs.

#### How it works:  

- As you go through the list, for each number:
  - Calculate `complement = target - current number`.
  - Check if `complement` is in the hash map.
  - If it is, you’ve found the pair! Return the index of `complement` (from the map) and the current index.
  - If not, add the current number and its index to the hash map.


You only need one pass through the list, and hash map lookups are very fast (O(1) on average).

#### Example Walkthrough:
For `nums = [3, 1, 5, 6]`, `target = 7`:  

- Start with an empty hash map: `{}`  
- Index `0`, number `3`: Is `7 - 3 = 4` in the map? No. Add `{3: 0}`.  
- Index `1`, number `1`: Is `7 - 1 = 6` in the map? No. Add `{3: 0, 1: 1}`.  
- Index `2`, number `5`: Is `7 - 5 = 2` in the map? No. Add `{3: 0, 1: 1, 5: 2}`.  
- Index `3`, number `6`: Is `7 - 6 = 1` in the map? Yes, at index `1`. Return `[1, 3]`.

Code:
```java  
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            int complement = target - nums[i];
            if (map.containsKey(complement)) {
                return new int[] {map.get(complement), i};
            }
            map.put(nums[i], i);
        }
        return new int[] {}; // No solution found
    }
}
```

#### Code Review:  

- **Correctness**: The code efficiently finds the pair using a hash map and handles the “no solution” case.
- **Edge Cases**: Properly handles duplicates (since we add to the map after checking) and valid inputs per LeetCode’s constraints.

**Time Complexity**: O(n) – One pass through the list, with O(1) average-time hash map operations.

**Space Complexity**: O(n) – The hash map may store up to n entries.  

#### When to use:
This is the go-to method for most cases. It’s fast, efficient, and straightforward once you understand hash maps.

### Strategy Comparison
Here’s how the three approaches stack up:


<table style="border-collapse: collapse; width: 100%;">
  <thead>
    <tr>
      <th style="border: 1px solid #ddd; padding: 8px; text-align: left;">Approach</th>
      <th style="border: 1px solid #ddd; padding: 8px; text-align: left;">Time Complexity</th>
      <th style="border: 1px solid #ddd; padding: 8px; text-align: left;">Space Complexity</th>
      <th style="border: 1px solid #ddd; padding: 8px; text-align: left;">When to Use</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Brute-Force</td>
      <td style="border: 1px solid #ddd; padding: 8px;">O(n²)</td>
      <td style="border: 1px solid #ddd; padding: 8px;">O(1)</td>
      <td style="border: 1px solid #ddd; padding: 8px;">Learning exercise, very small lists</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Two-Pointer</td>
      <td style="border: 1px solid #ddd; padding: 8px;">O(n log n)</td>
      <td style="border: 1px solid #ddd; padding: 8px;">O(n)</td>
      <td style="border: 1px solid #ddd; padding: 8px;">When sorting is acceptable</td>
    </tr>
    <tr>
      <td style="border: 1px solid #ddd; padding: 8px;">Hash-Map</td>
      <td style="border: 1px solid #ddd; padding: 8px;">O(n)</td>
      <td style="border: 1px solid #ddd; padding: 8px;">O(n)</td>
      <td style="border: 1px solid #ddd; padding: 8px;">Default choice for general use</td>
    </tr>
  </tbody>
</table>

## Summary
The Two Sum problem is a fantastic way to explore different problem-solving techniques. Here’s the rundown:

- **Brute Force**: Simple but slow, great for learning.
- **Two Pointers**: Faster than brute force but requires sorting and index tracking.
- **Hash Map**: The fastest and most efficient, perfect for most scenarios.

For most cases, go with the Hash Map approach—it’s quick and easy once you get the hang of hash maps.

Got another approach? Drop it in the comments!
