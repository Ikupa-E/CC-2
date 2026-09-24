---

# 33. Search in Rotated Sorted Array

## Problem

Given a rotated sorted array `nums` with distinct integers and a target value, return the index of the target if it exists. Otherwise, return `-1`.

The solution must have a time complexity of **O(log n)**.

## Optimal Solution

**Algorithm Used:** Modified Binary Search

### Idea

A rotated sorted array consists of two sorted halves. During each iteration:

1. Find the middle element.
2. Determine which half (left or right) is sorted.
3. Check whether the target lies within the sorted half.
4. Search only that half by updating the search boundaries.

This preserves the logarithmic time complexity of binary search.

### Time Complexity

- **O(log n)**

### Space Complexity

- **O(1)**

### Java Solution

```java
class Solution {
    public int search(int[] nums, int target) {

        int left = 0;
        int right = nums.length - 1;

        while (left <= right) {

            int mid = left + (right - left) / 2;

            if (nums[mid] == target) {
                return mid;
            }

            // Left half is sorted
            if (nums[left] <= nums[mid]) {

                if (target >= nums[left] && target < nums[mid]) {
                    right = mid - 1;
                } else {
                    left = mid + 1;
                }

            }
            // Right half is sorted
            else {

                if (target > nums[mid] && target <= nums[right]) {
                    left = mid + 1;
                } else {
                    right = mid - 1;
                }
            }
        }

        return -1;
    }
}
```

|
