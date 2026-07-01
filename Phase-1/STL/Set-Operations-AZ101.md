---
tags: [stl, set]
date: 2026-07-01
status: Solved
url: https://maang.in/problems/Set-Operations-AZ101-372
---

# Set Operations AZ101

## Problem

Given sets A and B, print union, intersection, and A-B in sorted order.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int n, m; cin >> n >> m;
    set<int> a, b;
    for(int i = 0; i < n; i++) { int x; cin >> x; a.insert(x); }
    for(int i = 0; i < m; i++) { int x; cin >> x; b.insert(x); }

    set<int> u = a;
    for(int x : b) u.insert(x);
    for(int x : u) cout << x << " ";
    cout << endl;

    for(int x : a) if(b.count(x)) cout << x << " ";
    cout << endl;

    for(int x : a) if(!b.count(x)) cout << x << " ";
    cout << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    int t; cin >> t;
    while(t--) solve();
    return 0;
}
```
