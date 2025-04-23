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

## Solutions

### Approach 1: Brute Force
The brute force approach involves checking all possible substrings to determine if they are palindromic. This method has a high time complexity due to the number of substrings checked.

Algorithm:

Iterate over all possible starting indices of substrings.

For each starting index, check all possible ending indices greater than the current maximum length.

Update the maximum length and substring if a longer palindrome is found.

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
Complexity Analysis:

Time Complexity: O(n³) - Checking each substring takes O(n) time, with O(n²) substrings.

Space Complexity: O(1) - No additional space used.

### Approach 2: Expand Around Center
This method leverages the symmetry of palindromes by expanding around each potential center (both odd and even length).

Algorithm:

For each character, expand around it for odd and even-length palindromes.

Track the maximum length palindrome found.

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
Complexity Analysis:

Time Complexity: O(n²) - Each expansion takes O(n) time for O(n) centers.

Space Complexity: O(1) - No extra space used.

Approach 3: Dynamic Programming
Using dynamic programming to store subproblem results and avoid redundant checks.

Algorithm:

Create a DP table where dp[i][j] is true if substring s[i...j] is a palindrome.

Initialize single and two-character palindromes.

Fill the table and track the longest palindrome.

Java Code:

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
Complexity Analysis:

Time Complexity: O(n²) - Filling the DP table.

Space Complexity: O(n²) - Storing the DP table.

Approach 4: Manacher's Algorithm
An optimized algorithm to find the longest palindromic substring in linear time.

Algorithm:

Transform the string to handle even and odd lengths uniformly.

Use a DP array to track palindrome radii and expand efficiently.

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
Complexity Analysis:

Time Complexity: O(n) - Linear time due to efficient expansions.

Space Complexity: O(n) - Transformed string and DP array.

Approach 5: Recursive (Time Limit Exceeded)
A highly inefficient recursive approach for educational purposes.

Algorithm:

Check if the string is a palindrome.

Recursively check left and right substrings for longer palindromes.

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
Note: This approach is not efficient and is included for completeness.

Complexity Analysis:

Time Complexity: O(n³) - Exponential due to repeated checks.

Space Complexity: O(n) - Recursion stack depth.

