---
tags: [prefix-sums, modular-arithmetic]
date: 2026-07-03
status: Solved
url: https://maang.in/problems/LR-Sum-Query-18?resourceUrl=cs71-cp481-pl3270-rs18
---

# LR Sum Query

## Problem

Given an array of N integers and Q queries. For each query [L, R], find (A[L] + A[L+1] + ... + A[R]) % (10^9 + 7).

**Constraints:** 1 ≤ N, Q ≤ 10^6, −10^9 ≤ A[i] ≤ 10^9

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long
const int MOD = 1e9 + 7;

void solve() {
    int n, q; cin >> n >> q;
    vector<int> pre(n + 1, 0);
    for (int i = 1; i <= n; i++) {
        int a; cin >> a;
        pre[i] = (pre[i-1] + a % MOD + MOD) % MOD;
    }
    while (q--) {
        int l, r; cin >> l >> r;
        cout << (pre[r] - pre[l-1] + MOD) % MOD << endl;
    }
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);
    solve();
    return 0;
}
```
