---
layout: post
title:  "Leetcode problem 4 - Median of Two Sorted Arrays | Problem and solution"
author: Laki
categories: [ leetcode, algorithm, interview questions ]
image: assets/images/2023-01-26-leetcode-problem1-with-solutions/leetcode_meme1.png
beforetoc: "This post shows the problem statement and several solutions for leetcode Median of Two Sorted Arrays problem"
toc: false
---

# [Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays/)

## Description

Given two sorted arrays nums1 and nums2 of size m and n respectively, return the median of the two sorted arrays.

The overall run time complexity should be O(log (m+n)).

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
0 <= m <= 1000
0 <= n <= 1000
1 <= m + n <= 2000
-106 <= nums1[i], nums2[i] <= 106
```

### Approach 1: Merge and Sort
This straightforward method merges both arrays into one, sorts it, and computes the median. While simple, it’s inefficient for large datasets due to its higher time complexity.

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
Explanation:

Merge: Combine nums1 and nums2 into a new array.

Sort: Use Arrays.sort() to order the merged array.

Median: Determine based on the merged array’s length (even or odd).

Approach 2: Two-Pointer Method
Efficiently traverse both sorted arrays using two pointers to track median elements without fully merging them.

Java Code:

```java
public class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int m = nums1.length, n = nums2.length;
        int total = m + n;
        int i = 0, j = 0, prev = 0, current = 0;
        
        for (int count = 0; count <= total / 2; count++) {
            prev = current;
            if (i < m && (j >= n || nums1[i] < nums2[j])) {
                current = nums1[i++];
            } else {
                current = nums2[j++];
            }
        }
        
        return (total % 2 == 0) ? (prev + current) / 2.0 : current;
    }
}
```
Explanation:

Pointers: i and j traverse nums1 and nums2, respectively.

Track Elements: Collect elements until reaching the median position(s).

Compute Median: Use the last two tracked values for even-length cases.

Approach 3: Binary Search (Optimal)
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
Explanation:

Partitioning: Split the smaller array (nums1) and derive the partition for nums2 automatically.

Edge Handling: Use MIN_VALUE and MAX_VALUE to manage partitions at array boundaries.

Validation: Check if the partitions are valid (left elements ≤ right elements).

Median Calculation: Use partition values to derive the median without merging arrays.

Conclusion
Brute Force (Approach 1): Simple but inefficient—use for small datasets.

Two-Pointer (Approach 2): Balanced approach for moderate-sized data.

Binary Search (Approach 3): Optimal for large datasets, achieving O(log(min(m,n))) time.

Understanding these methods helps choose the right strategy based on problem constraints and performance needs. The binary search approach, though complex, is a favorite in coding interviews for its efficiency!
