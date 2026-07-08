---
tags: [maths, modular-arithmetic, modular-inverse]
date: 2026-07-09
status: Solved
url: https://maang.in/problems/Solve-the-Equation-73
---

# Solve the Equation

## Problem

Given an equation of the form `(a op1 b op2 c) mod p`, where `op1`/`op2` ∈ `{+, -, *, /}` and standard math precedence applies (`*`/`/` before `+`/`-`), compute its value in `[0, p-1]`. Guaranteed `p` is prime and `a, b, c` are all coprime to `p` (so modular division is always well-defined via the modular inverse).

**Input Format**
- Line 1: T — number of test cases
- Per test case: one line, literally formatted as `(a op1 b op2 c) mod p`

**Output Format**
- Per test case: the equation's value mod `p`

**Constraints**
- 1 ≤ T ≤ 10^5
- 1 ≤ a, b, c, p ≤ 10^9

## Sample Test Cases

**Input 1:** `(1 + 2 / 1) mod 3` — here `2 / 1 = 2`, so `1 + 2 = 3`, `3 mod 3 = 0`
**Output 1:** `0`

**Input 2:** `(2 * 3 - 8) mod 5` — `2*3 - 8 = -2`, `-2 mod 5 = 3`
**Output 2:** `3`

## Key Insight ⚡

**Parsing**: the line's whitespace-delimited tokens are `"(a", "op1", "b", "op2", "c)", "mod", "p"` (7 tokens) — read them all as strings via `cin >>`, then strip the leading `(` from the first and trailing `)` from the fifth via `substr`.

**Precedence with only 3 operands**: there are exactly two "shapes" possible — either evaluate left-to-right (`(a op1 b) op2 c`), or evaluate the middle pair first (`a op1 (b op2 c)`) when `op2` is `*`/`/` and `op1` is `+`/`-` (i.e., `op2` binds tighter). Every other combination (both `+`/`-`, both `*`/`/`, or `op1` being `*`/`/` regardless of `op2`) reduces correctly to plain left-to-right evaluation.

**Modular division**: since `p` is prime, `x / y mod p = x * y^(p-2) mod p` (Fermat's little theorem gives the modular inverse). A single `apply(x, op, y, p)` helper handles all four operators uniformly, always reducing operands mod `p` (with a `+p` guard for negative results) first.

Verified against an independent brute force using exact `__int128` rational arithmetic (numerator/denominator, no premature modular reduction) across 20,000 random trials spanning all 16 `op1`×`op2` combinations — 0 mismatches.

## Complexity

O(log p) per test case, dominated by the modular inverse's exponentiation.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long
#define endl '\n'

long long modpow(long long base, long long exp, long long mod) {
    base %= mod; if (base < 0) base += mod;
    long long result = 1;
    while (exp > 0) {
        if (exp & 1) result = (result * base) % mod;
        base = (base * base) % mod;
        exp >>= 1;
    }
    return result;
}

long long inv(long long x, long long p) { return modpow(x, p - 2, p); }

long long apply(long long x, char op, long long y, long long p) {
    x %= p; if (x < 0) x += p;
    y %= p; if (y < 0) y += p;
    if (op == '+') return (x + y) % p;
    if (op == '-') return ((x - y) % p + p) % p;
    if (op == '*') return (x * y) % p;
    return (x * inv(y, p)) % p;
}

void solve(){
    string t0, t1, t2, t3, t4, t5, t6;
    cin >> t0 >> t1 >> t2 >> t3 >> t4 >> t5 >> t6;
    long long a = stoll(t0.substr(1));
    char op1 = t1[0];
    long long b = stoll(t2);
    char op2 = t3[0];
    long long c = stoll(t4.substr(0, t4.size() - 1));
    long long p = stoll(t6);

    long long ans;
    if ((op2 == '*' || op2 == '/') && (op1 == '+' || op1 == '-')) {
        long long bc = apply(b, op2, c, p);
        ans = apply(a, op1, bc, p);
    } else {
        long long ab = apply(a, op1, b, p);
        ans = apply(ab, op2, c, p);
    }
    cout << ans << endl;
}

signed main(){
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);cout.tie(NULL);
    int _t;cin>>_t;while(_t--)
    solve();
}
```
