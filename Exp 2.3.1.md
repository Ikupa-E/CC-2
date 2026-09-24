# Add Digits

## Problem

Given an integer `num`, repeatedly add all its digits until the result has only one digit.

Return the resulting single-digit number.

### Example

**Input:**

```text
num = 38
```

**Process:**

```text
3 + 8 = 11
1 + 1 = 2
```

**Output:**

```text
2
```

---

## Approach

The problem can be solved using the **Digital Root** concept.

For any positive integer, repeatedly adding its digits until only one digit remains gives its digital root.

The digital root can be calculated using:

```text
1 + (num - 1) % 9
```

For `num = 0`, the answer is `0`.

### Why does this work?

Numbers have the same remainder when divided by `9` as the sum of their digits.

For example:

```text
38 % 9 = 2
3 + 8 = 11
11 % 9 = 2
```

Therefore, instead of repeatedly calculating the sum of the digits, we can directly calculate the digital root using modulo `9`.

---

## Algorithm

1. Check if `num` is `0`.
2. If it is `0`, return `0`.
3. Otherwise, calculate:

   ```text
   1 + (num - 1) % 9
   ```
4. Return the result.

---

## Complexity

* **Time Complexity:** `O(1)`
* **Space Complexity:** `O(1)`

---

## Code

```java
class Solution {
    public int addDigits(int num) {
        if (num == 0) {
            return 0;
        }

        return 1 + (num - 1) % 9;
    }
}
```

---

## Key Takeaway

The important concept in this problem is the **digital root**.

Using modulo `9` allows us to find the final single digit directly without repeatedly adding the digits.
