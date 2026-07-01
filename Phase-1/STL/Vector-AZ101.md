---
tags: [stl, vector]
date: 2026-07-01
status: Solved
url: https://maang.in/problems/Vector-AZ101-344
---

# Vector AZ101

## Problem

Q queries: `add x`, `remove` (last), `print x` (0-indexed, 0 if out of bounds), `clear`.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int q; cin >> q;
    vector<int> v;

    while(q--) {
        string op; cin >> op;
        if(op == "add") {
            int x; cin >> x;
            v.push_back(x);
        } else if(op == "remove") {
            if(!v.empty()) v.pop_back();
        } else if(op == "print") {
            int x; cin >> x;
            cout << ((int)x < (int)v.size() ? v[x] : 0) << endl;
        } else {
            v.clear();
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
