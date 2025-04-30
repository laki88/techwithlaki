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

# Mastering the Longest Substring Without Repeating Characters Problem

## Problem Description

Imagine you're a treasure hunter exploring a string, searching for the longest sequence of unique characters. Your mission? Find the longest stretch where no character repeats. For example, in "abcabcbb0", the treasure is "abc"—a sequence of 3 unique characters. Can you write a program to uncover this for any string?

### Examples

1. Input: "cbfcbfbb"
   
   Output: 3
   
   Explanation: The longest substring without repeating characters is "cbf".

2. Input: "cccccc"
    
    Output: 1

    Explanation: Since all characters are the same, the longest substring is just "c".

3. Input: "qxxlfx"
   
   Output: 3

   Explanation: The longest substring is "xlf". Note: "qxlf" is a subsequence, not a substring, so it doesn’t count.

## Solution Approaches

Below are the four approaches to solve this problem, each explained in a way that’s easy to understand, even for beginners. We’ll use analogies to make the concepts stick!

### 1. Brute Force

**Time Complexity**: O(n³)

**Space Complexity**: O(n)

#### Explanation:
Picture yourself as a librarian checking every possible combination of books (substrings) on a shelf (the string) to find the longest set with no duplicate titles (characters). For each starting book, you check every possible ending book, ensuring all titles in between are unique. If they are, you note the length of that set and keep track of the longest one found.

This approach is simple but slow because you’re checking every possible combination, which grows rapidly for long strings. It’s like opening every box in a warehouse to find the biggest unique collection—exhaustive and time-consuming!


#### Solution Code in Java
``` java
public int bruteForce(String s) {
    int max = 0;
    for (int i = 0; i < s.length(); i++) {
        for (int j = i; j < s.length(); j++) {
            if (isUnique(s, i, j)) max = Math.max(max, j-i+1);
        }
    }
    return max;
}
private boolean isUnique(String s, int start, int end) {
    Set<Character> set = new HashSet<>();
    for (int i = start; i <= end; i++) {
        if (!set.add(s.charAt(i))) return false;
    }
    return true;
}
```

#### optimized brute force solution
``` java
public int bruteForce(String s) {
    int max = 0;
    for (int i = 0; i < s.length(); i++) {
        Set<Character> seen = new HashSet<>();
        for (int j = i; j < s.length(); j++) {
            if (!seen.add(s.charAt(j))) break;
            max = Math.max(max, j - i + 1);
        }
    }
    return max;
}
```
### 2. Sliding Window with HashSet

**Time Complexity**: O(n)

**Space Complexity**: O(k) (k ≤ 26 for letters)

#### Explanation:
Imagine you’re fishing in a river (the string) with two nets: a "left" net and a "right" net, defining your fishing area (window). You start with both nets at the river’s start. As you move the "right" net forward to catch more fish (characters), you keep track of them in a basket (HashSet), which only holds unique fish.

If you catch a fish you already have, you move the "left" net forward, releasing fish until the duplicate is gone. Each time you move the "right" net, you check if your current fishing area is the largest unique catch so far. This method is fast because each fish is caught and released at most once, making it efficient for long rivers!

#### Solution Code in Java

```java
public int slidingWindow(String s) {
    Set<Character> window = new HashSet<>();
    int left = 0, max = 0;
    
    for (int right = 0; right < s.length(); right++) {
        while (window.contains(s.charAt(right))) {
            window.remove(s.charAt(left++));
        }
        window.add(s.charAt(right));
        max = Math.max(max, right - left + 1);
    }
    return max;
}
```
### 3. Optimized Sliding Window with HashMap

**Time Complexity**: O(n)
**Space Complexity**: O(k) (k is the size of the character set)

#### Explanation:
This is like the fishing analogy, but instead of a basket, you use a logbook (HashMap) to note the exact spot (index) where you last caught each fish. When you catch a duplicate fish, you check your logbook and move the "left" net directly to just past where you caught that fish last time. This saves time by skipping over unnecessary fish.

Each time you move the "right" net, you update the logbook with the fish’s new position and check if your current fishing area is the longest unique catch. This approach is as fast as the HashSet method but can be smarter with duplicates, especially in strings with many repeated characters.

