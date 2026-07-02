---
tags: [bit-manipulation, greedy]
date: 2026-07-02
status: Solved
url: https://maang.in/problems/Bitwise-Operations-52
---

# Bitwise Operations

## Problem

Array of N integers. Operation: pick two elements, replace with (a OR b, a AND b). Maximize sum of squares.

## Key Insight ⚡

For any bit b, `cnt[b]` (count of elements with that bit set) is **invariant** under the operation. So we can freely rearrange which elements hold which bits.

To maximize sum of squares: concentrate bits into as few elements as possible. Give bit b to the first `cnt[b]` elements (0-indexed).

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int n; cin >> n;
    vector<int> a(n);
    for(int i = 0; i < n; i++) cin >> a[i];

    vector<int> cnt(20, 0);
    for(int i = 0; i < n; i++)
        for(int b = 0; b < 20; b++)
            if((a[i] >> b) & 1) cnt[b]++;

    vector<int> opt(n, 0);
    for(int b = 0; b < 20; b++)
        for(int i = 0; i < cnt[b]; i++)
            opt[i] |= (1LL << b);

    int ans = 0;
    for(int i = 0; i < n; i++) ans += opt[i] * opt[i];
    cout << ans << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    int t; cin >> t;
    while(t--) solve();
    return 0;
}
```
