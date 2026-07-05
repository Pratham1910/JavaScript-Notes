# Chapter 8 — Hashing

## 8.1 What & Why

**Hashing** maps a key to an array index via a **hash function**, giving average O(1) insert/search/delete — the fastest lookup possible without extra structure like sorting.

**The problem that created it**: Arrays give O(1) access *only if you already know the index*. Real-world keys are not small integers — they're names, emails, IP addresses, arbitrary objects. Searching a sorted array is O(log n); linear array is O(n). Hashing was invented to get **O(1) average lookup for arbitrary keys** by converting the key into an array index via a hash function — this is the backbone of database indexes, caches, compilers' symbol tables, and de-duplication systems.

**Real-life example**: A library that shelves books not alphabetically but by a computed "shelf number" derived from the ISBN (hash function) — you compute the shelf number and walk straight there, instead of scanning every shelf (array) or doing a binary search through an alphabetized catalog (BST).

## 8.2 Core Theory

- **Hash Function**: converts key → index (`index = hash(key) % table_size`). Good hash functions distribute keys uniformly to minimize collisions.
- **Collision**: two different keys map to the same index. Since key space >> table size, collisions are inevitable (Pigeonhole Principle) — good design handles them well, not avoids them.
- **Collision Resolution**:
  - **Chaining**: each bucket holds a linked list (or tree, in Java 8+ HashMap when a bucket gets large) of all keys hashing there.
  - **Open Addressing**: on collision, probe another slot (linear probing, quadratic probing, double hashing) — keeps all data in-place, better cache locality, but needs careful deletion handling (tombstones).
- **Load Factor** (`α = n/table_size`): when it exceeds a threshold (commonly 0.75), the table **resizes** (rehashes all keys into a bigger table) to maintain O(1) average performance — this resizing is why insert is **O(1) amortized**, not strictly O(1).
- **HashSet vs HashMap**: HashSet = HashMap where you only care about key existence, not an associated value.

## 8.3 Time & Space Complexity

| Operation | Average | Worst Case (bad hash/many collisions) |
|---|---|---|
| Insert | O(1) | O(n) |
| Search | O(1) | O(n) |
| Delete | O(1) | O(n) |
| Space | O(n) | O(n) |

Worst case O(n) happens if the hash function is poor and everything collides into one bucket (degrades to a linked list scan) — Java's HashMap mitigates this since Java 8 by treeifying long chains (O(log n) worst case per bucket).

## 8.4 Key Patterns

1. **Frequency Counting** — count occurrences of elements (anagrams, majority element, mode).
2. **Existence Check / Deduplication** — "have I seen this before?" in O(1), turning O(n²) brute-force pair-checking into O(n).
3. **Two Sum pattern** — store `value → index` while scanning, check `target - current` exists, O(n).
4. **Grouping by computed key** — group anagrams (key = sorted string), group by remainder (key = `sum % k`).
5. **Prefix Sum + Hashmap** — subarray sum problems: store `prefixSum → count/index` to find subarrays summing to k in O(n) (Ch. 1, problem 15).
6. **HashMap as a Graph Adjacency List** — foundational for Graph representation (Ch. 13).

### Code skeleton — Two Sum with HashMap
```java
int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> seen = new HashMap<>(); // value -> index
    for (int i = 0; i < nums.length; i++) {
        int need = target - nums[i];
        if (seen.containsKey(need)) return new int[]{seen.get(need), i};
        seen.put(nums[i], i);
    }
    return new int[]{-1, -1};
}
```

## 8.5 Problems (Basic → Medium → Hard)

### Basic
1. Two Sum (LeetCode 1) — O(n).
2. Check for duplicates in an array (LeetCode 217).
3. First non-repeating character in a string (LeetCode 387).
4. Valid Anagram (LeetCode 242).
5. Intersection of Two Arrays (LeetCode 349).
6. Majority Element (LeetCode 169) — hashmap, or Boyer-Moore voting for O(1) space.

### Medium
7. **Group Anagrams** (LeetCode 49).
8. **Subarray Sum Equals K** (LeetCode 560) — prefix sum + hashmap, O(n).
9. **Longest Consecutive Sequence** (LeetCode 128) — hashset, O(n).
10. **4Sum II** (LeetCode 454) — split into two pair-sum hashmaps, O(n²).
11. **Top K Frequent Elements** (LeetCode 347) — hashmap + heap/bucket sort.
12. **Copy List with Random Pointer** (LeetCode 138) — hashmap for node mapping.
13. **Design a HashMap from scratch** (LeetCode 706) — implement chaining/open addressing.
14. **Isomorphic Strings** (LeetCode 205).
15. **LRU Cache** (LeetCode 146) — hashmap + doubly linked list.

### Hard
16. **Substring with Concatenation of All Words** (LeetCode 30) — hashmap + sliding window.
17. **Longest Substring with At Most K Distinct Characters** (LeetCode 340) — hashmap + sliding window.
18. **Insert Delete GetRandom O(1)** (LeetCode 380) — hashmap + array combo.
19. **All O`one Data Structure** (LeetCode 432) — hashmap + doubly linked list, advanced design.
20. **Design Twitter** (LeetCode 355) — hashmap + heap combo, system-design-flavored DSA.

---
**Prev**: [Stacks & Queues](07-Stack-Queue.md) | **Next**: [Chapter 9 — Trees](09-Trees.md)
