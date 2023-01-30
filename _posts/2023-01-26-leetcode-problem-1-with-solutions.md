---
layout: post
title:  "Leetcode problem 1 - Two Sum | Problem and solution"
author: Laki
categories: [ leetcode, algorithm, interview questions ]
image: assets/images/2023-01-26-leetcode-problem1-with-solutions/leetcode_meme1.png
beforetoc: "This post shows the problem statement and several solutions for leetcode two sum problem"
toc: false
---

# [Two Sum](https://leetcode.com/problems/two-sum/)

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

## Better solutions

1. Using two pointers
    
    1. Copy the values from original array to second array and store a data structure that holds the value, index pair from original array in second array.
    2. Sort the second array based on values
    3. Keep two pointers at two ends of the second array
    4. Move the pointers towards each other until the sum of values pointed become equal to target

``` java
    public int[] twoSum(int[] nums, int target) {
        Pair[] paired = new Pair[nums.length]; // This will hold the value and index pair of input array
        int[] result =  new int[2];
        for (int i = 0; i < nums.length;i++) {
            paired[i] = new Pair(nums[i], i);
        }

        Arrays.sort(paired, Comparator.comparingInt(o -> o.value)); // Sort the array based on value

        int p1 = 0, p2 = paired.length - 1;

        while (p1 < p2) {
            int sum  = paired[p1].value + paired[p2].value;
            if (sum == target) {
                result[0] = paired[p1].index;
                result[1] = paired[p2].index;
                return result;
            } else if (sum < target) {
                p1++;
            } else {
                p2--;
            }
        }
        return null;
    }

    class Pair {
        int value;
        int index;
        Pair(int value, int index) {
            this.value = value;
            this.index = index;
        }
    }
```
### <ins> Complexity analysis </ins>

* Space Complexity : O(n)
* Time Complexity : O(n * log(n))

2. Using Hashmap
    1. Iterate the array and keep the (target - value) as the key  and index as the value in hashmap, if the hashmap doesn't have a value for current index
    2. If the hashmap has a value for current index, that means the index at the current value and index from hashmap is the solution.

``` java
   public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> tracker = new HashMap<>();
        int[] result = new int[2];
        for (int i = 0; i < nums.length; i++) {
            if (tracker.get(nums[i]) != null) {
                result[0] = i;
                result[1] = tracker.get(nums[i]);
                return result;
            } else {
                tracker.put(target - nums[i], i);
            }

        }
        return result;
    }
```
### <ins> Complexity analysis </ins>

* Space Complexity : O(n)
* Time Complexity : O(n)

Please let me know if anything unclear in comment.
