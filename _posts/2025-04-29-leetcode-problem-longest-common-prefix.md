---
layout: post
title:  "Leetcode problem 14 - Longest Common Prefix | Problem and solution"
author: Laki
categories: [ leetcode, algorithm, interview questions ]
image: assets/images/leetcode-problem-14/1.jpg
beforetoc: "This post shows the problem statement and several solutions for leetcode Longest Common Prefix problem"
toc: false
---

# [Longest Common Prefix](https://leetcode.com/problems/longest-common-prefix)

## Description

Write a function to find the longest common prefix string amongst an array of strings.

If there is no common prefix, return an empty string "".

 

Example 1:
```
Input: strs = ["flower","flow","flight"]
Output: "fl"
```
Example 2:
```
Input: strs = ["dog","racecar","car"]
Output: ""
Explanation: There is no common prefix among the input strings.
```

### Solution 1: Brute Force (Prefix Shrinking)
This solution checks the prefix character by character, shrinking it if needed until it matches each word.

Algorithm
Start with the first word as the prefix.

For each other word, shorten the prefix until it matches the beginning of that word.

Stop early if the prefix becomes empty.

Java Code
```java
class Solution {
    public String longestCommonPrefix(String[] strs) {
        if (strs == null || strs.length == 0) return "";

        String prefix = strs[0];

        for (int i = 1; i < strs.length; i++) {
            while (!strs[i].startsWith(prefix)) {
                prefix = prefix.substring(0, prefix.length() - 1);
                if (prefix.isEmpty()) return "";
            }
        }

        return prefix;
    }
}
```
Complexity
Time: O(n * m), where n is the number of strings and m is the average length of the prefix.

Space: O(1)

### Solution 2: Sorting-Based Optimization
Sort the array lexicographically and compare only the first and last strings, which will differ the most.

Intuition
The common prefix of the entire array must be shared between the first and last strings after sorting.

Java Code
```java
import java.util.Arrays;

class Solution {
    public String longestCommonPrefix(String[] strs) {
        if (strs.length == 0) return "";

        Arrays.sort(strs);
        String first = strs[0];
        String last = strs[strs.length - 1];
        int index = 0;

        while (index < first.length() && index < last.length()) {
            if (first.charAt(index) != last.charAt(index)) break;
            index++;
        }

        return first.substring(0, index);
    }
}
```
Complexity
Time: O(n log n + m), due to sorting and prefix comparison.

Space: O(1)

Solution 3: Vertical Scanning (Column-wise Comparison)
Instead of comparing entire strings, we scan character by character across all strings at each index.

Algorithm
For each character index, check if all strings have the same character.

Stop at the first mismatch.

Java Code
```java
class Solution {
    public String longestCommonPrefix(String[] strs) {
        if (strs == null || strs.length == 0) return "";

        for (int i = 0; i < strs[0].length(); i++) {
            char c = strs[0].charAt(i);
            for (int j = 1; j < strs.length; j++) {
                if (i >= strs[j].length() || strs[j].charAt(i) != c) {
                    return strs[0].substring(0, i);
                }
            }
        }

        return strs[0];
    }
}
```
Complexity
Time: O(n * m)

Space: O(1)

### Conclusion
Each solution brings a different trade-off:

Brute force prefix shrinking is intuitive and readable.

Sorting-based is elegant and efficient when sorting cost is acceptable.

Vertical scanning avoids sorting and works well when strings are similar in length.

Choose the one that fits your constraints or interview expectations best.