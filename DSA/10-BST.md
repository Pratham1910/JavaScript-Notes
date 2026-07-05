# Chapter 10 — Binary Search Trees (BST) & Balanced Trees

## 10.1 What & Why

A **BST** is a binary tree with an ordering invariant: for every node, all values in the left subtree are smaller, all values in the right subtree are larger. This makes it a **searchable tree** — combining a tree's hierarchical/dynamic nature with an array's fast search.

**The problem that created it**: A sorted array gives O(log n) search but O(n) insert/delete (shifting). A linked list gives O(1) insert/delete but O(n) search. BST was invented to get **O(log n) search, insert, AND delete simultaneously** — as long as the tree stays balanced. This matters for databases and file systems needing fast dynamic sorted-order data (range queries, "next largest," ordered iteration) that changes frequently.

**Real-life example**: A phone contact app where you binary-search by name — but unlike a static sorted array, you can add/remove contacts without shifting everything: you just re-link a few pointers, same as inserting into any tree, while the structure stays browsable in sorted order.

## 10.2 Core Theory

- **BST Invariant**: `left.val < node.val < right.val` (assuming no duplicates), recursively for every subtree.
- **Search/Insert/Delete**: navigate left/right based on comparison — O(height).
- **The Balance Problem**: if you insert already-sorted data into a plain BST, it degenerates into a linked list — O(n) height, destroying the O(log n) guarantee. This motivated **self-balancing trees**:
  - **AVL Tree**: strictly balanced (height difference between subtrees ≤ 1), rebalances via rotations on every insert/delete — O(log n) guaranteed, more rotations than Red-Black but tighter balance (faster lookups).
  - **Red-Black Tree**: looser balance invariant (color rules), fewer rotations on insert/delete than AVL — used in Java's `TreeMap`/`TreeSet`, C++'s `std::map`, Linux kernel schedulers.
  - **B-Trees / B+ Trees**: generalize BST to multiple children per node, minimizing disk reads — the standard structure behind **database indexes** (real motivating example: reading from disk is slow, so you want a *shallow, wide* tree, not a *deep, narrow* one).
- **Inorder traversal of a BST yields sorted order** — a defining, extremely useful property.

## 10.3 Time & Space Complexity

| Operation | Balanced BST (AVL/Red-Black) | Unbalanced/Skewed BST |
|---|---|---|
| Search | O(log n) | O(n) |
| Insert | O(log n) | O(n) |
| Delete | O(log n) | O(n) |
| Find Min/Max | O(log n) | O(n) |
| Inorder traversal (sorted output) | O(n) | O(n) |
| Space | O(n) | O(n) |

## 10.4 Key Patterns

1. **Recursive Search/Insert/Delete** using the ordering invariant to discard half the tree each step (like binary search, Ch. 4).
2. **Inorder traversal = sorted sequence** — use this to validate a BST, find kth smallest, or convert BST↔sorted array.
3. **BST deletion cases**: leaf (just remove), one child (replace with child), two children (replace with inorder successor/predecessor, then delete that).
4. **Range queries** — exploit the invariant to prune subtrees entirely outside `[low, high]`.
5. **Successor/Predecessor** — inorder successor is leftmost node of right subtree (or nearest ancestor where node is in left subtree).

### Code skeleton — BST Search/Insert
```java
Node search(Node root, int val) {
    if (root == null || root.val == val) return root;
    return val < root.val ? search(root.left, val) : search(root.right, val);
}

Node insert(Node root, int val) {
    if (root == null) return new Node(val);
    if (val < root.val) root.left = insert(root.left, val);
    else if (val > root.val) root.right = insert(root.right, val);
    return root;
}
```

## 10.5 Problems (Basic → Medium → Hard)

### Basic
1. Search/Insert/Delete in a BST (LeetCode 700, 701, 450).
2. Validate Binary Search Tree (LeetCode 98) — bounds-based recursion.
3. Find Min/Max in a BST.
4. Convert sorted array to a balanced BST (LeetCode 108).
5. Inorder Successor in a BST (LeetCode 285).

### Medium
6. **Kth Smallest Element in a BST** (LeetCode 230) — inorder traversal, O(h+k).
7. **Lowest Common Ancestor of a BST** (LeetCode 235) — O(h), exploit ordering (faster than general tree LCA).
8. **Construct BST from Preorder Traversal** (LeetCode 1008).
9. **Delete Node in a BST** (LeetCode 450) — handle all 3 deletion cases.
10. **Two Sum IV — Input is a BST** (LeetCode 653) — inorder + two pointers, or hashset.
11. **Balance a Binary Search Tree** (LeetCode 1382).
12. **Trim a Binary Search Tree** (LeetCode 669).
13. **Convert BST to Greater Sum Tree** (LeetCode 1038) — reverse inorder.

### Hard
14. **Recover Binary Search Tree** (LeetCode 99) — inorder traversal spotting swapped nodes, O(1) space with Morris traversal.
15. **Count of Range Sum-style BST/BIT hybrid problems.**
16. **Design a data structure combining BST properties (e.g., order statistics tree) for rank/kth-element queries in O(log n).**
17. **Merge Two BSTs** — convert to sorted arrays and merge, or advanced O(m+n) in-place merge.

---
**Prev**: [Trees](09-Trees.md) | **Next**: [Chapter 11 — Heaps & Priority Queues](11-Heap.md)
