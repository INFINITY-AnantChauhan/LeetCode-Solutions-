# LeetCode #20 — Valid Parentheses

**Difficulty:** Easy  
**Topic:** Stack, String  
**Link:** https://leetcode.com/problems/valid-parentheses/

---

## Problem Statement

Given a string `s` containing only `'('`, `')'`, `'{'`, `'}'`, `'['`, `']'`, determine if the input string is **valid**.

A string is valid if:
1. Open brackets must be closed by the **same type** of brackets.
2. Open brackets must be closed in the **correct order**.
3. Every close bracket has a **corresponding open bracket** of the same type.

---

## Examples

| Input | Output | Reason |
|-------|--------|--------|
| `"()"` | `true` | Simple match |
| `"()[]{}"` | `true` | Sequential matches |
| `"(]"` | `false` | Wrong type |
| `"([])"` | `true` | Nested correctly |
| `"([)]"` | `false` | Incorrect order |

---

## Approach — Stack

### Core Idea
- Push every **opening bracket** onto the stack.
- For every **closing bracket**, check if it matches the top of the stack.
  - If yes → pop the stack.
  - If no (or stack is empty) → return `false`.
- At the end, the stack must be **empty** (no unmatched openers left).

### Walkthrough

```
s = "([)]"

Step 1: '('  → push       stack: ['(']
Step 2: '['  → push       stack: ['(', '[']
Step 3: ')'  → top is '[' ≠ ')'  → return false ✗

---

s = "([])"

Step 1: '('  → push       stack: ['(']
Step 2: '['  → push       stack: ['(', '[']
Step 3: ']'  → top is '[' ✓  → pop   stack: ['(']
Step 4: ')'  → top is '(' ✓  → pop   stack: []
End: stack empty → return true ✓
```

---

## C++ Solution

```cpp
#include <stack>
#include <unordered_map>
using namespace std;

class Solution {
public:
    bool isValid(string s) {
        stack<char> st;

        unordered_map<char, char> match = {
            {')', '('},
            {']', '['},
            {'}', '{'}
        };

        for (char c : s) {
            if (c == '(' || c == '[' || c == '{') {
                st.push(c);                          // Opening → push
            } else {
                if (st.empty() || st.top() != match[c])
                    return false;                    // No match → invalid
                st.pop();                            // Matched → pop
            }
        }

        return st.empty();   // Valid only if nothing left unmatched
    }
};
```

---

## Complexity Analysis

| | Complexity | Reason |
|---|---|---|
| **Time** | O(n) | Single pass through the string |
| **Space** | O(n) | Stack holds at most n/2 brackets (worst case: all openers) |

> **This is optimal.** You must read every character at least once — Ω(n) lower bound. The stack is unavoidable for tracking nested structure.

---

## Key Edge Cases

| Case | Input | Why It Matters |
|------|-------|----------------|
| Empty stack on close | `"]"` | No opener exists — must check `st.empty()` |
| Unmatched openers | `"((("` | Loop passes, but stack isn't empty at end |
| Single char | `"("` | Returns false — stack not empty |
| Interleaved wrong order | `"([)]"` | Top of stack doesn't match closer |

---

## Why `st.empty()` at the End?

A string like `"((("` passes every loop iteration (no closing brackets trigger a check), but leaves unmatched openers on the stack. The final `st.empty()` check catches this.

---

## Mental Model

> Think of the stack as a **"waiting list"** of unmatched openers.  
> Each closer must pair with the **most recent** unmatched opener (LIFO).  
> If at any point there's no match — or the list isn't empty at the end — it's invalid.

---

## Tags
`Stack` `String` `Hash Map` `Easy`


## Leetcode Screenshot 
<img width="1919" height="866" alt="Screenshot 2026-06-02 103253" src="https://github.com/user-attachments/assets/86af223d-841a-4793-9c51-64f3523a03d0" />

