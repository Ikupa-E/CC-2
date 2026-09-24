236. Lowest Common Ancestor of a Binary Tree

Problem

Given a binary tree, find the lowest common ancestor (LCA) of two given nodes in the tree.

The lowest common ancestor is defined as the lowest node in the tree that has both p and q as descendants.

A node can be a descendant of itself.

Example

Input:
root = [3,5,1,6,2,0,8,null,null,7,4]
p = 5
q = 1

Output:
3

Optimal Solution

Algorithm Used: Recursive Depth-First Search (DFS)

Idea

We use recursion to search for nodes p and q in the left and right subtrees.

For each node:

If the current node is null, return null.

If the current node is p or q, return the current node.

Recursively search the left subtree.

Recursively search the right subtree.

If both left and right searches return a node, then the current node is the lowest common ancestor.

If only one side returns a node, return that node.

For example:

        3
       /       5   1

p = 5
q = 1

The left subtree finds 5 and the right subtree finds 1.
Since both sides contain one of the target nodes, 3 is their lowest common ancestor.

Time Complexity

O(n)

Space Complexity

O(h) where h is the height of the binary tree due to the recursion stack.

In the worst case, the tree can be completely unbalanced, making the space complexity O(n).

Java Solution

class Solution {

    public TreeNode lowestCommonAncestor(
        TreeNode root,
        TreeNode p,
        TreeNode q
    ) {

        // If the current node is null,
        // or it is one of the target nodes
        if (root == null || root == p || root == q) {
            return root;
        }

        // Search the left subtree
        TreeNode left = lowestCommonAncestor(root.left, p, q);

        // Search the right subtree
        TreeNode right = lowestCommonAncestor(root.right, p, q);

        // If both sides contain a target node,
        // current node is the lowest common ancestor
        if (left != null && right != null) {
            return root;
        }

        // Return whichever side contains a target node
        return left != null ? left : right;
    }
}

Summary

LeetCode Problem

Algorithm Used

Time Complexity

Space Complexity

236. Lowest Common Ancestor of a Binary Tree

Recursive DFS

O(n)

O(h)

Concepts Covered

Binary Trees

Tree Traversal

Depth-First Search (DFS)

Recursion

Lowest Common Ancestor

Binary Tree Structure

Recursive Backtracking

O(n) Time Complexity

O(h) Space Complexity
