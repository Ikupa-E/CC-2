# Find the Duplicate Number

## Problem

Given an array `nums` containing `n + 1` integers where each integer is in the range `[1, n]`, there is only **one repeated number**, but it may be repeated more than once.

Return the duplicate number.

You must solve the problem **without modifying the array** and using only **constant extra space**.

### Example

**Input:**

```text
nums = [1, 3, 4, 2, 2]
```

**Output:**

```text
2
```

The number `2` appears more than once, so it is the duplicate number.

---

## Approach

The problem can be solved using **Floyd's Tortoise and Hare algorithm**, which is commonly used for detecting cycles in a linked list.

We treat each number in the array as a pointer to another index:

```text
index → nums[index]
```

Because there are `n + 1` numbers but only `n` possible values, at least two positions must point to the same value. This creates a **cycle**.

We use two pointers:

* **Slow:** moves one step at a time.
* **Fast:** moves two steps at a time.

When they meet, a cycle has been detected.

Then, reset `slow` to the beginning and move both pointers one step at a time. The point where they meet again is the duplicate number.

---

## Algorithm

1. Initialize `slow` and `fast` to `nums[0]`.
2. Move `slow` one step:

   ```text
   slow = nums[slow]
   ```
3. Move `fast` two steps:

   ```text
   fast = nums[nums[fast]]
   ```
4. Continue until `slow == fast`.
5. Reset `slow` to `nums[0]`.
6. Move both pointers one step at a time.
7. When they meet, return that value as the duplicate number.

---

## Complexity

* **Time Complexity:** `O(n)`
* **Space Complexity:** `O(1)`

---

## Code

```java
class Solution {
    public int findDuplicate(int[] nums) {
        int slow = nums[0];
        int fast = nums[0];

        // Find the intersection point
        do {
            slow = nums[slow];
            fast = nums[nums[fast]];
        } while (slow != fast);

        // Find the entrance to the cycle
        slow = nums[0];

        while (slow != fast) {
            slow = nums[slow];
            fast = nums[fast];
        }

        return slow;
    }
}
```

---


The duplicate number creates a cycle, and finding the entrance of that cycle gives us the duplicate number without modifying the array or using extra memory.
