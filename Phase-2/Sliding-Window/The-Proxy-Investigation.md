---
tags: [sliding-window, two-pointers, form-3-multisequence, subsequence]
date: 2026-07-03
status: Solved
url: https://maang.in/problems/The-Proxy-Investigation-1143?resourceUrl=cs120-cp545-pl5062-rs1143
---

# The Proxy Investigation

## Problem

Given arrays A (length N) and B (length M), check if B is a subsequence of A (elements in same relative order, not necessarily consecutive).

**Constraints:** 1 ≤ T ≤ 10, 1 ≤ N, M ≤ 10^4, 1 ≤ A[i], B[i] ≤ 10^9

## Key Insight ⚡

**Form 3 (Multi-sequence):** One pointer through A, one through B. Advance B pointer when A[i] == B[j]. If j reaches m → YES.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int n, m; cin >> n >> m;
    vector<int> a(n), b(m);
    for (int i = 0; i < n; i++) cin >> a[i];
    for (int i = 0; i < m; i++) cin >> b[i];

    int j = 0;
    for (int i = 0; i < n && j < m; i++)
        if (a[i] == b[j]) j++;

    cout << (j == m ? "YES" : "NO") << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);
    int t; cin >> t;
    while(t--) solve();
    return 0;
}
```
