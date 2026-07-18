---
tags: [live-session, maths, binary-exponentiation, modular-arithmetic, number-theory]
status: In Progress
source: https://maang.in/live-sessions/Combinatorial-Ideas-to-Know-1290
---

# Maths for Competitive Programming

Math/number-theory groundwork picked up from live sessions — kept separate from combinatorics-specific notes since these are general prerequisite techniques, not combinatorics itself.

## 1. Modular Exponentiation — `(a^b) % c`

**Constraints:** `a, b, c ≤ 10^18` — far too large for naive repeated multiplication (`O(b)`), so the session builds up to binary/fast exponentiation, `O(log b)`.

Recurrence, split by parity of the exponent:

- **b odd:** `a^b = a * a^(b-1)`
- **b even:** `a^b = (a^(b/2))^2`

### Iterative (while-loop) implementation

```cpp
long long power(long long a, long long b, long long c) {
    long long result = 1;
    a = a % c;
    while (b > 0) {
        if (b & 1) {              // b is odd -> a^b = a * a^(b-1)
            result = (result * a) % c;
        }
        a = (a * a) % c;          // square a for the next bit
        b >>= 1;                  // b even -> a^b = (a^(b/2))^2, so halve b
    }
    return result;
}
```

Each loop iteration peels off one bit of `b`. When the current lowest bit of `b` is `1`, the current value of `a` (which represents `base^(2^i)` at iteration `i`) gets folded into `result`. Then `a` is squared and `b` is right-shifted by 1 bit. This runs in `O(log b)` time.

Mod is applied after every multiplication, not just at the end, to avoid overflow on 64-bit integers when `a`, `c` are close to `10^18` (for the tightest bounds, intermediate products may still need `__int128` before taking `% c`).

### Worked example: `5^7`

Chose `7` over a clean power of 2 like `8` deliberately — `8 = 2^3` has only one bit set, so it only exercises the squaring step. `7 = 111₂` has three bits set, so it actually exercises the "fold into result" step multiple times.

**`7` in binary:** `111₂ = 1×2² + 1×2¹ + 1×2⁰ = 4 + 2 + 1`

So `5^7 = 5^4 × 5^2 × 5^1` — each power-of-2 term included because its bit is `1`.

Trace of `a=5, b=7, result=1`:

| Step | b (binary) | b&1? | result after this step | a after squaring | b after shift |
|---|---|---|---|---|---|
| 1 | 111 | 1 → `result = 1×5` | 5 | 25 | 3 (011) |
| 2 | 011 | 1 → `result = 5×25` | 125 | 625 | 1 (001) |
| 3 | 001 | 1 → `result = 125×625` | 78125 | 390625 | 0 |

Loop ends when `b = 0`. **Result: `5^7 = 78125`** — matches direct computation.

Every bit was `1` in this example, so all three squared values (`5, 25, 625` = `5^1, 5^2, 5^4`) got multiplied into `result`. If a bit were `0` (as in the `8 = 1000₂` case), the squaring step still runs every iteration to build up `a`, but the multiply-into-`result` step is skipped for that bit.

### General binary-exponent expansion (from the earlier `5^1000` discussion)

For any exponent `b`, write `b` in binary: `b = Σ (bit_i × 2^i)`. Then:

```
a^b = a^(Σ bit_i·2^i) = Π (a^(2^i))^bit_i
```

i.e. multiply together `a^(2^i)` for every bit position `i` where `bit_i = 1`, skipping positions where the bit is `0`. This is exactly what the while-loop computes incrementally by repeated squaring.

---

*In progress — more math topics from live sessions to be added here as they come up.*
