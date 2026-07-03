# Chapter 11 — Heaps & Priority Queues

## 11.1 What & Why

A **Heap** is a complete binary tree (Ch. 9) stored in an array, satisfying the **heap property**: in a Max-Heap, every parent ≥ its children (root = maximum); in a Min-Heap, every parent ≤ its children (root = minimum).

**The problem that created it**: You often need repeated access to "the current maximum/minimum" from a changing collection — task schedulers need "highest priority task next," Dijkstra's algorithm needs "closest unvisited node next," event simulators need "next event in time order." Sorting the whole collection every time is O(n log n) per query — wasteful. A **full BST** gives O(log n) find-min but overkill (needs full ordering + more memory for pointers). The heap gives exactly what's needed — O(1) peek at the extreme value, O(log n) insert/remove — using a compact **array** (no pointers needed, since a complete binary tree's children are at predictable indices `2i+1, 2i+2`).

**Real-life example**: A hospital emergency room triage queue — patients aren't served first-come-first-served (queue) but by severity (priority). The "most critical patient" must always be quickly identifiable and extractable as new patients arrive continuously — exactly what a priority queue (heap) is built for.

## 11.2 Core Theory

- **Array representation**: for node at index `i`, children are at `2i+1` (left) and `2i+2` (right); parent is at `(i-1)/2`. No pointers needed — pure index math, thanks to the "complete tree" shape.
- **Heapify (sift-down/sift-up)**: restore heap property after insert (sift-up from the bottom) or removal (sift-down from the root) — O(log n) each.
- **Build-Heap**: converting an arbitrary array into a heap takes O(n), not O(n log n) — a classic surprising result from careful amortized analysis (most nodes are near the bottom, needing little sifting).
- **Priority Queue** is the *abstract concept* ("give me the highest priority item next"); **Heap** is the *typical implementation* of it (others exist: Fibonacci heap, pairing heap — used in advanced graph algorithms for better amortized complexity).

## 11.3 Time & Space Complexity

| Operation | Complexity |
|---|---|
| Peek min/max | O(1) |
| Insert | O(log n) |
| Extract min/max | O(log n) |
| Build heap from array | O(n) |
| Heap Sort | O(n log n) |
| Search arbitrary element | O(n) (heaps are NOT designed for search) |
| Space | O(n) |

## 11.4 Key Patterns

1. **Top-K problems** — maintain a heap of size K (min-heap for "K largest," max-heap for "K smallest") — O(n log k), better than sorting the full array O(n log n) when k << n.
2. **Two Heaps (median maintenance)** — a max-heap for the smaller half + min-heap for the larger half, keeping them balanced — O(log n) insert, O(1) median lookup.
3. **Merge K Sorted Lists/Arrays** — min-heap of size K holding the current front of each list, O(N log K).
4. **Greedy + Heap** — task scheduling, meeting rooms, always process/compare against the current best via a heap.
5. **Heap as Priority Queue in Graph Algorithms** — Dijkstra's shortest path, Prim's MST (both in Ch. 13) rely fundamentally on extracting "closest/cheapest next" in O(log n).

### Code skeleton — using a language's built-in heap (Java PriorityQueue)
```java
PriorityQueue<Integer> minHeap = new PriorityQueue<>();               // min-heap by default
PriorityQueue<Integer> maxHeap = new PriorityQueue<>(Collections.reverseOrder());
// Top-K largest using a min-heap of size k:
PriorityQueue<Integer> pq = new PriorityQueue<>();
for (int x : arr) {
    pq.offer(x);
    if (pq.size() > k) pq.poll();  // remove smallest, keep only k largest
}
```

## 11.5 Problems (Basic → Medium → Hard)

### Basic
1. Implement a Min-Heap and Max-Heap from scratch (array-based, with sift-up/sift-down).
2. Kth Largest Element in a Stream (LeetCode 703).
3. Last Stone Weight (LeetCode 1046).
4. Heap Sort implementation.

### Medium
5. **Kth Largest Element in an Array** (LeetCode 215) — heap O(n log k) or Quickselect O(n) avg.
6. **Top K Frequent Elements** (LeetCode 347) — hashmap + heap.
7. **K Closest Points to Origin** (LeetCode 973).
8. **Merge K Sorted Lists** (LeetCode 23) — O(N log K).
9. **Task Scheduler** (LeetCode 621) — greedy + heap.
10. **Reorganize String** (LeetCode 767) — greedy + max-heap.
11. **Meeting Rooms II** (LeetCode 253) — min-heap of end times.
12. **Ugly Number II** (LeetCode 264) — heap or 3-pointer DP.

### Hard
13. **Find Median from Data Stream** (LeetCode 295) — two-heap technique, O(log n) insert, O(1) median.
14. **Sliding Window Median** (LeetCode 480) — two heaps + lazy deletion.
15. **Trapping Rain Water II** (LeetCode 407) — min-heap + BFS on grid boundary.
16. **IPO / Maximize Capital** (LeetCode 502) — greedy + two heaps.
17. **Smallest Range Covering Elements from K Lists** (LeetCode 632) — min-heap across K lists.
18. **Design Twitter** (LeetCode 355) — heap-based feed merge.

---
**Prev**: [Binary Search Trees](10-BST.md) | **Next**: [Chapter 12 — Tries](12-Trie.md)
