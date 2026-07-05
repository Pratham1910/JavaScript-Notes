# Chapter 3 — Recursion & Backtracking

## 3.1 What & Why

**Recursion** is a function that calls itself on a smaller subproblem until it hits a **base case**. It exists because many problems are naturally **self-similar** — a tree is a node plus two smaller trees; a large factorial is n times a smaller factorial. Trying to express these iteratively can be awkward or require you to manually manage a stack — recursion lets the **call stack** do that bookkeeping for you.

**The problem that created it**: Traversing nested/hierarchical structures (file systems, org charts, HTML DOM, expression trees) iteratively requires an explicit stack — recursion is the natural language for "solve this by solving smaller versions of itself," directly modeling divide-and-conquer thinking (used later in Merge Sort, Quick Sort, Binary Search, Tree/Graph traversal, DP).

**Backtracking** is recursion + **undo**: try a choice, recurse, and if it doesn't lead to a solution, undo the choice (backtrack) and try the next one. It's how you exhaustively but *intelligently* explore all possibilities (pruning bad paths early) — used for Sudoku solvers, N-Queens, generating permutations/combinations, and maze-solving robots.

**Real-life example**: Recursion = Russian nesting dolls (matryoshka) — to open the whole set, you open the outer doll, revealing a smaller doll, and repeat until you hit the smallest (base case). Backtracking = solving a maze by walking a path; whenever you hit a dead end, you retrace your steps to the last junction and try a different corridor, instead of restarting from scratch.

## 3.2 Core Theory

- Every recursive function needs: (1) **base case** (stops recursion), (2) **recursive case** (calls itself on smaller input), (3) progress toward the base case (or infinite recursion → stack overflow).
- **Call stack**: each call adds a "stack frame" holding local variables & return address. Depth `n` recursion → O(n) space.
- **Recursion tree**: visualize calls as a tree to compute time complexity — count total nodes/work.
- **Tail recursion**: recursive call is the last operation — some languages optimize this into a loop (Java/Python do NOT optimize this — beware stack overflow for deep recursion).
- **Backtracking = DFS over a decision tree** with pruning: at each node, try each valid choice, recurse, then **undo** (remove from path / unmark visited) before trying the next choice.

## 3.3 Time & Space Complexity

| Concept | Complexity |
|---|---|
| Factorial / linear recursion | O(n) time, O(n) space (call stack) |
| Fibonacci naive recursion | O(2ⁿ) time, O(n) space — exponential blowup from recomputation (motivates DP/memoization, Ch. 14) |
| Fibonacci with memoization | O(n) time, O(n) space |
| Binary recursion (divide & conquer, e.g. merge sort) | O(n log n) time typically, O(log n) space if balanced |
| Backtracking (permutations) | O(n · n!) time, O(n) space |
| Backtracking (subsets) | O(n · 2ⁿ) time, O(n) space |
| Backtracking (N-Queens) | O(n!) worst case, pruned heavily in practice |

## 3.4 Key Patterns

1. **Divide and Conquer**: split into subproblems, solve independently, combine (Merge Sort, Quick Sort, Binary Search).
2. **Choice–Explore–Unchoose** (the backtracking template):
```java
void backtrack(State state, List<Choice> choices) {
    if (isGoal(state)) { record(state); return; }
    for (Choice c : choices) {
        if (!isValid(c, state)) continue;   // pruning
        state.apply(c);                     // choose
        backtrack(state, nextChoices);       // explore
        state.undo(c);                       // un-choose
    }
}
```
3. **Memoization**: cache results of subproblems keyed by input to avoid recomputation — turns exponential recursion into polynomial (segues directly into Dynamic Programming, Ch. 14).
4. **Recursion on strings/arrays**: process index `i`, recurse on `i+1`, with an "include/exclude" branch — foundational for subset/DP problems.

## 3.5 Problems (Basic → Medium → Hard)

### Basic
1. Factorial, power (x^n) via recursion — O(log n) with fast exponentiation.
2. Fibonacci (naive vs memoized).
3. Sum of digits, reverse a number recursively.
4. Check if a string is a palindrome recursively.
5. Print all subsets of a set (power set) — O(2ⁿ).

### Medium
6. **Permutations** (LeetCode 46) — backtracking, O(n·n!).
7. **Permutations II** (with duplicates) (LeetCode 47) — backtracking + skip-duplicate logic.
8. **Subsets / Subsets II** (LeetCode 78, 90).
9. **Combination Sum / Combination Sum II** (LeetCode 39, 40).
10. **Generate Parentheses** (LeetCode 22) — backtracking with validity pruning.
11. **Letter Combinations of a Phone Number** (LeetCode 17).
12. **Word Search** (LeetCode 79) — backtracking on grid, DFS + undo visited mark.
13. **Palindrome Partitioning** (LeetCode 131).

### Hard
14. **N-Queens** (LeetCode 51) — classic backtracking with column/diagonal pruning, O(n!).
15. **Sudoku Solver** (LeetCode 37) — backtracking with constraint checks.
16. **Word Break II** (backtracking + memo).
17. **Expression Add Operators** (LeetCode 282).
18. **Regular Expression Matching via recursion+memo** (LeetCode 10).
19. **The Knight's Tour** — backtracking on grid.

---
**Prev**: [Strings](02-Strings.md) | **Next**: [Chapter 4 — Searching](04-Searching.md)
