# 70. Climbing Stairs — Notes

## Problem in one line
You can climb `1` or `2` steps at a time — find the number of distinct ways to reach step `n`.

---

## Solution — Iterative DP (Space-Optimised Fibonacci)

```cpp
class Solution {
public:
    int climbStairs(int n) {
        if (n <= 2)
            return n;

        int a = 1, b = 2;

        for (int i = 3; i <= n; i++) {
            int c = a + b;
            a = b;
            b = c;
        }

        return b;
    }
};
```

---

## Intuition
To reach step `i`, you either came from:
- Step `i-1` (took 1 step), or
- Step `i-2` (took 2 steps)

So the number of ways to reach `i` is:

```
ways(i) = ways(i-1) + ways(i-2)
```

This is exactly the **Fibonacci recurrence**.

| n | 1 | 2 | 3 | 4 | 5 | 6 |
|---|---|---|---|---|---|---|
| ways | 1 | 2 | 3 | 5 | 8 | 13 |

Instead of an array, only the last two values are needed at any point → two variables `a` and `b`.

---

## Dry Run
Input: `n = 5`

| i | a (i-2) | b (i-1) | c = a+b (new) |
|---|---------|---------|---------------|
| — |    1    |    2    |       —       |
| 3 |    2    |    3    |       3       |
| 4 |    3    |    5    |       5       |
| 5 |    5    |    8    |       8       |

Return `b = 8` ✓

---

## Complexity

| | |
|---|---|
| Time  | **O(n)** — single loop from 3 to n |
| Space | **O(1)** — only two variables |

---

## Base Cases

| n | Return | Reason |
|---|--------|--------|
| `1` | `1` | Only one way: `{1}` |
| `2` | `2` | Two ways: `{1,1}` or `{2}` |

> Without the `if (n <= 2)` guard, `n = 1` would skip the loop entirely and return `b = 2`, which is wrong.

---

## Variable Roles

```
a  →  ways to reach step (i-2)   [two steps back]
b  →  ways to reach step (i-1)   [one step back]
c  →  ways to reach step  i      [current]
```

Each iteration slides the window forward:
```
a = b      (old i-1 becomes new i-2)
b = c      (current becomes new i-1)
```

---

## Common Mistakes

1. **Missing base case** — without `n <= 2` check, `n = 1` returns `2` instead of `1`
2. **Wrong init** — seeding `a = 0, b = 1` (pure Fibonacci) gives off-by-one; use `a = 1, b = 2`
3. **Off-by-one loop bound** — loop must go `i <= n`, not `i < n`

---

## Related Problems
| # | Title | Connection |
|---|---|---|
| 198 | House Robber | Same rolling DP pattern |
| 746 | Min Cost Climbing Stairs | Climbing stairs + cost minimisation |
| 509 | Fibonacci Number | This solution is Fibonacci with a shifted seed |


## LeetCode Screenshot
<img width="1919" height="867" alt="Screenshot 2026-06-05 085223" src="https://github.com/user-attachments/assets/c85d15af-f95b-49b9-8fbe-6b715e09fac4" />
