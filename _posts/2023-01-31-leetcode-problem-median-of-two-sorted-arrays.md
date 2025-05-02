---
layout: post
title:  "Leetcode problem 4 - Median of Two Sorted Arrays | Problem and solution"
author: Laki
categories: [ leetcode, algorithm, interview questions ]
image: assets/images/leetcode-problem-4/1.png
beforetoc: "This post shows the problem statement and several solutions for leetcode Median of Two Sorted Arrays problem"
toc: false
---

### [Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays/)

## Description

Picture yourself as a data analyst tasked with a puzzle: you have two lists of numbers, each already sorted in ascending order. Your goal is to find the median of all numbers as if you combined these lists into one. The catch? Merging and sorting would be too slow for large datasets. You need a clever method that runs in 𝑂(log(𝑚+𝑛)) time, where 𝑚 and 𝑛 are the sizes of the arrays. This isn’t just a coding exercise—it’s a challenge to think efficiently, like optimizing a massive database query!

### Example 1:
```
Input: nums1 = [1,3], nums2 = [2]
Output: 2.00000
Explanation: merged array = [1,2,3] and median is 2.
```
### Example 2:
```
Input: nums1 = [1,2], nums2 = [3,4]
Output: 2.50000
Explanation: merged array = [1,2,3,4] and median is (2 + 3) / 2 = 2.5.
```
### Constraints:
```
nums1.length == m
nums2.length == n
0 <= m,n <= 1000
1 <= m + n <= 2000
-106 <= nums1[i], nums2[i] <= 106
```

### Approach 1: Merge and Sort
Imagine you have two neatly stacked decks of cards (each already sorted), and you simply pour them into one big pile, shuffle them back into order, and then pick out the middle card(s). That’s exactly what this method does—but with numbers instead of playing cards.

#### 1. Combine both arrays
We allocate a new array large enough to hold every element from nums1 and nums2, then copy each array’s contents in sequence.

#### 2. Sort the combined array
Using Java’s built-in Arrays.sort(), we reorder the entire list from smallest to largest in one go.

#### 3. Pick the median

 - If the total count is odd, the median is the single middle element.

- If it’s even, it’s the average of the two center elements.

**Why it’s so straightforward**

- You leverage well-tested library code (System.arraycopy + Arrays.sort) and avoid any tricky index math.

- The logic is crystal clear: merge, sort, then grab the center.

**Drawbacks for large datasets**

- **Time Complexity**: O((m + n) log(m + n)) — sorting dominates, so doubling the data more than doubles the work.

- **Space Complexity**: O(m + n) — you need extra memory to hold the merged array

```java
import java.util.Arrays;

public class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int[] merged = new int[nums1.length + nums2.length];
        System.arraycopy(nums1, 0, merged, 0, nums1.length);
        System.arraycopy(nums2, 0, merged, nums1.length, nums2.length);
        Arrays.sort(merged);
        int n = merged.length;
        if (n % 2 == 0) {
            return (merged[n/2 - 1] + merged[n/2]) / 2.0;
        } else {
            return merged[n/2];
        }
    }
}
```

### Approach 2: Two-Pointer Method
Imagine you’re riffle-shuffling two sorted stacks of cards—but instead of fully melding them together, you only reveal cards one by one until you hit the middle. That’s the essence of the two-pointer method: we “merge” just enough to find the median without ever building a giant new array.

1. Set up your pointers

    - `i` scans through `nums1`, and `j` scans through `nums2`.

    - `current` holds the latest card (element) you’ve seen; `prev` remembers the one just before it.

2. Walk step-by-step

    - Loop from count = 0 up to total / 2 (inclusive).

    - At each step, compare `nums1[i]` and `nums2[j]` (if both are still available).

    - Whichever is smaller becomes your new current.

    - Advance that array’s pointer (i++ or j++), and shift the old current into prev.

3. Extract the median

    - If the combined length is odd, the last current you saw is the middle card.

    - If it’s even, take the average of prev and current—those are the two center cards.

Java Code:

```java
public class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int m = nums1.length, n = nums2.length;
        int total = m + n;
        int i = 0, j = 0, prev = 0, current = 0;
        
         for (int count = 0; count <= total / 2; count++) {
            prev = current;

            // 1) If nums1 is exhausted, take from nums2
            if (i >= m) {
                current = nums2[j++];
            }
            // 2) Else if nums2 is exhausted, take from nums1
            else if (j >= n) {
                current = nums1[i++];
            }
            // 3) Else pick the smaller head element
            else if (nums1[i] < nums2[j]) {
                current = nums1[i++];
            } 
            // 4) Otherwise take from nums2
            else {
                current = nums2[j++];
            }
        }
        return (total % 2 == 0) ? (prev + current) / 2.0 : current;
    }
}
```
#### Why this is slick
- Time: O(m + n). You only walk through half the elements at most, without an expensive sort.

- Space: O(1). No extra arrays—just a couple of integer pointers and temps.

- Intuition: You’re doing a partial merge; think “peek at the top of each deck and draw the lower card” until you see the middle.

#### When to choose this
- Perfect for moderately large lists where full sorting is overkill.

- Simple to implement once you wrap your head around maintaining two pointers.

### Approach 3: Binary Search (Optimal)
Leverage binary search on the smaller array to partition both arrays such that the median is found in logarithmic time.

Java Code:

```java
public class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        if (nums1.length > nums2.length) {
            return findMedianSortedArrays(nums2, nums1); // Ensure nums1 is the shorter array
        }
        int m = nums1.length, n = nums2.length;
        int left = 0, right = m;
        int totalHalf = (m + n + 1) / 2;
        
        while (left <= right) {
            int partitionA = (left + right) / 2;
            int partitionB = totalHalf - partitionA;
            
            int maxLeftA = (partitionA == 0) ? Integer.MIN_VALUE : nums1[partitionA - 1];
            int minRightA = (partitionA == m) ? Integer.MAX_VALUE : nums1[partitionA];
            int maxLeftB = (partitionB == 0) ? Integer.MIN_VALUE : nums2[partitionB - 1];
            int minRightB = (partitionB == n) ? Integer.MAX_VALUE : nums2[partitionB];
            
            if (maxLeftA <= minRightB && maxLeftB <= minRightA) {
                if ((m + n) % 2 == 0) {
                    return (Math.max(maxLeftA, maxLeftB) + Math.min(minRightA, minRightB)) / 2.0;
                } else {
                    return Math.max(maxLeftA, maxLeftB);
                }
            } else if (maxLeftA > minRightB) {
                right = partitionA - 1;
            } else {
                left = partitionA + 1;
            }
        }
        return 0.0;
    }
}
```
### Explanation:

- Partitioning: Split the smaller array (nums1) and derive the partition for nums2 automatically.

- Edge Handling: Use MIN_VALUE and MAX_VALUE to manage partitions at array boundaries.

- Validation: Check if the partitions are valid (left elements ≤ right elements).

- Median Calculation: Use partition values to derive the median without merging arrays.

### Conclusion
- Brute Force (Approach 1): Simple but inefficient—use for small datasets.

- Two-Pointer (Approach 2): Balanced approach for moderate-sized data.

- Binary Search (Approach 3): Optimal for large datasets, achieving O(log(min(m,n))) time.

Understanding these methods helps choose the right strategy based on problem constraints and performance needs. The binary search approach, though complex, is a favorite in coding interviews for its efficiency!
