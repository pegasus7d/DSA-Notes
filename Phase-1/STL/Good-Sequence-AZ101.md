---
tags: [stl, map, greedy]
date: 2026-07-01
status: Solved
url: https://maang.in/problems/Good-Sequence-AZ101-375
---

# Good Sequence AZ101

## Problem

Array is "good" if value x appears exactly x times. Find min deletions.

## Intuition

For each value x with frequency f:
- If f >= x: keep x of them → delete f-x
- If f < x: can't achieve x occurrences → delete all f

Sum over all distinct values.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int n; cin >> n;
    map<int,int> freq;
    for(int i = 0; i < n; i++) {
        int x; cin >> x;
        freq[x]++;
    }

    int del = 0;
    for(auto& [x, f] : freq) {
        if(f >= x) del += f - x;
        else del += f;
    }
    cout << del << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    int t; cin >> t;
    while(t--) solve();
    return 0;
}
```
