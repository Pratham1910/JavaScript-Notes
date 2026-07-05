# Chapter 20 — The Complete Question Bank (335 Problems, Checkbox-Tracked)

Every problem from every chapter's "Basic → Medium → Hard" list (Chapters 1–17), consolidated into one trackable bank. This is not a new set of problems — it's the same 335 problems from the tutorial, pulled out of their chapters so you can track progress across the *entire* syllabus at a glance instead of flipping between 17 files.

## 20.1 How to use this bank

1. **Don't start here on day one.** Read each chapter's theory first (Ch. 0–17) — this bank is for the *practice* phase per topic, and later for spaced-repetition review, not for discovering what a topic even is.
2. **Check a box only after solving from scratch, unaided, with the correct time/space complexity stated out loud** — not after reading a solution and nodding along. See Chapter 19.3 for the exact attempt-then-escalate process to follow per problem.
3. **Work top-to-bottom within a chapter's Basic tier before touching Medium**, and Medium before Hard — the ordering inside each chapter is deliberate, matching the difficulty progression in that chapter's theory.
4. **Re-open this file at Weeks 3, 6, and 10** of the roadmap (Chapter 19) and re-solve a random 10% of already-checked boxes *without looking at your old code* — this is the spaced-repetition pass Chapter 19.3 calls for, and it's the single highest-leverage use of this bank after first-pass completion.
5. Copy this file into your own tracker (Notion, a spreadsheet, or just keep editing this Markdown checklist directly) — GitHub, GitLab, and most editors render `- [ ]` / `- [x]` as clickable checkboxes.

## 20.2 Totals, by difficulty

| Difficulty | Count | Target from Ch. 19.2 |
|---|---|---|
| Basic | 82 | ~80–100 |
| Medium | 151 | ~150–200 |
| Hard | 102 | ~40–60 (this bank exceeds the suggested Hard target — treat Hard-tier as a deep pool to draw from, not a checklist to fully clear before interviews) |
| **Total** | **335** | |

> **Reading the Hard-tier target correctly.** Chapter 19.2 suggests 40–60 Hard problems as *enough to not be caught off guard*, not "solve all 102 Hard problems in this bank before you're ready." Treat the Hard sections per chapter as a pool: solve enough from each chapter's Hard tier to cover every distinct technique that chapter's Hard list represents (check the technique note after the `—` on each item), rather than grinding every single one linearly.

## 20.3 The full bank

## 1. Arrays

### Basic
- [ ] Find max/min in array — O(n) — linear scan.
- [ ] Reverse an array in place — two pointers.
- [ ] Find the "second largest" element — one pass tracking top-2.
- [ ] Move all zeros to the end (keep relative order) — two pointers.
- [ ] Check if array is sorted.
- [ ] Rotate array by k positions — reversal trick, O(n)/O(1).

### Medium
- [ ] Kadane's Algorithm — Maximum Subarray Sum (LeetCode 53) — O(n).
- [ ] Two Sum (sorted / unsorted) (LeetCode 1, 167) — hashmap O(n) or two-pointer O(n) if sorted.
- [ ] 3Sum (LeetCode 15) — sort + two pointers, O(n²).
- [ ] Product of Array Except Self (LeetCode 238) — prefix/suffix products, O(n)/O(1) extra (excl. output).
- [ ] Container With Most Water (LeetCode 11) — two pointers, O(n).
- [ ] Merge Intervals (LeetCode 56) — sort by start, sweep, O(n log n).
- [ ] Next Permutation (LeetCode 31).
- [ ] Set Matrix Zeroes (LeetCode 73) — O(1) space marker trick.
- [ ] Subarray Sum Equals K (LeetCode 560) — prefix sum + hashmap, O(n).
- [ ] Find Duplicate Number (LeetCode 287) — cyclic sort / Floyd's cycle detection, O(n)/O(1).
- [ ] Rotate Image / 2D Matrix rotate 90° (LeetCode 48).
- [ ] Spiral Matrix traversal (LeetCode 54).

