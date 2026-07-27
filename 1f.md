

# 328. Odd Even Linked List

## Problem

Given the head of a singly linked list, group all nodes with odd indices together followed by nodes with even indices.

The relative order inside the odd and even groups must remain the same.

Note:
- Indexing starts from **1**.

### Example

```
Input:
head = [1,2,3,4,5]

Output:
[1,3,5,2,4]
```

Explanation:

Odd indexed nodes:

```
1 → 3 → 5
```

Even indexed nodes:

```
2 → 4
```

Combined:

```
1 → 3 → 5 → 2 → 4
```

---

## Optimal Solution

**Algorithm Used:** Two Pointer Linked List Partitioning

### Idea

Maintain two separate linked lists:

- Odd list → stores nodes at odd positions.
- Even list → stores nodes at even positions.

Using two pointers:

1. Move through the linked list and rearrange nodes.
2. Connect all odd nodes together.
3. Connect the end of the odd list to the beginning of the even list.

This is done without creating extra nodes.

### Time Complexity

- **O(n)**

### Space Complexity

- **O(1)**

---

## Java Solution

```java
class Solution {

    public ListNode oddEvenList(ListNode head) {

        if (head == null || head.next == null) {
            return head;
        }

        ListNode odd = head;
        ListNode even = head.next;

        ListNode evenHead = even;


        while (even != null && even.next != null) {

            // Connect odd nodes
            odd.next = even.next;
            odd = odd.next;


            // Connect even nodes
            even.next = odd.next;
            even = even.next;
        }


        // Attach even list after odd list
        odd.next = evenHead;


        return head;
    }
}
```

---

# Summary

| LeetCode Problem | Algorithm Used | Time Complexity | Space Complexity |
|------------------|----------------|-----------------|------------------|
| 234. Palindrome Linked List | Fast & Slow Pointer + Reverse Linked List | O(n) | O(1) |
| 328. Odd Even Linked List | Two Pointer Linked List Partitioning | O(n) | O(1) |

---

## Concepts Covered

- Linked List Traversal
- Fast and Slow Pointer Technique
- Reversing Linked Lists
- In-place Modification
- Two Pointer Technique