#### Solution Code in Java
```java
public int optimizedWindow(String s) {
    Map<Character, Integer> lastIndex = new HashMap<>();
    int left = 0, max = 0;
    
    for (int right = 0; right < s.length(); right++) {
        char c = s.charAt(right);
        if (lastIndex.containsKey(c)) {
            left = Math.max(left, lastIndex.get(c) + 1);
        }
        lastIndex.put(c, right);
        max = Math.max(max, right - left + 1);
    }
    return max;
}
```

### 4. Ultimate ASCII Optimization

**Time Complexity**: O(n)

**Space Complexity**: O(1)

#### Explanation:
Now, imagine you’re fishing in a river where only 128 types of fish (ASCII characters) exist. Instead of a basket or logbook, you use a fixed chart with 128 slots, one for each fish type, to mark the last spot you caught each one. When you catch a fish, you check its slot. If it’s been caught before, you move the "left" net to just past that spot.

This chart never grows beyond 128 slots, so it uses constant space, making it super efficient for strings with standard keyboard characters. It’s like having a perfect, compact map for your fishing adventure, ideal for competitive scenarios!

#### Solution Code in Java
```java
public int asciiSolution(String s) {
    int[] index = new int[128];
    Arrays.fill(index, -1);
    int left = 0, max = 0;
    
    for (int right = 0; right < s.length(); right++) {
        char c = s.charAt(right);
        if (index[c] >= left) {
            left = index[c] + 1;
        }
        index[c] = right;
        max = Math.max(max, right - left + 1);
    }
    return max;
}
```

## Complexity Breakdown

<table style="border: 1px solid black; border-collapse: collapse;">
  <thead>
    <tr>
      <th style="border: 1px solid black; padding: 8px;">Approach</th>
      <th style="border: 1px solid black; padding: 8px;">Time</th>
      <th style="border: 1px solid black; padding: 8px;">Space</th>
      <th style="border: 1px solid black; padding: 8px;">Best Use Case</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border: 1px solid black; padding: 8px;">Brute Force</td>
      <td style="border: 1px solid black; padding: 8px;">O(n³)</td>
      <td style="border: 1px solid black; padding: 8px;">O(n)</td>
      <td style="border: 1px solid black; padding: 8px;">Understanding fundamentals</td>
    </tr>
    <tr>
      <td style="border: 1px solid black; padding: 8px;">HashSet Window</td>
      <td style="border: 1px solid black; padding: 8px;">O(n)</td>
      <td style="border: 1px solid black; padding: 8px;">O(k)</td>
      <td style="border: 1px solid black; padding: 8px;">Interview explanations</td>
    </tr>
    <tr>
      <td style="border: 1px solid black; padding: 8px;">HashMap Optimized</td>
      <td style="border: 1px solid black; padding: 8px;">O(n)</td>
      <td style="border: 1px solid black; padding: 8px;">O(k)</td>
      <td style="border: 1px solid black; padding: 8px;">Most real-world scenarios</td>
    </tr>
    <tr>
      <td style="border: 1px solid black; padding: 8px;">ASCII Array</td>
      <td style="border: 1px solid black; padding: 8px;">O(n)</td>
      <td style="border: 1px solid black; padding: 8px;">O(1)</td>
      <td style="border: 1px solid black; padding: 8px;">Competitions/optimized code</td>
    </tr>
  </tbody>
</table>

#### Applications

This problem has practical uses in various fields:

- Stream Processing: Identifying unique sequences in data streams, like network traffic analysis.

- Genome Sequence Matching: Finding unique subsequences in DNA for genetic research.

- Browser History Analysis: Detecting unique navigation patterns for user behavior studies.

- Real-Time Plagiarism Detection: Spotting copied content in documents or code.

### Summary

In this article, we tackled the "Longest Substring Without Repeating Characters" problem, a classic coding interview challenge. We explored four approaches: the simple but slow Brute Force, the efficient Sliding Window with HashSet, the smarter Optimized Sliding Window with HashMap, and the compact Ultimate ASCII Optimization. Each method offers unique insights, from brute-force simplicity to optimized elegance, and they shine in real-world applications like stream processing and genome analysis.
