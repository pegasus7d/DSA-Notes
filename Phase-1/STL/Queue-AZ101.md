---
tags: [stl, queue]
date: 2026-07-01
status: Solved
url: https://maang.in/problems/Queue-AZ101-349
---

# Queue AZ101

## Problem

Q queries: `add x` (enqueue), `remove` (dequeue front if not empty), `print` (front or 0).

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int q; cin >> q;
    queue<int> qu;

    while(q--) {
        string op; cin >> op;
        if(op == "add") {
            int x; cin >> x;
            qu.push(x);
        } else if(op == "remove") {
            if(!qu.empty()) qu.pop();
        } else {
            cout << (qu.empty() ? 0 : qu.front()) << endl;
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
