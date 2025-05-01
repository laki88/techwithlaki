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


### Problem Description

Picture yourself as a word detective, tasked with finding a hidden treasure in a string of letters. Your mission? Uncover the longest substring that reads the same forwards and backwards—a **palindrome**! For example, in the string `babad`, both `bab` and `aba` are palindromes, each 3 characters long, making them the longest palindromic substrings. In `cbbd`, the longest palindrome is `bb`, with 2 characters. Your goal is to create a function that takes any string and returns its longest palindromic substring.

A palindrome is a sequence that’s identical when reversed, like `racecar` or `level`. A substring is a contiguous chunk of characters within the string. The challenge is to find the longest substring that’s also a palindrome.

### Examples

- Example 1:
  
  Input: s = "babad"
  
  Output: "bab" (or "aba", both are valid)
  
  Explanation: "bab" and "aba" are the longest palindromic substrings, each with length 3.



- Example 2:
  
  Input: s = "cbbd"
  
  Output: "bb"
  
  Explanation: "bb" is the longest palindromic substring, with length 2.


### Constraints

- String length is between 1 and 1000.

- The string contains only digits and English letters.

Let’s explore how to crack this puzzle with different approaches!

## Solutions

There are several ways to solve this problem, each with its own pros and cons. Below, I break down five approaches in a way that’s easy to grasp, even if you’re new to coding. Think of these as different strategies to find the treasure in our word detective game.

### 1. Brute Force

- **Idea**: Like checking every room in a house for treasure, this approach examines every possible substring to see if it’s a palindrome, keeping track of the longest one found.

- **How it Works:**

  - A string of length n has about n*(n+1)/2 substrings (e.g., for "abc", you check "a", "b", "c", "ab", "bc", "abc").

  - For each substring, verify if it’s a palindrome by comparing it to its reverse.

  - If it’s a palindrome and longer than the current longest, update the record.


Java Code:

```java
public class Solution {
    public String longestPalindrome(String s) {
        if (s.length() <= 1) {
            return s;
        }

        int maxLen = 1;
        String maxStr = s.substring(0, 1);

        for (int i = 0; i < s.length(); i++) {
            for (int j = i + maxLen; j <= s.length(); j++) {
                if (j - i > maxLen && isPalindrome(s.substring(i, j))) {
                    maxLen = j - i;
                    maxStr = s.substring(i, j);
                }
            }
        }

        return maxStr;
    }

    private boolean isPalindrome(String str) {
        int left = 0;
        int right = str.length() - 1;

        while (left < right) {
            if (str.charAt(left) != str.charAt(right)) {
                return false;
            }
            left++;
            right--;
        }

        return true;
    }
}
```

- Pros: Super straightforward — no complex logic needed.

- Cons: Very slow for long strings. With O(n^2) substrings and O(n) time to check each, the time complexity is O(n^3).

- Example: For "babad", you check "b", "ba", "bab", "baba", "babad", "a", "ab", "aba", etc., finding "bab" or "aba" as the longest.

- Time Complexity: O(n^3)

- Space Complexity: O(1)

### 2. Expand Around Center

**Idea:** Imagine each character (or gap between characters) as the center of a potential palindrome. Expand outwards like ripples in a pond, checking if the characters on both sides match.

**How it Works:**

- There are 2n-1 possible centers:

  - n centers for odd-length palindromes (each character).

  - n-1 centers for even-length palindromes (between consecutive characters).

For each center, expand outwards as long as the characters match.

Track the longest palindrome found.

Java Code:

```java
public class Solution {
    public String longestPalindrome(String s) {
        if (s.length() <= 1) {
            return s;
        }

        String maxStr = s.substring(0, 1);

        for (int i = 0; i < s.length() - 1; i++) {
            String odd = expandFromCenter(s, i, i);
            String even = expandFromCenter(s, i, i + 1);

            if (odd.length() > maxStr.length()) {
                maxStr = odd;
            }
            if (even.length() > maxStr.length()) {
                maxStr = even;
            }
        }

        return maxStr;
    }

    private String expandFromCenter(String s, int left, int right) {
        while (left >= 0 && right < s.length() && s.charAt(left) == s.charAt(right)) {
            left--;
            right++;
        }
        return s.substring(left + 1, right);
    }
}
```


