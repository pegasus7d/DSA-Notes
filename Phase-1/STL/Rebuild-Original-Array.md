---
tags: [stl, multiset, subset-sum, greedy]
date: 2026-07-01
status: Solved
url: https://maang.in/problems/Rebuild-Original-Array-87
---

# Rebuild Original Array

## Problem

Given all 2^N subset sums, recover the original N elements in sorted order.

## Intuition

Greedy: the smallest non-zero subset sum is always the smallest original element. After picking x:
1. Remove x from S (it's the subset {x}).
2. Remove x + s for every existing subset sum s ≠ 0 (these are subsets that include x combined with previous elements).
3. Add x to all existing subset sums to get new ones.
4. Repeat N times.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int n; cin >> n;
    int total = 1 << n;
    multiset<int> S;
    for(int i = 0; i < total; i++) { int x; cin >> x; S.insert(x); }

    S.erase(S.begin()); // remove 0

    vector<int> curSubs = {0};
    vector<int> result;

    for(int i = 0; i < n; i++) {
        int x = *S.begin();
        S.erase(S.begin());
        result.push_back(x);

        vector<int> newSubs;
        for(int s : curSubs) {
            newSubs.push_back(x + s);
            if(s != 0) S.erase(S.find(x + s));
        }
        for(int v : newSubs) curSubs.push_back(v);
    }

    sort(result.begin(), result.end());
    for(int i = 0; i < n; i++) {
        if(i > 0) cout << " ";
        cout << result[i];
    }
    cout << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    int t; cin >> t;
    while(t--) solve();
    return 0;
}
```
