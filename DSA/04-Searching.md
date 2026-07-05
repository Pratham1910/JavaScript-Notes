# Chapter 4 — Searching

## 4.1 What & Why

Searching answers: "Is this element present, and where?" The naive approach (**Linear Search**) checks every element — O(n). The moment data is **sorted**, we can exploit ordering to eliminate half the search space each step — **Binary Search**, O(log n).

**The problem that created it**: Think of looking up a word in a physical dictionary. You don't start at page 1 and scan every word (linear search) — you open to the middle, see if your word is alphabetically before or after, and repeat, discarding half the book each time. This intuition, formalized, is binary search — one of the most impactful ideas in CS because O(log n) scales incredibly well (searching 1 billion sorted items takes ~30 comparisons).

**Real-life example**: Guessing a number between 1–100 in a "higher/lower" game — the optimal strategy is always guess the midpoint (binary search), solving it in at most 7 guesses instead of up to 100.

## 4.2 Core Theory

- Binary search requires **sorted (or monotonic) data** — it works on any search space with a monotonic predicate ("is this true or false"), not just literal sorted arrays. This generalization — **"binary search on answer"** — is a massively important interview pattern: binary search over a *range of possible answers*, checking feasibility with a helper function.
- Invariant to maintain: search space `[low, high]` always contains the answer (if it exists).
- Common bug: infinite loops from wrong midpoint update or off-by-one bounds — always verify loop shrinks the search space every iteration.

## 4.3 Time & Space Complexity

| Algorithm | Time (worst) | Time (best) | Space |
|---|---|---|---|
| Linear Search | O(n) | O(1) | O(1) |
| Binary Search (iterative) | O(log n) | O(1) | O(1) |
| Binary Search (recursive) | O(log n) | O(1) | O(log n) (call stack) |
| Ternary Search (unimodal functions) | O(log n) | — | O(1) |
| Exponential Search (unbounded/infinite array) | O(log n) | — | O(1) |

## 4.4 Key Patterns

1. **Standard Binary Search** — find exact target in sorted array.
2. **Lower Bound / Upper Bound (first/last occurrence)** — find boundary of a condition.
3. **Binary Search on Answer** — search space is "possible answers" (e.g., minimum capacity, minimum days), predicate is monotonic (feasible/infeasible), binary search the threshold.
4. **Binary Search on Rotated Sorted Array** — determine which half is properly sorted, then decide which half to recurse into.
5. **Binary Search in 2D matrix** — treat as flattened 1D, or eliminate row/col like in a staircase search.

### Code skeleton — standard binary search
```java
int binarySearch(int[] arr, int target) {
    int lo = 0, hi = arr.length - 1;
    while (lo <= hi) {
        int mid = lo + (hi - lo) / 2;   // avoids overflow
        if (arr[mid] == target) return mid;
        else if (arr[mid] < target) lo = mid + 1;
        else hi = mid - 1;
    }
    return -1;
}
```

### Code skeleton — binary search on answer
```java
int minFeasible(int lo, int hi, Predicate<Integer> feasible) {
    while (lo < hi) {
        int mid = lo + (hi - lo) / 2;
        if (feasible.test(mid)) hi = mid;      // mid works, try smaller
        else lo = mid + 1;                      // mid fails, need bigger
    }
    return lo;
}
```

## 4.5 Problems (Basic → Medium → Hard)

### Basic
1. Implement binary search iteratively & recursively.
2. Find first and last occurrence of an element in a sorted array (LeetCode 34).
3. Count occurrences of a number in sorted array.
4. Search in a nearly sorted array.
5. Find square root of a number using binary search (LeetCode 69).

### Medium
6. **Search in Rotated Sorted Array** (LeetCode 33) — O(log n).
7. **Find Minimum in Rotated Sorted Array** (LeetCode 153).
8. **Search a 2D Matrix** (LeetCode 74) — treat as 1D.
9. **Find Peak Element** (LeetCode 162) — binary search on unimodal shape.
10. **Koko Eating Bananas** (LeetCode 875) — binary search on answer.
11. **Capacity To Ship Packages Within D Days** (LeetCode 1011) — binary search on answer.
12. **Find K Closest Elements** (LeetCode 658).
13. **Single Element in a Sorted Array** (LeetCode 540) — binary search on parity.

### Hard
14. **Median of Two Sorted Arrays** (LeetCode 4) — binary search on partition, O(log(min(n,m))).
15. **Split Array Largest Sum** (LeetCode 410) — binary search on answer.
16. **Aggressive Cows / Allocate Minimum Pages** (classic binary-search-on-answer problems, GfG).
17. **Kth Smallest Element in a Sorted Matrix** (LeetCode 378) — binary search on value range.
18. **Median of a Data Stream** (LeetCode 295) — segues into Heaps (Ch. 11).

---
**Prev**: [Recursion & Backtracking](03-Recursion-Backtracking.md) | **Next**: [Chapter 5 — Sorting](05-Sorting.md)
