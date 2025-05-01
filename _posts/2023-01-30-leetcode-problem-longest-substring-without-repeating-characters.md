---
layout: post
title:  "Leetcode problem 3 - Longest Substring Without Repeating Characters | Problem and solution"
author: Laki
categories: [ leetcode, algorithm, interview questions ]
image: assets/images/2023-01-30-leetcode-problem-3-with-solutions/longest_substring_without_repeating_characters.jpg
beforetoc: "This post shows the problem statement and several solutions for leetcode Longest Substring Without Repeating Characters problem"
toc: false
---

### [Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

## Problem Description

Imagine you're a treasure hunter exploring a string, searching for the longest sequence of unique characters. Your mission? Find the longest stretch where no character repeats. For example, in "abcabcbb", the treasure is "abc"—a sequence of 3 unique characters. Can you write a program to uncover this for any string?

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

Below are the four approaches to solve this problem, each explained in a way that’s easy to understand, even for beginners.

### 1. Brute Force

**Time Complexity**: O(n³)

**Space Complexity**: O(n)

### Explanation:

**What it does**: Check every possible part of the word to see if all its characters are unique. Keep track of the longest part that works.

**How it works**:

1. Start at each character in the word.

2. For each starting point, look at parts that end at every character after it.

3. For each part, check if all characters are unique by keeping a list of seen characters.

4. If the part has all unique characters, compare its length to the longest you’ve found.

5. Repeat for all starting points.

**Example with "abcabcbb":**

- Start at "a":

  - Part "a": Unique, length 1.
 
  - Part "ab": Unique, length 2.

  - Part "abc": Unique, length 3.

  - Part "abca": "a" repeats, not unique.

- Start at "b":

  - Part "b": Unique, length 1.

  - Part "bc": Unique, length 2.

  - Part "bca": Unique, length 3.

- Continue for all starting points.

- Longest part is "abc" (or "bca"), length 3.

**Speed**: Slow, because it checks every possible part. For a word with (n) characters, it takes about (n^3) steps.


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

### Explanation:

**What it does**: Use two pointers to create a "window" of characters. Expand the window by adding new characters. If you find a repeat, shrink the window until the repeat is gone.

**How it works**:

1. Use two pointers: "left" and "right", both starting at the beginning.

2. Use a set to keep track of characters in the current window.

3. Move "right" to add a new character:

   - If the character isn’t in the set, add it and check if the window is the longest so far.

   - If the character is in the set, move "left" forward, removing characters until the repeat is gone.

4. Repeat until "right" reaches the end.

**Example with "abcabcbb":**

- Left = 0, Right = 0, Set = {}.

- Right = 0: Add "a". Set = {"a"}. Window = "a", length 1.

- Right = 1: Add "b". Set = {"a", "b"}. Window = "ab", length 2.

- Right = 2: Add "c". Set = {"a", "b", "c"}. Window = "abc", length 3.

- Right = 3: "a" is in set. Remove "a" (left = 1). Set = {"b", "c"}. Add "a". Window = "bca", length 3.

- Continue until right reaches the end.

- Longest length is 3.

**Speed**: Fast, takes about (n) steps, as each character is added and removed at most once.

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

### Explanation:
**What it does**: Like the HashSet method, but uses a map to store the last position of each character, so you can jump directly to where a repeat was last seen.

**How it works**:

1. Use two pointers: "left" and "right", starting at the beginning.

2. Use a map to store each character and its most recent position.

3. Move "right" to add a new character:

    - If the character is in the map and its last position is at or after "left", move "left" to just past that position.

    - Update the character’s position in the map.

4. Check if the current window is the longest so far.

5. Repeat until "right" reaches the end.

**Example with "abcabcbb":**

- Left = 0, Right = 0, Map = {}.

- Right = 0: Add "a" at 0. Map = {"a": 0}. Window = "a", length 1.

- Right = 1: Add "b" at 1. Map = {"a": 0, "b": 1}. Window = "ab", length 2.

- Right = 2: Add "c" at 2. Map = {"a": 0, "b": 1, "c": 2}. Window = "abc", length 3.

- Right = 3: "a" is in map at 0. Move left to 1. Map = {"a": 3, "b": 1, "c": 2}. Window = "bca", length 3.

- Continue until right reaches the end.

- Longest length is 3.

**Speed**: Also takes about (n) steps, slightly faster than HashSet for strings with many repeats.

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

### Explanation:

**What it does**: Like the HashMap method, but uses a fixed-size array for ASCII characters, which is very efficient for standard keyboard characters.

**How it works**:

1. Use two pointers: "left" and "right", starting at the beginning.

2. Use an array of size 128 (for ASCII characters) to store the last position of each character.

3. Move "right" to add a new character:

    - If the character has a position in the array, move "left" to just past that position.

    - Update the character’s position in the array.

4. Check if the current window is the longest so far.

5. Repeat until "right" reaches the end.

**Example with "abcabcbb":**

- Left = 0, Right = 0, Array = [-1] * 128.

- Right = 0: "a" (ASCII 97) at 0. Array[97] = 0. Window = "a", length 1.

- Right = 1: "b" (ASCII 98) at 1. Array[98] = 1. Window = "ab", length 2.

- Right = 2: "c" (ASCII 99) at 2. Array[99] = 2. Window = "abc", length 3.

- Right = 3: "a" at 0. Move left to 1. Array[97] = 3. Window = "bca", length 3.

- Continue until right reaches the end.

- Longest length is 3.

**Speed**: Takes about (n) steps, with constant space for ASCII characters.

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
