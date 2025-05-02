---
layout: post
title:  "Leetcode problem 6 - Zigzag Conversion | Problem and solution"
author: Laki
categories: [ leetcode, algorithm, interview questions ]
image: assets/images/leetcode-problem-4/1.jpg
beforetoc: "This post shows the problem statement and several solutions for leetcode Zigzag Conversion problem"
toc: false
---

### [Zigzag Conversion](https://leetcode.com/problems/zigzag-conversion/)

## Description

Picture this: you’re given a string, say "PAYPALISHIRING", and you need to transform it into a zigzag pattern across a specific number of rows. Imagine writing the characters in a way that they bounce up and down like a wave. For instance, with 3 rows, it might look like this:
```
P   A   H   N
A P L S I I G
Y   I   R
```
When you read this pattern row by row, you get "PAHNAPLSIIGYIR". Your mission is to create a function that takes a string and the number of rows as input and returns the string converted into this zigzag pattern. It’s like solving a puzzle that makes your code dance!

string convert(string s, int numRows);

### Example 1:
```
Input: s = "PAYPALISHIRING", numRows = 3
Output: "PAHNAPLSIIGYIR"
```
### Example 2:
```
Input: s = "PAYPALISHIRING", numRows = 4
Output: "PINALSIGYAHRPI"
Explanation:
P     I    N
A   L S  I G
Y A   H R
P     I
```
### Example 3:
```
Input: s = "A", numRows = 1
Output: "A"
```

### Constraints:
```
1 <= s.length <= 1000
s consists of English letters (lower-case and upper-case), ',' and '.'.
1 <= numRows <= 1000
```

To solve the Zigzag Conversion problem, we need to transform a given string into a zigzag pattern with a specified number of rows and then read the characters row by row. This problem can be approached in two efficient ways, both of which operate in linear time and space complexity.

### Approach 1: Simulating the Zigzag Movement
#### Intuition:
The problem can be visualized by simulating the movement of writing characters in a zigzag manner. We start at the top row, move down to the bottom row, then move up diagonally to the top row, repeating this pattern until all characters are placed. Finally, we concatenate all rows to form the result.

#### Steps:

- Edge Cases Handling: If the number of rows is 1 or the string length is less than or equal to the number of rows, return the string as is.

- Row Initialization: Create an array of StringBuilder objects, each representing a row in the zigzag pattern.

- Direction Simulation: Use a pointer to track the current row and a direction variable to move down or up. Append each character to the current row and adjust the direction when reaching the top or bottom row.

- Result Construction: Concatenate all rows to form the final result string.

Java Code:

```java
public class Solution {
    public String convert(String s, int numRows) {
        if (numRows == 1 || s.length() <= numRows) {
            return s;
        }
        
        StringBuilder[] rows = new StringBuilder[numRows];
        for (int i = 0; i < numRows; i++) {
            rows[i] = new StringBuilder();
        }
        
        int currentRow = 0;
        int step = 1;
        
        for (char c : s.toCharArray()) {
            rows[currentRow].append(c);
            if (currentRow == 0) {
                step = 1;
            } else if (currentRow == numRows - 1) {
                step = -1;
            }
            currentRow += step;
        }
        
        StringBuilder result = new StringBuilder();
        for (StringBuilder row : rows) {
            result.append(row);
        }
        return result.toString();
    }
}
```
### Approach 2: Calculating Indices Mathematically
#### Intuition:
Instead of simulating the zigzag movement, we can directly calculate the positions of characters in each row. The pattern repeats every 2*(numRows - 1) characters. For each row, characters are placed at intervals determined by this cycle. Middle rows have additional characters between these intervals, which can be calculated using a formula.

#### Steps:

- Edge Cases Handling: If the number of rows is 1, return the string as is.

- Cycle Calculation: Determine the cycle length as 2*(numRows - 1).

- Row-wise Character Placement: For each row, place characters at the main intervals and, for middle rows, add the intermediate characters using the formula cycle - 2 * currentRow.

Java Code:

```java
public class Solution {
    public String convert(String s, int numRows) {
        if (numRows == 1) {
            return s;
        }
        
        StringBuilder result = new StringBuilder();
        int cycle = 2 * (numRows - 1);
        int n = s.length();
        
        for (int i = 0; i < numRows; i++) {
            for (int j = i; j < n; j += cycle) {
                result.append(s.charAt(j));
                if (i != 0 && i != numRows - 1) {
                    int next = j + cycle - 2 * i;
                    if (next < n) {
                        result.append(s.charAt(next));
                    }
                }
            }
        }
        
        return result.toString();
    }
}
```
Explanation
Approach 1 simulates the natural zigzag movement by maintaining the current row and direction. This method is intuitive and mirrors the manual process of writing characters in a zigzag pattern.

Approach 2 leverages mathematical insights to directly compute the positions of characters in the zigzag pattern, avoiding the need for simulation. This method is efficient and reduces the overhead of tracking direction changes.

Both approaches efficiently solve the problem with linear time complexity 
O(n) and linear space complexity 
O(n), where 
n is the length of the input string. The choice between the two depends on readability preferences and specific use-case optimizations.

