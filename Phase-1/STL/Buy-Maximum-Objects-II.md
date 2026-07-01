---
tags: [stl, binary-search, prefix-sum, sorting]
date: 2026-07-01
status: Solved
url: https://maang.in/problems/Buy-Maximum-Objects-II-74
---

# Buy Maximum Objects II

## Problem

N objects with prices. Q queries each giving money M — find maximum objects you can buy.

**Input:** N Q, then N prices, then Q values of M  
**Output:** For each query, max objects purchasable

**Constraints:** 1 ≤ N, Q ≤ 10^5, 1 ≤ Ai, M ≤ 10^8

## Sample Test Cases

**Input:**
```
5 4
1 4 2 9 6
1
5
10
25
```
**Output:** `1 2 3 5`

## Intuition

Sort prices. Build prefix sum. For each query M, binary search (`upper_bound`) for the largest k such that prefix[k] ≤ M.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int n, q;
    cin >> n >> q;
    vector<int> a(n);
    for(int i = 0; i < n; i++) cin >> a[i];
    sort(a.begin(), a.end());

    vector<int> prefix(n+1, 0);
    for(int i = 0; i < n; i++) prefix[i+1] = prefix[i] + a[i];

    while(q--) {
        int m; cin >> m;
        int k = (int)(upper_bound(prefix.begin(), prefix.end(), m) - prefix.begin()) - 1;
        cout << k << endl;
    }
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    solve();
    return 0;
}
```
