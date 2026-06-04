# 53. Maximum Subarray — Notes

## Problem in one line
Find the contiguous subarray with the **largest sum** and return that sum.

---

## Solution — Kadane's Algorithm

```cpp
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int currentSum = nums[0];
        int maxSum = nums[0];
        for (int i = 1; i < nums.size(); i++) {
            currentSum = max(nums[i], currentSum + nums[i]);
            maxSum = max(maxSum, currentSum);
        }
        return maxSum;
    }
};
```

---

## Intuition
At every index `i`, you face one choice:

> *Extend the previous subarray, or start fresh at `i`?*

```
currentSum = max(nums[i], currentSum + nums[i])
```

- If `currentSum > 0` → **extend** (it adds value)
- If `currentSum ≤ 0` → **restart** (it only drags you down)

After each step, update the global best.

---

## Dry Run
Input: `[-2, 1, -3, 4, -1, 2, 1, -5, 4]`

| i | nums[i] | currentSum | maxSum |
|---|---------|------------|--------|
| — |   —     |    -2      |   -2   |
| 1 |   1     |     1      |    1   |
| 2 |  -3     |    -2      |    1   |
| 3 |   4     |     4      |    4   |
| 4 |  -1     |     3      |    4   |
| 5 |   2     |     5      |    5   |
| 6 |   1     |     6      |  **6** |
| 7 |  -5     |     1      |    6   |
| 8 |   4     |     5      |    6   |

Answer: `6`  →  subarray `[4, -1, 2, 1]`

---

## Complexity

| | |
|---|---|
| Time  | **O(n)** — single pass |
| Space | **O(1)** — two variables |

---

## Edge Cases

| Input | Expected | Reason |
|---|---|---|
| `[-1]` | `-1` | Single element; must pick it |
| `[-2, -1]` | `-1` | All negative; pick least negative |
| `[5, 4, -1, 7, 8]` | `23` | Entire array is optimal |

> A subarray must have **at least one** element — empty subarrays are not valid.

---

## Common Mistakes

1. **Init `maxSum = 0`** — fails on all-negative inputs; always use `nums[0]`
2. **Init `currentSum = 0`** — same problem; first element gets skipped
3. **Loop from `i = 0`** — re-processes `nums[0]` which was already used to seed both variables; start from `i = 1`

---

## Related Problems
| # | Title | Connection |
|---|---|---|
| 152 | Maximum Product Subarray | Same DP pattern; track min as well |
| 918 | Maximum Sum Circular Subarray | Kadane's + circular wraparound trick |
| 1749 | Maximum Absolute Sum of Any Subarray | Run Kadane's for max and min both |


## LeetCode Screenshot
<img width="1919" height="867" alt="Screenshot 2026-06-04 230822" src="https://github.com/user-attachments/assets/6eb4b5c3-9c1b-4deb-845d-1a31bd1708d7" />
