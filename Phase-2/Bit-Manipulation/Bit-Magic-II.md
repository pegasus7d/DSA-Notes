---
tags: [bit-manipulation, builtins]
date: 2026-07-02
status: Solved
url: https://maang.in/problems/Bit-Magic-II-873
---

# Bit Magic II

## Problem

For a 64-bit integer n, answer 6 queries: binary representation, MSB position, rightmost set bit, is power of 2, biggest power-of-2 divisor, smallest power of 2 ≥ n (k > 0).

## Key Insight ⚡

Cast to `unsigned long long` for all bit operations — handles negatives (two's complement) correctly.

| Query | Formula |
|-------|---------|
| 64-bit binary | print bits 63..0 of `(unsigned long long)n` |
| MSB position | `63 - __builtin_clzll(un)`, or -1 if n=0 |
| Rightmost set bit | `__builtin_ctzll(un)`, or -1 if n=0 |
| Power of 2 (x>0) | `n > 0 && (n & (n-1)) == 0` |
| Biggest 2^k divisor | `n & -n`, or -1 if n=0 |
| Smallest 2^k ≥ n (k>0) | if n≤1 → 2; if already power of 2 → n; else `1LL << (64 - clzll(un))` |

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    long long n; cin >> n;
    unsigned long long un = (unsigned long long)n;

    for(int b = 63; b >= 0; b--) cout << ((un >> b) & 1);
    cout << "\n";

    if(n == 0) cout << -1 << "\n";
    else cout << 63 - __builtin_clzll(un) << "\n";

    if(n == 0) cout << -1 << "\n";
    else cout << __builtin_ctzll(un) << "\n";

    cout << (n > 0 && (n & (n - 1)) == 0 ? 1 : 0) << "\n";

    if(n == 0) cout << -1 << "\n";
    else cout << (n & -n) << "\n";

    if(n <= 1) cout << 2 << "\n";
    else if((n & (n - 1)) == 0) cout << n << "\n";
    else cout << (1LL << (64 - __builtin_clzll(un))) << "\n";
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);
    int t; cin >> t;
    while(t--) solve();
    return 0;
}
```
