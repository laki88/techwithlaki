---
layout: post
title:  "Leetcode problem 13 - Roman to Integer | Problem and solution"
author: Laki
categories: [ leetcode, algorithm, interview questions ]
image: assets/images/leetcode-problem-13/1.jpg
beforetoc: "This post shows the problem statement and several solutions for leetcode Roman to Integer problem"
toc: false
---

Roman numerals are represented by seven different symbols: I, V, X, L, C, D and M.


<table style="border: 1px solid black; border-collapse: collapse;">
  <thead>
    <tr>
      <th style="border: 1px solid black; padding: 8px;">Symbol</th>
      <th style="border: 1px solid black; padding: 8px;">Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border: 1px solid black; padding: 8px;">I</td>
      <td style="border: 1px solid black; padding: 8px;">1</td>
    </tr>
    <tr>
      <td style="border: 1px solid black; padding: 8px;">V</td>
      <td style="border: 1px solid black; padding: 8px;">5</td>
    </tr>
    <tr>
      <td style="border: 1px solid black; padding: 8px;">X</td>
      <td style="border: 1px solid black; padding: 8px;">10</td>
    </tr>
    <tr>
      <td style="border: 1px solid black; padding: 8px;">L</td>
      <td style="border: 1px solid black; padding: 8px;">50</td>
    </tr>
    <tr>
      <td style="border: 1px solid black; padding: 8px;">C</td>
      <td style="border: 1px solid black; padding: 8px;">100</td>
    </tr>
    <tr>
      <td style="border: 1px solid black; padding: 8px;">D</td>
      <td style="border: 1px solid black; padding: 8px;">500</td>
    </tr>
    <tr>
      <td style="border: 1px solid black; padding: 8px;">M</td>
      <td style="border: 1px solid black; padding: 8px;">1000</td>
    </tr>
  </tbody>
</table>

For example, 2 is written as II in Roman numeral, just two ones added together. 12 is written as XII, which is simply X + II. The number 27 is written as XXVII, which is XX + V + II.

Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not IIII. Instead, the number four is written as IV. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as IX. There are six instances where subtraction is used:

I can be placed before V (5) and X (10) to make 4 and 9. 
X can be placed before L (50) and C (100) to make 40 and 90. 
C can be placed before D (500) and M (1000) to make 400 and 900.
Given a roman numeral, convert it to an integer.

 

Example 1:
```
Input: s = "III"
Output: 3
Explanation: III = 3.
```
Example 2:
```
Input: s = "LVIII"
Output: 58
Explanation: L = 50, V= 5, III = 3.
```
Example 3:
```
Input: s = "MCMXCIV"
Output: 1994
Explanation: M = 1000, CM = 900, XC = 90 and IV = 4.
```

## Brute Force Approach (Using a Helper Method)
We start with a simple and clear approach:
Create a helper method to map Roman numerals to integers. As we loop through the string, if the current numeral is smaller than the next, we subtract it; otherwise, we add it.

```java
class Solution {
    public int romanToInt(String s) {
        int result = 0;
        for (int i = 0; i < s.length(); i++) {
            int current = valueOfRomanChar(s.charAt(i));
            int next = (i + 1 < s.length()) ? valueOfRomanChar(s.charAt(i + 1)) : 0;

            if (current < next) {
                result -= current;
            } else {
                result += current;
            }
        }
        return result;
    }

    private int valueOfRomanChar(char c) {
        switch (c) {
            case 'I': return 1;
            case 'V': return 5;
            case 'X': return 10;
            case 'L': return 50;
            case 'C': return 100;
            case 'D': return 500;
            case 'M': return 1000;
            default: return 0;
        }
    }
}
```
Time Complexity: O(n)
Space Complexity: O(1)

This method works well and makes it easy to understand the Roman numeral rules.

### Using a HashMap for Better Lookup
Another version uses a HashMap to store Roman values. The main logic remains the same — traverse the string and decide whether to add or subtract based on the next character.

