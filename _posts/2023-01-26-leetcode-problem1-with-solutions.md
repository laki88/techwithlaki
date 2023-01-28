---
layout: post
title:  "Leetcode problem 1 - Two Sum | Problem and solution"
author: Laki
categories: [ leetcode, algorithm ]
image: assets/images/4.jpg
beforetoc: "This post shows the problem statement and several solutions for leetcode two sum problem"
toc: false
---

# Two Sum

## Description

Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

You can return the answer in any order.

### Example 1:
``` 
Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Explanation: Because nums[0] + nums[1] == 9, we return [0, 1].
```
### Example 2:
```
Input: nums = [3,2,4], target = 6
Output: [1,2]
```
### Example 3:
```
Input: nums = [3,3], target = 6
Output: [0,1]
```
### Constraints:
```
2 <= nums.length <= 104
-109 <= nums[i] <= 109
-109 <= target <= 109
Only one valid answer exists.
 ```

## Brute force solution 

Brute force solutions would be to search for all possible pair of numbers so that add up to the target

Java Code : 
``` java
    public int[] twoSum(int[] nums, int target) {
        for (int i = 0; i < nums.length; i++) {
            for (int j = i + 1; j < nums.length; j++) {
                if (nums[j] + nums[i] == target) {
                    return new int[] { i, j };
                }
            }
        }
        // In case there is no solution, we'll just return null
        return null;
    }
```
### <ins> Complexity analysis </ins>

* Space Complexity : O(1)
* Time Complexity : O(n<sup>2</sup>)

## More solutions

1. Using two pointers
    
    1. Copy the values from original array to second array and store a data structure that holds the value and indices from original array in each indices
    2. sort the second array based on values
    3. keep two pointers at two ends of the second array
    4. move the pointer towered each other until the sum of values pointed become equal to target
### <ins> Complexity analysis </ins>

* Space Complexity : O(n)
* Time Complexity : O(n * log(n))

2. using Hashmap
    1. iterate the array and keep the (target - value) as the key  and index as the value in hashmap.
    2. if the hashmap has a value for current index, that means the index at the current value and index from hashmap is the solution.
### <ins> Complexity analysis </ins>

* Space Complexity : O(n)
* Time Complexity : O(n)


