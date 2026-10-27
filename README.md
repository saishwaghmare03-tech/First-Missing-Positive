# First Missing Positive — Java Solution

[![Java](https://img.shields.io/badge/Language-Java-orange.svg)](https://www.oracle.com/java/)
[![Algorithm](https://img.shields.io/badge/Algorithm-Cyclic%20Sort-blue.svg)](https://en.wikipedia.org/wiki/Cycle_sort)
[![Difficulty](https://img.shields.io/badge/Difficulty-Hard-red.svg)](https://leetcode.com/problems/first-missing-positive/)

An optimal, in-place solution for **LeetCode 41: First Missing Positive**, implemented in Java using the **Cyclic Sort** algorithm.

---

## 📋 Problem Statement

Given an unsorted integer array `nums`, return the smallest positive integer that is not present in `nums`.

You must implement an algorithm that runs in **$O(n)$ time** and uses **$O(1)$ auxiliary space**.

### Examples
* **Example 1:**
  * **Input:** `nums = [1, 2, 0]`
  * **Output:** `3`
  * **Explanation:** The numbers in the range `[1, 2]` are all in the array.

* **Example 2:**
  * **Input:** `nums = [3, 4, -1, 1]`
  * **Output:** `2`
  * **Explanation:** `1` is in the array, but `2` is missing.

* **Example 3:**
  * **Input:** `nums = [7, 8, 9, 11, 12]`
  * **Output:** `1`
  * **Explanation:** The smallest positive integer `1` is missing.

---

## 💡 Approach: Cyclic Sort

To satisfy the $O(n)$ time and $O(1)$ space constraints without using extra data structures, we use a **Cyclic Sort** variation:
1. **Rearrange in place:** Traverse the array and place each valid number `x` (where $1 \le x \le n$) into its correct 0-indexed position, which is `x - 1`.
2. **Handle duplicates and out-of-bounds:** Skip numbers that are $\le 0$, $> n$, or already in their correct positions (`nums[j] == nums[nums[j] - 1]`) to prevent infinite loops.
3. **Scan for the missing integer:** Iterate through the sorted array from left to right. The first index `i` where `nums[i] != i + 1` reveals the missing positive integer `i + 1`. If all indices match, the answer is `n + 1`.

---

## 💻 Java Implementation

```java
class Solution {
    public int firstMissingPositive(int[] nums) {
        int n = nums.length;
        int j = 0;
        
        // Cyclic Sort: Place each number x (1 <= x <= n) at its correct index (x - 1)
        while (j < n) {
            if (nums[j] <= 0 || nums[j] > n || (nums[j] == nums[nums[j] - 1])) {
                j++;
                continue;
            }

            int index = nums[j] - 1;
            int temp = nums[index]; 
            nums[index] = nums[j]; 
            nums[j] = temp; 
        }
        
        // Scan for the first index where the value is not i + 1
        for (int i = 0; i < n; i++) {
            if (nums[i] != i + 1) {
                return i + 1;
            }
        }
        
        return n + 1;
    }

    public static void main(String[] args) {
        Solution sol = new Solution();

        // Test Case 1
        int[] nums1 = {1, 2, 0};
        System.out.println("Input: [1, 2, 0] -> Output: " + sol.firstMissingPositive(nums1)); 
        // Expected: 3

        // Test Case 2
        int[] nums2 = {3, 4, -1, 1};
        System.out.println("Input: [3, 4, -1, 1] -> Output: " + sol.firstMissingPositive(nums2)); 
        // Expected: 2

        // Test Case 3
        int[] nums3 = {7, 8, 9, 11, 12};
        System.out.println("Input: [7, 8, 9, 11, 12] -> Output: " + sol.firstMissingPositive(nums3)); 
        // Expected: 1
    }
}
