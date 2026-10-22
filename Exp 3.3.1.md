# 200. Number of Islands

**Difficulty:** Medium
**Topics:** Array, Depth-First Search, Breadth-First Search, Union Find, Matrix
**Link:** https://leetcode.com/problems/number-of-islands/

## Problem Statement

Given an `m x n` 2D binary grid `grid` which represents a map of `'1'`s (land) and `'0'`s (water), return the **number of islands**.

An **island** is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

### Examples

**Example 1:**

```
Input: grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
Output: 1
```

**Example 2:**

```
Input: grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
Output: 3
```

### Constraints

- `m == grid.length`
- `n == grid[i].length`
- `1 <= m, n <= 300`
- `grid[i][j]` is `'0'` or `'1'`

## Approach: DFS Flood Fill

Every island is a connected group of land cells. If we count each group exactly once, we have the answer.

1. Scan the grid cell by cell.
2. When we find a `'1'`, we have discovered a new island, so increment the count.
3. Run a DFS from that cell to "sink" the entire island, marking every connected land cell as `'0'` (visited). This way the same island is never counted again.
4. The DFS explores the four directions: up, down, left, and right. It stops when it goes out of bounds or reaches water.
5. After the scan finishes, the count is the number of islands.

### Walkthrough: Example 2

```
1 1 0 0 0        Scan finds (0,0) -> island #1, DFS sinks the 2x2 block
1 1 0 0 0
0 0 1 0 0        Scan finds (2,2) -> island #2, DFS sinks that single cell
0 0 0 1 1        Scan finds (3,3) -> island #3, DFS sinks (3,3) and (3,4)
```

Total islands: `3`.

## Java Solution

```java
class Solution {
    public int numIslands(char[][] grid) {
        int count = 0;
        int rows = grid.length;
        int cols = grid[0].length;

        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                if (grid[i][j] == '1') {
                    count++;
                    dfs(grid, i, j);
                }
            }
        }

        return count;
    }

    private void dfs(char[][] grid, int i, int j) {
        if (i < 0 || j < 0 || i >= grid.length || j >= grid[0].length
                || grid[i][j] == '0') {
            return;
        }

        grid[i][j] = '0'; // mark as visited by sinking the land

        dfs(grid, i + 1, j);
        dfs(grid, i - 1, j);
        dfs(grid, i, j + 1);
        dfs(grid, i, j - 1);
    }
}
```

### Alternative: BFS

BFS avoids deep recursion and uses an explicit queue instead.

```java
import java.util.LinkedList;
import java.util.Queue;

class Solution {
    public int numIslands(char[][] grid) {
        int count = 0;
        int rows = grid.length;
        int cols = grid[0].length;
        int[][] dirs = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};

        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                if (grid[i][j] == '1') {
                    count++;
                    grid[i][j] = '0';
                    Queue<int[]> queue = new LinkedList<>();
                    queue.offer(new int[]{i, j});

                    while (!queue.isEmpty()) {
                        int[] cell = queue.poll();
                        for (int[] d : dirs) {
                            int r = cell[0] + d[0];
                            int c = cell[1] + d[1];
                            if (r >= 0 && c >= 0 && r < rows && c < cols
                                    && grid[r][c] == '1') {
                                grid[r][c] = '0';
                                queue.offer(new int[]{r, c});
                            }
                        }
                    }
                }
            }
        }

        return count;
    }
}
```

## Complexity Analysis

Let `m` be the number of rows and `n` the number of columns.

| Version | Time       | Space                | Explanation                                                                                       |
|---------|------------|----------------------|---------------------------------------------------------------------------------------------------|
| **DFS** | `O(m * n)` | `O(m * n)` worst case| Each cell is visited a constant number of times. The recursion stack can reach `m * n` if the whole grid is land. |
| **BFS** | `O(m * n)` | `O(min(m, n))`       | Each cell is enqueued at most once. The queue holds at most about one diagonal's worth of cells.  |

## Notes

- Both solutions **modify the input grid** to track visited cells, which avoids an extra `visited` array. If the input must stay unchanged, use a `boolean[][] visited` array, which costs `O(m * n)` extra space.
- Connectivity is only horizontal and vertical. Diagonal neighbors do **not** belong to the same island.
- A Union-Find (Disjoint Set) solution also works: union adjacent land cells and count the number of distinct roots.
- With `m, n <= 300`, the DFS recursion depth can reach 90,000 in the worst case, which may risk a stack overflow in some environments. BFS or an iterative DFS is safer in that case.
