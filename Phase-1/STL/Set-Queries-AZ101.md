---
tags: [stl, map, interval-merging]
date: 2026-07-01
status: Solved
url: https://maang.in/problems/Set-Queries-AZ101-358
---

# Set Queries AZ101

## Problem

Two operations: `1 x` (insert x into set), `2 x` (find smallest integer ≥ x NOT in the set).

## Intuition

Use interval merging. Maintain `map<int,int> ivl` where `ivl[l] = r` represents interval `[l, r]` (all integers from l to r are in the set).

**Insert x:** find adjacent/overlapping intervals (ending at x-1 or starting at x+1) and merge.  
**Query x:** if x falls in some interval `[l, r]`, return `r+1`. Else return `x`.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int q; cin >> q;
    map<int,int> ivl;

    while(q--) {
        int type, x; cin >> type >> x;
        if(type == 1) {
            int l = x, r = x;
            auto it = ivl.upper_bound(x);
            if(it != ivl.begin()) {
                --it;
                if(it->second >= x - 1) { l = it->first; r = max(r, it->second); it = ivl.erase(it); }
                else ++it;
            }
            while(it != ivl.end() && it->first <= x + 1) { r = max(r, it->second); it = ivl.erase(it); }
            ivl[l] = r;
        } else {
            auto it = ivl.upper_bound(x);
            if(it != ivl.begin()) {
                --it;
                if(it->second >= x) { cout << it->second + 1 << endl; continue; }
            }
            cout << x << endl;
        }
    }
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    int t; cin >> t;
    while(t--) solve();
    return 0;
}
```