- Pros: Faster than brute force and easy to code.

- Cons: Still O(n^2) time, as there are O(n) centers, and each expansion can take O(n) time.

- Example: For "babad":

  - Center at "b" (index 0): Expands to "b" (length 1).

  - Center at "a" (index 1): Expands to "aba" (length 3).

  - Center at "b" (index 2): Expands to "bab" (length 3).

  - The longest is "bab" or "aba".

- Time Complexity: O(n^2)

- Space Complexity: O(1)

### 3. Dynamic Programming

**Idea:** Build a table to systematically track which substrings are palindromes, like filling out a detective’s notebook with clues.

**How it Works:**

- Create a 2D table dp where dp[i][j] is true if the substring from index i to j is a palindrome.

- Base cases: Single characters (i == j) are palindromes, and two identical characters (s[i] == s[i+1]) are palindromes.

- For longer substrings, dp[i][j] is true if:

    - s[i] == s[j] (first and last characters match), and

    - dp[i+1][j-1] is true (the substring between is a palindrome).

- Fill the table for all substring lengths, from 1 to n.

```java
public class Solution {
    public String longestPalindrome(String s) {
        if (s.length() <= 1) {
            return s;
        }

        int maxLen = 1;
        int start = 0;
        int end = 0;
        boolean[][] dp = new boolean[s.length()][s.length()];

        for (int i = 0; i < s.length(); ++i) {
            dp[i][i] = true;
            for (int j = 0; j < i; ++j) {
                if (s.charAt(j) == s.charAt(i) && (i - j <= 2 || dp[j + 1][i - 1])) {
                    dp[j][i] = true;
                    if (i - j + 1 > maxLen) {
                        maxLen = i - j + 1;
                        start = j;
                        end = i;
                    }
                }
            }
        }

        return s.substring(start, end + 1);
    }
}
```

- Pros: Organized and reliable.

- Cons: Uses O(n^2) space for the table, which can be a drawback for very long strings.

- Example: For "babad":

    - dp[0][0] = true ("b").

    - dp[1][3] = true ("aba"), since s[1] == s[3] and dp[2][2] is true.

- Time Complexity: O(n^2)

- Space Complexity: O(n^2)

### 4. Manacher’s Algorithm

**Idea**: A super-smart approach that finds palindromes in linear time, like a detective with a high-tech gadget that scans the string efficiently.

**How it Works:**

- Preprocess the string by inserting special characters (e.g., "#" between characters) to handle even-length palindromes (e.g., "babad" becomes "b#a#b#a#d").

- Use an array P where P[i] stores the radius of the palindrome centered at position i.

- Leverage previously computed P values to compute new ones quickly, avoiding redundant checks.


Java Code:

```java
public class Solution {
    public String longestPalindrome(String s) {
        if (s.length() <= 1) {
            return s;
        }

        // Transform s with special markers
        StringBuilder sb = new StringBuilder();
        sb.append("#");
        for (char c : s.toCharArray()) {
            sb.append(c);
            sb.append("#");
        }
        String transformed = sb.toString();

        int[] dp = new int[transformed.length()];
        int center = 0, right = 0;
        int maxLen = 0, maxCenter = 0;

        for (int i = 0; i < transformed.length(); i++) {
            int mirror = 2 * center - i;

            if (i < right) {
                dp[i] = Math.min(right - i, dp[mirror]);
            }

            int leftExp = i - (dp[i] + 1);
            int rightExp = i + (dp[i] + 1);
            while (leftExp >= 0 && rightExp < transformed.length() 
                   && transformed.charAt(leftExp) == transformed.charAt(rightExp)) {
                dp[i]++;
                leftExp--;
                rightExp++;
            }

            if (i + dp[i] > right) {
                center = i;
                right = i + dp[i];
            }

            if (dp[i] > maxLen) {
                maxLen = dp[i];
                maxCenter = i;
            }
        }

        int start = (maxCenter - maxLen) / 2;
        return s.substring(start, start + maxLen);
    }
}
```

