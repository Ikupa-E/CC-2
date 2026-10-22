# 1143. Longest Common Subsequence

**Difficulty:** Medium
**Topics:** String, Dynamic Programming
**Link:** https://leetcode.com/problems/longest-common-subsequence/

## Problem Statement

Given two strings `text1` and `text2`, return the length of their **longest common subsequence**. If there is no common subsequence, return `0`.

A **subsequence** of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.

- For example, `"ace"` is a subsequence of `"abcde"`.

A **common subsequence** of two strings is a subsequence that is common to both strings.

### Examples

**Example 1:**

```
Input: text1 = "abcde", text2 = "ace"
Output: 3
Explanation: The longest common subsequence is "ace" and its length is 3.
```

**Example 2:**

```
Input: text1 = "abc", text2 = "abc"
Output: 3
Explanation: The longest common subsequence is "abc" and its length is 3.
```

**Example 3:**

```
Input: text1 = "abc", text2 = "def"
Output: 0
Explanation: There is no such common subsequence, so the result is 0.
```

### Constraints

- `1 <= text1.length, text2.length <= 1000`
- `text1` and `text2` consist of only lowercase English characters.

## Approach: Bottom-Up Dynamic Programming

Let `dp[i][j]` be the length of the LCS of the first `i` characters of `text1` and the first `j` characters of `text2`.

1. Base case: `dp[0][j] = 0` and `dp[i][0] = 0`, since an empty string has no common subsequence with anything.
2. For each `i` from `1` to `m` and `j` from `1` to `n`:
   - If `text1.charAt(i - 1) == text2.charAt(j - 1)`, the characters match, so extend the previous best:

     ```
     dp[i][j] = dp[i - 1][j - 1] + 1
     ```

   - Otherwise, skip a character from one string or the other and take the better result:

     ```
     dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])
     ```

3. The answer is `dp[m][n]`.

### Walkthrough: `text1 = "abcde"`, `text2 = "ace"`

|           | "" | a | c | e |
|-----------|----|---|---|---|
| **""**    | 0  | 0 | 0 | 0 |
| **a**     | 0  | 1 | 1 | 1 |
| **ab**    | 0  | 1 | 1 | 1 |
| **abc**   | 0  | 1 | 2 | 2 |
| **abcd**  | 0  | 1 | 2 | 2 |
| **abcde** | 0  | 1 | 2 | 3 |

`dp[5][3] = 3`, matching the LCS `"ace"`.

## Java Solution

```java
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        int m = text1.length();
        int n = text2.length();
        int[][] dp = new int[m + 1][n + 1];

        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (text1.charAt(i - 1) == text2.charAt(j - 1)) {
                    dp[i][j] = dp[i - 1][j - 1] + 1;
                } else {
                    dp[i][j] = Math.max(dp[i - 1][j], dp[i][j - 1]);
                }
            }
        }

        return dp[m][n];
    }
}
```

### Space-Optimized Version

Each row only depends on the previous row, so two 1D arrays are enough. Using the shorter string for the columns keeps the arrays as small as possible.

```java
class Solution {
    public int longestCommonSubsequence(String text1, String text2) {
        // Make text2 the shorter string so the arrays are small
        if (text1.length() < text2.length()) {
            String temp = text1;
            text1 = text2;
            text2 = temp;
        }

        int m = text1.length();
        int n = text2.length();
        int[] prev = new int[n + 1];
        int[] curr = new int[n + 1];

        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (text1.charAt(i - 1) == text2.charAt(j - 1)) {
                    curr[j] = prev[j - 1] + 1;
                } else {
                    curr[j] = Math.max(prev[j], curr[j - 1]);
                }
            }
            int[] temp = prev;
            prev = curr;
            curr = temp;
        }

        return prev[n];
    }
}
```

## Complexity Analysis

Let `m = text1.length()` and `n = text2.length()`.

| Version         | Time        | Space              | Explanation                                                        |
|-----------------|-------------|--------------------|--------------------------------------------------------------------|
| **2D DP**       | `O(m * n)`  | `O(m * n)`         | Every cell of the `(m + 1) x (n + 1)` table is computed once.      |
| **Optimized**   | `O(m * n)`  | `O(min(m, n))`     | Only two rows of the shorter string's length are kept in memory.   |

## Notes

- A subsequence does not need to be contiguous. That is the difference from the *Longest Common Substring* problem, which requires consecutive characters.
- To recover the actual LCS string (not just its length), keep the full 2D table and backtrack from `dp[m][n]`: move diagonally on a match, otherwise move toward the larger neighbor.
- A top-down memoized recursion with the same recurrence also works, but bottom-up avoids recursion depth issues for strings up to 1000 characters.
