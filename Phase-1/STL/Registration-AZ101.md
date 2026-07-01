---
tags: [stl, map]
date: 2026-07-01
status: Solved
url: https://maang.in/problems/Registration-AZ101-360
---

# Registration AZ101

## Problem

Register users: first time → "OK", nth time (n>0) → print name+n.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int n; cin >> n;
    map<string, int> cnt;

    while(n--) {
        string s; cin >> s;
        if(cnt[s] == 0) cout << "OK" << endl;
        else cout << s << cnt[s] << endl;
        cnt[s]++;
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
