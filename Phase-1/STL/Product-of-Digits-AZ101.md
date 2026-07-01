---
tags: [stl, math, modular-arithmetic]
date: 2026-07-01
status: Solved
url: https://maang.in/problems/Product-of-Digits-AZ101-346
---

# Product of Digits AZ101

## Problem

Given N digits, print their product mod 10^9+7.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long
const int MOD = 1e9 + 7;

void solve() {
    int n; cin >> n;
    int prod = 1;
    for(int i = 0; i < n; i++) {
        int d; cin >> d;
        prod = (prod * d) % MOD;
    }
    cout << prod << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    int t; cin >> t;
    while(t--) solve();
    return 0;
}
```
