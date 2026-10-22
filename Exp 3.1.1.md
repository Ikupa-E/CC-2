70. Climbing Stairs
Problem
You are climbing a staircase that has n steps.

Each time, you can climb either 1 step or 2 steps. Return the number of distinct ways to reach the top of the staircase.
Example
Input:
n = 3

Output:
3
Explanation: There are three ways to climb to the top:
- 1 step + 1 step + 1 step
- 1 step + 2 steps
- 2 steps + 1 step
Optimal Solution
Algorithm Used: Dynamic Programming (Space-Optimized)
Idea
To reach step n, the final move must come from either:
- Step n - 1, by taking one step.
- Step n - 2, by taking two steps.
Therefore, the number of ways to reach step n is:

ways(n) = ways(n - 1) + ways(n - 2)

The base cases are:
- ways(1) = 1
- ways(2) = 2
We only need the previous two results to calculate the next result, so we can use two variables instead of storing a full DP array.
Time Complexity
- O(n) because we calculate the answer for each step once.
Space Complexity
- O(1) because we use only a constant number of variables.
Java Solution
class Solution {

    public int climbStairs(int n) {

        if (n <= 2) {
            return n;
        }

        int oneStepBefore = 2;
        int twoStepsBefore = 1;

        for (int i = 3; i <= n; i++) {

            int currentWays = oneStepBefore + twoStepsBefore;

            twoStepsBefore = oneStepBefore;
            oneStepBefore = currentWays;
        }

        return oneStepBefore;
    }
}
Summary
LeetCode Problem	Algorithm Used	Time Complexity	Space Complexity
70. Climbing Stairs	Space-Optimized Dynamic Programming	O(n)	O(1)
