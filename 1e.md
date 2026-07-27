

# 234. Palindrome Linked List

## Problem

Given the head of a singly linked list, return `true` if the linked list is a palindrome.

A palindrome is a sequence that reads the same forward and backward.

### Example

```
Input:
head = [1,2,2,1]

Output:
true
```

The linked list reads the same from both directions.

---

## Optimal Solution

**Algorithm Used:** Fast and Slow Pointer + Reverse Second Half of Linked List

### Idea

A linked list cannot be accessed backwards like an array, so we:

1. Use two pointers:
   - `slow` moves one step at a time.
   - `fast` moves two steps at a time.
2. When `fast` reaches the end, `slow` will be at the middle.
3. Reverse the second half of the linked list.
4. Compare the first half and the reversed second half.
5. Return `true` if all values match.

### Time Complexity

- **O(n)**

### Space Complexity

- **O(1)**

---

## Java Solution

```java
class Solution {

    public boolean isPalindrome(ListNode head) {

        if (head == null || head.next == null) {
            return true;
        }

        // Find middle of linked list
        ListNode slow = head;
        ListNode fast = head;

        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }

        // Reverse second half
        ListNode secondHalf = reverse(slow);

        // Compare both halves
        ListNode firstHalf = head;

        while (secondHalf != null) {

            if (firstHalf.val != secondHalf.val) {
                return false;
            }

            firstHalf = firstHalf.next;
            secondHalf = secondHalf.next;
        }

        return true;
    }


    private ListNode reverse(ListNode head) {

        ListNode previous = null;
        ListNode current = head;

        while (current != null) {

            ListNode next = current.next;

            current.next = previous;

            previous = current;
            current = next;
        }

        return previous;
    }
}
```

