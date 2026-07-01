---
tags: [stl, map, two-sum]
date: 2026-07-01
status: Solved
url: https://maang.in/problems/Common-Absolute-Difference-379
---

# Common Absolute Difference

## Problem

Count pairs (i,j) with i<j and |A[i]-A[j]|=K.

## Intuition

For each element x, look up freq[x+K] and freq[x-K] (both distinct since K≠0), add to answer, then increment freq[x].

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int n, k; cin >> n >> k;
    map<int,int> freq;
    int ans = 0;

    for(int i = 0; i < n; i++) {
        int x; cin >> x;
        if(freq.count(x + k)) ans += freq[x + k];
        if(freq.count(x - k)) ans += freq[x - k];
        freq[x]++;
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
