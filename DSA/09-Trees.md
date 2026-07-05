# Chapter 9 — Trees (Binary Trees)

## 9.1 What & Why

A **Tree** is a hierarchical structure: a root node with child nodes, each child itself the root of a subtree, with no cycles.

**The problem that created it**: Linear structures (arrays, linked lists) can't naturally represent **hierarchical relationships** — a file system (folders containing folders), an org chart (manager → reports), an HTML DOM (elements nested in elements), a decision process (yes/no branches). Trees generalize this: O(log n) search *and* natural hierarchy representation, which linked lists lack and arrays fake awkwardly.

**Real-life example**: A company org chart — the CEO (root) has VPs (children), who have directors (grandchildren), who have managers, etc. Asking "how many people report up through this VP" is a tree traversal. A file explorer's folder tree is the same idea.

## 9.2 Core Theory & Terminology

- **Root, Parent, Child, Leaf (no children), Height (longest path root→leaf), Depth (distance from root)**.
- **Binary Tree**: each node has at most 2 children (left, right) — no ordering constraint (that's BST, Ch. 10).
- **Full Binary Tree**: every node has 0 or 2 children.
- **Complete Binary Tree**: all levels full except possibly the last, filled left to right (basis of array-based Heaps, Ch. 11).
- **Balanced Tree**: height is O(log n) — guarantees efficient operations (vs a degenerate "linked-list-like" tree with height O(n)).
- **Traversals**:
  - **DFS-based**: Preorder (root, left, right), Inorder (left, root, right), Postorder (left, right, root) — typically recursive, O(n) time, O(h) space (h = height, due to call stack).
  - **BFS-based**: Level-order — uses a queue, O(n) time, O(w) space (w = max width).

## 9.3 Time & Space Complexity

| Operation | Balanced Tree | Skewed/Degenerate Tree |
|---|---|---|
| Search | O(log n) | O(n) |
| Insert/Delete | O(log n) | O(n) |
| Traversal (any) | O(n) | O(n) |
| Space (traversal recursion) | O(log n) | O(n) |
| Space (structure) | O(n) | O(n) |

## 9.4 Key Patterns

1. **Recursive Divide & Conquer on Trees** — solve for left subtree, right subtree, combine at root (height, diameter, balanced-check, sum problems).
2. **DFS with path tracking** — root-to-leaf path problems (path sum, all paths).
3. **BFS / Level-order** — level-by-level processing (zigzag traversal, right-side view, level averages).
4. **Two-node relationship problems** — LCA (Lowest Common Ancestor), diameter, distance between nodes.
5. **Serialization/Deserialization** — encode a tree to a string and back — tests full traversal understanding.
6. **Morris Traversal** — O(1) space inorder traversal using temporary thread pointers (advanced).

### Code skeleton — Traversals
```java
void inorder(Node root) {
    if (root == null) return;
    inorder(root.left);
    visit(root);
    inorder(root.right);
}

void levelOrder(Node root) {
    if (root == null) return;
    Queue<Node> q = new LinkedList<>();
    q.add(root);
    while (!q.isEmpty()) {
        int size = q.size();
        for (int i = 0; i < size; i++) {
            Node cur = q.poll();
            visit(cur);
            if (cur.left != null) q.add(cur.left);
            if (cur.right != null) q.add(cur.right);
        }
    }
}
```

### Code skeleton — Height / Diameter pattern
```java
int height(Node root) {
    if (root == null) return 0;
    return 1 + Math.max(height(root.left), height(root.right));
}
```

## 9.5 Problems (Basic → Medium → Hard)

### Basic
1. Preorder, Inorder, Postorder traversal (recursive & iterative with explicit stack).
2. Level Order Traversal (LeetCode 102) — BFS.
3. Maximum Depth of Binary Tree (LeetCode 104).
4. Check if two trees are identical (LeetCode 100).
5. Invert/Mirror a Binary Tree (LeetCode 226).
6. Count leaf nodes / count total nodes.

### Medium
7. **Balanced Binary Tree check** (LeetCode 110).
8. **Diameter of Binary Tree** (LeetCode 543) — combine subtree heights.
9. **Path Sum I & II** (LeetCode 112, 113).
10. **Lowest Common Ancestor of a Binary Tree** (LeetCode 236) — O(n).
11. **Zigzag Level Order Traversal** (LeetCode 103).
12. **Right Side View** (LeetCode 199).
13. **Construct Binary Tree from Preorder and Inorder Traversal** (LeetCode 105).
14. **Symmetric Tree** (LeetCode 101).
15. **Flatten Binary Tree to Linked List** (LeetCode 114).
16. **Vertical Order Traversal** (LeetCode 987).
17. **Sum Root to Leaf Numbers** (LeetCode 129).

### Hard
18. **Binary Tree Maximum Path Sum** (LeetCode 124) — global max tracked during recursion.
19. **Serialize and Deserialize Binary Tree** (LeetCode 297).
20. **Distinct Subsequences / advanced DP-on-tree problems.**
21. **Binary Tree Cameras** (LeetCode 968) — greedy + tree DP.
22. **House Robber III** (LeetCode 337) — tree DP (bridges into Ch. 14).
23. **Count Nodes in Complete Binary Tree** (LeetCode 222) — O((log n)²) exploiting completeness.

---
**Prev**: [Hashing](08-Hashing.md) | **Next**: [Chapter 10 — Binary Search Trees](10-BST.md)
