# Chapter 18 — Master Patterns Cheatsheet

This chapter is a **quick-recall index**: when you see a problem, match its shape to a pattern here, then jump to the relevant chapter for depth.

## 18.1 "If you see X in the problem → think Y"

| Signal in the problem | Likely Pattern | Chapter |
|---|---|---|
| "sorted array" + find target/pair | Binary Search / Two Pointers | 4, 1 |
| "subarray" / "substring" + contiguous + constraint | Sliding Window | 1, 2 |
| "all pairs summing to..." | Two Pointers (sorted) or HashMap | 1, 8 |
| "contiguous subarray sum equals K" | Prefix Sum + HashMap | 1, 8 |
| "all permutations / combinations / subsets" | Backtracking | 3 |
| "next greater/smaller element" | Monotonic Stack | 7 |
| "sliding window maximum/minimum" | Monotonic Deque | 7 |
| "kth largest/smallest" / "top K" | Heap | 11 |
| "median of a stream" | Two Heaps | 11 |
| "merge K sorted lists/arrays" | Min-Heap | 11 |
| "shortest path, unweighted graph" | BFS | 13 |
| "shortest path, weighted, non-negative" | Dijkstra (Heap) | 13, 11 |
| "shortest path, negative weights / detect neg cycle" | Bellman-Ford | 13 |
| "all-pairs shortest path" | Floyd-Warshall | 13 |
| "minimum spanning tree" | Kruskal's (Union-Find) or Prim's | 13, 17 |
| "detect cycle" | DFS (directed) / Union-Find (undirected) | 13, 17 |
| "order tasks with dependencies" | Topological Sort | 13 |
| "connected components / grouping" | Union-Find or BFS/DFS | 13, 17 |
| "grid / matrix traversal, flood fill, islands" | BFS/DFS on grid | 13 |
| "distance to nearest X from multiple sources" | Multi-source BFS | 13 |
| "autocomplete / prefix matching" | Trie | 12 |
| "word search with dictionary" | Trie + Backtracking | 12, 3 |
| "maximum XOR pair" | Binary Trie or Bitmask | 12, 16 |
| "counting subsets / include-exclude each item once" | 0/1 Knapsack DP | 14 |
| "unlimited reuse of items" | Unbounded Knapsack DP | 14 |
| "comparing two strings/sequences" | LCS-family DP | 14 |
| "longest increasing run" | LIS DP (O(n log n) with patience sorting) | 14 |
| "min/max path in grid" | Grid DP | 14 |
| "n ≤ ~20, subset states matter" | Bitmask DP | 14, 16 |
| "optimal parenthesization / merging cost" | Interval DP | 14 |
| "schedule with deadlines/intervals to maximize count" | Greedy (sort by end time) | 15 |
| "compress data / build optimal encoding" | Huffman (Greedy + Heap) | 15, 11 |
| "range sum/min query + updates" | Segment Tree / Fenwick Tree | 17 |
| "O(1) get/put with eviction" | HashMap + Doubly Linked List | 6, 8, 17 |
| "find the one unique/odd element" | XOR | 16 |
| "check power of two / count bits" | Bit tricks | 16 |
| "binary search but not on a sorted array — on 'possible answers'" | Binary Search on Answer | 4 |

## 18.2 Complexity Cheat Table (all structures at a glance)

| Structure | Access | Search | Insert | Delete | Space |
|---|---|---|---|---|---|
| Array (static) | O(1) | O(n) | — | — | O(n) |
| Dynamic Array | O(1) | O(n) | O(1) amortized (end) | O(n) | O(n) |
| Linked List (singly) | O(n) | O(n) | O(1) (given node) | O(1) (given node) | O(n) |
| Stack / Queue | O(n) | O(n) | O(1) | O(1) | O(n) |
| HashMap/HashSet | — | O(1) avg | O(1) avg | O(1) avg | O(n) |
| Binary Search Tree (balanced) | O(log n) | O(log n) | O(log n) | O(log n) | O(n) |
| Heap | O(1) (top only) | O(n) | O(log n) | O(log n) (top) | O(n) |
| Trie | — | O(m) | O(m) | O(m) | O(n·m) |
| Segment Tree | — | O(log n) query | O(log n) | O(log n) | O(n) |
| Fenwick Tree | — | O(log n) query | O(log n) | — | O(n) |
| Union-Find | — | O(α(n)) | O(α(n)) | — | O(n) |

## 18.3 Recursion Recurrence → Big-O Cheat Sheet (Master Theorem, informal)

For `T(n) = a·T(n/b) + O(n^d)`:
- If `d > log_b(a)` → **O(n^d)** (work dominated by the "combine" step — e.g., merge sort's merge).
- If `d = log_b(a)` → **O(n^d · log n)** (e.g., merge sort: a=2, b=2, d=1 → log_2(2)=1=d → O(n log n)).
- If `d < log_b(a)` → **O(n^(log_b a))** (work dominated by the recursive branching — e.g., naive Fibonacci-like exponential recursion).

## 18.4 Interview Problem-Solving Framework (UMPIRE-style)

1. **Understand**: restate the problem, clarify constraints (input size `n` tells you the target complexity — see Ch. 0.4 table), ask about edge cases (empty input, duplicates, negatives).
2. **Match**: use the table above — what pattern does this resemble?
3. **Plan**: state your approach and complexity *before* coding.
4. **Implement**: write clean code, use helper functions, meaningful names.
5. **Review**: trace through an example by hand, check edge cases (empty, single element, all same, extremes).
6. **Evaluate**: state final time/space complexity, mention trade-offs/alternatives if asked.

---
**Prev**: [Advanced Data Structures](17-Advanced-DS.md) | **Next**: [Chapter 19 — Study Roadmap](19-Roadmap.md)