```java
import java.util.HashMap;

class Solution {
    public int romanToInt(String s) {
        HashMap<Character, Integer> map = new HashMap<>();
        map.put('I', 1);
        map.put('V', 5);
        map.put('X', 10);
        map.put('L', 50);
        map.put('C', 100);
        map.put('D', 500);
        map.put('M', 1000);

        int result = 0;
        for (int i = 0; i < s.length(); i++) {
            int current = map.get(s.charAt(i));
            int next = (i + 1 < s.length()) ? map.get(s.charAt(i + 1)) : 0;

            if (current < next) {
                result -= current;
            } else {
                result += current;
            }
        }
        return result;
    }
}
```
Time Complexity: O(n)
Space Complexity: O(1)

Using a HashMap makes the code cleaner if you prefer separating data from logic. However, since the Roman numerals are fixed (only 7 characters), we can optimize even further.

### Optimized Approach (Switch Statement Only)
For the final and most optimized version, we avoid using extra space (no map needed) and use a simple switch to directly work with each character.

```java
class Solution {
    public int romanToInt(String s) {
        int result = 0;
        for (int i = 0; i < s.length(); i++) {
            switch (s.charAt(i)) {
                case 'M':
                    result += 1000;
                    break;
                case 'D':
                    result += 500;
                    break;
                case 'C':
                    if (i + 1 < s.length() && (s.charAt(i + 1) == 'D' || s.charAt(i + 1) == 'M')) {
                        result -= 100;
                    } else {
                        result += 100;
                    }
                    break;
                case 'L':
                    result += 50;
                    break;
                case 'X':
                    if (i + 1 < s.length() && (s.charAt(i + 1) == 'L' || s.charAt(i + 1) == 'C')) {
                        result -= 10;
                    } else {
                        result += 10;
                    }
                    break;
                case 'V':
                    result += 5;
                    break;
                case 'I':
                    if (i + 1 < s.length() && (s.charAt(i + 1) == 'V' || s.charAt(i + 1) == 'X')) {
                        result -= 1;
                    } else {
                        result += 1;
                    }
                    break;
            }
        }
        return result;
    }
}
```
Time Complexity: O(n)
Space Complexity: O(1)

By avoiding additional structures like HashMap, this solution becomes slightly faster and more memory-efficient.

Summary


<table style="border: 1px solid black; border-collapse: collapse;">
  <thead>
    <tr>
      <th style="border: 1px solid black; padding: 8px;">Symbol</th>
      <th style="border: 1px solid black; padding: 8px;">Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border: 1px solid black; padding: 8px;">Approach</td>
      <td style="border: 1px solid black; padding: 8px;">Time Complexity</td>
      <td style="border: 1px solid black; padding: 8px;">Space Complexity</td>
      <td style="border: 1px solid black; padding: 8px;">Notes</td>
    </tr>
    <tr>
      <td style="border: 1px solid black; padding: 8px;">Helper method + If conditions</td>
      <td style="border: 1px solid black; padding: 8px;">O(n)</td>
      <td style="border: 1px solid black; padding: 8px;">O(1)</td>
      <td style="border: 1px solid black; padding: 8px;">Easy to understand</td>
    </tr>
    <tr>
      <td style="border: 1px solid black; padding: 8px;">HashMap lookup</td>
      <td style="border: 1px solid black; padding: 8px;">O(n)</td>
      <td style="border: 1px solid black; padding: 8px;">O(1)</td>
      <td style="border: 1px solid black; padding: 8px;">Cleaner mapping, but slightly more space</td>
    </tr>
    <tr>
      <td style="border: 1px solid black; padding: 8px;">Optimized switch statement</td>
      <td style="border: 1px solid black; padding: 8px;">O(n)</td>
      <td style="border: 1px solid black; padding: 8px;">O(1)</td>
      <td style="border: 1px solid black; padding: 8px;">Fastest and minimal</td>
    </tr>
  </tbody>
</table>

Choosing the right approach often depends on whether you prioritize code clarity or raw performance. For interviews, starting with the simple version and then optimizing step-by-step shows your thinking clearly.