### Hard
- [ ] Trapping Rain Water (LeetCode 42) — two pointers or prefix max arrays, O(n).
- [ ] Median of Two Sorted Arrays (LeetCode 4) — binary search on partition, O(log(min(n,m))).
- [ ] Maximum Product Subarray (LeetCode 152) — track running max & min (handles negatives).
- [ ] First Missing Positive (LeetCode 41) — cyclic sort in-place, O(n)/O(1).
- [ ] Sliding Window Maximum (LeetCode 239) — monotonic deque, O(n).
- [ ] Count of Smaller Numbers After Self (LeetCode 315) — merge sort / BIT.
- [ ] Longest Consecutive Sequence (LeetCode 128) — hashset, O(n).

## 2. Strings

### Basic
- [ ] Reverse a string / reverse words in a sentence.
- [ ] Check palindrome (two pointers).
- [ ] Check anagram of two strings — frequency array, O(n).
- [ ] Count vowels/consonants, char frequency.
- [ ] String to integer (atoi) — edge case handling.
- [ ] Check if a string is a rotation of another (`s2` in `s1+s1`).

### Medium
- [ ] Longest Substring Without Repeating Characters (LeetCode 3) — sliding window + hashset, O(n).
- [ ] Longest Palindromic Substring (LeetCode 5) — expand-around-center O(n²) or Manacher O(n).
- [ ] Group Anagrams (LeetCode 49) — hashmap keyed by sorted string / char-count signature.
- [ ] Valid Parentheses (LeetCode 20) — stack (bridges into Chapter 7).
- [ ] String to Integer / Roman to Integer / Integer to Roman (LeetCode 8, 13, 12).
- [ ] Implement strStr() (LeetCode 28) — KMP.
- [ ] Minimum Window Substring (LeetCode 76) — sliding window + frequency map, O(n).
- [ ] Longest Common Prefix (LeetCode 14).
- [ ] Zigzag Conversion (LeetCode 6).
- [ ] Decode String (LeetCode 394) — stack-based parsing.

### Hard
- [ ] Regular Expression Matching (LeetCode 10) — DP, O(n·m).
- [ ] Wildcard Matching (LeetCode 44) — DP.
- [ ] Edit Distance (LeetCode 72) — DP (bridges into Chapter 14).
- [ ] Text Justification (LeetCode 68).
- [ ] Shortest Palindrome (LeetCode 214) — KMP trick.
- [ ] Longest Valid Parentheses (LeetCode 32) — stack/DP, O(n).
- [ ] Word Break II (LeetCode 140) — DP + backtracking.
- [ ] Distinct Subsequences (LeetCode 115) — DP.

## 3. Recursion & Backtracking

### Basic
- [ ] Factorial, power (x^n) via recursion — O(log n) with fast exponentiation.
- [ ] Fibonacci (naive vs memoized).
- [ ] Sum of digits, reverse a number recursively.
- [ ] Check if a string is a palindrome recursively.
- [ ] Print all subsets of a set (power set) — O(2ⁿ).

### Medium
- [ ] Permutations (LeetCode 46) — backtracking, O(n·n!).
- [ ] Permutations II (with duplicates) (LeetCode 47) — backtracking + skip-duplicate logic.
- [ ] Subsets / Subsets II (LeetCode 78, 90).
- [ ] Combination Sum / Combination Sum II (LeetCode 39, 40).
- [ ] Generate Parentheses (LeetCode 22) — backtracking with validity pruning.
- [ ] Letter Combinations of a Phone Number (LeetCode 17).
- [ ] Word Search (LeetCode 79) — backtracking on grid, DFS + undo visited mark.
- [ ] Palindrome Partitioning (LeetCode 131).

### Hard
- [ ] N-Queens (LeetCode 51) — classic backtracking with column/diagonal pruning, O(n!).
- [ ] Sudoku Solver (LeetCode 37) — backtracking with constraint checks.
- [ ] Word Break II (backtracking + memo).
- [ ] Expression Add Operators (LeetCode 282).
- [ ] Regular Expression Matching via recursion+memo (LeetCode 10).
- [ ] The Knight's Tour — backtracking on grid.

