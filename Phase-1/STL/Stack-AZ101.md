---
tags: [stl, stack]
date: 2026-07-01
status: Solved
url: https://maang.in/problems/Stack-AZ101-347
---

# Stack AZ101

## Problem

Q queries: `add x` (push), `remove` (pop if not empty), `print` (top or 0 if empty).

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int q; cin >> q;
    stack<int> st;

    while(q--) {
        string op; cin >> op;
        if(op == "add") {
            int x; cin >> x;
            st.push(x);
        } else if(op == "remove") {
            if(!st.empty()) st.pop();
        } else {
            cout << (st.empty() ? 0 : st.top()) << endl;
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
