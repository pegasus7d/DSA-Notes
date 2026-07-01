---
tags: [stl, greedy, sorting, game-theory]
date: 2026-07-01
status: Solved
url: https://maang.in/problems/Interesting-Game-76
---

# Interesting Game

## Problem

Alice (P1) and Bob (P2) alternate picking indices (Alice first). Picking index i gives Alice A[i] points or Bob B[i] points. Each index used once. Both play optimally. Who wins?

**Constraints:** 1 ≤ T ≤ 10, 2 ≤ N ≤ 1000, 1 ≤ Ai, Bi ≤ 10^5

## Sample Test Cases

**Input:**
```
3
3
1 3 4
5 3 1
2
1 1
1 1
2
2 2
3 3
```
**Output:** `Alice  Tie  Bob`

## Intuition

Sort by A[i]+B[i] descending. Each player greedily picks the highest combined-value slot. By taking the max A+B slot, you maximize your gain AND deny the opponent the most. Alice gets A[i] on her turns, Bob gets B[i] on his.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int n; cin >> n;
    vector<int> a(n), b(n);
    for(int i = 0; i < n; i++) cin >> a[i];
    for(int i = 0; i < n; i++) cin >> b[i];

    vector<tuple<int,int,int>> items(n);
    for(int i = 0; i < n; i++) items[i] = {a[i]+b[i], a[i], b[i]};
    sort(items.rbegin(), items.rend());

    int alice = 0, bob = 0;
    for(int turn = 0; turn < n; turn++) {
        auto [sum, ai, bi] = items[turn];
        if(turn % 2 == 0) alice += ai;
        else bob += bi;
    }

    if(alice > bob) cout << "Alice" << endl;
    else if(bob > alice) cout << "Bob" << endl;
    else cout << "Tie" << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    int t; cin >> t;
    while(t--) solve();
    return 0;
}
```
