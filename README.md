# LeetCode Solutions in Java

This repository contains optimal Java solutions for selected LeetCode problems along with an explanation of the algorithm used and its time and space complexity.

---

# 219. Contains Duplicate II

## Problem

Given an integer array `nums` and an integer `k`, return `true` if there are two distinct indices `i` and `j` such that:

- `nums[i] == nums[j]`
- `|i - j| <= k`

Otherwise, return `false`.

## Optimal Solution

**Algorithm Used:** HashMap

### Idea

Use a `HashMap` to store each number and the latest index where it appeared.

As you iterate through the array:

1. Check if the current number already exists in the map.
2. If it does, calculate the distance between the current index and the previous index.
3. If the distance is less than or equal to `k`, return `true`.
4. Otherwise, update the stored index with the current index.

### Time Complexity

- **O(n)**

### Space Complexity

- **O(n)**

### Java Solution

```java
import java.util.HashMap;

class Solution {
    public boolean containsNearbyDuplicate(int[] nums, int k) {

        HashMap<Integer, Integer> map = new HashMap<>();

        for (int i = 0; i < nums.length; i++) {

            if (map.containsKey(nums[i])) {

                int previousIndex = map.get(nums[i]);

                if (i - previousIndex <= k) {
                    return true;
                }
            }

            map.put(nums[i], i);
        }

        return false;
    }
}
```

---

# 238. Product of Array Except Self

## Problem

Given an integer array `nums`, return an array `answer` such that:

```
answer[i] = product of every element except nums[i]
```

The solution must run in **O(n)** time and **must not use division**.

## Optimal Solution

**Algorithm Used:** Prefix Product and Suffix Product (Two-Pass Traversal)

### Idea

Instead of using division:

1. Traverse from left to right and store the product of all elements to the left of each index.
2. Traverse from right to left while maintaining the product of all elements to the right.
3. Multiply the left product and right product to obtain the final answer.

This avoids division and satisfies the required time complexity.

### Time Complexity

- **O(n)**

### Space Complexity

- **O(1)** (excluding the output array)

### Java Solution

```java
class Solution {

    public int[] productExceptSelf(int[] nums) {

        int n = nums.length;
        int[] answer = new int[n];

        int left = 1;

        for (int i = 0; i < n; i++) {
            answer[i] = left;
            left *= nums[i];
        }

        int right = 1;

        for (int i = n - 1; i >= 0; i--) {
            answer[i] *= right;
            right *= nums[i];
        }

        return answer;
    }
}
```

---

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

---

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
| 238. Product of Array Except Self | Prefix & Suffix Products (Two-Pass Traversal) | O(n) | O(1)\* |

> **Note:** For Problem 238, the output array is not counted as extra space according to the LeetCode problem statement.