## 4. Searching

### Basic
- [ ] Implement binary search iteratively & recursively.
- [ ] Find first and last occurrence of an element in a sorted array (LeetCode 34).
- [ ] Count occurrences of a number in sorted array.
- [ ] Search in a nearly sorted array.
- [ ] Find square root of a number using binary search (LeetCode 69).

### Medium
- [ ] Search in Rotated Sorted Array (LeetCode 33) — O(log n).
- [ ] Find Minimum in Rotated Sorted Array (LeetCode 153).
- [ ] Search a 2D Matrix (LeetCode 74) — treat as 1D.
- [ ] Find Peak Element (LeetCode 162) — binary search on unimodal shape.
- [ ] Koko Eating Bananas (LeetCode 875) — binary search on answer.
- [ ] Capacity To Ship Packages Within D Days (LeetCode 1011) — binary search on answer.
- [ ] Find K Closest Elements (LeetCode 658).
- [ ] Single Element in a Sorted Array (LeetCode 540) — binary search on parity.

### Hard
- [ ] Median of Two Sorted Arrays (LeetCode 4) — binary search on partition, O(log(min(n,m))).
- [ ] Split Array Largest Sum (LeetCode 410) — binary search on answer.
- [ ] Aggressive Cows / Allocate Minimum Pages (classic binary-search-on-answer problems, GfG).
- [ ] Kth Smallest Element in a Sorted Matrix (LeetCode 378) — binary search on value range.
- [ ] Median of a Data Stream (LeetCode 295) — segues into Heaps (Ch. 11).

## 5. Sorting

### Basic
- [ ] Implement Bubble, Selection, Insertion sort from scratch.
- [ ] Implement Merge Sort and Quick Sort from scratch.
- [ ] Sort an array of 0s, 1s, and 2s (Dutch National Flag) (LeetCode 75) — O(n)/O(1).
- [ ] Find Kth largest/smallest using sorting.

### Medium
- [ ] Merge Sort on Linked List (LeetCode 148) — O(n log n).
- [ ] Sort Colors (LeetCode 75).
- [ ] Kth Largest Element in an Array (LeetCode 215) — Quickselect, average O(n).
- [ ] Merge Intervals (LeetCode 56) — sort by start.
- [ ] Largest Number (LeetCode 179) — custom comparator sort.
- [ ] Meeting Rooms II (LeetCode 253) — sort + min-heap.
- [ ] Relative Sort Array (LeetCode 1122) — counting sort variant.
- [ ] H-Index (LeetCode 274) — sort + scan.

### Hard
- [ ] Count of Smaller Numbers After Self (LeetCode 315) — merge sort with count, O(n log n).
- [ ] Reverse Pairs (LeetCode 493) — modified merge sort.
- [ ] Maximum Gap (LeetCode 164) — bucket sort / radix sort, O(n).
- [ ] Chalkboard Median-based scheduling problems — sort + greedy.
- [ ] External Sort simulation (theory) — merge sort applied to disk-based data larger than RAM.

## 6. Linked List

### Basic
- [ ] Reverse a linked list (LeetCode 206) — iterative & recursive.
- [ ] Find the middle of a linked list (LeetCode 876) — fast/slow pointers.
- [ ] Detect a cycle (LeetCode 141) — Floyd's algorithm.
- [ ] Find the Nth node from the end — two pointers with gap of N.
- [ ] Delete a node given only access to that node (LeetCode 237).
- [ ] Merge two sorted linked lists (LeetCode 21).

### Medium
- [ ] Detect Cycle Start Point (LeetCode 142) — Floyd's + math proof of meeting point.
- [ ] Remove Nth Node From End of List (LeetCode 19).
- [ ] Reorder List (LeetCode 143) — find middle + reverse second half + merge.
- [ ] Add Two Numbers (as linked lists) (LeetCode 2).
- [ ] Copy List with Random Pointer (LeetCode 138) — hashmap or interleaving trick.
- [ ] Rotate List (LeetCode 61).
- [ ] Swap Nodes in Pairs (LeetCode 24).
- [ ] Palindrome Linked List (LeetCode 234) — find middle + reverse + compare.
- [ ] Intersection of Two Linked Lists (LeetCode 160) — two-pointer switch-list trick, O(n+m)/O(1).

