# Chapter 5 — Sorting

## 5.1 What & Why

Sorting arranges elements in a defined order. It's foundational because **many algorithms become dramatically simpler/faster once data is sorted**: binary search needs sorted data, duplicate detection becomes a single pass, merge-based algorithms rely on sorted merges, scheduling/greedy algorithms sort by deadline/ratio first.

**The problem that created it**: Before computers, sorting punch cards or ledgers by hand was tedious and error-prone (early "sorting" = filing cabinets, library card catalogs alphabetized for O(log n)-ish lookup by a human). Computer science needed provably efficient, comparison-based general-purpose sorting — leading to the mathematical result that **comparison sorts cannot beat O(n log n)** in the worst case (proven via decision-tree argument: n! possible orderings need log₂(n!) ≈ n log n comparisons to distinguish).

**Real-life example**: Sorting exam scripts by roll number before distributing them — once sorted, a teacher can find any student's paper via binary search instead of shuffling through a random pile.

## 5.2 The Sorting Algorithms

| Algorithm | Best | Average | Worst | Space | Stable? | Notes |
|---|---|---|---|---|---|---|
| Bubble Sort | O(n) | O(n²) | O(n²) | O(1) | Yes | Repeatedly swaps adjacent out-of-order pairs |
| Selection Sort | O(n²) | O(n²) | O(n²) | O(1) | No | Repeatedly selects min and places it |
| Insertion Sort | O(n) | O(n²) | O(n²) | O(1) | Yes | Builds sorted prefix one element at a time; great for nearly-sorted/small data |
| Merge Sort | O(n log n) | O(n log n) | O(n log n) | O(n) | Yes | Divide & conquer; guarantees O(n log n) always |
| Quick Sort | O(n log n) | O(n log n) | O(n²) | O(log n) | No | Divide & conquer via pivot partition; fastest in practice (cache-friendly, in-place) |
| Heap Sort | O(n log n) | O(n log n) | O(n log n) | O(1) | No | Build max-heap, repeatedly extract max |
| Counting Sort | O(n+k) | O(n+k) | O(n+k) | O(k) | Yes | Non-comparison; needs small integer range k |
| Radix Sort | O(d·(n+k)) | O(d·(n+k)) | O(d·(n+k)) | O(n+k) | Yes | Non-comparison; sorts digit by digit |
| Bucket Sort | O(n+k) | O(n+k) | O(n²) | O(n) | Yes | Distributes into buckets, sorts each |

**Stability** matters when sorting objects by one key but wanting to preserve relative order of equal keys (e.g., sort employees by department, then within a stable sort, original name order within a department is preserved).

## 5.3 Core Theory — How the Big Ones Work

- **Merge Sort**: split array into halves recursively until size 1, then merge two sorted halves in O(n) using a temp array. Recurrence: `T(n) = 2T(n/2) + O(n)` → O(n log n) by the Master Theorem. Always O(n log n) regardless of input — great when worst-case guarantees matter (external sorting, linked lists).
- **Quick Sort**: pick a **pivot**, partition array so smaller elements go left, larger go right, recurse on both sides. Average O(n log n); worst case O(n²) happens when pivot choice is consistently bad (e.g., always picking first element on an already-sorted array) — mitigated with **random pivot** or **median-of-three**.
- **Heap Sort**: build a max-heap (Ch. 11) in O(n), then repeatedly swap root (max) to the end and sift down — O(n log n), O(1) extra space (in-place), but not stable and worse cache locality than quicksort.
- **Counting/Radix Sort**: exploit known, bounded value ranges to sort **without comparisons**, breaking the O(n log n) comparison-sort lower bound — used when sorting small integers, digit-limited numbers, or fixed-length strings (e.g., sorting phone numbers, sorting scores 0-100).

## 5.4 When to use what (interview heuristic)

- Need guaranteed O(n log n) & stability & don't mind extra memory → **Merge Sort** (also the go-to for sorting linked lists, and for external sorting of data too big for RAM).
- Need fastest average speed, in-place, don't care about worst case → **Quick Sort** (with randomized pivot).
- Need O(1) extra space and O(n log n) worst case guarantee → **Heap Sort**.
- Data is integers in a small known range → **Counting Sort**.
- Small or nearly-sorted array (e.g., n < 20, or insertion into an already sorted array) → **Insertion Sort**.

## 5.5 Problems (Basic → Medium → Hard)

### Basic
1. Implement Bubble, Selection, Insertion sort from scratch.
2. Implement Merge Sort and Quick Sort from scratch.
3. Sort an array of 0s, 1s, and 2s (Dutch National Flag) (LeetCode 75) — O(n)/O(1).
4. Find Kth largest/smallest using sorting.

### Medium
5. **Merge Sort on Linked List** (LeetCode 148) — O(n log n).
6. **Sort Colors** (LeetCode 75).
7. **Kth Largest Element in an Array** (LeetCode 215) — Quickselect, average O(n).
8. **Merge Intervals** (LeetCode 56) — sort by start.
9. **Largest Number** (LeetCode 179) — custom comparator sort.
10. **Meeting Rooms II** (LeetCode 253) — sort + min-heap.
11. **Relative Sort Array** (LeetCode 1122) — counting sort variant.
12. **H-Index** (LeetCode 274) — sort + scan.

### Hard
13. **Count of Smaller Numbers After Self** (LeetCode 315) — merge sort with count, O(n log n).
14. **Reverse Pairs** (LeetCode 493) — modified merge sort.
15. **Maximum Gap** (LeetCode 164) — bucket sort / radix sort, O(n).
16. **Chalkboard Median-based scheduling problems** — sort + greedy.
17. **External Sort simulation** (theory) — merge sort applied to disk-based data larger than RAM.

---
**Prev**: [Searching](04-Searching.md) | **Next**: [Chapter 6 — Linked List](06-LinkedList.md)
