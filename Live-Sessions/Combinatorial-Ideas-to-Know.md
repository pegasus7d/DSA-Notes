---
tags: [live-session, combinatorics, stars-and-bars, permutations, committees]
status: In Progress
source: https://maang.in/live-sessions/Combinatorial-Ideas-to-Know-1290
---

# Combinatorial Ideas to Know

Note: this session opened with modular exponentiation as prerequisite math — that content lives in [[Maths]] instead, since it's number theory rather than combinatorics proper.

## Problem 1 — Total minus Invalid

A competition has 3 rounds: round 1 has 4 challenges, round 2 has 5, round 3 has 3. Restriction: if a contestant picks challenge 1 in round 1, they cannot pick challenge 2 in round 2. How many different ordered sequences of challenges can a contestant attempt?

**Technique: Total − Invalid.**

- Total unrestricted: `4 × 5 × 3 = 60`
- Invalid (challenge 1 in round 1 AND challenge 2 in round 2): fixes round 1 and round 2 choices, round 3 is free → `1 × 1 × 3 = 3`
- **Answer: `60 − 3 = 57`**

General idea: when a restriction only forbids one specific combination (rather than being complex to count directly), it's often easier to count everything and subtract the invalid cases than to count valid cases directly.

## Problem 2 — Glue/Block Method (with case-splitting)

8 distinct books; two specific books A and B must have **at least 2** other books between them. How many arrangements?

**Technique: Total − (0-between case) − (1-between case).**

- Total unrestricted: `8! = 40320`
- **0 books between (A, B adjacent):** glue `[AB]` into a single block → 7 units to arrange → `7! × 2!` (the `×2!` accounts for `AB` vs `BA` order within the block) = `5040 × 2 = 10080`
- **1 book between:** choose which 1 of the remaining 6 books sits between A and B (`6C1`), account for A/B order (`2!`), then treat the resulting `[A_B]` group plus the other 5 books as 6 units (`6!`) → `6 × 2 × 720 = 8640`
- **Answer: `40320 − 10080 − 8640 = 21600`**

The "glue" trick (treating adjacent-required elements as one merged block, arranging the reduced set of units, then multiplying by internal orderings) is the general technique for "must be together" or "must be within N of each other" problems.

## Problem 3 — Split by Cases on a Restricted Subset

From 15 candidates, form a committee of 6. There are 3 particular candidates such that **at most 1** of them may be on the committee. How many ways?

**Technique: split into cases by how many of the "restricted" 3 are chosen, then sum.**

Split the 15 into the 3 particular candidates and the other 12:

- 0 of the 3 chosen: `3C0 × 12C6 = 1 × 924 = 924`
- 1 of the 3 chosen: `3C1 × 12C5 = 3 × 792 = 2376`
- **Answer: `924 + 2376 = 3300`**

`nCr = n! / ((n−r)! × r!)` used throughout.

## Problem 4 — Glue Method with Repeated Letters

Word "BALLOON" (letters: B, A, L, L, O, O, N). How many distinct arrangements if the two O's must appear together?

**Technique: glue + divide by repeats.**

Glue the two O's into one block `[OO]`. Units to arrange: `B, A, L, L, [OO], N` — 6 units, but the two L's are still identical, so divide by `2!` for that repetition.

**Answer: `6! / 2! = 720 / 2 = 360`**

## Problem 5 — Stars and Bars

Find the number of valid integer solutions to `x1 + x2 + x3 + x4 + x5 = 40`, where every `xi ≥ 3`.

### The general theorem

For `x1 + x2 + ... + xr = N` with `xi ≥ 0`, the number of solutions is:

**`C(N + r − 1, r − 1)`**

**Why not just "choose 2 of the gaps between stars"?** With `N` stars there are `N−1` gaps *between* them, and it's tempting to think you just choose `r−1` of those gaps for the bars. This undercounts, because **multiple bars can occupy the same position** (representing a variable equal to 0), and bars sitting at the very start or end aren't "between" two stars at all. Example with `x1+x2+x3=4`: the arrangement `|| ****` means `x1=0, x2=0, x3=4` — both bars sit together before any stars, which isn't a "gap between stars" case at all.

**The fix:** instead of placing bars into gaps, arrange all `N + (r−1)` symbols (stars and bars combined) as one sequence, and choose which `r−1` of those positions hold bars (the rest are stars). This naturally allows bars to be adjacent to each other or at either end, correctly capturing every case including `xi = 0`.

