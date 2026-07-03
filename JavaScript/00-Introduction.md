# Chapter 0 — Introduction to DSA & Complexity Analysis

## 0.1 What is a Data Structure? What is an Algorithm?

- **Data Structure (DS)**: A way of organizing and storing data in memory so it can be used efficiently. Think of it as *how* you arrange your stuff.
- **Algorithm**: A finite, step-by-step procedure to solve a problem or transform input to output. Think of it as *what steps* you take.

**Real-life analogy**: A kitchen (memory) needs organization (data structure) — spices in a rack, vegetables in a fridge, knives in a block. The recipe you follow (algorithm) works *with* that organization. A brilliant recipe is slow if your kitchen is a mess; a perfectly organized kitchen is useless without a recipe. DSA is the study of pairing the right kitchen layout with the right recipe for the job.

## 0.2 Why does DSA even matter? (The problem it solves)

Early computing problems were solved with whatever ad-hoc code worked. As data grew (millions → billions of records: search engines, social graphs, GPS routes, bank transactions), naive approaches that were "fast enough" for 100 items became unusable for 100 million items. DSA emerged because:

1. **Scale exposes bad choices.** A linear search over 100 items takes microseconds. Over 10 billion web pages, it's unusable — Google needed hash maps, tries, and inverted indexes.
2. **Different access patterns need different structures.** A phone contacts app needs fast lookup by name (hashing/trees), a music "undo" feature needs LIFO order (stack), a print queue needs FIFO order (queue), GPS navigation needs shortest-path graphs.
3. **Resources are finite.** CPU time and RAM are limited and cost money (cloud bills scale with inefficiency). An O(n²) algorithm on 1 million records = 10¹² operations ≈ minutes-to-hours; an O(n log n) version = ~2×10⁷ operations ≈ milliseconds.

This is precisely why DSA is the core interview filter for high-paying SDE roles: it proves you can reason about **scale**, not just "make it work on my laptop with 5 sample rows."

## 0.3 How to read this tutorial

Every chapter follows the same template:
1. **What & Why** — the concept, and the real problem that forced its invention.
2. **Core theory** — mental model, invariants, how it works internally.
3. **Time & Space Complexity** — for every core operation.
4. **Code skeleton** — canonical implementation (Java/C++-style pseudocode, adapt to your language).
5. **Patterns** — the recurring "shapes" of problems on this topic.
6. **Problems** — Basic → Medium → Hard, with the pattern each teaches.

Work sequentially. Do not skip Arrays/Strings/Recursion — everything else builds on them.

## 0.4 Big-O, Big-Ω, Big-Θ — measuring "efficiency" precisely

We don't measure algorithms in seconds (that depends on hardware). We measure how the number of **operations grows** as input size `n` grows. This is **asymptotic analysis**.

- **Big-O (O)** — *upper bound* — "worst case won't be worse than this." This is what we quote 95% of the time.
- **Big-Ω (Omega)** — *lower bound* — best case.
- **Big-Θ (Theta)** — *tight bound* — when best and worst are the same order.

**Real-life example**: You tell a friend "the drive takes at most 2 hours" (upper bound / Big-O), "at least 40 minutes if there's zero traffic" (lower bound / Big-Ω). If traffic is always predictable, "exactly 1 hour" is Big-Θ.

### Rules of thumb for computing Big-O
1. Drop constants: `O(2n) → O(n)`.
2. Drop lower-order terms: `O(n² + n) → O(n²)`.
3. Different inputs get different variables: comparing two arrays of size `n` and `m` → `O(n + m)`, not `O(n)`.
4. Nested loops over the same input → multiply: two nested loops over `n` → `O(n²)`.
5. Sequential blocks → add, then drop lower order: a loop `O(n)` followed by another `O(n²)` → `O(n²)`.

### Common complexity classes (best to worst, growth of operations for n = 10⁶)

| Complexity | Name | Ops @ n=10⁶ | Typical example |
|---|---|---|---|
| O(1) | Constant | 1 | Array index access, hash map get |
| O(log n) | Logarithmic | ~20 | Binary search, balanced BST ops |
| O(n) | Linear | 10⁶ | Single loop, linear scan |
| O(n log n) | Linearithmic | ~2×10⁷ | Merge sort, heap sort, sort-based algorithms |
| O(n²) | Quadratic | 10¹² | Nested loops, bubble/insertion sort |
| O(n³) | Cubic | 10¹⁸ | Naive matrix multiply, 3 nested loops |
| O(2ⁿ) | Exponential | astronomical | Brute-force subsets, naive recursion (fib) |
| O(n!) | Factorial | astronomical | Brute-force permutations, TSP brute force |

**Practical rule for interviews**: if `n ≤ 10⁸`, aim for O(n log n) or better; if `n ≤ 10⁴`, O(n²) may be fine; if `n ≤ 20`, O(2ⁿ) bitmask DP is fine; if `n ≤ 10`, O(n!) brute force may pass.

## 0.5 Space Complexity

Extra memory an algorithm uses *beyond* the input, as a function of `n`.
- **Auxiliary space**: extra space only (excludes input storage) — this is what interviewers usually mean.
- Recursion uses **call-stack space** — O(depth) — often overlooked! A recursive function with depth `n` uses O(n) space even if it allocates no extra variables.

**Real-life example**: Space complexity is like asking "how much *extra* counter space do I need to cook this recipe," not "how big is the fridge holding my ingredients" (that's the input).

## 0.6 Amortized Analysis (preview)

Some operations are usually cheap but occasionally expensive (e.g., dynamic array resize doubles capacity — most `push` calls are O(1), but one in every `n` is O(n) to copy). **Amortized cost** averages this over many operations: dynamic array `push_back` is **O(1) amortized** even though worst-case single call is O(n). This concept recurs in hash tables, dynamic arrays (ArrayList/vector), and union-find with path compression.

## 0.7 The 3 Cases: Best, Average, Worst

- **Best case**: most favorable input (e.g., already-sorted array for insertion sort → O(n)).
- **Worst case**: least favorable input (interviewers care about this most, since it's a guarantee).
- **Average case**: expected performance over random inputs (relevant for quicksort: O(n log n) average, O(n²) worst).

---
**Next**: [Chapter 1 — Arrays](01-Arrays.md)
