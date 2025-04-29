---
layout: post
title:  "Leetcode problem 17 - Letter Combinations of a Phone Number | Problem and solution"
author: Laki
categories: [ leetcode, algorithm, interview questions ]
image: assets/images/2023-01-26-leetcode-problem1-with-solutions/leetcode_meme1.png
beforetoc: "This post shows the problem statement and several solutions for leetcode Letter Combinations of a Phone Number problem"
toc: false
---

# [Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number)

## Description

Given a string containing digits from 2-9 inclusive, return all possible letter combinations that the number could represent. Return the answer in any order.

A mapping of digits to letters (just like on the telephone buttons) is given below. Note that 1 does not map to any letters.


Example 1:
```
Input: digits = "23"
Output: ["ad","ae","af","bd","be","bf","cd","ce","cf"]
```
Example 2:
```
Input: digits = ""
Output: []
```
Example 3:
```
Input: digits = "2"
Output: ["a","b","c"]
```
## Solutions

We'll look at multiple solutions, starting from a straightforward brute-force approach and progressing to more refined backtracking techniques.

---

### 🚀 Solution 1: Brute-force with Queue (Iterative)

**Idea:**  
We simulate combinations using a queue. For each digit, we dequeue all partial results so far and append each possible letter to form new combinations.

**Code:**

```java
class Solution {
    public List<String> letterCombinations(String digits) {
        if (digits == null || digits.length() == 0) return new ArrayList<>();

        String[] map = {"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
        Queue<String> queue = new LinkedList<>();
        queue.add("");

        for (char digit : digits.toCharArray()) {
            int size = queue.size();
            String letters = map[digit - '0'];
            for (int i = 0; i < size; i++) {
                String current = queue.poll();
                for (char c : letters.toCharArray()) {
                    queue.add(current + c);
                }
            }
        }

        return new ArrayList<>(queue);
    }
}
```
Complexity:

Time: O(3ⁿ · 4ᵐ) where n is the number of digits that map to 3 letters, and m is the number that map to 4.

Space: O(3ⁿ · 4ᵐ) for storing combinations.

### 🧭 Solution 2: Basic Backtracking (Recursive)

**Idea:** 
Use recursion to explore all paths by appending one letter at a time and backtracking once we reach the end of the digit string.

**Code:**

```java
class Solution {
    private static final String[] map = {
        "", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"
    };

    public List<String> letterCombinations(String digits) {
        List<String> result = new ArrayList<>();
        if (digits == null || digits.isEmpty()) return result;

        backtrack(digits, 0, new StringBuilder(), result);
        return result;
    }

    private void backtrack(String digits, int index, StringBuilder path, List<String> result) {
        if (index == digits.length()) {
            result.add(path.toString());
            return;
        }

        String letters = map[digits.charAt(index) - '0'];
        for (char c : letters.toCharArray()) {
            path.append(c);
            backtrack(digits, index + 1, path, result);
            path.deleteCharAt(path.length() - 1); // backtrack
        }
    }
}
```
Complexity:

Time: O(3ⁿ · 4ᵐ)

Space: O(n) for recursion stack

### 🔁 Solution 3: Optimized Backtracking with Early Exit
**Idea:**
Same recursive approach, but this version exits earlier and avoids unnecessary object creation by using a single shared buffer (StringBuilder).

**Code:**

```java
class Solution {
    public List<String> letterCombinations(String digits) {
        List<String> result = new ArrayList<>();
        if (digits.isEmpty()) return result;

        String[] map = {"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
        dfs(0, digits, new StringBuilder(), result, map);
        return result;
    }

    private void dfs(int index, String digits, StringBuilder current, List<String> result, String[] map) {
        if (index == digits.length()) {
            result.add(current.toString());
            return;
        }

        String letters = map[digits.charAt(index) - '0'];
        for (char c : letters.toCharArray()) {
            current.append(c);
            dfs(index + 1, digits, current, result, map);
            current.deleteCharAt(current.length() - 1); // backtrack
        }
    }
}
```
Optimizations:

Reuses StringBuilder to reduce memory footprint.

Avoids unnecessary function calls if input is empty.

### Summary

<table style="border-collapse: collapse; width: 100%;">
  <thead>
    <tr>
      <th style="border: 1px solid black; padding: 8px;">Approach</th>
      <th style="border: 1px solid black; padding: 8px;">Technique</th>
      <th style="border: 1px solid black; padding: 8px;">Time Complexity</th>
      <th style="border: 1px solid black; padding: 8px;">Space Complexity</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="border: 1px solid black; padding: 8px;">Queue-based</td>
      <td style="border: 1px solid black; padding: 8px;">Iterative BFS</td>
      <td style="border: 1px solid black; padding: 8px;">O(3ⁿ · 4ᵐ)</td>
      <td style="border: 1px solid black; padding: 8px;">O(3ⁿ · 4ᵐ)</td>
    </tr>
    <tr>
      <td style="border: 1px solid black; padding: 8px;">Backtracking</td>
      <td style="border: 1px solid black; padding: 8px;">Recursive DFS</td>
      <td style="border: 1px solid black; padding: 8px;">O(3ⁿ · 4ᵐ)</td>
      <td style="border: 1px solid black; padding: 8px;">O(n)</td>
    </tr>
    <tr>
      <td style="border: 1px solid black; padding: 8px;">Optimized Backtracking</td>
      <td style="border: 1px solid black; padding: 8px;">Recursive with buffer</td>
      <td style="border: 1px solid black; padding: 8px;">O(3ⁿ · 4ᵐ)</td>
      <td style="border: 1px solid black; padding: 8px;">O(n)</td>
    </tr>
  </tbody>
</table>

While the backtracking approaches are most commonly used in interviews, the iterative method is intuitive and equally valid.

