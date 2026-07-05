# Chapter 1 — Arrays

## 1.1 What & Why

An **array** is a contiguous block of memory holding elements of the same type, accessed by an index.

**The problem that created it**: Before arrays, if you stored `n` separate variables (`x1, x2, x3...`), you had no way to say "give me the 500th one" without writing 500 `if` statements. Arrays solve **random access** — because memory addresses are contiguous, `address(i) = base_address + i * element_size`, giving O(1) access to *any* element by index — no traversal needed.

**Real-life example**: A row of numbered lockers in a gym. If you know your locker number (index), you walk straight to it — you don't check every locker on the way. Compare this to a treasure hunt with clues chained one to the next (linked list) — there you *must* visit each clue in order.

## 1.2 Core Theory

- Fixed-size (static arrays, e.g., C arrays, Java arrays) vs **dynamic arrays** (Python list, Java ArrayList, C++ vector, JS Array) which resize automatically by allocating a new, larger block (commonly 2x) and copying over — this is where **amortized O(1)** append comes from.
- Elements are stored in **row-major order** for multi-dimensional arrays (2D grid: `row * numCols + col`).
- Memory locality (cache-friendliness) makes arrays fast in practice even when a "better" Big-O structure (like a linked list) exists — this is why arrays are the default choice unless you need frequent insert/delete in the middle.

## 1.3 Time & Space Complexity

| Operation | Static Array | Dynamic Array (amortized) |
|---|---|---|
| Access by index | O(1) | O(1) |
| Search (unsorted) | O(n) | O(n) |
| Search (sorted, binary search) | O(log n) | O(log n) |
| Insert/Delete at end | N/A (fixed) | O(1) amortized |
| Insert/Delete at beginning/middle | O(n) (shift) | O(n) (shift) |
| Space | O(n) | O(n) (up to 2x overhead) |

## 1.4 Key Techniques / Patterns on Arrays

1. **Two Pointers** — one pointer from start, one from end (or both moving forward at different speeds). Used for pair-sum in sorted array, reversing, partitioning.
2. **Sliding Window** — maintain a window `[left, right]` and expand/shrink it to satisfy a constraint; avoids recomputation. Used for subarray sum/length problems.
3. **Prefix Sum** — precompute `prefix[i] = arr[0]+...+arr[i-1]` so range-sum queries become O(1).
4. **Kadane's Algorithm** — running max for maximum subarray sum, O(n).
5. **Dutch National Flag / 3-way partition** — sort array of 0s,1s,2s in one pass.
6. **Cyclic Sort** — when array has values in range `[1..n]`, place each value at its index in O(n) to find missing/duplicate numbers.
7. **In-place rotation** — reverse-based trick to rotate array by k in O(n) time O(1) space.

### Code skeleton — Sliding Window (variable size)
```java
int left = 0, sum = 0, best = 0;
for (int right = 0; right < n; right++) {
    sum += arr[right];
    while (sum > target) {           // shrink while constraint violated
        sum -= arr[left++];
    }
    best = Math.max(best, right - left + 1);
}
```

### Code skeleton — Prefix Sum
```java
int[] prefix = new int[n + 1];
for (int i = 0; i < n; i++) prefix[i + 1] = prefix[i] + arr[i];
// sum of arr[l..r] inclusive = prefix[r+1] - prefix[l]
```

## 1.5 Problems (Basic → Medium → Hard)

### Basic
1. Find max/min in array — O(n) — linear scan.
2. Reverse an array in place — two pointers.
3. Find the "second largest" element — one pass tracking top-2.
4. Move all zeros to the end (keep relative order) — two pointers.
5. Check if array is sorted.
6. Rotate array by k positions — reversal trick, O(n)/O(1).

### Medium
7. **Kadane's Algorithm** — Maximum Subarray Sum (LeetCode 53) — O(n).
8. **Two Sum** (sorted / unsorted) (LeetCode 1, 167) — hashmap O(n) or two-pointer O(n) if sorted.
9. **3Sum** (LeetCode 15) — sort + two pointers, O(n²).
10. **Product of Array Except Self** (LeetCode 238) — prefix/suffix products, O(n)/O(1) extra (excl. output).
11. **Container With Most Water** (LeetCode 11) — two pointers, O(n).
12. **Merge Intervals** (LeetCode 56) — sort by start, sweep, O(n log n).
13. **Next Permutation** (LeetCode 31).
14. **Set Matrix Zeroes** (LeetCode 73) — O(1) space marker trick.
15. **Subarray Sum Equals K** (LeetCode 560) — prefix sum + hashmap, O(n).
16. **Find Duplicate Number** (LeetCode 287) — cyclic sort / Floyd's cycle detection, O(n)/O(1).
17. **Rotate Image / 2D Matrix rotate 90°** (LeetCode 48).
18. **Spiral Matrix traversal** (LeetCode 54).

### Hard
19. **Trapping Rain Water** (LeetCode 42) — two pointers or prefix max arrays, O(n).
20. **Median of Two Sorted Arrays** (LeetCode 4) — binary search on partition, O(log(min(n,m))).
21. **Maximum Product Subarray** (LeetCode 152) — track running max & min (handles negatives).
22. **First Missing Positive** (LeetCode 41) — cyclic sort in-place, O(n)/O(1).
23. **Sliding Window Maximum** (LeetCode 239) — monotonic deque, O(n).
24. **Count of Smaller Numbers After Self** (LeetCode 315) — merge sort / BIT.
25. **Longest Consecutive Sequence** (LeetCode 128) — hashset, O(n).

---
**Prev**: [Introduction](00-Introduction.md) | **Next**: [Chapter 2 — Strings](02-Strings.md)
