---
tags: [stl, order-statistics-tree, pb_ds]
date: 2026-07-01
status: Solved
url: https://maang.in/problems/Indexed-Set-367
---

# Indexed Set

## Problem

Ordered set with: `add x`, `remove x`, `find k` (k-th element, 0-indexed, or -1), `findpos x` (position of x or where it would go).

## Key Insight ⚡

Use policy-based `ordered_set` from `ext/pb_ds`:
- `find_by_order(k)` → iterator to k-th element
- `order_of_key(x)` → number of elements strictly less than x (= position of x or insertion point)

## Code

```cpp
#include <bits/stdc++.h>
#include <ext/pb_ds/assoc_container.hpp>
#include <ext/pb_ds/tree_policy.hpp>
using namespace std;
using namespace __gnu_pbds;
#define int long long

typedef tree<int, null_type, less<int>, rb_tree_tag, tree_order_statistics_node_update> ordered_set;

void solve() {
    int q; cin >> q;
    ordered_set os;

    while(q--) {
        string op; cin >> op;
        if(op == "add") {
            int x; cin >> x; os.insert(x);
        } else if(op == "remove") {
            int x; cin >> x; os.erase(x);
        } else if(op == "find") {
            int x; cin >> x;
            if(x < (int)os.size()) cout << *os.find_by_order(x) << endl;
            else cout << -1 << endl;
        } else {
            int x; cin >> x;
            cout << os.order_of_key(x) << endl;
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
