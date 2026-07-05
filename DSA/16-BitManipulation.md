# Chapter 16 — Bit Manipulation

## 16.1 What & Why

**Bit Manipulation** operates directly on the binary representation of numbers using bitwise operators (`&, |, ^, ~, <<, >>`).

**The problem that created it**: At the hardware level, everything is bits — CPUs execute bitwise operations in a **single cycle**, far faster than arithmetic in some cases, and using far less memory than storing booleans/flags as full integers or objects (a 32-bit int can represent 32 boolean flags — this is literally how Unix file permissions, network protocol flags, and embedded systems firmware pack state efficiently). Bit tricks also unlock elegant O(n) or O(1) solutions to problems that seem to need extra space or higher complexity (finding a unique element among duplicates, checking power-of-2, counting set bits).

**Real-life example**: Unix file permissions (`rwxr-xr--`) are literally stored and checked as bits — checking "can this user write?" is a single bitwise AND, not a database lookup. Network packet headers pack multiple flags into a byte for compact transmission.

## 16.2 Core Theory & Operators

| Operator | Meaning | Common use |
|---|---|---|
| `&` (AND) | 1 if both bits are 1 | Check/clear specific bits, check even/odd (`n & 1`) |
| `\|` (OR) | 1 if either bit is 1 | Set a specific bit |
| `^` (XOR) | 1 if bits differ | Toggle bits, find unique element (a^a=0, a^0=a) |
| `~` (NOT) | Flips all bits | Two's complement negation |
| `<<` (left shift) | Multiply by 2 per shift | `n << k` = `n * 2^k` |
| `>>` (right shift) | Divide by 2 per shift (arithmetic, sign-preserving) | `n >> k` = `n / 2^k` |

### Common bit tricks
- `n & (n-1)` → clears the lowest set bit (used to count set bits, check power of 2: `n & (n-1) == 0`).
- `n & (-n)` → isolates the lowest set bit.
- `n ^ n = 0`, `n ^ 0 = n`, XOR is commutative/associative → used to find a "lonely" unique element among pairs.
- `1 << k` → creates a bitmask with only bit k set; used to check/set/clear bit k: `n & (1<<k)`, `n | (1<<k)`, `n & ~(1<<k)`.

## 16.3 Time & Space Complexity

| Operation | Complexity |
|---|---|
| Any single bitwise operator | O(1) |
| Count set bits (Brian Kernighan's) | O(number of set bits) |
| Count set bits (built-in `popcount`) | O(1) (hardware instruction) |
| Bitmask DP over subsets of n items | O(2ⁿ) states |
| Generating all subsets via bitmask | O(n · 2ⁿ) |

## 16.4 Key Patterns

1. **XOR for uniqueness** — find the single non-duplicate number when every other number appears twice/thrice.
2. **Bitmask for subsets** — represent "which elements are chosen" as an integer; iterate `0` to `2ⁿ-1` to enumerate all subsets — foundational for Bitmask DP (Ch. 14).
3. **Counting set bits** — Brian Kernighan's algorithm (`n & (n-1)` repeatedly) or DP (`dp[i] = dp[i>>1] + (i&1)`).
4. **Power of Two / Power of Four checks** — `n > 0 && (n & (n-1)) == 0`.
5. **Bit DP / State compression** — replacing "visited set" with an integer bitmask for O(1) state comparison/storage instead of a HashSet.

### Code skeleton — Count set bits (Brian Kernighan's)
```java
int countBits(int n) {
    int count = 0;
    while (n != 0) {
        n = n & (n - 1);   // clears lowest set bit
        count++;
    }
    return count;
}
```

## 16.5 Problems (Basic → Medium → Hard)

### Basic
1. Check if a number is a power of two (LeetCode 231).
2. Count set bits / Number of 1 Bits (LeetCode 191).
3. Single Number (LeetCode 136) — XOR trick, O(n)/O(1).
4. Swap two numbers without a temp variable (XOR trick).
5. Check if the ith bit is set.

### Medium
6. **Single Number II** (LeetCode 137) — bit counting mod 3 trick.
7. **Single Number III** (LeetCode 260) — XOR + partition by differing bit.
8. **Counting Bits** (LeetCode 338) — DP + bit trick, O(n).
9. **Sum of Two Integers without + or -** (LeetCode 371) — bitwise addition (XOR + carry via AND/shift).
10. **Reverse Bits** (LeetCode 190).
11. **Missing Number** (LeetCode 268) — XOR trick.
12. **Subsets via Bitmask** (LeetCode 78) — bitmask enumeration alternative to backtracking.
13. **Bitwise AND of Numbers Range** (LeetCode 201).

### Hard
14. **Maximum XOR of Two Numbers in an Array** (LeetCode 421) — Binary Trie (also Ch. 12), O(32n).
15. **Maximum XOR With an Element From Array** (LeetCode 1707) — offline + Trie.
16. **UTF-8 Validation** (LeetCode 393) — bit-pattern parsing.
17. **Minimum XOR-based Bitmask DP problems** (e.g., Partition to K Equal Sum Subsets combined with bitmask).
18. **Traveling Salesman via Bitmask DP** (see Ch. 14) — bits represent visited-city sets.

---
**Prev**: [Greedy Algorithms](15-Greedy.md) | **Next**: [Chapter 17 — Advanced Data Structures](17-Advanced-DS.md)
