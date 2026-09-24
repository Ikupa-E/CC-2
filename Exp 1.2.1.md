
# 35. Search Insert Position

## Problem

Given a sorted array of distinct integers and a target value, return the index if the target is found.

If not, return the index where it should be inserted to maintain the sorted order.

The solution must have **O(log n)** runtime complexity.

## Optimal Solution

**Algorithm Used:** Binary Search

### Idea

Perform a standard binary search:

1. If the target is found, return its index.
2. If not found, the `left` pointer will indicate the correct insertion position after the loop ends.

### Time Complexity

- **O(log n)**

### Space Complexity

- **O(1)**

### Java Solution

```java
class Solution {
    public int searchInsert(int[] nums, int target) {

        int left = 0;
        int right = nums.length - 1;

        while (left <= right) {

            int mid = left + (right - left) / 2;

            if (nums[mid] == target) {
                return mid;
            }

            if (nums[mid] < target) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }

        return left;
    }
}
```

---

# Updated Summary

| LeetCode Problem | Algorithm Used | Time Complexity | Space Complexity |
|------------------|----------------|-----------------|------------------|
| 33. Search in Rotated Sorted Array | Modified Binary Search | O(log n) | O(1) |
| 35. Search Insert Position | Binary Search | O(log n) | O(1) |
| 219. Contains Duplicate II | HashMap | O(n) | O(n) |
| 238. Product of Array Except Self | Prefix & Suffix Products (Two-Pass Traversal) | O(n) | O(1)\* 
