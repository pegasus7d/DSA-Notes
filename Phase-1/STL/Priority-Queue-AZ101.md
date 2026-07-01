---
tags: [stl, priority-queue]
date: 2026-07-01
status: Solved
url: https://maang.in/problems/Priority-Queue-AZ101-364
---

# Priority Queue AZ101

## Problem

`add x`, `remove` (pop max), `print` (top or 0).

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int q; cin >> q;
    priority_queue<int> pq;

    while(q--) {
        string op; cin >> op;
        if(op == "add") {
            int x; cin >> x;
            pq.push(x);
        } else if(op == "remove") {
            if(!pq.empty()) pq.pop();
        } else {
            cout << (pq.empty() ? 0 : pq.top()) << endl;
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
