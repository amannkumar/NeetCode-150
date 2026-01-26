# **Arrays & Hashing Question-9**

# **Longest Consecutive Sequence**

---

## **Intuition**

> **⚠️ Note:** This intuition is taken from the **NeetCode website**.

We are given an **unsorted array of integers** and need to find the **length of the longest consecutive elements sequence**.

The key challenge:

- Sorting would simplify the problem, but it increases time complexity.
- We want a solution better than `O(n log n)`.

---

## **Key Observation**

A number can be the **start of a sequence** only if:

If it exists, then this number is **not the beginning** of a new sequence.

---

## **Solutions Overview**

| Solution | Approach          | Time Complexity | Space Complexity |
| -------- | ----------------- | --------------- | ---------------- |
| 1        | Brute Force       | `O(n²)`         | `O(1)`           |
| 2        | Sorting           | `O(n log n)`    | `O(1)`           |
| 3        | HashMap.          | `O(n)`          | `O(n)`           |
| 4        | HashSet (Optimal) | `O(n)`          | `O(n)`           |

---

## **2nd Solution – Sorting**

''