Example: `x1+x2+x3=4` → `4` stars `+` `2` bars `=` `6` symbols total → choose 2 of the 6 positions for the bars → `C(6, 2)`.

### Applying to this problem

Since every `xi ≥ 3` (not `≥ 0`), substitute `yi = xi − 3` so `yi ≥ 0`:

```
x1 + x2 + x3 + x4 + x5 = 40
(y1+3) + (y2+3) + (y3+3) + (y4+3) + (y5+3) = 40
y1 + y2 + y3 + y4 + y5 = 40 − 15 = 25
```

Now apply the standard formula with `N=25`, `r=5`:

**`C(25 + 5 − 1, 5 − 1) = C(29, 4) = 23751`**

*(This final substitution and computation follows directly from the stated formula — worth double-checking against the recording's actual final board if the exact number matters, since it wasn't independently re-confirmed via a later screenshot.)*

## Computing nCr — Three Approaches

After the worked problems, the session covers how to actually *compute* `nCr` in code, three ways:

### 1. Direct (no modulo, precise value)

Multiplicative formula, derived from `nCr = n! / (r! × (n-r)!)` by cancelling `(n-r)!`:

```
nCr = [n × (n-1) × ... × (n-r+1)] / r! = Π_{i=0}^{r-1} (n-i) / r!
```

```cpp
long long ncr(long long n, long long r) {
    r = min(r, n - r);

    long long ans = 1;                // starts at 1, representing C(n,0)
    for (long long i = 0; i < r; i++) {
        ans = ans * (n - i);
        ans = ans / (i + 1);
    }
    return ans;
}
```

**Bug fixed:** `ans` (and `n`/`r`) need to be `long long`, not `int` — `C(n,r)` grows fast enough that even moderately-sized `n` (e.g. `C(34,17) ≈ 2.33e9`) overflows a 32-bit `int`, silently producing a wrong value despite the whole point of this method being to "avoid overflow."

**Why multiply-then-divide immediately (not compute the full numerator, then divide by `r!` at the end)?** Because of the recurrence `C(n, k+1) = C(n, k) × (n-k) / (k+1)` — at every step, `ans` holds a genuine binomial coefficient (`C(n,0)`, then `C(n,1)`, then `C(n,2)`, ...), and each of those divisions is guaranteed to be exact (binomial coefficients are always integers). This keeps intermediate values small and avoids overflow that a naive "compute full numerator product first" approach would risk for large `n`.

### 2. Table Method

Mentioned as a second approach — build a Pascal's-triangle-style DP table using `C(n,r) = C(n-1,r-1) + C(n-1,r)`. (Not captured in detail from the whiteboard in this pass — standard technique, useful when you need many `nCr` values up to a bounded `n`, without division at all.)

### 3. Precompute Factorial (with modulo)

Best for competitive programming where answers are required `% mod` and there are many queries. Relies on [[Maths#2. Modular Multiplicative Inverse — `inv(a, mod)`|inv()]], which itself relies on binary exponentiation — this is the payoff for learning that technique first in the session.

```cpp
long long mod = 1e9 + 7;

long long fact[1000100];
void pre() {
    fact[0] = 1;
    for (int i = 1; i <= 1000000; i++) {
        fact[i] = (fact[i-1] * i) % mod;
    }
}

long long ncr(int n, int r) {
    long long num = fact[n];
    long long den = (fact[n-r] * fact[r]) % mod;
    long long ans = (num * inv(den, mod)) % mod;
    return ans;
}
```

**Bug fixed:** `fact`/`mod`/`ans` need to be `long long`. With `int`, `fact[i-1] * i` and `fact[n-r] * fact[r]` are ~1e9-scale values multiplied before the `% mod`, overflowing 32-bit `int` (products up to ~1e15–1e18) and silently corrupting every `nCr` result for all but the smallest inputs.

Factorials are precomputed once in `O(max_n)`, so every subsequent `nCr` query is `O(log mod)` — the cost of the `inv()` call. Since normal division isn't defined under a modulus, `inv(den, mod)` (modular multiplicative inverse, via Fermat's Little Theorem) stands in for dividing by `den`.

---

*Note compiled from whiteboard screenshots taken at intervals while watching the recording. Session in progress — additional topics to be appended as captured.*
