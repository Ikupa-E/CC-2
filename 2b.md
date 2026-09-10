40. Combination Sum II
Problem

Given a collection of candidate numbers candidates and a target integer target, return all unique combinations where the chosen numbers sum to target.

Each number in candidates may be used at most once.

The solution must not contain duplicate combinations.

Example
Input: 
candidates = [10,1,2,7,6,1,5] 
target = 8 
 
Output: 
[[1,1,6],[1,2,5],[1,7],[2,6]] 

Explanation:

1 + 1 + 6 = 8
1 + 2 + 5 = 8
1 + 7 = 8
2 + 6 = 8

These are all the unique combinations whose sum is equal to 8.

Optimal Solution

Algorithm Used: Backtracking + Sorting + Duplicate Skipping

Idea

We use backtracking to generate possible combinations and use sorting to efficiently handle duplicate values.

We:

Sort the array so duplicate values are placed next to each other.
Start with an empty combination.
Choose an element and subtract it from the target.
Recursively search for the remaining target.
Move to i + 1 so that the same element cannot be reused.
Skip duplicate values at the same recursion level.
Stop searching when the current value is greater than the remaining target.

The duplicate check is performed using:

if (i > start && candidates[i] == candidates[i - 1]) {
    continue;
}

This prevents the same combination from being generated multiple times.

Time Complexity
O(2^n) in the general backtracking search.
Sorting takes O(n log n).

The actual runtime depends on the number of combinations explored and the pruning performed during backtracking.

Space Complexity
O(n) auxiliary space for the recursion and current combination.
Additional space is required for storing the output combinations.
Java Solution
import java.util.*;

class Solution {

    public List<List<Integer>> combinationSum2(
        int[] candidates,
        int target
    ) {

        List<List<Integer>> result = new ArrayList<>();

        // Sort the array to handle duplicates
        // and enable pruning
        Arrays.sort(candidates);

        backtrack(
            candidates,
            target,
            0,
            new ArrayList<>(),
            result
        );

        return result;
    }

    private void backtrack(
        int[] candidates,
        int target,
        int start,
        List<Integer> current,
        List<List<Integer>> result
    ) {

        // Target reached
        if (target == 0) {
            result.add(new ArrayList<>(current));
            return;
        }

        for (int i = start; i < candidates.length; i++) {

            // Skip duplicate values at the same level
            if (i > start && candidates[i] == candidates[i - 1]) {
                continue;
            }

            // Since the array is sorted,
            // no further elements can be used
            if (candidates[i] > target) {
                break;
            }

            // Choose
            current.add(candidates[i]);

            // Move to i + 1 so each element
            // can be used only once
            backtrack(
                candidates,
                target - candidates[i],
                i + 1,
                current,
                result
            );

            // Backtrack
            current.remove(current.size() - 1);
        }
    }
}
