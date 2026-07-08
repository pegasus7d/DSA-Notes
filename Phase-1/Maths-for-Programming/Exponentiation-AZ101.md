---
tags: [maths, modular-arithmetic, fermat]
date: 2026-07-09
status: Solved
url: https://maang.in/problems/Exponentiation-AZ101-387
---

# Exponentiation AZ101

## Problem

Given integers `A, B, C, P` where `P` is prime, compute `A^(B^C) mod P` (with the convention `0^0 = 1`).

**Input Format**
- Line 1: T — number of test cases
- Per test case: four space-separated integers `A B C P`

**Output Format**
- Per test case: `A^(B^C) mod P`

**Constraints**
- 1 ≤ T ≤ 2×10^5
- 0 ≤ A, B, C ≤ 10^9
- 2 ≤ P ≤ 10^4, P prime

## Sample Test Cases

**Input 1:** `2 4 2 7` → `2^(4^2) % 7 = 2^16 % 7 = 65536 % 7`
**Output 1:** `2`

**Input 2:** `1 6 20 5` → `1^(anything) % 5`
**Output 2:** `1`

**Input 3:** `3 1 200 3` → `3^(1^200) % 3 = 3^1 % 3`
**Output 3:** `0`

## Key Insight ⚡

The tower exponent `B^C` can be astronomically large, so it can't be computed directly — but by Fermat's little theorem, for `A` coprime to prime `P`: `A^k mod P = A^(k mod (P-1)) mod P` for any `k ≥ 0`. So reduce the exponent via `modpow(B, C, P-1)` before the outer `modpow(A, ..., P)`.

The one case Fermat's trick can't handle is when `P | A` (i.e., `A mod P == 0`): then `A^k mod P` is `0` for any `k > 0`, but must be `1` if the *actual* (unreduced) exponent `B^C` is exactly `0` — which only happens when `B == 0` and `C > 0` (since `x^0 = 1` for any `x`, including `x = 0`, by the problem's own convention). Checking "is the real exponent zero" must use the raw `B, C` values directly (a simple boolean check), never the *reduced* exponent — reducing `B^C` mod `(P-1)` first would wrongly zero out large-but-positive exponents that happen to be multiples of `P-1`.

Structuring the three cases as flat, top-level branches (rather than nesting the "exponent is zero" check inside the "`A % P == 0`" branch) makes the logic read more clearly, since "the real exponent is 0" is an independent condition that overrides everything else regardless of `A`:
```cpp
if (B == 0 && C > 0)      ans = 1 % P;                                  // real exponent B^C == 0
else if (A % P == 0)       ans = 0;                                      // real exponent > 0, A ≡ 0 mod P
else                        ans = modpow(A, modpow(B, C, P - 1), P);     // Fermat reduction applies
```

Verified against a brute force (computing `B^C` exactly for small bounded values) across 5000 random trials — 0 mismatches.

## Complexity

O(log C + log(B^C mod (P-1))) ≈ O(log(10^9)) per test case via two nested modular exponentiations.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long
#define endl '\n'

long long modpow(long long base, long long exp, long long mod) {
    if (mod == 1) return 0;
    base %= mod;
    if (base < 0) base += mod;
    long long result = 1;
    while (exp > 0) {
        if (exp & 1) result = (result * base) % mod;
        base = (base * base) % mod;
        exp >>= 1;
    }
    return result;
}

void solve(){
    long long A, B, C, P;
    cin >> A >> B >> C >> P;
    long long ans;
    if (B == 0 && C > 0) {
        ans = 1 % P;
    } else if (A % P == 0) {
        ans = 0;
    } else {
        ans = modpow(A, modpow(B, C, P - 1), P);
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
