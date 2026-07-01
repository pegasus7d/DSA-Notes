---
tags: [graphs, dijkstra, shortest-path, math]
date: 2026-06-29
status: Solved
url: https://maang.in/problems/Burn-them-All-618
---

# Burn them All

## Problem

Undirected weighted graph. Fire starts at node A, spreads at speed 1 unit/time along edges. Find minimum time T for all edges to burn completely. Output 10T (guaranteed integer).

**Input Format**
- Line 1: N, Line 2: M
- M lines: u v d (edge u-v of length d)
- Last line: A (fire source)

**Output Format**
- 10T

**Constraints**
- 1 ≤ N, M ≤ 2×10^5, 1 ≤ d ≤ 10^9

## Intuition

Run Dijkstra from A to get `dist[u]` for all nodes.

For each edge u-v (weight d), fire burns it from both ends simultaneously:
- Fire reaches u at `dist[u]`, reaches v at `dist[v]`
- WLOG `dist[u] ≤ dist[v]`
- If `dist[v] - dist[u] ≥ d` → fire from u covers entire edge → burn_time = `dist[u] + d`
- Else → fires meet in middle → burn_time = `(dist[u] + dist[v] + d) / 2`

To avoid floats, compute 10×burn_time as integers:
- Case 1: `10 * (du + d)`
- Case 2: `5 * (du + dv + d)`

Answer = max over all edges.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int n; cin >> n;
    int m; cin >> m;

    vector<vector<pair<int,int>>> adj(n + 1);
    vector<vector<int>> edges(m, vector<int>(3));

    for (int i = 0; i < m; i++) {
        cin >> edges[i][0] >> edges[i][1] >> edges[i][2];
        adj[edges[i][0]].push_back({edges[i][1], edges[i][2]});
        adj[edges[i][1]].push_back({edges[i][0], edges[i][2]});
    }

    int a; cin >> a;

    const int INF = 1e18;
    vector<int> dist(n + 1, INF);
    dist[a] = 0;
    priority_queue<pair<int,int>, vector<pair<int,int>>, greater<pair<int,int>>> pq;
    pq.push({0, a});

    while (!pq.empty()) {
        int d = pq.top().first, u = pq.top().second;
        pq.pop();
        if (d > dist[u]) continue;
        for (int i = 0; i < (int)adj[u].size(); i++) {
            int v = adj[u][i].first, w = adj[u][i].second;
            if (dist[u] + w < dist[v]) {
                dist[v] = dist[u] + w;
                pq.push({dist[v], v});
            }
        }
    }

    int ans = 0;
    for (int i = 0; i < m; i++) {
        int du = dist[edges[i][0]], dv = dist[edges[i][1]], d = edges[i][2];

        // burn_time = (du + dv + d) / 2  (two fires, combined speed = 2)
        // 10 * burn_time = 10 * (du + dv + d) / 2 = 5 * (du + dv + d)
        int burn_time_x10 = 5 * (du + dv + d);
        ans = max(ans, burn_time_x10);
    }

    cout << ans << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    solve();
}
```
