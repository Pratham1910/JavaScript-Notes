# Chapter 12 — Tries (Prefix Trees)

## 12.1 What & Why

A **Trie** (from re**trie**val) is a tree where each edge represents a character, and each root-to-node path represents a string prefix. Words sharing a prefix share the same path down the tree.

**The problem that created it**: Autocomplete (Google search suggestions, IDE code completion, phone keyboard predictions) needs "give me all words starting with prefix X" *fast*, and repeatedly, for prefixes typed character by character. A hashmap of all words gives O(1) exact lookup but **cannot efficiently answer prefix queries** — you'd have to scan every word checking `startsWith()`, O(n·m). A sorted array/BST of words gives O(log n + k) prefix range but is complex to maintain. A Trie gives O(m) prefix lookup (m = prefix length, independent of how many words are stored!) by literally walking down shared prefix paths, and naturally deduplicates shared prefixes to save space (e.g., "cat," "car," "card" share the "ca" path).

**Real-life example**: A dictionary's physical structure if organized as a decision tree — to find all words starting with "pre", you walk p→r→e and everything below that node is your answer set, without needing to touch any word that doesn't share that prefix path.

## 12.2 Core Theory

- Each **Trie node** holds: an array/map of children (indexed by character, e.g. 26 slots for lowercase English), and a boolean flag `isEndOfWord`.
- **Insert**: walk/create nodes character by character, mark the last node `isEndOfWord = true`.
- **Search (exact word)**: walk character by character; word exists if path exists AND last node's `isEndOfWord` is true.
- **StartsWith (prefix search)**: walk character by character; prefix exists if the path exists (regardless of `isEndOfWord`).
- **Space tradeoff**: a Trie can use more memory than a hashset of the same words (many small node objects with pointer arrays) — this is the cost paid for O(m) prefix operations.

## 12.3 Time & Space Complexity

Let `m` = length of the word/prefix being processed, `n` = number of words, `Σ` = alphabet size.

| Operation | Complexity |
|---|---|
| Insert a word | O(m) |
| Search exact word | O(m) |
| Search prefix (startsWith) | O(m) |
| Space | O(n · m · Σ) worst case (shared prefixes reduce this in practice) |

Compare to a HashSet: O(1) average exact search, but **cannot** do prefix search without O(n) scan — this is the Trie's whole value proposition.

## 12.4 Key Patterns

1. **Autocomplete / Prefix matching** — walk to the prefix node, then DFS the subtree to collect all completions.
2. **Word Search on a Grid with a Dictionary** — build a Trie of the dictionary, then DFS the grid, pruning any path whose prefix isn't in the Trie (turns brute-force exponential search into something tractable).
3. **Wildcard / Fuzzy Matching in a Trie** — DFS with backtracking, treating `.`/wildcard as "try all children".
4. **Bitwise Trie (Binary Trie)** — a Trie over the *bits* of numbers (32 levels for 32-bit ints) — used for Maximum XOR pair problems (bridges to Ch. 16 Bit Manipulation).
5. **Trie + DP** — Word Break style problems combine Trie prefix lookup with DP over string indices.

### Code skeleton — Trie
```java
class TrieNode {
    TrieNode[] children = new TrieNode[26];
    boolean isEndOfWord = false;
}

class Trie {
    TrieNode root = new TrieNode();

    void insert(String word) {
        TrieNode node = root;
        for (char c : word.toCharArray()) {
            int idx = c - 'a';
            if (node.children[idx] == null) node.children[idx] = new TrieNode();
            node = node.children[idx];
        }
        node.isEndOfWord = true;
    }

    boolean search(String word) {
        TrieNode node = traverse(word);
        return node != null && node.isEndOfWord;
    }

    boolean startsWith(String prefix) {
        return traverse(prefix) != null;
    }

    private TrieNode traverse(String s) {
        TrieNode node = root;
        for (char c : s.toCharArray()) {
            int idx = c - 'a';
            if (node.children[idx] == null) return null;
            node = node.children[idx];
        }
        return node;
    }
}
```

## 12.5 Problems (Basic → Medium → Hard)

### Basic
1. Implement Trie (Prefix Tree) (LeetCode 208).
2. Count words with a given prefix.
3. Longest common prefix of a set of strings using a Trie.

### Medium
4. **Design Add and Search Words Data Structure** (LeetCode 211) — Trie + wildcard DFS.
5. **Replace Words** (LeetCode 648) — Trie prefix matching for stemming.
6. **Map Sum Pairs** (LeetCode 677).
7. **Implement Magic Dictionary** (LeetCode 676).
8. **Word Break** (LeetCode 139) — can be solved with Trie + DP.

### Hard
9. **Word Search II** (LeetCode 212) — Trie + grid DFS backtracking, classic hard-tier combo.
10. **Maximum XOR of Two Numbers in an Array** (LeetCode 421) — Binary Trie over bits, O(n·32).
11. **Palindrome Pairs** (LeetCode 336) — Trie of reversed words.
12. **Stream of Characters** (LeetCode 1032) — reverse Trie + streaming matching.
13. **Short Encoding of Words** (LeetCode 820) — Trie built from suffixes.

---
**Prev**: [Heaps & Priority Queues](11-Heap.md) | **Next**: [Chapter 13 — Graphs](13-Graphs.md)
