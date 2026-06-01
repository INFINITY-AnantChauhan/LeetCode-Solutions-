# 1. Two Sum

**Link:** https://leetcode.com/problems/two-sum/
**Difficulty:** 🟢 Easy
**Date:** *1st June 2026*
**Tags:** `Arrays` `HashMap` `Two Pass`

---

## Approach

For each element `nums[i]`, check if its complement (`target - nums[i]`) has
been seen before. Store every value and its index in a hash map as you go.
One pass through the array is enough — the moment you find a complement in
the map, you have your answer.

```
nums = [2, 7, 11, 15],  target = 9

i=0  val=2   complement=7   map={}          → not found, insert {2:0}
i=1  val=7   complement=2   map={2:0}       → found at index 0!  return [0, 1]
```

---

## Complexity

| | C++ (`unordered_map`) |
|---|---|
| **Time** | O(n) average |
| **Space** | O(n) |

---

## Brute force vs optimised

| Approach | Time | Space | Notes |
|----------|------|-------|-------|
| Nested loops | O(n²) | O(1) | Check every pair — simple but slow |
| Hash map (this) | O(n) avg | O(n) | One pass; trade space for time |

The brute-force double loop is fine for tiny inputs but blows up at
`n = 10^4`. The hash map cuts it to a single scan.

---

## C++ notes

`unordered_map<int,int>` handles everything — hashing, collision resolution,
resizing. `seen.find(complement)` returns an iterator; checking against
`seen.end()` is the idiomatic way to test for presence and get the value in
one lookup (avoids double lookup vs `seen.count() + seen[]`).

---

## What tripped me up

- Make sure to insert `nums[i]` **after** the complement check — otherwise a
  single element could match itself (e.g. `nums = [3,3], target = 6` would
  wrongly match index 0 with itself on the first iteration).


## LeetCode Screenshot
<img width="1917" height="872" alt="image" src="https://github.com/user-attachments/assets/78ccd0eb-46fd-4fb7-a23b-3172c8318761" />
