# LeetCode #21 — Merge Two Sorted Lists

**Difficulty:** Easy  
**Topic:** Linked List, Two Pointers  
**Link:** https://leetcode.com/problems/merge-two-sorted-lists/

---

## Problem Statement

You are given the heads of two sorted linked lists `list1` and `list2`.  
Merge the two lists into one **sorted** list by **splicing together** the nodes of the first two lists.  
Return the **head** of the merged linked list.

---

## Examples

| Input | Output |
|-------|--------|
| `list1 = [1,2,4], list2 = [1,3,4]` | `[1,1,2,3,4,4]` |
| `list1 = [], list2 = []` | `[]` |
| `list1 = [], list2 = [0]` | `[0]` |

---

## Approach — Dummy Node + Two Pointers

### Core Idea
- Create a **dummy head node** to avoid null-check edge cases.
- Use a `curr` pointer to build the result list.
- At each step, compare `list1->val` and `list2->val` — attach the smaller node.
- When one list is exhausted, directly attach the remaining nodes of the other.

### Walkthrough

```
list1: 1 → 2 → 4
list2: 1 → 3 → 4

dummy → ?
curr = dummy

Step 1: l1(1) == l2(1) → pick l1(1)   curr→1,  l1→2
Step 2: l2(1) <  l1(2) → pick l2(1)   curr→1,  l2→3
Step 3: l1(2) <  l2(3) → pick l1(2)   curr→2,  l1→4
Step 4: l2(3) <  l1(4) → pick l2(3)   curr→3,  l2→4
Step 5: l1(4) == l2(4) → pick l1(4)   curr→4,  l1→null
Step 6: l1 exhausted   → attach l2(4) directly

Result: dummy → 1 → 1 → 2 → 3 → 4 → 4
Return: dummy.next
```

---

## C++ Solution

```cpp
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
        ListNode dummy(0);          // Dummy head — avoids null checks
        ListNode* curr = &dummy;    // Pointer to build result

        while (list1 && list2) {
            if (list1->val <= list2->val) {
                curr->next = list1;   // Pick from list1
                list1 = list1->next;
            } else {
                curr->next = list2;   // Pick from list2
                list2 = list2->next;
            }
            curr = curr->next;        // Advance builder
        }

        // Attach remaining nodes (at most one list has nodes left)
        curr->next = list1 ? list1 : list2;

        return dummy.next;   // Skip dummy, return actual head
    }
};
```

---

## Complexity Analysis

| | Complexity | Reason |
|---|---|---|
| **Time** | O(n + m) | Every node is visited exactly once |
| **Space** | O(1) | Only rewiring `.next` pointers — no new nodes created |

> **Optimal** — must inspect every node at least once. No extra memory since we reuse existing nodes (splicing, not copying).

---

## Key Edge Cases

| Case | Input | Handled By |
|------|-------|------------|
| Both empty | `[], []` | Loop skips, `curr->next = null` |
| One empty | `[], [0]` | Loop skips, `curr->next = list2` |
| One list exhausted early | `[1,2], [1,3,4,5]` | `curr->next = list1 ? list1 : list2` |
| All equal values | `[1,1], [1,1]` | `<=` ensures stable left-priority order |

---

## Why Dummy Node?

Without it, you'd need messy special-case code just to set the head:

```cpp
// Without dummy — ugly
ListNode* head = nullptr;
if (list1->val <= list2->val) { head = list1; list1 = list1->next; }
else { head = list2; list2 = list2->next; }
// ... then continue loop
```

The dummy node gives a **free "pre-head"** so every node including the first is handled uniformly inside the loop. Return `dummy.next` to skip it.

---

## Bonus — Recursive Solution

```cpp
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
        if (!list1) return list2;   // Base case
        if (!list2) return list1;   // Base case

        if (list1->val <= list2->val) {
            list1->next = mergeTwoLists(list1->next, list2);
            return list1;
        } else {
            list2->next = mergeTwoLists(list1, list2->next);
            return list2;
        }
    }
};
```

| | Iterative | Recursive |
|---|---|---|
| Time | O(n + m) | O(n + m) |
| Space | **O(1)** ✅ | O(n + m) call stack |

> Prefer **iterative** in interviews — O(1) space is strictly better.

---

## Mental Model

> Imagine **merging two sorted card piles** — always pick the smaller top card and place it in the result pile. When one pile runs out, dump the rest of the other pile directly into the result.

---

## Tags
`Linked List` `Two Pointers` `Dummy Node` `Easy`


## Leetcode Screenshot
<img width="1919" height="870" alt="Screenshot 2026-06-03 120009" src="https://github.com/user-attachments/assets/f6d7c502-c1e5-47dc-a6cf-d039a3ef4033" />
