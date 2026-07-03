# Chapter 7 — Stacks & Queues

## 7.1 What & Why

- **Stack**: LIFO (Last In, First Out). Push/pop only from one end ("top").
- **Queue**: FIFO (First In, First Out). Enqueue at "rear", dequeue from "front".

**The problem that created them**: Programs need to track "what to do next" or "what to undo" in specific orders. Function calls naturally nest (the last function called is the first to return) — this is *why* every program's call stack is literally a stack. Meanwhile, tasks that must be processed in arrival order (print jobs, customer service tickets, CPU task scheduling in round-robin, BFS traversal) need FIFO order — a queue.

**Real-life examples**:
- **Stack** = a stack of plates. You add/remove from the top only. Ctrl+Z (undo) works this way — the most recent action is undone first. Browser "back" button is a stack of visited pages.
- **Queue** = a line at a ticket counter. First person in line is served first. Print queue, customer support ticket queue, CPU job scheduling.

## 7.2 Core Theory

- Both can be implemented via **array** (with resizing) or **linked list**.
- **Circular Queue**: fixed-size array queue that wraps around using modulo arithmetic to reuse freed space — avoids the "shifting" cost of a naive array queue.
- **Deque (Double-Ended Queue)**: insertion/removal from both ends in O(1) — generalizes stack and queue; backbone of the **sliding window maximum** pattern (monotonic deque).
- **Monotonic Stack/Queue**: maintain elements in increasing/decreasing order by popping violators before pushing — key trick for "next greater/smaller element" style problems, achieving O(n) instead of O(n²).
- The **call stack** during recursion (Ch. 3) *is* a stack — any recursive algorithm can be rewritten iteratively using an explicit stack.

## 7.3 Time & Space Complexity

| Operation | Stack (array/LL) | Queue (array/LL) | Deque |
|---|---|---|---|
| Push/Enqueue | O(1) | O(1) | O(1) both ends |
| Pop/Dequeue | O(1) | O(1) | O(1) both ends |
| Peek/Front | O(1) | O(1) | O(1) |
| Search | O(n) | O(n) | O(n) |
| Space | O(n) | O(n) | O(n) |

## 7.4 Key Patterns

1. **Matching/Balancing** — valid parentheses, nested structure validation using a stack.
2. **Monotonic Stack** — next greater/smaller element, largest rectangle in histogram, O(n).
3. **Monotonic Deque** — sliding window maximum/minimum, O(n).
4. **Two Stacks → Queue** and **Two Queues → Stack** — classic implementation exercise.
5. **Stack for Expression Evaluation** — infix→postfix conversion, evaluating postfix/prefix expressions, calculator problems.
6. **BFS uses a Queue**; **DFS uses a Stack** (or recursion) — this distinction is central to Graph traversal (Ch. 13).

### Code skeleton — Monotonic Stack (Next Greater Element)
```java
int[] nextGreater(int[] arr) {
    int n = arr.length;
    int[] res = new int[n];
    Arrays.fill(res, -1);
    Deque<Integer> stack = new ArrayDeque<>(); // stores indices
    for (int i = 0; i < n; i++) {
        while (!stack.isEmpty() && arr[stack.peek()] < arr[i]) {
            res[stack.pop()] = arr[i];
        }
        stack.push(i);
    }
    return res;
}
```

## 7.5 Problems (Basic → Medium → Hard)

### Basic
1. Implement a stack and queue from scratch (array-based & linked-list-based).
2. Valid Parentheses (LeetCode 20).
3. Implement Queue using Stacks (LeetCode 232) and Stack using Queues (LeetCode 225).
4. Design a Circular Queue (LeetCode 622).
5. Min Stack — O(1) getMin (LeetCode 155).

### Medium
6. **Next Greater Element I & II** (LeetCode 496, 503) — monotonic stack.
7. **Daily Temperatures** (LeetCode 739) — monotonic stack.
8. **Evaluate Reverse Polish Notation** (LeetCode 150).
9. **Decode String** (LeetCode 394).
10. **Asteroid Collision** (LeetCode 735).
11. **Implement a Basic Calculator (I, II, III)** (LeetCode 224, 227, 772).
12. **Sliding Window Maximum** (LeetCode 239) — monotonic deque, O(n).
13. **Remove K Digits** (LeetCode 402) — monotonic stack.

### Hard
14. **Largest Rectangle in Histogram** (LeetCode 84) — monotonic stack, O(n).
15. **Maximal Rectangle** (LeetCode 85) — extends histogram trick per row.
16. **Trapping Rain Water using Stack** (LeetCode 42) — alternative to two-pointer solution.
17. **Basic Calculator III** (nested parens + operators, full expression parser).
18. **LRU Cache** (see Ch. 6) — combines list + hashmap; conceptually a queue-like eviction policy.

---
**Prev**: [Linked Lists](06-LinkedList.md) | **Next**: [Chapter 8 — Hashing](08-Hashing.md)
