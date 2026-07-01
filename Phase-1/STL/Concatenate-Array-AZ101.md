---
tags: [stl, set, lis, math]
date: 2026-07-01
status: Solved
url: https://maang.in/problems/Concatenate-Array-AZ101-373
---

# Concatenate Array AZ101

## Problem

LIS of array A concatenated to itself N times (strictly increasing).

## Key Insight ⚡

LIS of A repeated N times = number of **distinct elements** in A.

**Why:** We can pick the distinct values in sorted order across copies (one per copy if needed). Max possible is D distinct values; and it's always achievable since each copy contains all D values.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int n; cin >> n;
    set<int> distinct;
    for(int i = 0; i < n; i++) {
        int x; cin >> x;
        distinct.insert(x);
    }
    cout << distinct.size() << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    int t; cin >> t;
    while(t--) solve();
    return 0;
}
```
