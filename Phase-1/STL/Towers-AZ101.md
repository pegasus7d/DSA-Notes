---
tags: [stl, multiset, greedy, patience-sort]
date: 2026-07-01
status: Solved
url: https://maang.in/problems/Towers-AZ101-362
---

# Towers AZ101

## Problem

Place blocks on towers (upper block must be strictly smaller than lower). Process in order. Minimize number of towers.

## Intuition

Greedy: for each block x, find the tower whose top is the smallest value strictly greater than x (upper_bound(x) in a multiset of tops). Place x there. If none, new tower.

## Key Insight ⚡

`upper_bound(x)` finds smallest element > x — perfect for "just larger" greedy.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int n; cin >> n;
    multiset<int> tops;

    for(int i = 0; i < n; i++) {
        int x; cin >> x;
        auto it = tops.upper_bound(x);
        if(it != tops.end()) {
            tops.erase(it);
        }
        tops.insert(x);
    }
    cout << tops.size() << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    int t; cin >> t;
    while(t--) solve();
    return 0;
}
```
