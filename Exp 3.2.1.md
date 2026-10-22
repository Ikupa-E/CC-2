# 322. Coin Change

**Difficulty:** Medium
**Topics:** Array, Dynamic Programming, Breadth-First Search
**Link:** https://leetcode.com/problems/coin-change/

## Problem Statement

You are given an integer array `coins` representing coins of different denominations and an integer `amount` representing a total amount of money.

Return the **fewest number of coins** that you need to make up that amount. If that amount of money cannot be made up by any combination of the coins, return `-1`.

You may assume that you have an **infinite number of each kind of coin**.

### Examples

**Example 1:**

```
Input: coins = [1,2,5], amount = 11
Output: 3
Explanation: 11 = 5 + 5 + 1
```

**Example 2:**

```
Input: coins = [2], amount = 3
Output: -1
```

**Example 3:**

```
Input: coins = [1], amount = 0
Output: 0
```

### Constraints

- `1 <= coins.length <= 12`
- `1 <= coins[i] <= 2^31 - 1`
- `0 <= amount <= 10^4`

## Approach: Bottom-Up Dynamic Programming

A greedy strategy (always pick the largest coin) fails in general. For example, with `coins = [1, 3, 4]` and `amount = 6`, greedy gives `4 + 1 + 1` (3 coins), but the optimal answer is `3 + 3` (2 coins).

Instead, we build the answer for every amount from `0` up to `amount`:

1. Let `dp[i]` be the fewest coins needed to make amount `i`.
2. Base case: `dp[0] = 0`.
3. Initialize every other `dp[i]` to `amount + 1`, which acts as "infinity" because no valid answer can ever exceed `amount` coins (all coins are at least 1).
4. For each amount `i` from `1` to `amount`, and for each coin `c` with `c <= i`:

   ```
   dp[i] = min(dp[i], dp[i - c] + 1)
   ```

5. If `dp[amount] > amount`, the amount is unreachable, so return `-1`. Otherwise return `dp[amount]`.

### Walkthrough: `coins = [1, 2, 5]`, `amount = 11`

| i     | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 |
|-------|---|---|---|---|---|---|---|---|---|---|----|----|
| dp[i] | 0 | 1 | 1 | 2 | 2 | 1 | 2 | 2 | 3 | 3 | 2  | 3  |

`dp[11] = 3` (`5 + 5 + 1`).

## Java Solution

```java
import java.util.Arrays;

class Solution {
    public int coinChange(int[] coins, int amount) {
        int[] dp = new int[amount + 1];
        Arrays.fill(dp, amount + 1); // "infinity": more than any possible answer
        dp[0] = 0;

        for (int i = 1; i <= amount; i++) {
            for (int coin : coins) {
                if (coin <= i) {
                    dp[i] = Math.min(dp[i], dp[i - coin] + 1);
                }
            }
        }

        return dp[amount] > amount ? -1 : dp[amount];
    }
}
```

## Complexity Analysis

Let `n` be the number of coins and `amount` be the target amount.

| Complexity | Value                | Explanation                                                        |
|------------|----------------------|--------------------------------------------------------------------|
| **Time**   | `O(amount * n)`      | For each amount from 1 to `amount`, we try every coin once.        |
| **Space**  | `O(amount)`          | The `dp` array stores one value for each amount from 0 to `amount`.|

## Notes

- `coin <= i` also protects against very large coin values (up to `2^31 - 1`), since they are skipped rather than used in arithmetic.
- An alternative approach is BFS, treating each amount as a node and each coin as an edge. The first time you reach `0`, the number of levels traversed is the answer. It has the same worst-case complexity.
- A top-down (memoized recursion) version works too, but the bottom-up version avoids recursion overhead and stack depth concerns.
