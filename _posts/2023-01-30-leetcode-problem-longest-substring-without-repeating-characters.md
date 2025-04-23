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

## Unlocking the Secret to Finding the Longest Unique Substring
Have you ever wondered how to efficiently find the longest substring without repeating characters in a given string? Whether you're preparing for coding interviews or just love solving algorithmic challenges, this problem is a classic that tests your understanding of strings and efficient searching techniques. Today, we'll explore multiple approaches to solve this problem, complete with Java implementations.

## Description

Given a string s, find the length of the longest substring without repeating characters.

### Example 1:
```
Input: s = "cbfcbfbb"
Output: 3
Explanation: The answer is "cbf", with the length of 3.
```
### Example 2:
```
Input: s = "cccccc"
Output: 1
Explanation: The answer is "c", with the length of 1.
```
### Example 3:
```
Input: s = "qxxlfx"
Output: 3
Explanation: The answer is "xlf", with the length of 3.
Notice that the answer must be a substring, "qxlf" is a subsequence and not a substring.
```

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


## Solution Arsenal
### 1. Brute Force Approach
Time: O(n³)
Space: O(n)

The foundation for understanding better solutions. Checks all possible substrings:

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

optimized brute force solution
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
Time: O(2n) → O(n)
Space: O(k) (k = character set size, ≤26 for letters)

The classic sliding window approach used in 60% of interview solutions:

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
### 3. Optimized Sliding Window (HashMap)
Time: O(n)
Space: O(k)

Jump duplicates instantly using last-seen positions:

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
Time: O(n)
Space: O(1) (fixed 128-size array)

The fastest solution for competition coding:

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

## Why This Matters
#### Mastering these techniques helps with:
- Stream processing algorithms (financial transactions, IoT data)
- Genome sequence pattern matching
- Browser history analysis (longest unique URL sequences)
- Real-time plagiarism detection

Final Challenge: Try implementing the ASCII solution without looking - it's the version Google engineers optimize for in production systems!