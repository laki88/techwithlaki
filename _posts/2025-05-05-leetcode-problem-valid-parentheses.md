---
layout: post
title:  "Leetcode problem 20 - Valid Parentheses | Problem and solution"
author: Laki
categories: [ leetcode, algorithm, interview questions ]
image: assets/images/2023-01-26-leetcode-problem1-with-solutions/leetcode_meme1.png
beforetoc: "This post shows the problem statement and several solutions for leetcode Valid Parentheses problem"
toc: false
---

# [Valid Parentheses](https://leetcode.com/problems/valid-parentheses)

## Description

Given a string s containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.

An input string is valid if:

Open brackets must be closed by the same type of brackets.
Open brackets must be closed in the correct order.
Every close bracket has a corresponding open bracket of the same type.
 

Example 1:
```
Input: s = "()"

Output: true
```
Example 2:
```
Input: s = "()[]{}"

Output: true
```
Example 3:
```
Input: s = "(]"

Output: false
```
Example 4:
```
Input: s = "([])"

Output: true
```
 

Constraints:

- 1 <= s.length <= 104
- s consists of parentheses only '()[]{}'.

## Solutions
