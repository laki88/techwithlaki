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

When an input integer array and a target integer some given, return the indices of two elements from array that adds up to the target integer. Result indices can be in any order.

It can be assumed that there is no more than one solution to this problem and same element cannot be used twice.

### Example 1:
``` 
Input: nums = [3,1,5,6], target = 7
Output: [1,3]
Explanation: Because nums[1] + nums[6] == 7, we return [1, 3].
```
### Example 2:
```
Input: nums = [5,3,8], target = 11
Output: [1,2]
Explanation: Because nums[1] + nums[2] == 11, we return [1,2].
```
### Example 3:
```
Input: nums = [5,4], target = 9
Output: [0,1]
Explanation: Because nums[0] + nums[1] == 9, we return [0,1].
```

## Brute force solution 

Brute force solutions would be to search for all possible pair of numbers so that add up to the target

Java Code: 
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

* Space Complexity : Since there is no additional space used space complexity would be constant. O(1)
* Time Complexity : Since we iterate the array twice time complexity would be O(n<sup>2</sup>)

## Better solutions

### 1. Using two pointers
    
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

* Space Complexity : Since we use an additional array that is same length as input array to hold the Pair objects space complexity would be O(n).
* Time Complexity : Since we have to sort the array, time complexity would be O(n * log(n))

### 2. Using Hashmap
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

* Space Complexity : Since a hashmap grows to the same size as input array space complexity would be O(n).
* Time Complexity : Since we need to iterate the whole input array for one time time comlexity would be O(n).


## Strategy Comparison

| Approach    | Time Complexity | Space Complexity | When to Use                     |
|-------------|------------------|-------------------|----------------------------------|
| Brute-Force | O(n²)            | O(1)              | Learning exercise, very small n |
| Two-Pointer | O(n log n)       | O(n)              | When sorting is acceptable      |
| Hash-Map    | O(n)             | O(n)              | Default “go-to” for general use |

Got another approach? Drop it in the comments!
