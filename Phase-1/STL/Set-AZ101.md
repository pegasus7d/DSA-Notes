---
tags: [stl, set]
date: 2026-07-01
status: Solved
url: https://maang.in/problems/Set-AZ101-356
---

# Set AZ101

## Problem

`add x`, `erase x`, `find x` (YES/NO), `print` (sorted), `empty`.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int q; cin >> q;
    set<int> s;

    while(q--) {
        string op; cin >> op;
        if(op == "add") {
            int x; cin >> x;
            s.insert(x);
        } else if(op == "erase") {
            int x; cin >> x;
            s.erase(x);
        } else if(op == "find") {
            int x; cin >> x;
            cout << (s.count(x) ? "YES" : "NO") << endl;
        } else if(op == "print") {
            for(int x : s) cout << x << " ";
            cout << endl;
        } else {
            s.clear();
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
