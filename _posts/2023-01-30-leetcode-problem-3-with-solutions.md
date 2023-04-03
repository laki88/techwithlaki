---
layout: post
title:  "Leetcode problem 3 - Longest Substring Without Repeating Characters | Problem and solution"
author: Laki
categories: [ leetcode, algorithm, interview questions ]
image: assets/images/2023-01-30-leetcode-problem-3-with-solutions/longest_substring_without_repeating_characters.jpg
beforetoc: "This post shows the problem statement and several solutions for leetcode Longest Substring Without Repeating Characters problem"
toc: false
---

# [Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

## Description

Given a string s, find the length of the longest substring without repeating characters.

### Example 1:

Input: s = "cbfcbfbb"
Output: 3
Explanation: The answer is "cbf", with the length of 3.

### Example 2:

Input: s = "cccccc"
Output: 1
Explanation: The answer is "c", with the length of 1.

### Example 3:

Input: s = "qxxlfx"
Output: 3
Explanation: The answer is "xlf", with the length of 3.
Notice that the answer must be a substring, "qxlf" is a subsequence and not a substring.


## Solution

Approach 1 : Brute force solution

It can be checked, if there is any duplicate characters in all substring that can be produced using this string. 

1. Write a function to return all the substrings of input string
2. Write a function to return true if the substring doesn't have duplicate character, false otherwise

Approach 2 : Sliding window 

1. Keep two indices to track a substring of the input string
2. Increment end index until a duplicate character found. Hashset can be used to check if there is any duplicate.
3. Once a duplicate character found, update begin index so that it discards leftmost characters until there are no duplicate characters in substring.


Approach 3 : Optimized sliding window

Use the same approach as approach 2, but use a hash map to store the character as the key and it's index as the value so that it can be skip  