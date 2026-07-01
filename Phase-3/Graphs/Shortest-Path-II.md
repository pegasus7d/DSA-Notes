---
tags: [graphs, floyd-warshall, all-pairs-shortest-path]
date: 2026-06-29
status: Solved
url: https://maang.in/problems/Shortest-Path-II-208
---

# Shortest Path II

## Problem

N cities, M bidirectional weighted roads, Q queries. For each query (a, b), find shortest path length. Print -1 if no path.

**Input Format**
- Line 1: N M Q
- M lines: a b c (bidirectional edge, weight c)
- Q lines: a b (query)

**Output Format**
- For each query: shortest distance or -1

**Constraints**
- 1 ≤ N ≤ 500, 1 ≤ M ≤ N², 1 ≤ Q ≤ 10^5

## Sample Test Cases

**Input:**
```
4 4 5
1 2 5
2 3 3
1 2 7
```
**Output:** `5 5 8 -1 3`

## Intuition

N ≤ 500 → O(N³) Floyd-Warshall precomputes all-pairs shortest paths. Then Q queries answered in O(1).

**Key idea:** For every intermediate node k (1..N), try routing i→k→j. If that's shorter than current dist[i][j], update it.

- Init: `dist[i][i] = 0`, `dist[i][j] = edge weight` (min if multiple edges), else INF
- Triple loop: k outer, i middle, j inner
- Query: if `dist[a][b] == INF` → print -1

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int n, m, q;
    cin >> n >> m >> q;

    const int INF = 1e18;
    vector<vector<int>> dist(n + 1, vector<int>(n + 1, INF));

    for (int i = 1; i <= n; i++) dist[i][i] = 0;

    for (int i = 0; i < m; i++) {
        int a, b, c;
        cin >> a >> b >> c;
        dist[a][b] = min(dist[a][b], c);
        dist[b][a] = min(dist[b][a], c);
    }

    // try every intermediate node k
    for (int k = 1; k <= n; k++) {
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= n; j++) {
                if (dist[i][k] != INF && dist[k][j] != INF) {
                    dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j]);
                }
            }
        }
    }

    while (q--) {
        int a, b;
        cin >> a >> b;
        if (dist[a][b] == INF) cout << -1 << endl;
        else cout << dist[a][b] << endl;
    }
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    solve();
}
```

## Complexity
- Time: O(N³ + Q)
- Space: O(N²)

## Key Insight ⚡
k must be the **outermost** loop — ensures that when we compute dist[i][j] via k, all paths through nodes 1..k-1 are already optimal.

## Pattern Tag
`floyd-warshall` `all-pairs-shortest-path`
