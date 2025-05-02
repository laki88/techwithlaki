---
layout: post
title:  "Leetcode problem 8 - String to Integer (atoi) | Problem and solution"
author: Laki
categories: [ leetcode, algorithm, interview questions ]
image: assets/images/leetcode-problem-8/1.jpeg
beforetoc: "This post shows the problem statement and several solutions for leetcode String to Integer (atoi) problem"
toc: false
---

### [String to Integer (atoi)](https://leetcode.com/problems/string-to-integer-atoi/)

## Description

Implement the `myAtoi(string s)` function, which converts a string to a 32-bit signed integer (similar to C/C++'s atoi function).

The algorithm for `myAtoi(string s)` is as follows:

1. Whitespace: Ignore any leading whitespace `(" ")`.
2. Signedness: Determine the sign by checking if the next character is `'-'` or `'+'`, assuming positivity if neither present.
3. Conversion: Read the integer by skipping leading zeros until a non-digit character is encountered or the end of the string is reached. If no digits were read, then the result is 0.
4. Rounding: If the integer is out of the 32-bit signed integer range [-2<sup>31</sup>, 2<sup>31</sup> - 1], then round the integer to remain in the range. Specifically, integers less than -2<sup>31</sup> should be rounded to -2<sup>31</sup>, and integers greater than 2<sup>31</sup> - 1 should be rounded to 2<sup>31</sup> - 1.
Return the integer as the final result.
 

### Example 1:
```
Input: s = "42"

Output: 42

Explanation:

The underlined characters are what is read in and the caret is the current reader position.
Step 1: "42" (no characters read because there is no leading whitespace)
         ^
Step 2: "42" (no characters read because there is neither a '-' nor '+')
         ^
Step 3: "42" ("42" is read in)
           ^
```
### Example 2:
```
Input: s = " -042"

Output: -42

Explanation:

Step 1: "   -042" (leading whitespace is read and ignored)
            ^
Step 2: "   -042" ('-' is read, so the result should be negative)
             ^
Step 3: "   -042" ("042" is read in, leading zeros ignored in the result)
               ^
```
### Example 3:
```
Input: s = "1337c0d3"

Output: 1337

Explanation:

Step 1: "1337c0d3" (no characters read because there is no leading whitespace)
         ^
Step 2: "1337c0d3" (no characters read because there is neither a '-' nor '+')
         ^
Step 3: "1337c0d3" ("1337" is read in; reading stops because the next character is a non-digit)
             ^
```
### Example 4:

```
Input: s = "0-1"

Output: 0

Explanation:

Step 1: "0-1" (no characters read because there is no leading whitespace)
         ^
Step 2: "0-1" (no characters read because there is neither a '-' nor '+')
         ^
Step 3: "0-1" ("0" is read in; reading stops because the next character is a non-digit)
          ^
```
### Example 5:
```
Input: s = "words and 987"

Output: 0

Explanation:

Reading stops at the first non-digit character 'w'.
```

## Approaches and Solutions

Let's dive into different ways to solve this problem, starting from the simplest version and improving it step by step.

### 🛠️ Brute-Force Parsing Approach
This first solution directly parses the string by handling leading spaces, the sign, and numeric characters without much optimization. It's straightforward but solid for understanding the problem mechanics.

```java
class Solution {
    public int myAtoi(String s) {
        if (s == null || s.isEmpty()) return 0;
        
        int i = 0, n = s.length(), sign = 1;
        long result = 0; // Use long to detect overflow early
        
        // Skip leading whitespaces
        while (i < n && s.charAt(i) == ' ') i++;

        // Handle sign
        if (i < n && (s.charAt(i) == '-' || s.charAt(i) == '+')) {
            sign = (s.charAt(i) == '-') ? -1 : 1;
            i++;
        }

        // Process numeric characters
        while (i < n && Character.isDigit(s.charAt(i))) {
            int digit = s.charAt(i) - '0';
            result = result * 10 + digit;

            // Check overflow conditions
            if (sign * result >= Integer.MAX_VALUE) return Integer.MAX_VALUE;
            if (sign * result <= Integer.MIN_VALUE) return Integer.MIN_VALUE;

            i++;
        }

        return (int)(sign * result);
    }
}
```

Time Complexity: O(n)
Space Complexity: O(1)

### 🚀 Improved Version (Optimized Overflow Check)
In this version, we predict overflow before adding the next digit, improving the solution's reliability for edge cases.

```java
class Solution {
    public int myAtoi(String s) {
        int i = 0, sign = 1, result = 0;
        int n = s.length();
        
        // Skip leading whitespaces
        while (i < n && s.charAt(i) == ' ') i++;

        // Handle sign
        if (i < n && (s.charAt(i) == '+' || s.charAt(i) == '-')) {
            sign = (s.charAt(i) == '-') ? -1 : 1;
            i++;
        }

        // Process digits
        while (i < n && Character.isDigit(s.charAt(i))) {
            int digit = s.charAt(i) - '0';

            // Check if result will overflow after appending digit
            if (result > (Integer.MAX_VALUE - digit) / 10) {
                return (sign == 1) ? Integer.MAX_VALUE : Integer.MIN_VALUE;
            }

            result = result * 10 + digit;
            i++;
        }

        return result * sign;
    }
}
```
Key Difference:
Instead of storing numbers in long, we smartly check if multiplying result by 10 and adding a new digit would cause an overflow.

Time Complexity: O(n)
Space Complexity: O(1)

### 🧠 Cleaner Version with Early Exit
This version slightly improves code clarity by reducing the number of conditionals inside the loop.

```java
class Solution {
    public int myAtoi(String s) {
        s = s.strip(); // Java 11+ to remove leading and trailing spaces
        if (s.isEmpty()) return 0;
        
        int i = 0, sign = 1, result = 0;

        if (s.charAt(i) == '+' || s.charAt(i) == '-') {
            sign = (s.charAt(i) == '-') ? -1 : 1;
            i++;
        }

        while (i < s.length() && Character.isDigit(s.charAt(i))) {
            int digit = s.charAt(i) - '0';
            if (result > (Integer.MAX_VALUE - digit) / 10) {
                return (sign == 1) ? Integer.MAX_VALUE : Integer.MIN_VALUE;
            }
            result = result * 10 + digit;
            i++;
        }

        return result * sign;
    }
}
```
Tip:
Use strip() instead of trim() if you're using Java 11+, as it handles Unicode whitespaces better.

Final Thoughts
Starting with a brute-force parsing approach gives a good sense of the problem structure.

Improving overflow checks before appending digits leads to a more robust solution.

Clean code and early exits simplify debugging and maintenance, especially in competitive programming.

This problem teaches careful string parsing and handling numeric edge cases, which are crucial skills for technical interviews.