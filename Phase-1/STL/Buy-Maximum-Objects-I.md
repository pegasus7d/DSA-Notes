---
tags: [stl, greedy, sorting]
date: 2026-07-01
status: Solved
url: https://maang.in/problems/Buy-Maximum-Objects-I-75
---

# Buy Maximum Objects I

## Problem

Given N objects with prices, budget M. Buy maximum number of objects.

## Intuition

Sort ascending, greedily pick cheapest first until budget runs out.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int n, m; cin >> n >> m;
    vector<int> a(n);
    for(int i = 0; i < n; i++) cin >> a[i];
    sort(a.begin(), a.end());

    int count = 0;
    for(int i = 0; i < n && m >= a[i]; i++) {
        m -= a[i];
        count++;
    }
    cout << count << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    int t; cin >> t;
    while(t--) solve();
    return 0;
}
```
