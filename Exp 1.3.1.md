232. Implement Queue using Stacks
Problem

Implement a first in first out (FIFO) queue using only two stacks.

The implemented queue should support the following operations:

push(x) – Push element x to the back of the queue.
pop() – Removes the element from the front of the queue and returns it.
peek() – Returns the element at the front of the queue.
empty() – Returns whether the queue is empty.
Example
Input:
["MyQueue", "push", "push", "peek", "pop", "empty"]
[[], [1], [2], [], [], []]

Output:
[null, null, null, 1, 1, false]
Optimal Solution

Algorithm Used: Two Stacks

Idea

A queue follows FIFO (First In, First Out) order, while a stack follows LIFO (Last In, First Out) order.

To implement a queue using stacks, we use two stacks:

stack1 stores the elements in their normal insertion order.
stack2 is used temporarily to reverse the order when performing pop() or peek().

For push():

Add the new element directly to stack1.

For pop():

Move all elements from stack1 to stack2.
The oldest element is now at the top of stack2.
Remove and store that element.
Move the remaining elements back to stack1.
Return the removed element.

For peek():

Perform the same transfer as pop(), but use peek() instead of removing the element.

For empty():

Check whether stack1 is empty.

This converts the stack's LIFO behavior into the queue's FIFO behavior.

Time Complexity
push() → O(1)
pop() → O(n)
peek() → O(n)
empty() → O(1)
Space Complexity
O(n)
Java Solution
import java.util.Stack;

class MyQueue {

    Stack<Integer> stack1;
    Stack<Integer> stack2;

    public MyQueue() {
        stack1 = new Stack<>();
        stack2 = new Stack<>();
    }

    public void push(int x) {
        stack1.push(x);
    }

    public int pop() {

        while (!stack1.isEmpty()) {
            stack2.push(stack1.pop());
        }

        int result = stack2.pop();

        while (!stack2.isEmpty()) {
            stack1.push(stack2.pop());
        }

        return result;
    }

    public int peek() {

        while (!stack1.isEmpty()) {
            stack2.push(stack1.pop());
        }

        int result = stack2.peek();

        while (!stack2.isEmpty()) {
            stack1.push(stack2.pop());
        }

        return result;
    }

    public boolean empty() {
        return stack1.isEmpty();
    }
}
Summary
LeetCode Problem	Algorithm Used	Time Complexity	Space Complexity
232. Implement Queue using Stacks	Two Stacks	O(n) for pop() / peek()	O(n)
Concepts Covered
Queue
Stack
FIFO
LIFO
Two-Stack Technique
Data Structure Implementation
Stack Operations
O(n) Time Complexity
O(n) Space Complexity
