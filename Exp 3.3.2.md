# 1971. Find if Path Exists in Graph

**Difficulty:** Easy
**Topics:** Depth-First Search, Breadth-First Search, Union Find, Graph
**Link:** https://leetcode.com/problems/find-if-path-exists-in-graph/

## Problem Statement

There is a **bi-directional** graph with `n` vertices, where each vertex is labeled from `0` to `n - 1` (**inclusive**). The edges in the graph are represented as a 2D integer array `edges`, where each `edges[i] = [ui, vi]` denotes a bi-directional edge between vertex `ui` and vertex `vi`. Every vertex pair is connected by **at most one** edge, and no vertex has an edge to itself.

You want to determine if there is a **valid path** that exists from vertex `source` to vertex `destination`.

Given `edges` and the integers `n`, `source`, and `destination`, return `true` if there is a valid path from `source` to `destination`, or `false` otherwise.

### Examples

**Example 1:**

```
Input: n = 3, edges = [[0,1],[1,2],[2,0]], source = 0, destination = 2
Output: true
Explanation: There are two paths from vertex 0 to vertex 2:
- 0 -> 1 -> 2
- 0 -> 2
```

**Example 2:**

```
Input: n = 6, edges = [[0,1],[0,2],[3,5],[5,4],[4,3]], source = 0, destination = 5
Output: false
Explanation: There is no path from vertex 0 to vertex 5.
```

### Constraints

- `1 <= n <= 2 * 10^5`
- `0 <= edges.length <= 2 * 10^5`
- `edges[i].length == 2`
- `0 <= ui, vi <= n - 1`
- `ui != vi`
- `0 <= source, destination <= n - 1`
- There are no duplicate edges.
- There are no self edges.

## Approach: BFS on an Adjacency List

The question is really "are `source` and `destination` in the same connected component?"

1. If `source == destination`, return `true` immediately (a vertex is always connected to itself).
2. Build an adjacency list. Since the graph is bi-directional, add each edge in **both** directions.
3. Start a BFS from `source`, using a `visited` array so no vertex is processed twice.
4. If `destination` is ever reached, return `true`.
5. If the queue empties without reaching `destination`, return `false`.

### Walkthrough: Example 2

```
Components:  {0, 1, 2}   and   {3, 4, 5}

BFS from 0 visits: 0 -> 1 -> 2, then the queue is empty.
Vertex 5 was never visited, so the answer is false.
```

## Java Solution

```java
import java.util.ArrayDeque;
import java.util.ArrayList;
import java.util.List;
import java.util.Queue;

class Solution {
    public boolean validPath(int n, int[][] edges, int source, int destination) {
        if (source == destination) {
            return true;
        }

        // Build adjacency list
        List<List<Integer>> graph = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            graph.add(new ArrayList<>());
        }
        for (int[] edge : edges) {
            graph.get(edge[0]).add(edge[1]);
            graph.get(edge[1]).add(edge[0]); // bi-directional
        }

        // BFS from source
        boolean[] visited = new boolean[n];
        Queue<Integer> queue = new ArrayDeque<>();
        queue.offer(source);
        visited[source] = true;

        while (!queue.isEmpty()) {
            int node = queue.poll();
            for (int neighbor : graph.get(node)) {
                if (neighbor == destination) {
                    return true;
                }
                if (!visited[neighbor]) {
                    visited[neighbor] = true;
                    queue.offer(neighbor);
                }
            }
        }

        return false;
    }
}
```

### Alternative: Union-Find (Disjoint Set)

Union the endpoints of every edge, then check whether `source` and `destination` share the same root. This avoids building an adjacency list.

```java
class Solution {
    public boolean validPath(int n, int[][] edges, int source, int destination) {
        int[] parent = new int[n];
        int[] rank = new int[n];
        for (int i = 0; i < n; i++) {
            parent[i] = i;
        }

        for (int[] edge : edges) {
            union(parent, rank, edge[0], edge[1]);
        }

        return find(parent, source) == find(parent, destination);
    }

    private int find(int[] parent, int x) {
        while (parent[x] != x) {
            parent[x] = parent[parent[x]]; // path compression
            x = parent[x];
        }
        return x;
    }

    private void union(int[] parent, int[] rank, int a, int b) {
        int rootA = find(parent, a);
        int rootB = find(parent, b);
        if (rootA == rootB) {
            return;
        }
        // union by rank
        if (rank[rootA] < rank[rootB]) {
            parent[rootA] = rootB;
        } else if (rank[rootA] > rank[rootB]) {
            parent[rootB] = rootA;
        } else {
            parent[rootB] = rootA;
            rank[rootA]++;
        }
    }
}
```

## Complexity Analysis

Let `V = n` be the number of vertices and `E = edges.length` the number of edges.

| Version        | Time              | Space      | Explanation                                                                                   |
|----------------|-------------------|------------|-----------------------------------------------------------------------------------------------|
| **BFS**        | `O(V + E)`        | `O(V + E)` | Building the graph takes `O(V + E)`. BFS visits each vertex and edge at most once. The adjacency list, `visited` array, and queue take `O(V + E)` space. |
| **Union-Find** | `O(V + E * α(V))` | `O(V)`     | `α` is the inverse Ackermann function, which is effectively constant. Only the `parent` and `rank` arrays are stored. |

## Notes

- With `n` up to `2 * 10^5`, a **recursive DFS** can overflow the stack on a long chain of vertices. Use BFS or an iterative DFS to be safe.
- Remember to add edges in **both directions**. Forgetting this treats the graph as directed and gives wrong answers.
- Union-Find is a good choice when you need to answer many connectivity queries, since the structure is built once and each query is nearly constant time.
