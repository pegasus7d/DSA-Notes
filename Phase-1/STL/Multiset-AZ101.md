---
tags: [stl, multiset]
date: 2026-07-01
status: Solved
url: https://maang.in/problems/Multiset-AZ101-361
---

# Multiset AZ101

## Problem

`add x`, `erase x` (one occurrence), `eraseall x`, `find x` (YES/NO), `count x`, `print` (sorted), `empty`.

## Key

`ms.erase(it)` removes one occurrence; `ms.erase(x)` removes all.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int q; cin >> q;
    multiset<int> ms;

    while(q--) {
        string op; cin >> op;
        if(op == "add") {
            int x; cin >> x; ms.insert(x);
        } else if(op == "erase") {
            int x; cin >> x;
            auto it = ms.find(x);
            if(it != ms.end()) ms.erase(it);
        } else if(op == "eraseall") {
            int x; cin >> x; ms.erase(x);
        } else if(op == "find") {
            int x; cin >> x;
            cout << (ms.count(x) ? "YES" : "NO") << endl;
        } else if(op == "count") {
            int x; cin >> x; cout << ms.count(x) << endl;
        } else if(op == "print") {
            for(int x : ms) cout << x << " ";
            cout << endl;
        } else {
            ms.clear();
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
