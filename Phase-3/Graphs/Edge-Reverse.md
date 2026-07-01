---
tags: [graphs, 0-1-bfs, shortest-path]
date: 2026-06-28
status: Solved
url: https://maang.in/problems/Edge-Reverse-900
---

# Edge Reverse

## Problem

Directed graph with N vertices, M edges. Find minimum number of edges to reverse to have at least one path from vertex 1 to vertex N. Print -1 if impossible.

**Input Format**
- First line: T (test cases)
- Each test case: N M, then M lines of X Y (directed edge X→Y)

**Output Format**
- Minimum reversals needed, or -1

**Constraints**
- 1 ≤ T ≤ 10, 1 ≤ N, M ≤ 10^5

## Intuition

Model as 0-1 BFS:
- Original edge u→v: add with weight 0 (use as-is, free)
- Reverse edge v→u: add with weight 1 (reversing costs 1)

0-1 BFS from node 1 to node N gives minimum reversals needed.

**0-1 BFS:** Use deque instead of queue. Weight-0 edges → push_front. Weight-1 edges → push_back. Keeps deque sorted by distance → O(N+M).

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
        int x, y;
        cin >> x >> y;
        adj[x].push_back({y, 0});  // use as-is, cost 0
        adj[y].push_back({x, 1});  // reverse it, cost 1
    }

    const int INF = 1e18;
    vector<int> dist(n + 1, INF);
    dist[1] = 0;
    deque<int> dq;
    dq.push_back(1);

    while (!dq.empty()) {
        int u = dq.front(); dq.pop_front();
        for (int i = 0; i < (int)adj[u].size(); i++) {
            int v = adj[u][i].first, w = adj[u][i].second;
            if (dist[u] + w < dist[v]) {
                dist[v] = dist[u] + w;
                if (w == 0) dq.push_front(v);
                else        dq.push_back(v);
            }
        }
    }

    if (dist[n] == INF) cout << -1 << endl;
    else cout << dist[n] << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    int t; cin >> t;
    while (t--) solve();
}
```
