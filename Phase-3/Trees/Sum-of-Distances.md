---
tags: [trees, floyd-warshall, apsp, reverse-thinking]
date: 2026-07-02
status: Solved
url: https://maang.in/problems/Sum-of-Distances-431
---

# Sum of Distances (All Pairs Shortest Path)

## Problem

Directed weighted graph. Vertices removed one by one. Before each removal, print sum of all-pairs shortest paths.

## Key Insight ⚡

**Reverse the process**: instead of removing vertices, add them in reverse order and apply Floyd-Warshall incrementally.

When adding vertex k to active set S:
1. Relax all pairs: `dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j])` for all i,j
2. Sum dist[i][j] over all active pairs (i≠j) → answer for this step

Store answers in array, print in original order.

**Why it works**: FW relaxation using k as intermediate is correct regardless of order — same as standard Floyd-Warshall.

## Complexity

O(N³) — N steps × O(N²) relaxation per step.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

int dist[505][505];

void solve() {
    int n; cin >> n;
    for(int i = 1; i <= n; i++)
        for(int j = 1; j <= n; j++)
            cin >> dist[i][j];

    vector<int> order(n);
    for(int i = 0; i < n; i++) cin >> order[i];

    vector<int> ans(n);
    vector<int> activeList;

    for(int step = n - 1; step >= 0; step--) {
        int k = order[step];
        for(int i = 1; i <= n; i++)
            for(int j = 1; j <= n; j++)
                dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j]);

        activeList.push_back(k);
        int sum = 0;
        for(int i : activeList)
            for(int j : activeList)
                if(i != j) sum += dist[i][j];
        ans[step] = sum;
    }

    for(int i = 0; i < n; i++) {
        cout << ans[i];
        if(i < n - 1) cout << " ";
    }
    cout << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    solve();
    return 0;
}
```
