---
tags: [stl, multimap]
date: 2026-07-01
status: Solved
url: https://maang.in/problems/Multimap-AZ101-363
---

# Multimap AZ101

## Problem

`add x y`, `erase x` (first entry), `eraseall x`, `print x` (first entry or 0).

## Key

`multimap::find` returns first entry. `mm.erase(name)` deletes all. `mm.erase(it)` deletes one.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int q; cin >> q;
    multimap<string, int> mm;

    while(q--) {
        string op; cin >> op;
        if(op == "add") {
            string name; int y; cin >> name >> y;
            mm.insert({name, y});
        } else if(op == "erase") {
            string name; cin >> name;
            auto it = mm.find(name);
            if(it != mm.end()) mm.erase(it);
        } else if(op == "eraseall") {
            string name; cin >> name;
            mm.erase(name);
        } else {
            string name; cin >> name;
            auto it = mm.find(name);
            cout << (it != mm.end() ? it->second : 0) << endl;
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
