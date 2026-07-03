

# Chapter 2 — Strings

## 2.1 What & Why

A **string** is an array of characters, but treated as a distinct type because text has its own operations: comparison, pattern matching, parsing. Strings are usually **immutable** in languages like Java, Python, JS — every "modification" creates a new string, which is why naive string concatenation in a loop is O(n²) (build with `StringBuilder`/list + join instead, O(n)).

**The problem that created dedicated string algorithms**: Searching for a substring naively (check every position, compare every character) is O(n·m). Real systems — search engines, DNA sequence matching, spell-checkers, plagiarism detectors — need to search text at massive scale, so specialized algorithms (KMP, Rabin-Karp, Z-algorithm, Tries, Suffix Arrays) were invented to bring this down to O(n+m) or better.

**Real-life example**: Ctrl+F in a browser searching a 10,000-word document for "algorithm" — naive char-by-char comparison at every position is wasteful because when a mismatch happens, you *already know* information about the partial match (KMP exploits this to never re-check characters).

## 2.2 Core Theory

- Strings support **indexing** (O(1)) like arrays.
- **Immutability** cost: `s += c` in a loop → O(n) per op → O(n²) total. Fix: mutable buffer (StringBuilder/list of chars).
- **ASCII vs Unicode**: character sets matter for hashing and frequency-count array sizing (26 for lowercase, 128/256 for ASCII).
- **Palindrome, Anagram, Permutation** checks are recurring conceptual building blocks.

## 2.3 Time & Space Complexity

| Operation                 | Complexity                                   |
| ------------------------- | -------------------------------------------- |
| Access char at index      | O(1)                                         |
| Concatenation (immutable) | O(n) per op                                  |
| Substring extraction      | O(k) (k = substring length)                  |
| Naive substring search    | O(n·m)                                      |
| KMP / Z-algorithm search  | O(n + m)                                     |
| Rabin-Karp (avg)          | O(n + m); worst O(n·m) with hash collisions |
| Sorting characters        | O(n log n)                                   |

## 2.4 Key Patterns

1. **Two Pointers** — palindrome check, reverse words.
2. **Sliding Window** — longest substring without repeating characters, minimum window substring.
3. **Frequency Count array/HashMap** — anagrams, character counting.
4. **KMP (Knuth-Morris-Pratt)** — build a "failure function" (longest proper prefix that's also suffix, LPS array) so matching never backtracks in the text — O(n+m).
5. **Rabin-Karp** — rolling hash to compare substrings in O(1) after O(m) preprocessing.
6. **Trie** — prefix-based storage (see Chapter 12).
7. **Manacher's Algorithm** — longest palindromic substring in O(n).

### KMP Failure Function (LPS array) — code skeleton

```java
int[] buildLPS(String pat) {
    int[] lps = new int[pat.length()];
    int len = 0, i = 1;
    while (i < pat.length()) {
        if (pat.charAt(i) == pat.charAt(len)) {
            lps[i++] = ++len;
        } else if (len != 0) {
            len = lps[len - 1];
        } else {
            lps[i++] = 0;
        }
    }
    return lps;
}
```

## 2.5 Problems (Basic → Medium → Hard)

### Basic

1. Reverse a string / reverse words in a sentence.
2. Check palindrome (two pointers).
3. Check anagram of two strings — frequency array, O(n).
4. Count vowels/consonants, char frequency.
5. String to integer (atoi) — edge case handling.
6. Check if a string is a rotation of another (`s2` in `s1+s1`).

### Medium

7. **Longest Substring Without Repeating Characters** (LeetCode 3) — sliding window + hashset, O(n).
8. **Longest Palindromic Substring** (LeetCode 5) — expand-around-center O(n²) or Manacher O(n).
9. **Group Anagrams** (LeetCode 49) — hashmap keyed by sorted string / char-count signature.
10. **Valid Parentheses** (LeetCode 20) — stack (bridges into Chapter 7).
11. **String to Integer / Roman to Integer / Integer to Roman** (LeetCode 8, 13, 12).
12. **Implement strStr()** (LeetCode 28) — KMP.
13. **Minimum Window Substring** (LeetCode 76) — sliding window + frequency map, O(n).
14. **Longest Common Prefix** (LeetCode 14).
15. **Zigzag Conversion** (LeetCode 6).
16. **Decode String** (LeetCode 394) — stack-based parsing.

### Hard

17. **Regular Expression Matching** (LeetCode 10) — DP, O(n·m).
18. **Wildcard Matching** (LeetCode 44) — DP.
19. **Edit Distance** (LeetCode 72) — DP (bridges into Chapter 14).
20. **Text Justification** (LeetCode 68).
21. **Shortest Palindrome** (LeetCode 214) — KMP trick.
22. **Longest Valid Parentheses** (LeetCode 32) — stack/DP, O(n).
23. **Word Break II** (LeetCode 140) — DP + backtracking.
24. **Distinct Subsequences** (LeetCode 115) — DP.

---

**Prev**: [Arrays](01-Arrays.md) | **Next**: [Chapter 3 — Recursion &amp; Backtracking](03-Recursion-Backtracking.md)