- Pros: The fastest solution, with O(n) time complexity.

- Cons: More complex to understand and implement.

- Example: For "babad", the algorithm computes palindrome radii for each position in the preprocessed string, identifying "bab" or "aba" as the longest.

- Time Complexity: O(n)

- Space Complexity: O(n)

### 5. Recursive

**Idea**: Recursively explore all possible substrings to check if they’re palindromes, like a detective retracing steps through every possible path.

**How it Works:**

- For each starting index i, recursively check all substrings starting at i.

- Base case: A single character is a palindrome.

- Recursive case: A substring from i to j is a palindrome if s[i] == s[j] and the substring from i+1 to j-1 is a palindrome.

Java Code:

```java
public class Solution {
    public String longestPalindrome(String s) {
        if (isPalindrome(s)) {
            return s;
        }

        String left = longestPalindrome(s.substring(1));
        String right = longestPalindrome(s.substring(0, s.length() - 1));

        return left.length() > right.length() ? left : right;
    }

    private boolean isPalindrome(String s) {
        int left = 0, right = s.length() - 1;
        while (left < right) {
            if (s.charAt(left++) != s.charAt(right--)) {
                return false;
            }
        }
        return true;
    }
}
```

- Pros: Intuitive for learning recursion.

- Cons: As slow as brute force (O(n^3)) and can cause stack overflow for long strings.

- Note: This is included for educational purposes but isn’t practical for real-world use.

- Time Complexity: O(n^3)

- Space Complexity: O(n) (due to recursion stack)

## Summary

The "Longest Palindromic Substring" problem is a fascinating challenge that tests your string manipulation skills. Here’s a quick overview of the solutions:

<table style="border-collapse: collapse; width: 100%;">
  <thead>
    <tr>
      <th style="border: 1px solid black; padding: 8px;">Approach</th>
      <th style="border: 1px solid black; padding: 8px;">Time Complexity</th>
      <th style="border: 1px solid black; padding: 8px;">Space Complexity</th>
      <th style="border: 1px solid black; padding: 8px;">Best For</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border: 1px solid black; padding: 8px;">Brute Force</td>
      <td style="border: 1px solid black; padding: 8px;">O(n³)</td>
      <td style="border: 1px solid black; padding: 8px;">O(1)</td>
      <td style="border: 1px solid black; padding: 8px;">Small strings, learning</td>
    </tr>
    <tr>
      <td style="border: 1px solid black; padding: 8px;">Expand Around Center</td>
      <td style="border: 1px solid black; padding: 8px;">O(n²)</td>
      <td style="border: 1px solid black; padding: 8px;">O(1)</td>
      <td style="border: 1px solid black; padding: 8px;">General use, simplicity</td>
    </tr>
    <tr>
      <td style="border: 1px solid black; padding: 8px;">Dynamic Programming</td>
      <td style="border: 1px solid black; padding: 8px;">O(n²)</td>
      <td style="border: 1px solid black; padding: 8px;">O(n²)</td>
      <td style="border: 1px solid black; padding: 8px;">Systematic approach</td>
    </tr>
    <tr>
      <td style="border: 1px solid black; padding: 8px;">Manacher’s Algorithm</td>
      <td style="border: 1px solid black; padding: 8px;">O(n)</td>
      <td style="border: 1px solid black; padding: 8px;">O(n)</td>
      <td style="border: 1px solid black; padding: 8px;">Large strings, efficiency</td>
    </tr>
    <tr>
      <td style="border: 1px solid black; padding: 8px;">Recursive</td>
      <td style="border: 1px solid black; padding: 8px;">O(n^3)</td>
      <td style="border: 1px solid black; padding: 8px;">O(n)</td>
      <td style="border: 1px solid black; padding: 8px;">Educational purposes</td>
    </tr>
  </tbody>
</table>



For most cases, the `Expand Around Center` approach is a great balance of simplicity and performance. If you’re working with very long strings, `Manacher’s Algorithm` is the go-to for its linear time complexity. The `Brute Force` and `Recursive` approaches are too slow for practical use but help build understanding.