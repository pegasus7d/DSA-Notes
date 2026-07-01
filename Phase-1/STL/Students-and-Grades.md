---
tags: [stl, set]
date: 2026-07-01
status: Solved
url: https://maang.in/problems/Students-and-Grades-89
---

# Students and Grades

## Problem

Section A has N students with grades. Section B students enter one by one. For each B student: if someone in the room has same grade → YES, else NO. B students also join the room after entering.

## Intuition

Use a set. Pre-fill with A grades. For each B student: check count(grade) > 0, print YES/NO, then insert into set.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int n, m; cin >> n >> m;
    set<int> s;
    for(int i = 0; i < n; i++) {
        int x; cin >> x;
        s.insert(x);
    }

    for(int i = 0; i < m; i++) {
        int x; cin >> x;
        cout << (s.count(x) ? "YES" : "NO") << endl;
        s.insert(x);
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
