---
tags: [live-session, number-theory, gcd, lcm, sieve, primes, divisors, spf]
status: In Progress
source: https://maang.in/live-sessions/Number-Theory-Foundations-1302
---

# Number Theory Foundations

## 1. GCD — Euclidean Algorithm

```cpp
int gcd(int a, int b) {
    if (a == 0) return b;
    return gcd(b % a, a);
}
```

Recursive relation: `gcd(a, b) = gcd(b % a, a)`, with base case `gcd(0, b) = b`. Each call strictly shrinks the smaller argument, so this terminates in `O(log(min(a,b)))` steps.

### GCD of an array

```cpp
int ans = arr[0];
for (int i = 1; i < n; i++) {
    ans = gcd(ans, arr[i]);
}
```

GCD is associative, so `gcd(a,b,c,...) = gcd(gcd(gcd(a,b),c),...)` — fold pairwise across the array, carrying the running GCD forward.

## 2. LCM

```cpp
int lcm(int a, int b) {
    return a / gcd(a, b) * b;
}
```

Relationship: `lcm(a,b) = (a × b) / gcd(a,b)`. Written as `a / gcd(a,b) * b` (divide first, then multiply) rather than `a * b / gcd(a,b)`, to reduce overflow risk on large inputs.

### LCM of an array

Same fold pattern as GCD:

```cpp
int arrgcd = arr[0];
int arrlcm = arr[0];
for (int i = 1; i < n; i++) {
    arrgcd = gcd(arrgcd, arr[i]);
    arrlcm = lcm(arrlcm, arr[i]);
}
```

Verified with test case `[8, 24, 10]` → `gcd = 2`, `lcm = 120` (`lcm(8,24)=24`, then `lcm(24,10)=120`).

### Gotcha: LCM under a modulus

**`gcd(a % m, b % m)` is WRONG** — do not use it as a shortcut for computing `gcd`/`lcm` under a modulus. Reducing values mod `m` first destroys the actual number-theoretic structure GCD depends on; the GCD of the reduced residues has no reliable relationship to the true GCD of the original numbers.

The correct approach: compute the real (unreduced) `gcd(a,b)` first, then apply the modulus carefully to the final `a × b` product — likely via the modular inverse (`inv()`, see [[Maths]]) for the division-by-gcd step, since normal division isn't valid under a modulus. *(The board was mid-derivation of the exact corrected formula when this was captured — worth re-checking the recording for the final form if you need it precisely.)*

## 3. Divisors of a Single Number — `O(√x)`

```cpp
vector<int> divisors(int x) {  // O(sqrt(x))
    vector<int> ans;
    for (int i = 1; i * i <= x; i++) {
        if (x % i == 0) {
            ans.push_back(i);
            if (i != x / i)
                ans.push_back(x / i);
        }
    }
    return ans;
}
```

Divisors come in pairs `(i, x/i)`, so only checking up to `√x` is enough — beyond that you'd just find the same pairs in reverse. The `if (i != x/i)` guard avoids double-counting the square root itself when `x` is a perfect square (e.g. `x=16`: without the guard, `i=4` would push `4` twice).

## 4. Precomputing Divisors for All Numbers 1..N — `O(N log N)`

When you need divisors for *every* number up to `N` (not just one), it's faster to sieve once than call the `O(√x)` version `N` times:

```cpp
vector<int> divisors[n + 1];
for (int i = 1; i <= n; i++) {
    for (int j = i; j <= n; j += i) {
        divisors[j].push_back(i);
    }
}
```

**Idea:** for each `i` from `1` to `N`, append `i` to the divisor list of every multiple of `i` (`i, 2i, 3i, ...`). Total work is the harmonic sum `N/1 + N/2 + N/3 + ... ≈ O(N log N)`. Divisors for each `j` come out in increasing order of `i` automatically.

## 5. Sieve of Eratosthenes — Primality up to N

```cpp
int is_prime[100100];
void init() {
    for (int i = 2; i <= 100000; i++) {
        is_prime[i] = 1;
    }
    for (int i = 2; i <= 100000; i++) {
        if (is_prime[i]) {
            for (int j = 2 * i; j <= 100000; j += i) {
                is_prime[j] = 0;
            }
        }
    }
}
```

Initialize everything as potentially prime, then for each `i` still marked prime, cross off all its multiples (`2i, 3i, ...`) as composite. Runs in `O(n log log n)`.

## 6. SPF (Smallest Prime Factor) Array + Fast Prime Factorization

**Motivation:** `12 = 2² × 3` — want to factorize numbers quickly, especially when factorizing many numbers up to some bound.

### Building the SPF array

```cpp
int spf[100100];
void init() {
    for (int i = 2; i <= MAXN; i++) {
        spf[i] = i;
    }
    for (int i = 2; i <= MAXN; i++) {
        if (spf[i] == i) {                          // i is prime (untouched by any smaller prime)
            for (int j = 2 * i; j <= MAXN; j += i) {
                if (spf[j] == j) {                   // only claim j if no smaller prime has already
                    spf[j] = i;
                }
            }
        }
    }
}
```

Initialize `spf[i] = i` for all `i`. Process `i` in increasing order; if `spf[i]` is still `i`, then `i` is prime (nothing smaller claimed it), so mark every multiple `j` of `i` with `spf[j] = i` — but only if `j` hasn't already been claimed by a smaller prime. Since primes are processed in increasing order, the first one to reach a composite number is guaranteed to be its smallest prime factor.

Verified against test output: `16:2`, `17:17`, `18:2`, `19:19`, `20:2`, `21:3`, `22:2`, `23:23` — all correct (e.g. `21 = 3×7`, smallest factor `3`; primes map to themselves).

### Fast factorization using SPF

```cpp
vector<int> primeFactors(int x) {
    vector<int> factors;
    while (x != 1) {
        factors.push_back(spf[x]);   // spf[x] is guaranteed to be prime
        x = x / spf[x];
    }
    return factors;
}
```

Repeatedly divide out the smallest prime factor and record it, until `x` reaches `1`. Since `x` shrinks by at least a factor of 2 each step, this runs in `O(log x)` — much faster than trial division per query when factorizing many numbers.

**Example:** `x=12` → `spf[12]=2` push 2, `x=6` → `spf[6]=2` push 2, `x=3` → `spf[3]=3` push 3, `x=1` stop → `[2,2,3]`, matching `12 = 2²×3`.

To get grouped exponents (e.g. `{2:2, 3:1}` instead of a flat list), count consecutive equal values in the output, or accumulate directly into a `map<int,int>`.

---

*Note compiled from whiteboard/IDE screenshots taken at intervals while watching the recording. Session in progress — additional topics to be appended as captured.*
