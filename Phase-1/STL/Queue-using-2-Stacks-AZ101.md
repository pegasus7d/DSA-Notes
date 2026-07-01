---
tags: [stl, stack, queue]
date: 2026-07-01
status: Solved
url: https://maang.in/problems/Queue-using-2-Stacks-AZ101-350
---

# Queue using 2 Stacks AZ101

## Intuition

Two stacks s1 (inbox) and s2 (outbox). Push to s1. For pop/front: if s2 empty, move all of s1 to s2. Then s2.top() is the front.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int q; cin >> q;
    stack<int> s1, s2;

    while(q--) {
        string op; cin >> op;
        if(op == "push") {
            int x; cin >> x;
            s1.push(x);
        } else if(op == "pop") {
            if(s2.empty()) {
                while(!s1.empty()) { s2.push(s1.top()); s1.pop(); }
            }
            cout << s2.top() << endl;
            s2.pop();
        } else {
            if(s2.empty()) {
                while(!s1.empty()) { s2.push(s1.top()); s1.pop(); }
            }
            cout << s2.top() << endl;
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