### Hard
- [ ] Merge K Sorted Lists (LeetCode 23) — min-heap or divide & conquer merge, O(N log k).
- [ ] Reverse Nodes in k-Group (LeetCode 25).
- [ ] LRU Cache (LeetCode 146) — doubly linked list + hashmap, O(1) get/put (bridges to Ch. 8 Hashing).
- [ ] LFU Cache (LeetCode 460) — advanced doubly-linked-list + hashmap design.
- [ ] Flatten a Multilevel Doubly Linked List (LeetCode 430).

## 7. Stacks & Queues

### Basic
- [ ] Implement a stack and queue from scratch (array-based & linked-list-based).
- [ ] Valid Parentheses (LeetCode 20).
- [ ] Implement Queue using Stacks (LeetCode 232) and Stack using Queues (LeetCode 225).
- [ ] Design a Circular Queue (LeetCode 622).
- [ ] Min Stack — O(1) getMin (LeetCode 155).

### Medium
- [ ] Next Greater Element I & II (LeetCode 496, 503) — monotonic stack.
- [ ] Daily Temperatures (LeetCode 739) — monotonic stack.
- [ ] Evaluate Reverse Polish Notation (LeetCode 150).
- [ ] Decode String (LeetCode 394).
- [ ] Asteroid Collision (LeetCode 735).
- [ ] Implement a Basic Calculator (I, II, III) (LeetCode 224, 227, 772).
- [ ] Sliding Window Maximum (LeetCode 239) — monotonic deque, O(n).
- [ ] Remove K Digits (LeetCode 402) — monotonic stack.

### Hard
- [ ] Largest Rectangle in Histogram (LeetCode 84) — monotonic stack, O(n).
- [ ] Maximal Rectangle (LeetCode 85) — extends histogram trick per row.
- [ ] Trapping Rain Water using Stack (LeetCode 42) — alternative to two-pointer solution.
- [ ] Basic Calculator III (nested parens + operators, full expression parser).
- [ ] LRU Cache (see Ch. 6) — combines list + hashmap; conceptually a queue-like eviction policy.

## 8. Hashing

### Basic
- [ ] Two Sum (LeetCode 1) — O(n).
- [ ] Check for duplicates in an array (LeetCode 217).
- [ ] First non-repeating character in a string (LeetCode 387).
- [ ] Valid Anagram (LeetCode 242).
- [ ] Intersection of Two Arrays (LeetCode 349).
- [ ] Majority Element (LeetCode 169) — hashmap, or Boyer-Moore voting for O(1) space.

### Medium
- [ ] Group Anagrams (LeetCode 49).
- [ ] Subarray Sum Equals K (LeetCode 560) — prefix sum + hashmap, O(n).
- [ ] Longest Consecutive Sequence (LeetCode 128) — hashset, O(n).
- [ ] 4Sum II (LeetCode 454) — split into two pair-sum hashmaps, O(n²).
- [ ] Top K Frequent Elements (LeetCode 347) — hashmap + heap/bucket sort.
- [ ] Copy List with Random Pointer (LeetCode 138) — hashmap for node mapping.
- [ ] Design a HashMap from scratch (LeetCode 706) — implement chaining/open addressing.
- [ ] Isomorphic Strings (LeetCode 205).
- [ ] LRU Cache (LeetCode 146) — hashmap + doubly linked list.

