---
layout: post
title:  "Leetcode problem 11 - Container With Most Water | Problem and solution"
author: Laki
categories: [ leetcode, algorithm, interview questions ]
image: assets/images/2023-01-26-leetcode-problem1-with-solutions/leetcode_meme1.png
beforetoc: "This post shows the problem statement and several solutions for leetcode Container With Most Water problem"
toc: false
---

You are given an integer array height of length n. There are n vertical lines drawn such that the two endpoints of the ith line are (i, 0) and (i, height[i]).

Find two lines that together with the x-axis form a container, such that the container contains the most water.

Return the maximum amount of water a container can store.

Notice that you may not slant the container.


Example 1:
```
Input: height = [1,8,6,2,5,4,8,3,7]
Output: 49
Explanation: The above vertical lines are represented by array [1,8,6,2,5,4,8,3,7]. In this case, the max area of water (blue section) the container can contain is 49.
```
Example 2:
```
Input: height = [1,1]
Output: 1
```

## Initial Thoughts

At first glance, a **brute-force** approach seems natural — try every pair of lines, calculate the water they can hold, and keep track of the maximum.

However, a smarter approach exists that saves time by using the **two-pointer technique**.

Let’s explore both.

---

## 1. Brute Force Approach (Simple but Inefficient)

We try every possible pair of lines and compute the area between them.

**Java Code:**
```java
class Solution {
    public int maxArea(int[] height) {
        int maxArea = 0;
        for (int i = 0; i < height.length; i++) {
            for (int j = i + 1; j < height.length; j++) {
                int area = (j - i) * Math.min(height[i], height[j]);
                maxArea = Math.max(maxArea, area);
            }
        }
        return maxArea;
    }
}
```
- Time Complexity: O(n²)
- Space Complexity: O(1)

This approach works but is too slow for large input sizes.

2. Optimized Approach (Two Pointers)
Instead of trying all pairs, we can be smarter:

Start with two pointers, one at the beginning and one at the end.

Calculate the area between them.

Move the pointer pointing to the shorter line inward — hoping to find a taller line that can create a larger container.

Intuition:
The area is limited by the shorter line. Moving the taller line inward won’t help — we must move the shorter side to potentially find a larger area.

Java Code:

```java
class Solution {
    public int maxArea(int[] height) {
        int left = 0;
        int right = height.length - 1;
        int maxArea = 0;

        while (left < right) {
            int width = right - left;
            int area = Math.min(height[left], height[right]) * width;
            maxArea = Math.max(maxArea, area);

            // Move the pointer at the shorter line
            if (height[left] < height[right]) {
                left++;
            } else {
                right--;
            }
        }

        return maxArea;
    }
}
```
- Time Complexity: O(n)
- Space Complexity: O(1)

Using two pointers cuts the time complexity dramatically, making this solution efficient even for large arrays.

#### Step-by-Step Algorithm
1. Initialize:

- left = 0, pointing to the first line.

- right = n - 1, pointing to the last line.

- maxArea = 0.

2. Loop while left < right:

- Calculate the width: right - left.

- Calculate the current area: min(height[left], height[right]) * width.

- Update maxArea if the current area is larger.

- Move the pointer pointing to the shorter line:

  - If height[left] < height[right], move left++.

  - Else, move right--.

3. Return the maxArea.

### Why This Works
At each step, we are trying to maximize the area:

- The width decreases as we move pointers inward.

- To compensate for the smaller width, we need taller lines.

- Hence, moving the shorter line inward increases our chances of finding a better container.

This greedy approach ensures we never miss the best possible solution.

### Conclusion
The Container With Most Water problem is a great example of how moving beyond brute force and thinking carefully about the problem's properties can lead to a much more efficient solution.

- Start simple with brute-force to understand the mechanics.

- Then optimize using two pointers to drastically improve performance.