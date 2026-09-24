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


