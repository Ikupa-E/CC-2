Frog Jump (Problem 1)

Problem
A frog is standing on the first stair of a staircase containing n stairs. An array heights gives the height of each stair.

The frog can jump either:
- One stair: the energy used is abs(heights[i] - heights[i - 1]).
- Two stairs: the energy used is abs(heights[i] - heights[i - 2]).
Return the minimum total energy required for the frog to reach the last stair.

Example
Input:
n = 4
heights = [10, 20, 30, 10]

Output:
20

Explanation: One minimum-energy path is:
- Jump from stair 1 (height 10) to stair 2 (height 20): energy = 10.
- Jump from stair 2 (height 20) to stair 4 (height 10): energy = 10.
Total energy = 10 + 10 = 20.

Optimal Solution
Algorithm Used: Dynamic Programming (Space-Optimized)
Idea
Let dp[i] represent the minimum energy required to reach stair i.

To reach stair i, the frog can come from either stair i - 1 or stair i - 2. Therefore:
- oneStep = dp[i - 1] + abs(heights[i] - heights[i - 1])
- twoSteps = dp[i - 2] + abs(heights[i] - heights[i - 2])
- dp[i] = min(oneStep, twoSteps)
The first stair requires no energy, so dp[0] = 0.

Instead of storing a full DP array, we keep only the previous two results because each new result depends only on them.

Time Complexity
- O(n) because each stair is processed once.
Space Complexity
- O(1) because only a constant number of variables are used.
Java Solution
class Solution {

    public int frogJump(int n, int[] heights) {

        if (n <= 1) {
            return 0;
        }

        int previous = 0;
        int twoStepsPrevious = 0;

        for (int i = 1; i < n; i++) {

            int oneStep = previous
                + Math.abs(heights[i] - heights[i - 1]);

            int twoSteps = Integer.MAX_VALUE;

            if (i > 1) {
                twoSteps = twoStepsPrevious
                    + Math.abs(heights[i] - heights[i - 2]);
            }

            int current = Math.min(oneStep, twoSteps);

            twoStepsPrevious = previous;
            previous = current;
        }

        return previous;
    }
}
Note: Different coding platforms may require a different method signature. Keep the algorithm the same and adjust the method name or parameters to match your platform.
Summary
Problem	Algorithm Used	Time Complexity	Space Complexity
Frog Jump (Problem 1)	Space-Optimized Dynamic Programming	O(n)	O(1)
