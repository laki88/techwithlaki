---
layout: post
title:  "Leetcode problem 9 - Palindrome Number | Problem and solution"
author: Laki
categories: [ leetcode, algorithm, interview questions ]
image: assets/images/leetcode-problem-9/1.jpg
beforetoc: "This post shows the problem statement and several solutions for leetcode Palindrome Number problem"
toc: false
---

### [Palindrome Number](https://leetcode.com/problems/palindrome-number/)

## Description
Given an integer x, return true if x is a 
palindrome, and false otherwise.

### Example 1:
```
Input: x = 121
Output: true
Explanation: 121 reads as 121 from left to right and from right to left.
```
### Example 2:
```
Input: x = -121
Output: false
Explanation: From left to right, it reads -121. From right to left, it becomes 121-. Therefore it is not a palindrome.
```
### Example 3:
```
Input: x = 10
Output: false
Explanation: Reads 01 from right to left. Therefore it is not a palindrome.
``` 

### Constraints:
```
-231 <= x <= 231 - 1
```

## Solution Approaches
In this post, we walk through several approaches to solving the Palindrome Number problem, from basic methods to more optimized techniques.

🔹 Brute Force: Convert to String and Compare

Idea:
Convert the integer to a string and compare characters from the start and end.

Java Code:

```java
Copy
Edit
class Solution {
    public boolean isPalindrome(int x) {
        String s = String.valueOf(x);
        int n = s.length();
        for (int i = 0; i < n / 2; i++) {
            if (s.charAt(i) != s.charAt(n - i - 1)) {
                return false;
            }
        }
        return true;
    }
}
```
Complexity:

- Time: O(n), where n is the number of digits.

- Space: O(n), because of the string representation.


🔹 Improved Approach: Reverse the Number
Idea:
Rather than converting to a string, reverse the number and check if it matches the original.

Java Code:

```java

class Solution {
    public boolean isPalindrome(int x) {
        if (x < 0) {
            return false;
        }
        int original = x;
        int reversed = 0;
        while (x != 0) {
            int digit = x % 10;
            reversed = reversed * 10 + digit;
            x /= 10;
        }
        return original == reversed;
    }
}
```
Complexity:

- Time: O(log₁₀(n)), because we process each digit once.

- Space: O(1).

🔹 Further Optimization: Reverse Half of the Number
Idea:
Reverse only half of the number to avoid possible integer overflow and improve efficiency.

Steps:

If x is negative or ends with 0 (but not 0 itself), it cannot be a palindrome.

Build the reversed number until it is greater than or equal to the remaining part.

Java Code:

```java
class Solution {
    public boolean isPalindrome(int x) {
        if (x < 0 || (x % 10 == 0 && x != 0)) {
            return false;
        }
        
        int revertedNumber = 0;
        while (x > revertedNumber) {
            revertedNumber = revertedNumber * 10 + x % 10;
            x /= 10;
        }
        
        return x == revertedNumber || x == revertedNumber / 10;
    }
}
```
Example Walkthrough:

Input: x = 121

First step: reverse digits until revertedNumber >= x.

Midway:

x = 12, revertedNumber = 1

x = 1, revertedNumber = 12

Now compare x and revertedNumber / 10 (since it has an odd number of digits).

Complexity:

Time: O(log₁₀(n)), processing half of the digits.

Space: O(1), only using integers.

## ✍️ Conclusion
This problem can be approached in different ways depending on whether you are allowed to use extra space or not.

Beginner-friendly solution: String reversal and comparison.

Better approach: Reverse the whole number and compare.

Best approach: Reverse only half of the number to optimize performance and prevent overflow.

Choosing the right method depends on the problem constraints and interview expectations. Practicing different approaches builds a strong problem-solving foundation.
