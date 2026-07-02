---
tags: [bit-manipulation, greedy]
date: 2026-07-02
status: Solved
url: https://maang.in/problems/Max-AND-Subsequences-54
---

# Max AND Subsequences

## Problem

Find the maximum AND value among all subsequences of length exactly X.

## Key Insight ⚡

Greedy bit-by-bit from highest to lowest. Maintain `ans`. For each bit b:
- candidate = ans | (1 << b)
- Count elements where `(a[i] & candidate) == candidate`
- If count >= X, set ans = candidate

Once we commit to a set of bits, we only count elements that have ALL those bits set.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int n, x; cin >> n >> x;
    vector<int> a(n);
    for(int i = 0; i < n; i++) cin >> a[i];

    int ans = 0;
    for(int b = 29; b >= 0; b--) {
        int candidate = ans | (1LL << b);
        int cnt = 0;
        for(int i = 0; i < n; i++)
            if((a[i] & candidate) == candidate) cnt++;
        if(cnt >= x) ans = candidate;
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
