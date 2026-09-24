78. Subsets
Problem

Given an integer array nums containing unique elements, return all possible subsets (the power set).

The solution must not contain duplicate subsets.

Example
Input: 
nums = [1,2,3] 
 
Output: 
[[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]] 

A subset can contain any number of elements from the original array, including an empty subset and the complete array.

Optimal Solution

Algorithm Used: Backtracking

Idea

The problem requires generating every possible subset of the given array. Since each element can either be included or excluded, we use backtracking to explore all possible combinations.

We:

Start with an empty subset.
Add the current subset to the result.
Try adding each available element to the current subset.
Recursively generate further subsets.
Remove the last element and continue with the next possibility.

For an array containing n elements, there are 2^n possible subsets.

Time Complexity
O(n × 2^n)

There are 2^n possible subsets, and copying each subset can take up to O(n) time.

Space Complexity
O(n) auxiliary space for the recursion and current subset.
The output requires O(n × 2^n) space.
Java Solution
import java.util.*;

class Solution {

    public List<List<Integer>> subsets(int[] nums) {

        List<List<Integer>> result = new ArrayList<>();

        backtrack(nums, 0, new ArrayList<>(), result);

        return result;
    }

    private void backtrack(
        int[] nums,
        int start,
        List<Integer> current,
        List<List<Integer>> result
    ) {

        // Add current subset to result
        result.add(new ArrayList<>(current));

        // Generate further subsets
        for (int i = start; i < nums.length; i++) {

            // Choose the current element
            current.add(nums[i]);

            // Explore
            backtrack(nums, i + 1, current, result);

            // Backtrack
            current.remove(current.size() - 1);
        }
    }
}
