---
layout: post
title:  "Leetcode problem 7 - Reverse Integer | Problem and solution"
author: Laki
categories: [ leetcode, algorithm, interview questions ]
image: assets/images/2023-01-26-leetcode-problem1-with-solutions/leetcode_meme1.png
beforetoc: "This post shows the problem statement and several solutions for leetcode Reverse Integer problem"
toc: false
---

### [Reverse Integer](https://leetcode.com/problems/reverse-integer/)

# Description

Given a signed 32-bit integer x, return x with its digits reversed. If reversing x causes the value to go outside the signed 32-bit integer range [-231, 231 - 1], then return 0.

Assume the environment does not allow you to store 64-bit integers (signed or unsigned).

### Example 1:

Input: x = 123
Output: 321

### Example 2:

Input: x = -123
Output: -321

### Example 3:

Input: x = 120
Output: 21
 

### Constraints:

-231 <= x <= 231 - 1

# Solutions

We'll start with a simple approach and gradually move towards an optimized version.

---

## Solution 1: Convert to String

A straightforward idea:  
- Convert the integer to a string.
- Reverse the string.
- Convert it back to an integer.

We'll also need to handle negatives carefully, and check if the reversed value fits inside the 32-bit integer range.

```java
class Solution {
    public int reverse(int x) {
        String str = Integer.toString(x);
        StringBuilder sb = new StringBuilder();
        
        int start = 0;
        if (str.charAt(0) == '-') {
            sb.append('-');
            start = 1;
        }
        
        for (int i = str.length() - 1; i >= start; i--) {
            sb.append(str.charAt(i));
        }
        
        try {
            return Integer.parseInt(sb.toString());
        } catch (NumberFormatException e) {
            return 0; // Overflow occurred
        }
    }
}
```

Highlights:
- Very simple and readable.

- Relies on Java's built-in parsing and exception handling.

- Not memory-efficient because of string operations.


## Solution 2: Math Approach (Basic Version)

Instead of strings, we can work directly with digits:

- Pop the last digit (x % 10).

- Push it into a new number.


```java
public int reverse(int x) {
        int res = 0;
        boolean isNegative = x < 0;
        x = Math.abs(x);
        
        while (x != 0) {
            int digit = x % 10;
            res = res * 10 + digit;
            x /= 10;
        }
        
        res = isNegative ? -res : res;
        
        // Check for overflow
        if (res > Integer.MAX_VALUE || res < Integer.MIN_VALUE) {
            return 0;
        }
        
        return res;
    }
```

Note:
Here, the overflow is checked after building the result, which may already be too late in some cases. Let's improve that in the next version.

## Solution 3: Math Approach (Optimized with Overflow Check)
To avoid overflow before it happens, we check if the current number is safe to multiply by 10 and add the next digit.
```java
    public int reverse(int x) {
        int res = 0;
        
        while (x != 0) {
            int digit = x % 10;
            
            // Overflow check:
            if (res > Integer.MAX_VALUE / 10 || (res == Integer.MAX_VALUE / 10 && digit > 7)) {
                return 0;
            }
            if (res < Integer.MIN_VALUE / 10 || (res == Integer.MIN_VALUE / 10 && digit < -8)) {
                return 0;
            }
            
            res = res * 10 + digit;
            x /= 10;
        }
        
        return res;
    }
```
Why check 7 and -8?
- Integer.MAX_VALUE = 2,147,483,647 → Last digit is 7.

- Integer.MIN_VALUE = -2,147,483,648 → Last digit is -8.

- When res is near the boundary (214748364), the next digit must not push it over.

Summary

<table style="border-collapse: collapse; width: 100%;">
  <thead>
    <tr>
      <th style="border: 1px solid black; padding: 8px;">Approach</th>
      <th style="border: 1px solid black; padding: 8px;">Pros</th>
      <th style="border: 1px solid black; padding: 8px;">Cons</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border: 1px solid black; padding: 8px;">String Conversion</td>
      <td style="border: 1px solid black; padding: 8px;">Very easy to implement</td>
      <td style="border: 1px solid black; padding: 8px;">Extra space, slower, exception handling needed</td>
    </tr>
    <tr>
      <td style="border: 1px solid black; padding: 8px;">Basic Math</td>
      <td style="border: 1px solid black; padding: 8px;">No extra memory</td>
      <td style="border: 1px solid black; padding: 8px;">Still may cause overflow</td>
    </tr>
    <tr>
      <td style="border: 1px solid black; padding: 8px;">Optimized Math</td>
      <td style="border: 1px solid black; padding: 8px;">Memory efficient and safe</td>
      <td style="border: 1px solid black; padding: 8px;">Slightly more careful code</td>
    </tr>
  </tbody>
</table>

## Final Thoughts
Always think about integer overflow when reversing numbers manually.

If string methods are allowed in interviews, it's okay to use them for a quick first solution.

Math-based solutions show deeper understanding and are usually preferred when constraints are tight