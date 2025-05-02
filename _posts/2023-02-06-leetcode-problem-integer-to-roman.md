---
layout: post
title:  "Leetcode problem 12 - Integer to Roman | Problem and solution"
author: Laki
categories: [ leetcode, algorithm, interview questions ]
image: assets/images/leetcode-problem-12/1.jpg
beforetoc: "This post shows the problem statement and several solutions for leetcode Integer to Roman problem"
toc: false
---
Seven different symbols represent Roman numerals with the following values:

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
Roman numerals are formed by appending the conversions of decimal place values from highest to lowest. Converting a decimal place value into a Roman numeral has the following rules:

- If the value does not start with 4 or 9, select the symbol of the maximal value that can be subtracted from the input, append that symbol to the result, subtract its value, and convert the remainder to a Roman numeral.
- If the value starts with 4 or 9 use the subtractive form representing one symbol subtracted from the following symbol, for example, 4 is 1 (I) less than 5 (V): IV and 9 is 1 (I) less than 10 (X): IX. Only the following subtractive forms are used: 4 (IV), 9 (IX), 40 (XL), 90 (XC), 400 (CD) and 900 (CM).
- Only powers of 10 (I, X, C, M) can be appended consecutively at most 3 times to represent multiples of 10. You cannot append 5 (V), 50 (L), or 500 (D) multiple times. If you need to append a symbol 4 times use the subtractive form.
Given an integer, convert it to a Roman numeral.

 
Example 1:
```
Input: num = 3749

Output: "MMMDCCXLIX"

Explanation:

3000 = MMM as 1000 (M) + 1000 (M) + 1000 (M)
 700 = DCC as 500 (D) + 100 (C) + 100 (C)
  40 = XL as 10 (X) less of 50 (L)
   9 = IX as 1 (I) less of 10 (X)
Note: 49 is not 1 (I) less of 50 (L) because the conversion is based on decimal places
```
Example 2:
```
Input: num = 58

Output: "LVIII"

Explanation:

50 = L
 8 = VIII
Example 3:

Input: num = 1994

Output: "MCMXCIV"

Explanation:

1000 = M
 900 = CM
  90 = XC
   4 = IV
 
```
#### Constraints:

1 <= num <= 3999

## Solutions from Basic to Optimized
Let’s walk through how different Java solutions approach this problem, starting simple and gradually refining.

### 1. Brute-force Mapping and Subtraction
In this approach, we directly map integers to Roman numerals, starting from the highest value and subtracting greedily.

```java
class Solution {
    public String intToRoman(int num) {
        int[] values = {1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1};
        String[] symbols = {
            "M", "CM", "D", "CD", "C", "XC",
            "L", "XL", "X", "IX", "V", "IV", "I"
        };
        
        StringBuilder sb = new StringBuilder();
        
        for (int i = 0; i < values.length && num > 0; i++) {
            while (num >= values[i]) {
                sb.append(symbols[i]);
                num -= values[i];
            }
        }
        
        return sb.toString();
    }
}
```
Why it works:
- We iterate through predefined values.

- For each value, we subtract as many times as possible while appending the corresponding symbol.

Time Complexity: O(1) (constant operations)
Space Complexity: O(1)

### 2. Optimized Lookup Using Precomputed Tables
Instead of checking one-by-one, we can directly build strings using precomputed arrays for thousands, hundreds, tens, and ones.

```java
class Solution {
    public String intToRoman(int num) {
        String[] thousands = {"", "M", "MM", "MMM"};
        String[] hundreds = {"", "C", "CC", "CCC", "CD", "D", "DC", "DCC", "DCCC", "CM"};
        String[] tens = {"", "X", "XX", "XXX", "XL", "L", "LX", "LXX", "LXXX", "XC"};
        String[] ones = {"", "I", "II", "III", "IV", "V", "VI", "VII", "VIII", "IX"};
        
        return thousands[num / 1000] + 
               hundreds[(num % 1000) / 100] +
               tens[(num % 100) / 10] +
               ones[num % 10];
    }
}
```
Why it's faster:
- No loops inside the method.

- Directly index into pre-built arrays based on digit place values.

Time Complexity: O(1)
Space Complexity: O(1)

### 3. Minimalist Clean Version (Same Greedy Idea, Short Code)
For those who prefer minimal lines without sacrificing clarity:

```java
class Solution {
    public String intToRoman(int num) {
        int[] nums = {1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1};
        String[] romans = {
            "M", "CM", "D", "CD", "C", "XC",
            "L", "XL", "X", "IX", "V", "IV", "I"
        };
        
        StringBuilder roman = new StringBuilder();
        
        for (int i = 0; i < nums.length; i++) {
            int count = num / nums[i];
            num %= nums[i];
            while (count-- > 0) roman.append(romans[i]);
        }
        
        return roman.toString();
    }
}
```
Highlights:
- Very compact and neat.

- Slightly more mathematical, using division and modulus directly.

### Key Takeaways
Start with greedy subtraction using a simple mapping.

Optimize by avoiding repeated checks via precomputed tables.

For very clean code, a minimalist greedy approach fits well.

All three methods work within constant time and space, as the input range is tightly bounded (1 <= num <= 3999).