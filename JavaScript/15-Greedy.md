# Chapter 15 — Greedy Algorithms

## 15.1 What & Why

A **Greedy Algorithm** builds a solution step by step, always choosing the option that looks best *right now*, without reconsidering past choices — and this happens to yield the global optimum for a specific class of problems.

**The problem that created it**: DP correctly explores/caches all relevant subproblem choices, but that's overkill when a problem has a special structure where the **locally optimal choice is always safe** (never needs to be undone). Making change with standard currency denominations, scheduling non-overlapping jobs, or building a minimum spanning tree can be solved greedily, avoiding DP's extra bookkeeping and getting simpler, often faster algorithms. Greedy was formalized by recognizing **matroid structure** and **exchange arguments** in optimization theory — proving that a greedy choice never eliminates the possibility of an optimal solution.

**Real-life example**: Making change for ₹87 using coins of ₹1, ₹2, ₹5, ₹10, ₹20 — you greedily take the largest coin that fits (₹20, ₹20, ₹20, ₹20, ₹5, ₹2), never needing to "undo" a coin choice, and this happens to be optimal for this denomination system (though NOT for all coin systems — this is exactly why greedy needs *proof*, not just intuition).

## 15.2 Core Theory

- **Greedy Choice Property**: a globally optimal solution can be reached by making a locally optimal (greedy) choice at each step.
- **Optimal Substructure** (shared with DP): the optimal solution contains optimal solutions to subproblems.
- **The critical difference from DP**: greedy makes ONE choice per step and never looks back or reconsiders; DP considers multiple choices and combines them, because in DP problems the "best now" choice is NOT always safe.
- **Proving greedy correctness** (interview-relevant): typically via an **exchange argument** — show that any optimal solution can be transformed into the greedy solution without making it worse, or via **induction** on the greedy choice being extendable to a full optimal solution.
- **Warning**: greedy does NOT always work — e.g., 0/1 Knapsack is NOT solvable greedily (must use DP), while Fractional Knapsack IS (take items by best value/weight ratio) — the difference (divisibility) determines whether the greedy choice is truly safe.

## 15.3 Time & Space Complexity

Greedy algorithms are typically fast because they make a single pass (often after sorting):

| Pattern | Typical Time | Typical Space |
|---|---|---|
| Sort + single pass greedy | O(n log n) | O(1) to O(n) |
| Greedy + heap (priority-based decisions) | O(n log n) | O(n) |
| Greedy + Union-Find (Kruskal's MST) | O(E log E) | O(V) |

## 15.4 Key Patterns

1. **Activity Selection / Interval Scheduling** — sort by end time, greedily pick the earliest-ending compatible activity — maximizes count of non-overlapping intervals.
2. **Fractional Knapsack** — sort by value/weight ratio, take greedily until capacity fills.
3. **Huffman Coding** — build an optimal prefix-free encoding using a min-heap, always merging the two least-frequent nodes — classic greedy + heap combo (used in real compression algorithms).
4. **Interval Merging/Point Covering** — sort by start/end, sweep once.
5. **Two-Pointer Greedy** — e.g., gas station problem, candy distribution — one pass with a greedy local rule proven globally correct.
6. **Job Sequencing with Deadlines** — sort by profit, greedily assign to latest available slot before deadline.

## 15.5 Problems (Basic → Medium → Hard)

### Basic
1. Activity Selection Problem (classic GfG) — sort by end time.
2. Fractional Knapsack (classic GfG) — sort by value/weight ratio.
3. Assign Cookies (LeetCode 455).
4. Lemonade Change (LeetCode 860).

### Medium
5. **Jump Game I & II** (LeetCode 55, 45) — greedy reachability tracking, O(n).
6. **Gas Station** (LeetCode 134) — one-pass greedy with proof via total-sum argument.
7. **Candy** (LeetCode 135) — two-pass greedy (left-to-right, right-to-left).
8. **Non-overlapping Intervals** (LeetCode 435) — sort by end, greedy count removal.
9. **Partition Labels** (LeetCode 763) — greedy interval merging using last-occurrence index.
10. **Task Scheduler** (LeetCode 621) — greedy + heap (also Ch. 11).
11. **Minimum Number of Arrows to Burst Balloons** (LeetCode 452) — sort by end, greedy.
12. **Queue Reconstruction by Height** (LeetCode 406) — sort + greedy insertion.

### Hard
13. **Job Sequencing Problem with Deadlines** (classic GfG) — sort by profit + greedy slot assignment (often with Union-Find optimization).
14. **Huffman Encoding/Decoding** (classic GfG) — min-heap-based greedy construction.
15. **Minimum Platforms Required** (classic GfG, railway scheduling) — sort arrivals/departures, greedy sweep.
16. **Candy Distribution variants / IPO (Maximize Capital)** (LeetCode 502) — greedy + two heaps.
17. **Course Schedule III** (LeetCode 630) — greedy + max-heap.

---
**Prev**: [Dynamic Programming](14-DP.md) | **Next**: [Chapter 16 — Bit Manipulation](16-BitManipulation.md)