### Hard
- [ ] Substring with Concatenation of All Words (LeetCode 30) — hashmap + sliding window.
- [ ] Longest Substring with At Most K Distinct Characters (LeetCode 340) — hashmap + sliding window.
- [ ] Insert Delete GetRandom O(1) (LeetCode 380) — hashmap + array combo.
- [ ] All O`one Data Structure (LeetCode 432) — hashmap + doubly linked list, advanced design.
- [ ] Design Twitter (LeetCode 355) — hashmap + heap combo, system-design-flavored DSA.

## 9. Trees

### Basic
- [ ] Preorder, Inorder, Postorder traversal (recursive & iterative with explicit stack).
- [ ] Level Order Traversal (LeetCode 102) — BFS.
- [ ] Maximum Depth of Binary Tree (LeetCode 104).
- [ ] Check if two trees are identical (LeetCode 100).
- [ ] Invert/Mirror a Binary Tree (LeetCode 226).
- [ ] Count leaf nodes / count total nodes.

### Medium
- [ ] Balanced Binary Tree check (LeetCode 110).
- [ ] Diameter of Binary Tree (LeetCode 543) — combine subtree heights.
- [ ] Path Sum I & II (LeetCode 112, 113).
- [ ] Lowest Common Ancestor of a Binary Tree (LeetCode 236) — O(n).
- [ ] Zigzag Level Order Traversal (LeetCode 103).
- [ ] Right Side View (LeetCode 199).
- [ ] Construct Binary Tree from Preorder and Inorder Traversal (LeetCode 105).
- [ ] Symmetric Tree (LeetCode 101).
- [ ] Flatten Binary Tree to Linked List (LeetCode 114).
- [ ] Vertical Order Traversal (LeetCode 987).
- [ ] Sum Root to Leaf Numbers (LeetCode 129).

### Hard
- [ ] Binary Tree Maximum Path Sum (LeetCode 124) — global max tracked during recursion.
- [ ] Serialize and Deserialize Binary Tree (LeetCode 297).
- [ ] Distinct Subsequences / advanced DP-on-tree problems.
- [ ] Binary Tree Cameras (LeetCode 968) — greedy + tree DP.
- [ ] House Robber III (LeetCode 337) — tree DP (bridges into Ch. 14).
- [ ] Count Nodes in Complete Binary Tree (LeetCode 222) — O((log n)²) exploiting completeness.

## 10. Binary Search Trees (BST) & Balanced Trees

### Basic
- [ ] Search/Insert/Delete in a BST (LeetCode 700, 701, 450).
- [ ] Validate Binary Search Tree (LeetCode 98) — bounds-based recursion.
- [ ] Find Min/Max in a BST.
- [ ] Convert sorted array to a balanced BST (LeetCode 108).
- [ ] Inorder Successor in a BST (LeetCode 285).

### Medium
- [ ] Kth Smallest Element in a BST (LeetCode 230) — inorder traversal, O(h+k).
- [ ] Lowest Common Ancestor of a BST (LeetCode 235) — O(h), exploit ordering (faster than general tree LCA).
- [ ] Construct BST from Preorder Traversal (LeetCode 1008).
- [ ] Delete Node in a BST (LeetCode 450) — handle all 3 deletion cases.
- [ ] Two Sum IV — Input is a BST (LeetCode 653) — inorder + two pointers, or hashset.
- [ ] Balance a Binary Search Tree (LeetCode 1382).
- [ ] Trim a Binary Search Tree (LeetCode 669).
- [ ] Convert BST to Greater Sum Tree (LeetCode 1038) — reverse inorder.

### Hard
- [ ] Recover Binary Search Tree (LeetCode 99) — inorder traversal spotting swapped nodes, O(1) space with Morris traversal.
- [ ] Count of Range Sum-style BST/BIT hybrid problems.
- [ ] Design a data structure combining BST properties (e.g., order statistics tree) for rank/kth-element queries in O(log n).
- [ ] Merge Two BSTs — convert to sorted arrays and merge, or advanced O(m+n) in-place merge.

## 11. Heaps & Priority Queues

### Basic
- [ ] Implement a Min-Heap and Max-Heap from scratch (array-based, with sift-up/sift-down).
- [ ] Kth Largest Element in a Stream (LeetCode 703).
- [ ] Last Stone Weight (LeetCode 1046).
- [ ] Heap Sort implementation.

### Medium
- [ ] Kth Largest Element in an Array (LeetCode 215) — heap O(n log k) or Quickselect O(n) avg.
- [ ] Top K Frequent Elements (LeetCode 347) — hashmap + heap.
- [ ] K Closest Points to Origin (LeetCode 973).
- [ ] Merge K Sorted Lists (LeetCode 23) — O(N log K).
- [ ] Task Scheduler (LeetCode 621) — greedy + heap.
- [ ] Reorganize String (LeetCode 767) — greedy + max-heap.
- [ ] Meeting Rooms II (LeetCode 253) — min-heap of end times.
- [ ] Ugly Number II (LeetCode 264) — heap or 3-pointer DP.

### Hard
- [ ] Find Median from Data Stream (LeetCode 295) — two-heap technique, O(log n) insert, O(1) median.
- [ ] Sliding Window Median (LeetCode 480) — two heaps + lazy deletion.
- [ ] Trapping Rain Water II (LeetCode 407) — min-heap + BFS on grid boundary.
- [ ] IPO / Maximize Capital (LeetCode 502) — greedy + two heaps.
- [ ] Smallest Range Covering Elements from K Lists (LeetCode 632) — min-heap across K lists.
- [ ] Design Twitter (LeetCode 355) — heap-based feed merge.

## 12. Tries (Prefix Trees)

### Basic
- [ ] Implement Trie (Prefix Tree) (LeetCode 208).
- [ ] Count words with a given prefix.
- [ ] Longest common prefix of a set of strings using a Trie.

### Medium
- [ ] Design Add and Search Words Data Structure (LeetCode 211) — Trie + wildcard DFS.
- [ ] Replace Words (LeetCode 648) — Trie prefix matching for stemming.
- [ ] Map Sum Pairs (LeetCode 677).
- [ ] Implement Magic Dictionary (LeetCode 676).
- [ ] Word Break (LeetCode 139) — can be solved with Trie + DP.

### Hard
- [ ] Word Search II (LeetCode 212) — Trie + grid DFS backtracking, classic hard-tier combo.
- [ ] Maximum XOR of Two Numbers in an Array (LeetCode 421) — Binary Trie over bits, O(n·32).
- [ ] Palindrome Pairs (LeetCode 336) — Trie of reversed words.
- [ ] Stream of Characters (LeetCode 1032) — reverse Trie + streaming matching.
- [ ] Short Encoding of Words (LeetCode 820) — Trie built from suffixes.

## 13. Graphs

### Basic
- [ ] Implement Graph (adjacency list) + BFS + DFS traversal.
- [ ] Number of Connected Components in an Undirected Graph (LeetCode 323).
- [ ] Find if Path Exists in Graph (LeetCode 1971).
- [ ] Flood Fill (LeetCode 733).

### Medium
- [ ] Number of Islands (LeetCode 200) — grid BFS/DFS, O(rows·cols).
- [ ] Rotting Oranges (LeetCode 994) — multi-source BFS.
- [ ] Course Schedule I & II (LeetCode 207, 210) — topological sort / cycle detection in directed graph.
- [ ] Clone Graph (LeetCode 133) — BFS/DFS + hashmap.
- [ ] Pacific Atlantic Water Flow (LeetCode 417) — multi-source DFS from both borders.
- [ ] Graph Valid Tree (LeetCode 261) — Union-Find or DFS cycle check.
- [ ] Word Ladder (LeetCode 127) — BFS on implicit word graph.
- [ ] Network Delay Time (LeetCode 743) — Dijkstra's algorithm.
- [ ] Number of Provinces (LeetCode 547) — Union-Find.
- [ ] Redundant Connection (LeetCode 684) — Union-Find cycle detection.
- [ ] Surrounded Regions (LeetCode 130) — boundary DFS.
- [ ] 01 Matrix (LeetCode 542) — multi-source BFS.

### Hard
- [ ] Alien Dictionary (LeetCode 269) — build graph from char ordering + topological sort.
- [ ] Minimum Spanning Tree (Kruskal's/Prim's) — classic construction, various LeetCode/GfG problems.
- [ ] Cheapest Flights Within K Stops (LeetCode 787) — modified Bellman-Ford / BFS with state (node, stops).
- [ ] Swim in Rising Water (LeetCode 778) — binary search + BFS, or Dijkstra-variant with min-heap on "max edge on path".
- [ ] Bus Routes (LeetCode 815) — BFS on a transformed graph.
- [ ] Reconstruct Itinerary (LeetCode 332) — Eulerian path via Hierholzer's algorithm.
- [ ] Critical Connections in a Network (LeetCode 1192) — Tarjan's bridge-finding algorithm, O(V+E).
- [ ] Strongly Connected Components — Tarjan's or Kosaraju's algorithm, O(V+E).
- [ ] Floyd-Warshall applications: Find the City With the Smallest Number of Neighbors at a Threshold Distance (LeetCode 1334).

## 14. Dynamic Programming (DP)

### Basic
- [ ] Climbing Stairs (LeetCode 70) — O(n)/O(1).
- [ ] House Robber (LeetCode 198) — O(n)/O(1).
- [ ] Fibonacci with memoization vs tabulation.
- [ ] Min Cost Climbing Stairs (LeetCode 746).
- [ ] Coin Change (minimum coins) (LeetCode 322).

### Medium
- [ ] Coin Change II (count ways) (LeetCode 518) — unbounded knapsack.
- [ ] Longest Increasing Subsequence (LeetCode 300) — O(n²) then optimize to O(n log n).
- [ ] Longest Common Subsequence (LeetCode 1143) — O(n·m).
- [ ] Edit Distance (LeetCode 72) — O(n·m).
- [ ] 0/1 Knapsack / Partition Equal Subset Sum (LeetCode 416).
- [ ] Target Sum (LeetCode 494).
- [ ] Unique Paths I & II (LeetCode 62, 63) — grid DP.
- [ ] Minimum Path Sum (LeetCode 64).
- [ ] Word Break (LeetCode 139).
- [ ] Decode Ways (LeetCode 91).
- [ ] House Robber II (LeetCode 213) — circular array variant.
- [ ] Longest Palindromic Subsequence (LeetCode 516).
- [ ] Maximal Square (LeetCode 221) — grid DP.

### Hard
- [ ] Edit Distance variants: Distinct Subsequences (LeetCode 115).
- [ ] Burst Balloons (LeetCode 312) — interval DP, O(n³).
- [ ] Regular Expression Matching / Wildcard Matching (LeetCode 10, 44).
- [ ] Palindrome Partitioning II (LeetCode 132) — min cuts, interval DP.
- [ ] Best Time to Buy and Sell Stock III & IV (LeetCode 123, 188) — state machine DP with transaction limits.
- [ ] Cherry Pickup (LeetCode 741) — 2-agent grid DP.
- [ ] Traveling Salesman Problem — Bitmask DP, O(n²·2ⁿ).
- [ ] Shortest Common Supersequence (LeetCode 1092).
- [ ] Matrix Chain Multiplication (classic GfG) — interval DP.
- [ ] Count of Numbers in Range with digit property — Digit DP.

## 15. Greedy Algorithms

### Basic
- [ ] Activity Selection Problem (classic GfG) — sort by end time.
- [ ] Fractional Knapsack (classic GfG) — sort by value/weight ratio.
- [ ] Assign Cookies (LeetCode 455).
- [ ] Lemonade Change (LeetCode 860).

### Medium
- [ ] Jump Game I & II (LeetCode 55, 45) — greedy reachability tracking, O(n).
- [ ] Gas Station (LeetCode 134) — one-pass greedy with proof via total-sum argument.
- [ ] Candy (LeetCode 135) — two-pass greedy (left-to-right, right-to-left).
- [ ] Non-overlapping Intervals (LeetCode 435) — sort by end, greedy count removal.
- [ ] Partition Labels (LeetCode 763) — greedy interval merging using last-occurrence index.
- [ ] Task Scheduler (LeetCode 621) — greedy + heap (also Ch. 11).
- [ ] Minimum Number of Arrows to Burst Balloons (LeetCode 452) — sort by end, greedy.
- [ ] Queue Reconstruction by Height (LeetCode 406) — sort + greedy insertion.

### Hard
- [ ] Job Sequencing Problem with Deadlines (classic GfG) — sort by profit + greedy slot assignment (often with Union-Find optimization).
- [ ] Huffman Encoding/Decoding (classic GfG) — min-heap-based greedy construction.
- [ ] Minimum Platforms Required (classic GfG, railway scheduling) — sort arrivals/departures, greedy sweep.
- [ ] Candy Distribution variants / IPO (Maximize Capital) (LeetCode 502) — greedy + two heaps.
- [ ] Course Schedule III (LeetCode 630) — greedy + max-heap.

## 16. Bit Manipulation

### Basic
- [ ] Check if a number is a power of two (LeetCode 231).
- [ ] Count set bits / Number of 1 Bits (LeetCode 191).
- [ ] Single Number (LeetCode 136) — XOR trick, O(n)/O(1).
- [ ] Swap two numbers without a temp variable (XOR trick).
- [ ] Check if the ith bit is set.

### Medium
- [ ] Single Number II (LeetCode 137) — bit counting mod 3 trick.
- [ ] Single Number III (LeetCode 260) — XOR + partition by differing bit.
- [ ] Counting Bits (LeetCode 338) — DP + bit trick, O(n).
- [ ] Sum of Two Integers without + or - (LeetCode 371) — bitwise addition (XOR + carry via AND/shift).
- [ ] Reverse Bits (LeetCode 190).
- [ ] Missing Number (LeetCode 268) — XOR trick.
- [ ] Subsets via Bitmask (LeetCode 78) — bitmask enumeration alternative to backtracking.
- [ ] Bitwise AND of Numbers Range (LeetCode 201).

### Hard
- [ ] Maximum XOR of Two Numbers in an Array (LeetCode 421) — Binary Trie (also Ch. 12), O(32n).
- [ ] Maximum XOR With an Element From Array (LeetCode 1707) — offline + Trie.
- [ ] UTF-8 Validation (LeetCode 393) — bit-pattern parsing.
- [ ] Minimum XOR-based Bitmask DP problems (e.g., Partition to K Equal Sum Subsets combined with bitmask).
- [ ] Traveling Salesman via Bitmask DP (see Ch. 14) — bits represent visited-city sets.

## 17. Advanced Data Structures (Segment Tree, Fenwick Tree, DSU, LRU)

### Basic
- [ ] Build a Fenwick Tree; support prefix sum query + point update.
- [ ] Build a Segment Tree; support range sum query + point update.
- [ ] Implement Union-Find with path compression + union by rank.

### Medium
- [ ] Range Sum Query - Mutable (LeetCode 307) — Segment Tree or Fenwick Tree.
- [ ] Count of Smaller Numbers After Self (LeetCode 315) — Fenwick Tree (also solvable via merge sort, Ch. 5).
- [ ] Number of Islands II (LeetCode 305) — Union-Find with dynamic additions.
- [ ] Redundant Connection (LeetCode 684) — Union-Find (also Ch. 13).
- [ ] LRU Cache (LeetCode 146) — HashMap + Doubly Linked List.
- [ ] LFU Cache (LeetCode 460) — HashMap + frequency-bucketed Doubly Linked Lists.

### Hard
- [ ] Range Sum Query 2D - Mutable (LeetCode 308) — 2D Binary Indexed Tree.
- [ ] The Skyline Problem (LeetCode 218) — segment tree / sweep line + heap.
- [ ] Count of Range Sum (LeetCode 327) — merge sort or Fenwick Tree over prefix sums.
- [ ] My Calendar III (LeetCode 732) — Segment Tree with lazy propagation for range increments.
- [ ] Falling Squares (LeetCode 699) — coordinate compression + Segment Tree.
- [ ] Design Excel Sum Formula / advanced DSU with union-by-size for dynamic connectivity queries.

**Total problems: 335**

---
**Prev**: [Study Roadmap](19-Roadmap.md) | **Next**: [Chapter 21 — Daily Routine](21-Daily-Routine.md)
