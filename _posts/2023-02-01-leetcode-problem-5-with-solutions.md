---
layout: post
title:  "Leetcode problem 5 - Longest Palindromic Substring | Problem and solution"
author: Laki
categories: [ leetcode, algorithm, interview questions ]
image: assets/images/2023-01-26-leetcode-problem1-with-solutions/leetcode_meme1.png
beforetoc: "This post shows the problem statement and several solutions for leetcode Longest Palindromic Substring problem"
toc: false
---

# [Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring/)

## Description
Given a string s, return the longest palindromic substring in s.

Note that palindrome string is a string which read same as from left to right and right to left.

ex: 
palindrome 

level
rotator

Non Palindrome

tree
table

### Example 1:

Input: s = "babad"
Output: "bab"
Explanation: "aba" is also a valid answer.

### Example 2:

Input: s = "cbbd"
Output: "bb"
 
### Constraints:

1 <= s.length <= 1000
s consist of only digits and English letters.

Solution

approach 1 : Brute force method

Check all possible substrings of the input string for palindroms and get the longest one amongst them

Approach 2 : 