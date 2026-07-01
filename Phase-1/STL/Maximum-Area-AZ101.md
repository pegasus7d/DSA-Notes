---
tags: [stl, set, multiset]
date: 2026-07-01
status: Solved
url: https://maang.in/problems/Maximum-Area-AZ101-374
---

# Maximum Area AZ101

## Problem

H×W paper, N cuts (horizontal/vertical). After each cut, print max fragment area.

## Intuition

Max area = max_H_gap × max_V_gap. Each cut splits one gap into two.
Use `set<int>` for cut positions (+ boundaries 0,H/W) and `multiset<int>` for gap sizes.

For cut at pos: find neighbors (lower_bound/upper_bound), remove old gap, add two new gaps. Max gap = `*gaps.rbegin()`.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int H, W, n; cin >> H >> W >> n;

    set<int> hcuts, vcuts;
    multiset<int> hgaps, vgaps;

    hcuts.insert(0); hcuts.insert(H); hgaps.insert(H);
    vcuts.insert(0); vcuts.insert(W); vgaps.insert(W);

    while(n--) {
        char type; int pos; cin >> type >> pos;
        if(type == 'H') {
            auto it = hcuts.upper_bound(pos);
            int hi = *it; --it; int lo = *it;
            hgaps.erase(hgaps.find(hi - lo));
            hgaps.insert(pos - lo); hgaps.insert(hi - pos);
            hcuts.insert(pos);
        } else {
            auto it = vcuts.upper_bound(pos);
            int hi = *it; --it; int lo = *it;
            vgaps.erase(vgaps.find(hi - lo));
            vgaps.insert(pos - lo); vgaps.insert(hi - pos);
            vcuts.insert(pos);
        }
        cout << *hgaps.rbegin() * *vgaps.rbegin() << endl;
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
