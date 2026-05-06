
# 🔢 Binary Numbers, Bits & Bytes — Complete Tutorial

  

> Everything a computer does — displaying text, playing video, running code — comes down to one thing: **billions of switches that are either ON or OFF**. This tutorial explains how that fundamental idea scales from a single switch all the way to the software you write every day.

  

---

  

## 📚 Table of Contents

  

1. [Why Binary? The Story of the Switch](#1-why-binary-the-story-of-the-switch)

2. [Number Systems Explained](#2-number-systems-explained)

3. [Binary — The Base-2 System](#3-binary--the-base-2-system)

4. [Converting Any Number to Binary (Step-by-Step)](#4-converting-any-number-to-binary-step-by-step)

5. [Converting Binary Back to Decimal](#5-converting-binary-back-to-decimal)

6. [Bits, Bytes, and Data Units](#6-bits-bytes-and-data-units)

7. [Hexadecimal — The Programmer's Shorthand](#7-hexadecimal--the-programmers-shorthand)

8. [Octal — Base-8](#8-octal--base-8)

9. [How Computers Store Text (ASCII & Unicode)](#9-how-computers-store-text-ascii--unicode)

10. [How Computers Store Numbers](#10-how-computers-store-numbers)

11. [How Computers Store Images, Audio & Video](#11-how-computers-store-images-audio--video)

12. [Binary Arithmetic](#12-binary-arithmetic)

13. [Bitwise Operators in Programming](#13-bitwise-operators-in-programming)

14. [Binary in Real-World Coding](#14-binary-in-real-world-coding)

15. [How the CPU Executes Code](#15-how-the-cpu-executes-code)

16. [From Human-Readable Code to Binary](#16-from-human-readable-code-to-binary)

17. [Quick Reference Cheat Sheet](#17-quick-reference-cheat-sheet)

  

---

  

## 1. Why Binary? The Story of the Switch

  

### The Physical Reality

  

Inside every computer chip are **billions of transistors** — tiny electronic switches that are either:

  

```

OFF = no electrical current = 0

ON  = electrical current    = 1

```

  

There are only **two possible states**. This is why computers use the **binary** (base-2) number system. A switch cannot be "half on" — it's either on or off.

  

### Why Not Use 10 States (Like Humans)?

  

You might wonder: why not build computers that work with 0–9 (10 states) like our decimal system?

  

```

Problem with multiple voltage levels:

  0.0V  = 0

  0.5V  = 1

  1.0V  = 2

  1.5V  = 3

  ...

  4.5V  = 9

  

Issues:

  ❌ Hard to distinguish reliably (noise, heat cause voltage drift)

  ❌ Requires complex circuitry

  ❌ Error-prone at high speeds

  

Binary (two states):

  Low voltage  (~0V)  = 0

  High voltage (~5V or 3.3V or 1.8V) = 1

  

  ✅ Easy to distinguish — even with noise/interference

  ✅ Simple circuitry (transistors = switches)

  ✅ Extremely fast and reliable

```

  

### The Brilliant Simplicity

  

```

A transistor = a light switch

0 = off     1 = on

  

Two switches can represent 4 combinations:

  00  01  10  11

  

Eight switches can represent 256 combinations:

  00000000 to 11111111

  

64 switches → over 18 quintillion combinations (2⁶⁴)

  

Your modern CPU has BILLIONS of transistors.

```

  

---

  

## 2. Number Systems Explained

  

A **number system** is a way of representing quantities using symbols. The key concept is the **base** (or radix) — how many unique digits exist.

  

### Decimal — Base 10 (Human System)

  

We use 10 digits: `0 1 2 3 4 5 6 7 8 9`

  

Each position is a power of 10:

  

```

Number: 4 2 5

  

Position values:

  4 × 10² = 4 × 100 = 400

  2 × 10¹ = 2 ×  10 =  20

  5 × 10⁰ = 5 ×   1 =   5

                     ─────

                      425

```

  

### The Pattern (Works for ANY Base)

  

```

Each digit is multiplied by:   base ^ position

Position counting starts at 0, from the RIGHT.

```

  

| System | Base | Digits Used | Prefix |

|--------|------|-------------|--------|

| Binary | 2 | `0, 1` | `0b` |

| Octal | 8 | `0–7` | `0o` |

| Decimal | 10 | `0–9` | (none) |

| Hexadecimal | 16 | `0–9, A–F` | `0x` |

  

---

  

## 3. Binary — The Base-2 System

  

Binary uses only **2 digits**: `0` and `1`

  

Each position is a power of 2:

  

```

Powers of 2 (memorize these!):

  

2⁰  =   1

2¹  =   2

2²  =   4

2³  =   8

2⁴  =  16

2⁵  =  32

2⁶  =  64

2⁷  = 128

2⁸  = 256

2⁹  = 512

2¹⁰ = 1024  (≈ 1 thousand — this is where "kilobyte" comes from)

2²⁰ = 1,048,576  (≈ 1 million — megabyte)

2³⁰ = 1,073,741,824  (≈ 1 billion — gigabyte)

```

  

### Reading a Binary Number

  

```

Binary:  1  0  1  1  0  1

  

Position values (powers of 2):

         2⁵ 2⁴ 2³ 2² 2¹ 2⁰

         32 16  8  4  2  1

  

Multiply each digit by its position value:

  1 × 32 =  32

  0 × 16 =   0

  1 ×  8 =   8

  1 ×  4 =   4

  0 ×  2 =   0

  1 ×  1 =   1

           ───

            45

  

So:  101101₂  =  45₁₀

```

  

---

  

## 4. Converting Any Number to Binary (Step-by-Step)

  

### Method 1: Division by 2 (Repeated Division)

  

Divide the number by 2 repeatedly. Read the **remainders from bottom to top**.

  

#### Example: Convert 45 to binary

  

```

45 ÷ 2 = 22 remainder 1   ← least significant bit (rightmost)

22 ÷ 2 = 11 remainder 0

11 ÷ 2 =  5 remainder 1

 5 ÷ 2 =  2 remainder 1

 2 ÷ 2 =  1 remainder 0

 1 ÷ 2 =  0 remainder 1   ← most significant bit (leftmost)

  

Read remainders bottom to top: 1 0 1 1 0 1

  

45₁₀ = 101101₂

```

  

#### Example: Convert 255 to binary

  

```

255 ÷ 2 = 127 r 1

127 ÷ 2 =  63 r 1

 63 ÷ 2 =  31 r 1

 31 ÷ 2 =  15 r 1

 15 ÷ 2 =   7 r 1

  7 ÷ 2 =   3 r 1

  3 ÷ 2 =   1 r 1

  1 ÷ 2 =   0 r 1

  

255₁₀ = 11111111₂  (eight 1s — this is the maximum value of 1 byte!)

```

  

### Method 2: Subtraction of Powers of 2

  

Find the largest power of 2 that fits, subtract it, repeat.

  

#### Example: Convert 200 to binary

  

```

Powers of 2: 128, 64, 32, 16, 8, 4, 2, 1

  

200 ≥ 128? YES → bit = 1, remainder = 200 - 128 = 72

 72 ≥  64? YES → bit = 1, remainder = 72 - 64   = 8

  8 ≥  32?  NO → bit = 0

  8 ≥  16?  NO → bit = 0

  8 ≥   8? YES → bit = 1, remainder = 8 - 8     = 0

  0 ≥   4?  NO → bit = 0

  0 ≥   2?  NO → bit = 0

  0 ≥   1?  NO → bit = 0

  

Result: 11001000₂

  

Verify: 128 + 64 + 8 = 200 ✅

```

  

### Conversion Table (0–20)

  

| Decimal | Binary | Hex |

|---------|--------|-----|

| 0  | 0000 | 0 |

| 1  | 0001 | 1 |

| 2  | 0010 | 2 |

| 3  | 0011 | 3 |

| 4  | 0100 | 4 |

| 5  | 0101 | 5 |

| 6  | 0110 | 6 |

| 7  | 0111 | 7 |

| 8  | 1000 | 8 |

| 9  | 1001 | 9 |

| 10 | 1010 | A |

| 11 | 1011 | B |

| 12 | 1100 | C |

| 13 | 1101 | D |

| 14 | 1110 | E |

| 15 | 1111 | F |

| 16 | 0001 0000 | 10 |

| 17 | 0001 0001 | 11 |

| 20 | 0001 0100 | 14 |

  

### In JavaScript / Python

  

```javascript

// JavaScript

(45).toString(2)    // "101101"   → decimal to binary string

(255).toString(16)  // "ff"       → decimal to hex string

(77).toString(8)    // "115"      → decimal to octal string

  

parseInt("101101", 2)  // 45  → binary string to decimal

parseInt("ff", 16)     // 255 → hex string to decimal

```

  

```python

# Python

bin(45)     # '0b101101'

hex(255)    # '0xff'

oct(77)     # '0o115'

  

int('101101', 2)  # 45  → binary to decimal

int('ff', 16)     # 255 → hex to decimal

```

  

---

  

## 5. Converting Binary Back to Decimal

  

### The Positional Method

  

```

Binary: 1 1 0 1 0 0 1 0

  

Write position values:

128  64  32  16   8   4   2   1

  

Multiply digit × value for each 1:

  1×128 = 128

  1× 64 =  64

  0× 32 =   0

  1× 16 =  16

  0×  8 =   0

  0×  4 =   0

  1×  2 =   2

  0×  1 =   0

           ───

           210

  

11010010₂ = 210₁₀

```

  

### Quick Mental Trick

  

Group bits into nibbles (4 bits), convert each to decimal:

  

```

Binary: 1101 0010

  

1101 = 8+4+0+1 = 13 = D (hex)

0010 = 0+0+2+0 =  2 = 2 (hex)

  

= 0xD2 = 210 ✅

```

  

---

  

## 6. Bits, Bytes, and Data Units

  

### Bit — The Smallest Unit

  

```

1 bit = one binary digit = 0 or 1

       = one transistor = one switch

  

A single bit can only represent 2 states:

  0 = false / off / no

  1 = true  / on  / yes

```

  

### Nibble

  

```

4 bits = 1 nibble

Can represent 2⁴ = 16 values (0–15)

Exactly one hexadecimal digit (0–F)

```

  

### Byte

  

```

8 bits = 1 byte

Can represent 2⁸ = 256 values (0–255)

  

This is the fundamental unit of computer storage.

Every character in ASCII is 1 byte.

Memory addresses are measured in bytes.

```

  

### Why 8 Bits = 1 Byte?

  

```

Early computers used 6-bit bytes, some used 7.

IBM standardized 8 bits = 1 byte in the 1960s for the IBM System/360.

8 is a power of 2 → divides evenly into 16, 32, 64 (common data widths)

8 bits can encode all 128 ASCII characters (and then some for extended sets)

```

  

### Data Size Units

  

```

1 bit           = 0 or 1

4 bits          = 1 nibble

8 bits          = 1 byte (B)

1,024 bytes     = 1 kilobyte   (KB)   = 2¹⁰ bytes

1,024 KB        = 1 megabyte   (MB)   = 2²⁰ bytes

1,024 MB        = 1 gigabyte   (GB)   = 2³⁰ bytes

1,024 GB        = 1 terabyte   (TB)   = 2⁴⁰ bytes

1,024 TB        = 1 petabyte   (PB)   = 2⁵⁰ bytes

```

  

### Bits vs Bytes — Critical Distinction

  

```

Bits  → used for NETWORK SPEED  (lowercase b: Mbps, Gbps)

Bytes → used for STORAGE SIZE   (uppercase B: MB, GB, TB)

  

Example:

  100 Mbps internet = 100 megaBITS per second

  = 100 ÷ 8 = 12.5 MB/s download speed

  

This is why your 100Mbps connection only downloads at ~12 MB/s!

```

  

### How Much Can Different Sizes Store?

  

| Bits | Possible Values | Stores |

|------|----------------|--------|

| 1 bit | 2 | True/False |

| 8 bits (1 byte) | 256 | One ASCII character |

| 16 bits (2 bytes) | 65,536 | Unicode BMP characters |

| 32 bits (4 bytes) | ~4.3 billion | IPv4 address, 32-bit integer |

| 64 bits (8 bytes) | ~18.4 quintillion | Modern integer/float |

| 128 bits (16 bytes) | 2¹²⁸ (~3.4 × 10³⁸) | IPv6 address, UUID |

  

---

  

## 7. Hexadecimal — The Programmer's Shorthand

  

Hexadecimal (hex) is **base-16**. It uses digits: `0 1 2 3 4 5 6 7 8 9 A B C D E F`

  

### Why Programmers Love Hex

  

```

4 binary digits    = 1 hex digit

8 binary digits    = 2 hex digits = 1 byte

  

Binary is hard to read:

  11111111 00001101 10110100 00100001

  

Same in hex:

  FF 0D B4 21

  

4× shorter but exactly equivalent!

```

  

### Hex Digit to Binary Mapping (Memorize This!)

  

```

0 = 0000    4 = 0100    8 = 1000    C = 1100

1 = 0001    5 = 0101    9 = 1001    D = 1101

2 = 0010    6 = 0110    A = 1010    E = 1110

3 = 0011    7 = 0111    B = 1011    F = 1111

```

  

### Converting Binary → Hex (Instant Method)

  

```

Binary: 1011 0110 1010 0011

  

Split into groups of 4:

  1011 = B

  0110 = 6

  1010 = A

  0011 = 3

  

= 0xB6A3

  

That's it! No math needed — just look up each nibble.

```

  

### Where You See Hex in Programming

  

```css

/* CSS Colors — each pair = R, G, B channel (0–255) */

color: #FF6600;   /* FF=red, 66=green, 00=blue */

color: #1A2B3C;

```

  

```

Memory addresses (debugger):    0x7FFF5FBFF8A0

IPv6 addresses:                 2001:0db8:85a3:0000:0000:8a2e:0370:7334

File magic numbers:             FF D8 FF (JPEG), 89 50 4E 47 (PNG)

Unicode code points:            U+1F600 = 😀

SHA-256 hash:                   a665a45920422f9d417e4867efdc4fb8a04a...

Git commit hash:                3a8f2c1d94e0b7...

UUID:                           550e8400-e29b-41d4-a716-446655440000

```

  

---

  

## 8. Octal — Base-8

  

Octal uses 8 digits: `0 1 2 3 4 5 6 7`. Each octal digit = **3 binary digits**.

  

```

Octal digit → Binary:

0 = 000    2 = 010    4 = 100    6 = 110

1 = 001    3 = 011    5 = 101    7 = 111

```

  

### Where Octal Appears

  

```bash

# Linux/Unix file permissions (chmod)

chmod 755 file.sh

  

7 = 111 = rwx (owner: read, write, execute)

5 = 101 = r-x (group: read, no write, execute)

5 = 101 = r-x (others: read, no write, execute)

  

chmod 644 file.txt

6 = 110 = rw- (owner: read, write, no execute)

4 = 100 = r-- (group: read only)

4 = 100 = r-- (others: read only)

```

  

---

  

## 9. How Computers Store Text (ASCII & Unicode)

  

### ASCII — American Standard Code for Information Interchange (1963)

  

ASCII assigns a number (0–127) to every English character, digit, and symbol.

  

```

Character → ASCII Decimal → Binary

  

'A' →  65 → 01000001

'B' →  66 → 01000010

'Z' →  90 → 01011010

'a' →  97 → 01100001

'z' → 122 → 01111010

'0' →  48 → 00110000

'9' →  57 → 00111001

' ' →  32 → 00100000  (space)

'\n'→  10 → 00001010  (newline)

'!' →  33 → 00100001

```

  

### ASCII Table (Key Values)

  

| Dec | Hex | Char | Description |

|-----|-----|------|-------------|

| 0 | 00 | NUL | Null |

| 9 | 09 | TAB | Tab |

| 10 | 0A | LF | Line Feed (newline) |

| 13 | 0D | CR | Carriage Return |

| 32 | 20 | ` ` | Space |

| 48–57 | 30–39 | 0–9 | Digits |

| 65–90 | 41–5A | A–Z | Uppercase letters |

| 97–122 | 61–7A | a–z | Lowercase letters |

  

### How "Hello" is Stored

  

```

"Hello" in ASCII:

  

H  = 72  = 01001000

e  = 101 = 01100101

l  = 108 = 01101100

l  = 108 = 01101100

o  = 111 = 01101111

  

In memory (hex): 48 65 6C 6C 6F

= 5 bytes total

```

  

### Why '0' ≠ 0

  

```

The number 0 (integer):     stored as 00000000 (binary zero)

The character '0' (text):   stored as 00110000 (ASCII 48)

  

This is why:

parseInt("0") === 0    // true  — converts string to number

"0" === 0              // false — different types!

```

  

### ASCII Limitation

  

ASCII only covers 128 characters — useless for Chinese, Arabic, Hindi, emoji, etc.

  

### Unicode — The Universal Solution

  

Unicode assigns a unique **code point** (U+XXXXXX) to over **149,000 characters** from 161 languages, plus emoji.

  

```

A = U+0041

Ω = U+03A9

中 = U+4E2D

😀 = U+1F600

❤️ = U+2764

```

  

### UTF-8 — The Most Common Encoding

  

UTF-8 is a **variable-length** encoding of Unicode:

  

```

ASCII range (U+0000–U+007F):    1 byte  (backward compatible with ASCII!)

Latin, Greek, etc (U+0080–U+07FF): 2 bytes

Most languages (U+0800–U+FFFF): 3 bytes

Emoji, rare chars (U+10000+):   4 bytes

  

Example:

  'A'  = 0x41 = 1 byte  = 01000001

  'Ω'  = 2 bytes = 11001110 10101001

  '中' = 3 bytes = 11100100 10111000 10101101

  '😀' = 4 bytes = 11110000 10011111 10011000 10000000

```

  

### Story of a Single Emoji

  

```

You type: 😀

Unicode:  U+1F600

Binary:   11110000 10011111 10011000 10000000 (4 bytes in UTF-8)

Hex:      F0 9F 98 80

Decimal:  240 159 152 128

```

  

---

  

## 10. How Computers Store Numbers

  

### Unsigned Integers

  

```

8-bit  unsigned: 0 to 255           (2⁸ - 1)

16-bit unsigned: 0 to 65,535        (2¹⁶ - 1)

32-bit unsigned: 0 to 4,294,967,295 (2³² - 1)

64-bit unsigned: 0 to 18,446,744,073,709,551,615 (2⁶⁴ - 1)

```

  

### Signed Integers (Two's Complement)

  

To represent negative numbers, computers use **two's complement**:

  

```

For 8-bit signed integers:

  Range: -128 to +127

  

Bit 7 (leftmost) is the sign bit:

  0 = positive

  1 = negative

  

0000 0000 =    0

0000 0001 =    1

0111 1111 =  127  (max positive)

1000 0000 = -128  (min negative)

1111 1111 =   -1

1111 1110 =   -2

  

To get -X from X:

  Step 1: Flip all bits (invert)

  Step 2: Add 1

  

Example: find -5 in 8-bit

  5     = 0000 0101

  Flip  = 1111 1010

  Add 1 = 1111 1011  = -5 ✅

```

  

### Why Two's Complement?

  

```

It makes addition work for both positive and negative numbers:

  

  5  =  0000 0101

+ -3  =  1111 1101

      = 10000 0010  → drop the carry bit → 0000 0010 = 2 ✅

```

  

### Integer Overflow

  

```javascript

// JavaScript's Number is a 64-bit float

console.log(Number.MAX_SAFE_INTEGER);   // 9007199254740991 (2⁵³ - 1)

  

// C/C++ 32-bit signed integer overflow example:

// INT_MAX = 2,147,483,647

// INT_MAX + 1 = -2,147,483,648 (wraps around!)

// This caused the Samsung Galaxy Note 7 issues and Ariane 5 rocket crash

```

  

### Floating-Point Numbers (IEEE 754)

  

Real numbers with decimals are stored using a scientific notation format:

  

```

32-bit float (float):

  Sign:     1 bit

  Exponent: 8 bits

  Mantissa: 23 bits

  

Example: 3.14159 stored as float

  

Binary scientific notation: 1.10010010000111111011011 × 2¹

  

64-bit double (double):

  Sign:     1 bit

  Exponent: 11 bits

  Mantissa: 52 bits  → more precision

```

  

### The Famous Floating-Point Weirdness

  

```javascript

0.1 + 0.2 === 0.3   // false!

0.1 + 0.2           // 0.30000000000000004

  

Why?

0.1 in binary = 0.0001100110011001100...  (repeating!)

Just like 1/3 = 0.333... in decimal — no exact representation.

```

  

---

  

## 11. How Computers Store Images, Audio & Video

  

### Images — Pixels and RGB

  

```

Every image = a grid of pixels

Each pixel  = 3 color channels: Red, Green, Blue

Each channel = 1 byte (0–255)

Each pixel   = 3 bytes = 24 bits

  

Example: A 1920×1080 image

  Pixels: 1920 × 1080 = 2,073,600 pixels

  Bytes:  2,073,600 × 3 = 6,220,800 bytes ≈ 6 MB (uncompressed!)

  

  With alpha (transparency):

  Each pixel = 4 bytes (R, G, B, A) → 32-bit color

```

  

```

Color examples:

  Red:    255, 0,   0   = FF 00 00

  Green:  0,   255, 0   = 00 FF 00

  Blue:   0,   0,   255 = 00 00 FF

  White:  255, 255, 255 = FF FF FF

  Black:  0,   0,   0   = 00 00 00

  Orange: 255, 102, 0   = FF 66 00

```

  

### Image Compression

  

```

PNG:  Lossless compression (no quality loss) — best for graphs, logos

JPEG: Lossy compression (discards some data) — best for photos (10:1 ratio)

WebP: Google's format — lossless or lossy, 30% smaller than JPEG

AVIF: AV1-based image format — best compression available

GIF:  Limited to 256 colors, lossless — only good for animations

SVG:  Not binary — uses XML/math to describe shapes, infinitely scalable

```

  

### Audio

  

```

Sound = air pressure waves (analog)

  

Digitizing audio:

  Sample rate:   44,100 samples per second (CD quality)

  Bit depth:     16 bits per sample (0–65,535 levels)

  Channels:      2 (stereo)

  

1 second of uncompressed audio:

  44,100 × 2 bytes × 2 channels = 176,400 bytes ≈ 172 KB/s

  

MP3 compression:

  ~128 Kbps bitrate → about 16 KB/s

  10:1 compression ratio (discards frequencies humans struggle to hear)

```

  

### Video

  

```

Video = sequence of images (frames) + audio

  

Example: 1080p at 30fps (uncompressed)

  One frame: 1920 × 1080 × 3 bytes = 6.2 MB

  30 frames/sec: 6.2 MB × 30 = 186 MB/second!

  

H.264/H.265 compression (MP4):

  Same 1080p30 → ~4–8 Mbps (~0.5–1 MB/s)

  200:1 compression using temporal and spatial compression

```

  

---

  

## 12. Binary Arithmetic

  

### Addition

  

```

Rules:

  0 + 0 = 0

  0 + 1 = 1

  1 + 0 = 1

  1 + 1 = 10  (0, carry 1)  — just like 9+1=10 in decimal

  

Example: 1011 + 0110 (11 + 6 = 17)

  

    1011

  + 0110

  ──────

    Rightmost: 1+0 = 1

    Next:      1+1 = 10, write 0 carry 1

    Next:  1 + 0+1 = 10, write 0 carry 1

    Next:  1 + 1+0 = 10, write 0 carry 1

    Leftmost carry: 1

  = 10001 = 17 ✅

```

  

### Subtraction (Using Two's Complement)

  

```

10 - 6 = ?

  

Convert 6 to -6:

  6 = 00000110

  Flip:  11111001

  +1:    11111010  = -6

  

Now add:

  00001010  (10)

+ 11111010  (-6)

= 100000100 → drop carry → 00000100 = 4 ✅

```

  

### Multiplication (Shift and Add)

  

```

Binary multiplication = shift left (×2) + add

  

5 × 3 = 5 × (2+1) = 5<<1 + 5<<0

  101 × 011

  ─────────

  101   (5 × 1)

 1010   (5 × 2, shifted left)

+0000   (5 × 0)

──────

 1111 = 15 ✅

```

  

### Division (Shift Right)

  

```

Dividing by a power of 2 = right shift:

  16 ÷ 2  = 10000 >> 1 = 01000 = 8

  16 ÷ 4  = 10000 >> 2 = 00100 = 4

  16 ÷ 8  = 10000 >> 3 = 00010 = 2

```

  

---

  

## 13. Bitwise Operators in Programming

  

Bitwise operators work on individual bits of a number. They're extremely fast (single CPU instruction).

  

### The 6 Bitwise Operators

  

```

& — AND:    both bits must be 1

| — OR:     at least one bit is 1

^ — XOR:    exactly one bit is 1 (exclusive or)

~ — NOT:    invert all bits

<< — Left Shift:  multiply by 2

>> — Right Shift: divide by 2

```

  

### AND (`&`)

  

```

  1010  (10)

& 1100  (12)

──────

  1000  (8)  — only where BOTH are 1

  

Common use: Check if a bit is set / masking

  if (flags & 0b0100) { /* bit 2 is set */ }

```

  

### OR (`|`)

  

```

  1010  (10)

| 1100  (12)

──────

  1110  (14) — where EITHER is 1

  

Common use: Set a specific bit

  flags = flags | 0b0100;   // turn on bit 2

  flags |= 0b0100;          // shorthand

```

  

### XOR (`^`)

  

```

  1010  (10)

^ 1100  (12)

──────

  0110  (6)  — where bits DIFFER

  

Common uses:

  Toggle a bit:     flags ^= 0b0100;

  Find difference:  diff = a ^ b;

  Swap without temp:

    a = a ^ b;

    b = a ^ b;

    a = a ^ b;

```

  

### NOT (`~`)

  

```

~1010  = 0101  (flip every bit)

~10    = -(10+1) = -11  (in signed integers)

  

Use: Create bitmasks

  ~0b0100 = 11111011  → clear bit 2

  flags &= ~0b0100;   // turn off bit 2

```

  

### Left Shift (`<<`)

  

```

5 << 1 = 10  (×2)

5 << 2 = 20  (×4)

5 << 3 = 40  (×8)

  

Rule: n << k = n × 2ᵏ

  

0000 0101  (5)

<< 2

0001 0100  (20)

```

  

### Right Shift (`>>`)

  

```

20 >> 1 = 10  (÷2)

20 >> 2 =  5  (÷4)

  

Rule: n >> k = n ÷ 2ᵏ (integer division)

  

0001 0100  (20)

>> 2

0000 0101  (5)

```

  

### Real-World Bitwise Examples

  

```javascript

// 1. Check if number is even or odd

const isOdd  = n => (n & 1) === 1;

const isEven = n => (n & 1) === 0;

isOdd(7)   // true  (0111 & 0001 = 0001)

isEven(8)  // true  (1000 & 0001 = 0000)

  

// 2. Power of 2 check

const isPowerOf2 = n => n > 0 && (n & (n-1)) === 0;

isPowerOf2(16)  // true (10000 & 01111 = 00000)

  

// 3. Swap two variables without temp

let a = 5, b = 9;

a = a ^ b;  // a = 5^9 = 12

b = a ^ b;  // b = 12^9 = 5

a = a ^ b;  // a = 12^5 = 9

  

// 4. Fast multiplication/division by powers of 2

const double  = n => n << 1;  // much faster than n * 2

const half    = n => n >> 1;  // much faster than Math.floor(n / 2)

  

// 5. Permission flags (bit field)

const PERM_READ    = 0b001;  // 1

const PERM_WRITE   = 0b010;  // 2

const PERM_EXECUTE = 0b100;  // 4

  

let userPerms = PERM_READ | PERM_WRITE;  // 0b011 = 3

  

// Check permission

if (userPerms & PERM_READ)    console.log("Can read");

if (userPerms & PERM_EXECUTE) console.log("Can execute");  // false

  

// Grant permission

userPerms |= PERM_EXECUTE;   // 0b111 = 7

  

// Revoke permission

userPerms &= ~PERM_WRITE;    // 0b101 = 5

  

// 6. RGB color manipulation

const red   =  (0xFF6600 >> 16) & 0xFF;  // 255

const green = (0xFF6600 >>  8) & 0xFF;   // 102

const blue  =  0xFF6600        & 0xFF;   // 0

  

// Pack RGB into a single number

const color = (red << 16) | (green << 8) | blue;  // 0xFF6600

```

  

```python

# Python

# Check if bit N is set

def is_bit_set(value, n):

    return bool(value & (1 << n))

  

# Count set bits (Brian Kernighan's algorithm)

def count_bits(n):

    count = 0

    while n:

        n &= n - 1  # clear lowest set bit

        count += 1

    return count

```

  

---

  

## 14. Binary in Real-World Coding

  

### Memory Addresses

  

```

Every byte in RAM has a unique address, expressed in hex:

  

0x00000000  ← first byte of memory

0x7FFFFFFFFFFF ← typical user space limit (64-bit)

  

In C:

int x = 42;

printf("%p", &x);  // prints address like 0x7fff5fbff8a4

```

  

### Data Type Sizes

  

```c

// C / C++

char     = 1 byte  (8 bits)

short    = 2 bytes (16 bits)

int      = 4 bytes (32 bits)

long     = 4 or 8 bytes

float    = 4 bytes (32-bit IEEE 754)

double   = 8 bytes (64-bit IEEE 754)

bool     = 1 byte  (only uses 1 bit, wastes 7)

```

  

```javascript

// JavaScript — all numbers are 64-bit floats (IEEE 754 double)

// Except for bitwise ops — handled as 32-bit signed integers

  

// TypedArrays for binary data control:

const buffer = new ArrayBuffer(8);       // 8 bytes

const view   = new DataView(buffer);

view.setUint8(0, 0xFF);

view.setInt32(4, -1, true);              // little-endian

```

  

### Networking & Binary

  

```

IP Address (IPv4):  192.168.1.1

  192 = 11000000

  168 = 10101000

    1 = 00000001

    1 = 00000001

Binary: 11000000.10101000.00000001.00000001 = 32 bits total

  

Subnet Mask:  255.255.255.0

  = 11111111.11111111.11111111.00000000

  The 1s show the network part, 0s show the host part.

  

IP & Mask (bitwise AND) = Network Address:

  192.168.1.1   & 255.255.255.0 = 192.168.1.0

```

  

### File Magic Numbers

  

Every file type starts with a specific byte sequence (signature):

  

```

JPEG:  FF D8 FF

PNG:   89 50 4E 47 0D 0A 1A 0A

GIF:   47 49 46 38 ("GIF8")

PDF:   25 50 44 46 ("%PDF")

ZIP:   50 4B 03 04 ("PK")

MP3:   FF FB (or ID3 for tagged)

EXE:   4D 5A ("MZ" — Mark Zbikowski, DOS designer)

  

This is why renaming file.jpg → file.txt doesn't change the file type.

The OS reads the magic number, not just the extension.

```

  

### Hashing

  

```

Cryptographic hashes produce fixed-size binary output:

  

MD5:     128 bits = 32 hex characters

SHA-1:   160 bits = 40 hex characters

SHA-256: 256 bits = 64 hex characters

  

"hello" → SHA-256:

2cf24dba5fb0a30e26e83b2ac5b9e29e1b161e5c1fa7425e73043362938b9824

  

One bit change in input → completely different output

Used for: password storage, file integrity, digital signatures

```

  

---

  

## 15. How the CPU Executes Code

  

### The Fetch-Decode-Execute Cycle

  

Every CPU instruction follows this loop — billions of times per second:

  

```

1. FETCH:   CPU reads the next instruction from RAM (at the address in the Program Counter)

2. DECODE:  CPU's control unit interprets the binary opcode

3. EXECUTE: CPU's ALU (Arithmetic Logic Unit) performs the operation

4. WRITEBACK: Result stored in a register or RAM

→ Increment Program Counter → Repeat

```

  

### Machine Code (Binary Instructions)

  

```

CPU instructions are binary patterns.

  

Example (x86-64 assembly → machine code):

  

Assembly instruction:         ADD EAX, 01H  (add 1 to register EAX)

Machine code (binary):        00000011 00000001

Machine code (hex):           03 01

  

MOV EAX, 5  (put value 5 in register EAX)

Machine code:  B8 05 00 00 00

  

RET  (return from function)

Machine code:  C3

```

  

### CPU Registers

  

```

Registers are tiny, ultra-fast memory inside the CPU:

  

64-bit CPU has general purpose registers:

  RAX, RBX, RCX, RDX  — general purpose (64-bit)

  EAX, EBX, ECX, EDX  — lower 32 bits of the above

  AX, BX, CX, DX      — lower 16 bits

  AL, AH, BL, BH      — lower/upper 8 bits

  

Special registers:

  RIP  — Instruction Pointer (address of next instruction)

  RSP  — Stack Pointer

  RFLAGS — Status flags (zero, carry, overflow, etc.)

```

  

### The ALU (Arithmetic Logic Unit)

  

```

The ALU is built from logic gates (AND, OR, NOT, XOR, NAND, NOR, XNOR)

made from transistors.

  

Logic gate truth tables:

  

AND:          OR:           NOT:          XOR:

A B | Out    A B | Out    A | Out    A B | Out

0 0 |  0     0 0 |  0    0 |  1     0 0 |  0

0 1 |  0     0 1 |  1    1 |  0     0 1 |  1

1 0 |  0     1 0 |  1                1 0 |  1

1 1 |  1     1 1 |  1                1 1 |  0

  

A half adder (adds 2 bits) is built with XOR + AND gates.

A full adder extends this for carry.

Chain 64 full adders → 64-bit addition circuit.

```

  

---

  

## 16. From Human-Readable Code to Binary

  

The journey from what you write to what the CPU runs:

  

```

┌─────────────────────────────────────────────────────────────┐

│  You write (Source Code):                                    │

│      let result = 5 + 3;                                    │

└──────────────────────┬──────────────────────────────────────┘

                       │

                  COMPILATION

                       │

┌──────────────────────▼──────────────────────────────────────┐

│  Lexer / Tokenizer:                                          │

│  'let' 'result' '=' '5' '+' '3' ';'                        │

└──────────────────────┬──────────────────────────────────────┘

                       │

                  PARSING

                       │

┌──────────────────────▼──────────────────────────────────────┐

│  Abstract Syntax Tree (AST):                                 │

│  VariableDeclaration                                         │

│    └─ Identifier: result                                     │

│    └─ BinaryExpression: +                                    │

│         ├─ Literal: 5                                        │

│         └─ Literal: 3                                        │

└──────────────────────┬──────────────────────────────────────┘

                       │

          OPTIMIZATION + CODE GENERATION

                       │

┌──────────────────────▼──────────────────────────────────────┐

│  Assembly:                                                   │

│      MOV EAX, 5       ; put 5 in register EAX               │

│      ADD EAX, 3       ; add 3 to EAX (result = 8)           │

│      MOV [result], EAX ; store in memory                    │

└──────────────────────┬──────────────────────────────────────┘

                       │

                   ASSEMBLING

                       │

┌──────────────────────▼──────────────────────────────────────┐

│  Machine Code (hex):                                         │

│      B8 05 00 00 00   ; MOV EAX, 5                          │

│      83 C0 03         ; ADD EAX, 3                          │

│      89 05 XX XX XX XX; MOV [addr], EAX                    │

└──────────────────────┬──────────────────────────────────────┘

                       │

                CPU EXECUTES

                       │

┌──────────────────────▼──────────────────────────────────────┐

│  Transistors switch on/off                                   │

│  Electrons flow (or don't) through silicon gates             │

│  Result: register RAX = 00000000 00000000 00000000 00001000  │

│                       = 8 in binary ✅                       │

└─────────────────────────────────────────────────────────────┘

```

  

### Interpreted vs Compiled Languages

  

```

COMPILED (C, C++, Rust, Go):

  Source code → Compiler → Machine code (binary)

  Runs directly on CPU → FASTEST

  

INTERPRETED (Python, Ruby, PHP classic):

  Source code → Interpreter reads + executes line by line

  No separate machine code → SLOWER (interpreter overhead)

  

JIT COMPILED (JavaScript V8, Java JVM, C# .NET):

  Source code → Bytecode → JIT compiler → Machine code at runtime

  Best of both: portability + near-native speed

  

  JavaScript journey (V8 engine):

  .js file → Parser → AST → Bytecode (Ignition) → Hot code JIT compiled

                                                   (TurboFan) → Machine code

```

  

### Bytecode Example (Python)

  

```python

# Python source

def add(a, b):

    return a + b

  

# Python bytecode (dis module)

import dis

dis.dis(add)

  

# Output:

#   LOAD_FAST   0 (a)

#   LOAD_FAST   1 (b)

#   BINARY_ADD

#   RETURN_VALUE

```

  

---

  

## 17. Quick Reference Cheat Sheet

  

### Number System Conversions

  

```

Decimal → Binary:   Divide by 2, read remainders bottom-up

Binary → Decimal:   Sum of (bit × 2^position)

Binary → Hex:       Group into nibbles, each nibble = 1 hex digit

Hex → Binary:       Each hex digit → 4-bit binary (use lookup table)

Decimal → Hex:      Divide by 16, read remainders bottom-up

```

  

### Powers of 2

  

```

2⁰=1   2¹=2   2²=4    2³=8    2⁴=16   2⁵=32

2⁶=64  2⁷=128 2⁸=256  2⁹=512 2¹⁰=1024

2²⁰=1,048,576 (1M)    2³⁰=1,073,741,824 (1B)

```

  

### Units

  

```

8 bits    = 1 byte

1024 B    = 1 KB

1024 KB   = 1 MB

1024 MB   = 1 GB

```

  

### Bitwise Operators

  

```

a & b   → AND    (both 1)

a | b   → OR     (either 1)

a ^ b   → XOR    (differ)

~a      → NOT    (flip all)

a << n  → ×2ⁿ

a >> n  → ÷2ⁿ

```

  

### Common Bit Tricks

  

```javascript

n & 1        // odd check (1=odd, 0=even)

n & (n-1)    // clears lowest set bit (0 if power of 2)

n | (1<<k)   // set bit k

n & ~(1<<k)  // clear bit k

n ^ (1<<k)   // toggle bit k

(n>>k) & 1   // check if bit k is set

```

  

### Key ASCII Values

  

```

'0'–'9' = 48–57

'A'–'Z' = 65–90

'a'–'z' = 97–122

' '     = 32

'\n'    = 10

```

  

---

  

> **The Big Picture:**

>

> At the bottom of every app, website, game, and AI system is the same thing:

> **billions of tiny switches — on or off, 1 or 0**.

>

> Binary is not a limitation — it's the most reliable foundation possible.

> Everything else (text, images, music, video, your code) is just an

> agreement between humans about how to **interpret those patterns of bits**.

  

*Now you understand what computers actually do. Happy hacking! ⚡*