---
tags: [stl, map]
date: 2026-07-01
status: Solved
url: https://maang.in/problems/Map-AZ101-359
---

# Map AZ101

## Problem

Q queries: `add x y` (insert/update), `erase x`, `print x` (value or 0).

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int q; cin >> q;
    map<string, int> mp;

    while(q--) {
        string op; cin >> op;
        if(op == "add") {
            string name; int y; cin >> name >> y;
            mp[name] = y;
        } else if(op == "erase") {
            string name; cin >> name;
            mp.erase(name);
        } else {
            string name; cin >> name;
            cout << (mp.count(name) ? mp[name] : 0) << endl;
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
