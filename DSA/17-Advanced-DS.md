# Chapter 17 — Advanced Data Structures (Segment Tree, Fenwick Tree, DSU, LRU)

## 17.1 What & Why

Arrays give O(1) point access but O(n) range-sum/range-min queries; recomputing a sum over a range every query doesn't scale to millions of queries with interleaved updates. **Segment Trees** and **Fenwick Trees (Binary Indexed Trees)** were invented to support **both range queries AND point/range updates in O(log n)**, which neither a plain array nor a prefix-sum array (fast query, but O(n) update) can do simultaneously.

**Real-life example**: A stock-tracking dashboard needs "what's the max price in the last 10 days?" (range query) recomputed continuously as new prices stream in (updates) — recalculating from scratch each time is too slow at scale; a Segment Tree answers both in O(log n).

## 17.2 Segment Tree

- **Structure**: a binary tree where each node represents a range `[l, r]` of the array, storing an aggregate (sum/min/max/gcd) of that range; leaves are individual elements; each internal node = combine(left child, right child).
- **Build**: O(n). **Range Query**: O(log n) — decompose the query range into O(log n) tree nodes. **Point/Range Update**: O(log n) (or O(log n) with lazy propagation for range updates).
- **Lazy Propagation**: defers updates to child nodes until needed, enabling O(log n) **range updates** (e.g., "add 5 to every element in [3,7]") instead of O(n).

| Operation | Complexity |
|---|---|
| Build | O(n) |
| Range Query | O(log n) |
| Point Update | O(log n) |
| Range Update (with lazy propagation) | O(log n) |
| Space | O(n) (typically 4n array size) |

## 17.3 Fenwick Tree (Binary Indexed Tree, BIT)

- A more compact, simpler-to-code alternative to Segment Trees, but **limited to prefix-sum-like (invertible) operations** — sum, XOR — not min/max (which aren't invertible).
- Uses the binary representation of indices (`i & (-i)` to find the next/parent index) to maintain implicit partial sums.
- Simpler, less memory, faster constant factor than Segment Tree — preferred when the operation is a sum/count/XOR.

| Operation | Complexity |
|---|---|
| Build | O(n log n) (or O(n) with a smarter build) |
| Prefix Sum Query | O(log n) |
| Point Update | O(log n) |
| Space | O(n) |

### Code skeleton — Fenwick Tree
```java
int[] bit;
void update(int i, int delta) {
    for (; i < bit.length; i += i & (-i)) bit[i] += delta;
}
int query(int i) {  // prefix sum [1..i]
    int sum = 0;
    for (; i > 0; i -= i & (-i)) sum += bit[i];
    return sum;
}
```

## 17.4 Disjoint Set Union (DSU / Union-Find) — recap & why it matters here

Already introduced in Ch. 13 (Graphs) — grouped here because it's a general-purpose "advanced" structure beyond graphs: used for detecting cycles, dynamic connectivity queries, Kruskal's MST, and offline query processing tricks (e.g., "process queries in reverse to turn deletions into unions").

With **path compression + union by rank/size**, operations run in **O(α(n))** amortized — α is the inverse Ackermann function, which is ≤ 4 for any practically conceivable input — effectively O(1).

## 17.5 LRU/LFU Cache Design (recap link)

Covered in Ch. 6/8 — worth remembering here as the canonical "combine two data structures for O(1) each op" advanced design pattern: HashMap (O(1) lookup) + Doubly Linked List (O(1) reorder/evict).

## 17.6 Sparse Table (bonus, static range queries)

- For **immutable** arrays with range-min/max/gcd queries (no updates), a Sparse Table precomputes answers for all power-of-2-length ranges — O(n log n) build, **O(1) query** (better than Segment Tree's O(log n), but only works when there are no updates).

## 17.7 When to use which (interview heuristic)

| Need | Structure |
|---|---|
| Range sum/min/max query + point update, need it general | Segment Tree |
| Range sum/XOR query + point update, want simpler code | Fenwick Tree |
| Range update + range query | Segment Tree with Lazy Propagation |
| Static array, range min/max query, no updates | Sparse Table (O(1) query) |
| Dynamic connectivity / cycle detection | Union-Find (DSU) |
| O(1) get/put with eviction policy | HashMap + Doubly Linked List (LRU/LFU) |

## 17.8 Problems (Basic → Medium → Hard)

### Basic
1. Build a Fenwick Tree; support prefix sum query + point update.
2. Build a Segment Tree; support range sum query + point update.
3. Implement Union-Find with path compression + union by rank.

### Medium
4. **Range Sum Query - Mutable** (LeetCode 307) — Segment Tree or Fenwick Tree.
5. **Count of Smaller Numbers After Self** (LeetCode 315) — Fenwick Tree (also solvable via merge sort, Ch. 5).
6. **Number of Islands II** (LeetCode 305) — Union-Find with dynamic additions.
7. **Redundant Connection** (LeetCode 684) — Union-Find (also Ch. 13).
8. **LRU Cache** (LeetCode 146) — HashMap + Doubly Linked List.
9. **LFU Cache** (LeetCode 460) — HashMap + frequency-bucketed Doubly Linked Lists.

### Hard
10. **Range Sum Query 2D - Mutable** (LeetCode 308) — 2D Binary Indexed Tree.
11. **The Skyline Problem** (LeetCode 218) — segment tree / sweep line + heap.
12. **Count of Range Sum** (LeetCode 327) — merge sort or Fenwick Tree over prefix sums.
13. **My Calendar III** (LeetCode 732) — Segment Tree with lazy propagation for range increments.
14. **Falling Squares** (LeetCode 699) — coordinate compression + Segment Tree.
15. **Design Excel Sum Formula / advanced DSU with union-by-size for dynamic connectivity queries.**

---
**Prev**: [Bit Manipulation](16-BitManipulation.md) | **Next**: [Chapter 18 — Patterns Cheatsheet](18-Patterns-Cheatsheet.md)
