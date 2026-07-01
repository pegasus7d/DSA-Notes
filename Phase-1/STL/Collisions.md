---
tags: [stl, map, math]
date: 2026-07-01
status: Solved
url: https://maang.in/problems/Collisions-82
---

# Collisions

## Problem

N balls on X-axis (pos Xi, speed Ui moving +Y), M balls on Y-axis (pos Yj, speed Vj moving +X). Each ball collides at most once. Count total collisions.

## Key Insight ⚡

Ball from X-axis at (Xi, Ui*t) meets ball from Y-axis at (Vj*t, Yj) when:
`Xi = Vj*t` and `Ui*t = Yj` → `Xi*Ui = Yj*Vj`

For each product value P: collisions += min(count_X[P], count_Y[P]).

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int n, m; cin >> n >> m;
    map<int,int> xprod, yprod;

    for(int i = 0; i < n; i++) {
        int x, u; cin >> x >> u;
        xprod[x * u]++;
    }
    for(int i = 0; i < m; i++) {
        int y, v; cin >> y >> v;
        yprod[y * v]++;
    }

    int ans = 0;
    for(auto& [p, cx] : xprod) {
        if(yprod.count(p)) ans += min(cx, yprod[p]);
    }
    cout << ans << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    int t; cin >> t;
    while(t--) solve();
    return 0;
}
```
