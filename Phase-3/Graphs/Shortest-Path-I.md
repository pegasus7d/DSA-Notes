---
tags: [graphs, dijkstra, shortest-path]
date: 2026-06-29
status: Solved
url: https://maang.in/problems/Shortest-Path-I-207
---

# Shortest Path I

## Problem

N cities, M directed weighted roads. Find shortest path from city 1 (capital) to every other city.

**Input Format**
- First line: N M
- M lines: a b c (directed edge a→b with weight c)

**Output Format**
- N space-separated shortest distances from city 1

**Constraints**
- 1 ≤ N ≤ 10^5, 1 ≤ M ≤ 2×10^5, 1 ≤ c ≤ 10^9

## Sample Test Cases

**Input:** 3 cities, edges 1→2(6), 1→3(2), 3→2(3) → **Output:** `0 5 2`

## Intuition

Standard Dijkstra from node 1. Min-heap stores {distance, node}. Skip stale entries with `if (d > dist[u]) continue`.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int n, m;
    cin >> n >> m;

    vector<vector<pair<int,int>>> adj(n + 1);
    for (int i = 0; i < m; i++) {
        int a, b, c;
        cin >> a >> b >> c;
        adj[a].push_back({b, c});
    }

    const int INF = 1e18;
    vector<int> dist(n + 1, INF);
    dist[1] = 0;

    priority_queue<pair<int,int>, vector<pair<int,int>>, greater<pair<int,int>>> pq;
    pq.push({0, 1});

    while (!pq.empty()) {
        int d = pq.top().first, u = pq.top().second;
        pq.pop();
        if (d > dist[u]) continue;  // stale entry, skip

        for (int i = 0; i < (int)adj[u].size(); i++) {
            int v = adj[u][i].first, w = adj[u][i].second;
            if (dist[u] + w < dist[v]) {
                dist[v] = dist[u] + w;
                pq.push({dist[v], v});
            }
        }
    }

    for (int i = 1; i <= n; i++) {
        cout << dist[i];
        if (i < n) cout << " ";
    }
    cout << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    solve();
}
```
