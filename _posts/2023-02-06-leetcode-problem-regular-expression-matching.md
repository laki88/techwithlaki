---
layout: post
title:  "Leetcode problem 10 - Regular Expression Matching | Problem and solution"
author: Laki
categories: [ leetcode, algorithm, interview questions ]
image: assets/images/leetcode-problem-10/1.jpg
beforetoc: "This post shows the problem statement and several solutions for leetcode Regular Expression Matching problem"
toc: false
---

# [Regular Expression Matching](https://leetcode.com/problems/regular-expression-matching/)

## Description

Given an input string s and a pattern p, implement regular expression matching with support for '.' and '*' where:

'.' Matches any single character.​​​​
'*' Matches zero or more of the preceding element.
The matching should cover the entire input string (not partial).

### Example 1:

Input: s = "aa", p = "a"
Output: false
Explanation: "a" does not match the entire string "aa".

### Example 2:

Input: s = "aa", p = "a*"
Output: true
Explanation: '*' means zero or more of the preceding element, 'a'. Therefore, by repeating 'a' once, it becomes "aa".

### Example 3:

Input: s = "ab", p = ".*"
Output: true
Explanation: ".*" means "zero or more (*) of any character (.)".
 

### Constraints:

1 <= s.length <= 20
1 <= p.length <= 30
s contains only lowercase English letters.
p contains only lowercase English letters, '.', and '*'.
It is guaranteed for each appearance of the character '*', there will be a previous valid character to match.


# Approaches and Solutions

Let's work our way from basic ideas to an optimized solution.

---

## 1. Brute Force (Backtracking)

### Intuition
At the simplest level, we can think recursively:  
- If pattern character is `.` or matches the current string character, move both pointers forward.
- If pattern character is `*`, try two options:
  - Skip the `*` and its preceding element (zero occurrence).
  - If previous character matches, consume one character from the string and stay on `*`.

However, this approach quickly becomes inefficient due to redundant checks.

### Code (Java)

```java
public class Solution {
    public boolean isMatch(String s, String p) {
        if (p.isEmpty()) return s.isEmpty();

        boolean firstMatch = (!s.isEmpty() &&
                              (s.charAt(0) == p.charAt(0) || p.charAt(0) == '.'));

        if (p.length() >= 2 && p.charAt(1) == '*') {
            return (isMatch(s, p.substring(2)) ||
                   (firstMatch && isMatch(s.substring(1), p)));
        } else {
            return firstMatch && isMatch(s.substring(1), p.substring(1));
        }
    }
}
```
Time Complexity
Exponential in worst case.

2. Top-Down DP (Memoization)
Intuition
We can improve the brute force by caching results for string and pattern positions already computed.

Code (Java)
```java
import java.util.HashMap;
import java.util.Map;

public class Solution {
    private Map<String, Boolean> memo = new HashMap<>();

    public boolean isMatch(String s, String p) {
        String key = s + "," + p;
        if (memo.containsKey(key)) {
            return memo.get(key);
        }

        boolean result;
        if (p.isEmpty()) {
            result = s.isEmpty();
        } else {
            boolean firstMatch = (!s.isEmpty() &&
                                  (s.charAt(0) == p.charAt(0) || p.charAt(0) == '.'));

            if (p.length() >= 2 && p.charAt(1) == '*') {
                result = (isMatch(s, p.substring(2)) ||
                          (firstMatch && isMatch(s.substring(1), p)));
            } else {
                result = firstMatch && isMatch(s.substring(1), p.substring(1));
            }
        }

        memo.put(key, result);
        return result;
    }
}
```
Time Complexity
O(m * n) where m = s.length() and n = p.length().

3. Bottom-Up DP (2D Table)
Intuition
We define a 2D array dp[i][j]:

dp[i][j] = whether s.substring(0, i) matches p.substring(0, j).

Build the solution from smallest substrings to the entire string.

Key Points
dp[0][0] = true (empty string matches empty pattern).

Fill first row carefully for patterns like a*, a*b* which can match an empty string.

Handle cases for:

Normal character match or ..

* matches zero or more preceding characters.

Code (Java)
```java
public class Solution {
    public boolean isMatch(String s, String p) {
        int m = s.length(), n = p.length();
        boolean[][] dp = new boolean[m + 1][n + 1];
        dp[0][0] = true;

        // Fill first row for patterns like "a*", "a*b*"
        for (int j = 2; j <= n; j++) {
            if (p.charAt(j - 1) == '*') {
                dp[0][j] = dp[0][j - 2];
            }
        }

        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (p.charAt(j - 1) == '.' || p.charAt(j - 1) == s.charAt(i - 1)) {
                    dp[i][j] = dp[i - 1][j - 1];
                } else if (p.charAt(j - 1) == '*') {
                    dp[i][j] = dp[i][j - 2] || 
                              ((p.charAt(j - 2) == '.' || p.charAt(j - 2) == s.charAt(i - 1)) && dp[i - 1][j]);
                }
            }
        }

        return dp[m][n];
    }
}
```
Time and Space Complexity
Time: O(m * n)

Space: O(m * n)

Summary

<table style="border-collapse: collapse; width: 100%;">
  <thead>
    <tr>
      <th style="border: 1px solid black; padding: 8px;">Approach</th>
      <th style="border: 1px solid black; padding: 8px;">Time Complexity</th>
      <th style="border: 1px solid black; padding: 8px;">Space Complexity</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border: 1px solid black; padding: 8px;">Brute Force (Backtracking)</td>
      <td style="border: 1px solid black; padding: 8px;">Exponential</td>
      <td style="border: 1px solid black; padding: 8px;">Exponential (recursion stack)</td>
    </tr>
    <tr>
      <td style="border: 1px solid black; padding: 8px;">Top-Down DP (Memoization)</td>
      <td style="border: 1px solid black; padding: 8px;">O(m * n)</td>
      <td style="border: 1px solid black; padding: 8px;">O(m * n)</td>
    </tr>
    <tr>
      <td style="border: 1px solid black; padding: 8px;">Bottom-Up DP (Tabulation)</td>
      <td style="border: 1px solid black; padding: 8px;">O(m * n)</td>
      <td style="border: 1px solid black; padding: 8px;">O(m * n)</td>
    </tr>
  </tbody>
</table>

For interviews, the Bottom-Up DP approach is usually preferred for its clarity and reliability.

Top-Down with Memoization is also good if you are more comfortable with recursion.