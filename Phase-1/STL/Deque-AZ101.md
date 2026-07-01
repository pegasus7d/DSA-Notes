---
tags: [stl, deque]
date: 2026-07-01
status: Solved
url: https://maang.in/problems/Deque-AZ101-353
---

# Deque AZ101

## Problem

`insertback x`, `eraseback`, `insertfront x`, `erasefront`, `printfront`, `printback`, `print x` (0-indexed or 0).

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int q; cin >> q;
    deque<int> dq;

    while(q--) {
        string op; cin >> op;
        if(op == "insertback") {
            int x; cin >> x; dq.push_back(x);
        } else if(op == "eraseback") {
            if(!dq.empty()) dq.pop_back();
        } else if(op == "insertfront") {
            int x; cin >> x; dq.push_front(x);
        } else if(op == "erasefront") {
            if(!dq.empty()) dq.pop_front();
        } else if(op == "printfront") {
            cout << (dq.empty() ? 0 : dq.front()) << endl;
        } else if(op == "printback") {
            cout << (dq.empty() ? 0 : dq.back()) << endl;
        } else {
            int x; cin >> x;
            cout << ((int)x < (int)dq.size() ? dq[x] : 0) << endl;
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
