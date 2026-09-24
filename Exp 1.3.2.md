84. Largest Rectangle in Histogram

Problem

Given an array of integers heights representing the histogram's bar height where the width of each bar is 1, return the area of the largest rectangle in the histogram.

Example

Input:
heights = [2,1,5,6,2,3]

Output:
10

Optimal Solution

Algorithm Used: Monotonic Stack

Idea

The main goal is to find the largest rectangle that can be formed using one or more consecutive bars.

We use a monotonic increasing stack to keep track of the indices of bars whose heights are in increasing order.

For every bar:

If the current bar is taller than the bar at the top of the stack, push its index.

If the current bar is shorter, the rectangle represented by the bar at the top of the stack can no longer extend to the right.

Remove the index from the stack and calculate its rectangle area.

Continue removing bars while the current height is smaller than the height at the top of the stack.

The width of the rectangle is determined by the current index and the new stack top.

A final height of 0 is processed after the last bar to make sure all remaining bars are removed from the stack and their areas are calculated.

For example:

heights = [2,1,5,6,2,3]

The largest rectangle is formed by heights 5 and 6.

Height = 5
Width  = 2

Area = 5 × 2 = 10

Time Complexity

O(n)

Space Complexity

O(n)

Java Solution

import java.util.Stack;

class Solution {

    public int largestRectangleArea(int[] heights) {

        Stack<Integer> stack = new Stack<>();
        int maxArea = 0;

        for (int i = 0; i <= heights.length; i++) {

            int currentHeight =
                (i == heights.length) ? 0 : heights[i];

            while (!stack.isEmpty() &&
                   currentHeight < heights[stack.peek()]) {

                int height = heights[stack.pop()];

                int width;

                if (stack.isEmpty()) {
                    width = i;
                } else {
                    width = i - stack.peek() - 1;
                }

                int area = height * width;

                maxArea = Math.max(maxArea, area);
            }

            if (i < heights.length) {
                stack.push(i);
            }
        }

        return maxArea;
    }
}

Summary

LeetCode Problem

Algorithm Used

Time Complexity

Space Complexity

84. Largest Rectangle in Histogram

Monotonic Stack

O(n)

O(n)

Concepts Covered

Stack

Monotonic Stack

Histograms

Rectangle Area

Array Traversal

Finding Maximum Area

O(n) Time Complexity

O(n) Space Complexity
