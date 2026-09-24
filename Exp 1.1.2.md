
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

This avoids division and satisfies the required time complexity

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



> **Note:** For Problem 238, the output array is not counted as extra space according to the LeetCode problem statement.
