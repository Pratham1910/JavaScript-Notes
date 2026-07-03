# Chapter 6 — Linked Lists

## 6.1 What & Why

A **Linked List** stores elements as nodes, each holding data + a pointer/reference to the next node. Unlike arrays, nodes are **not contiguous** in memory.

**The problem that created it**: Arrays are great for random access but terrible for frequent insertion/deletion in the middle — shifting elements costs O(n). Early systems (like early Lisp, and OS-level free memory lists) needed a structure where **inserting/deleting a known node is O(1)** — you just re-point a few pointers instead of shifting a whole block. This is why OS memory allocators, browser history (doubly linked list), music playlists (next/prev), and undo-chains use linked lists.

**Real-life example**: A treasure hunt where each clue tells you where to find the next clue — you can't jump straight to clue #7 (no random access, must walk from the start), but if you want to insert a new clue between #3 and #4, you just change what clue #3 points to — no need to renumber/shift anything, unlike inserting a new page into a bound book (array).

## 6.2 Core Theory

- **Singly Linked List**: each node → next node only. Traversal is one-directional.
- **Doubly Linked List**: each node has `prev` and `next` — allows O(1) backward traversal and easier deletion (no need to track previous node separately). Costs extra memory per node.
- **Circular Linked List**: tail points back to head — useful for round-robin scheduling, circular buffers.
- **Dummy/Sentinel Node**: a fake head node used to simplify edge cases (empty list, deleting head) in code — extremely common trick.
- **Fast & Slow Pointers (Floyd's Tortoise and Hare)**: two pointers moving at different speeds to detect cycles, find the middle, or find the Nth-from-end node — O(n) time, O(1) space, avoiding a second pass or extra storage.

## 6.3 Time & Space Complexity

| Operation | Array | Singly Linked List | Doubly Linked List |
|---|---|---|---|
| Access by index | O(1) | O(n) | O(n) |
| Search | O(n) | O(n) | O(n) |
| Insert at head | O(n) | O(1) | O(1) |
| Insert at tail (with tail pointer) | O(1) amortized | O(1) | O(1) |
| Insert/Delete in middle (given node ref) | O(n) | O(1)* | O(1) |
| Delete at head | O(n) | O(1) | O(1) |
| Space per element | O(1) | O(1) + pointer | O(1) + 2 pointers |

*O(1) only if you already have a reference to the node before it (singly) — otherwise O(n) to find it.

## 6.4 Key Patterns

1. **Dummy Head Node** — simplifies insert/delete-at-head edge cases.
2. **Fast & Slow Pointers** — cycle detection (Floyd's), find middle node, find Nth node from end.
3. **Reverse a Linked List (iterative in-place)** — three pointers: `prev, curr, next`.
4. **In-place merge of two sorted lists** — pointer manipulation, no extra space.
5. **Recursive Linked List processing** — many list problems have elegant recursive solutions (reverse, merge, palindrome check) but cost O(n) stack space vs O(1) iterative.

### Code skeleton — Reverse a Linked List
```java
ListNode reverse(ListNode head) {
    ListNode prev = null, curr = head;
    while (curr != null) {
        ListNode next = curr.next;
        curr.next = prev;
        prev = curr;
        curr = next;
    }
    return prev;
}
```

### Code skeleton — Floyd's Cycle Detection
```java
boolean hasCycle(ListNode head) {
    ListNode slow = head, fast = head;
    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
        if (slow == fast) return true;
    }
    return false;
}
```

## 6.5 Problems (Basic → Medium → Hard)

### Basic
1. Reverse a linked list (LeetCode 206) — iterative & recursive.
2. Find the middle of a linked list (LeetCode 876) — fast/slow pointers.
3. Detect a cycle (LeetCode 141) — Floyd's algorithm.
4. Find the Nth node from the end — two pointers with gap of N.
5. Delete a node given only access to that node (LeetCode 237).
6. Merge two sorted linked lists (LeetCode 21).

### Medium
7. **Detect Cycle Start Point** (LeetCode 142) — Floyd's + math proof of meeting point.
8. **Remove Nth Node From End of List** (LeetCode 19).
9. **Reorder List** (LeetCode 143) — find middle + reverse second half + merge.
10. **Add Two Numbers (as linked lists)** (LeetCode 2).
11. **Copy List with Random Pointer** (LeetCode 138) — hashmap or interleaving trick.
12. **Rotate List** (LeetCode 61).
13. **Swap Nodes in Pairs** (LeetCode 24).
14. **Palindrome Linked List** (LeetCode 234) — find middle + reverse + compare.
15. **Intersection of Two Linked Lists** (LeetCode 160) — two-pointer switch-list trick, O(n+m)/O(1).

### Hard
16. **Merge K Sorted Lists** (LeetCode 23) — min-heap or divide & conquer merge, O(N log k).
17. **Reverse Nodes in k-Group** (LeetCode 25).
18. **LRU Cache** (LeetCode 146) — doubly linked list + hashmap, O(1) get/put (bridges to Ch. 8 Hashing).
19. **LFU Cache** (LeetCode 460) — advanced doubly-linked-list + hashmap design.
20. **Flatten a Multilevel Doubly Linked List** (LeetCode 430).

---
**Prev**: [Sorting](05-Sorting.md) | **Next**: [Chapter 7 — Stacks & Queues](07-Stack-Queue.md)
