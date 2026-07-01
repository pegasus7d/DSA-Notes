---
tags: [graphs, dijkstra, graph-formulation, supernode]
date: 2026-06-29
status: Solved
url: https://maang.in/problems/Jump-Game-897
---

# Jump Game

## Problem

Given an array having n elements, the cost to move from i-th element to its adjacent element (if exist) at i+1 and i−1 is b, and the cost to move to other same valued index is a. Find min cost to reach every index from a given source index of the array.

**Input Format**
- Line 1: N a b
- Line 2: arr[1..N]
- Line 3: src

**Output Format**
- N space-separated min costs from src

**Constraints**
- 1 ≤ N ≤ 2×10^5, 1 ≤ a,b ≤ 10^9, 1 ≤ arr[i] ≤ 100

## Intuition

**Graph Formulation with supernodes.** Values are in [1,100] → create one supernode per value.

- Index i → supernode[arr[i]]: cost 0 (enter for free)
- supernode[arr[i]] → index j: cost a (exit costs a, total jump = a)
- Index i ↔ i+1: cost b (adjacent)

Run Dijkstra on this graph (N + 100 nodes).

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int n, a, b;
    cin >> n >> a >> b;

    vector<int> arr(n+1);
    for (int i = 1; i <= n; i++) cin >> arr[i];

    int src;
    cin >> src;

    // Nodes 1..n = indices, n+1..n+100 = supernodes per value
    int total = n + 101;
    vector<vector<pair<int,int>>> adj(total + 1);

    for (int i = 1; i <= n; i++) {
        if (i < n) {
            adj[i].push_back({i+1, b});
            adj[i+1].push_back({i, b});
        }
        int sv = n + arr[i];
        adj[i].push_back({sv, 0});   // enter supernode: free
        adj[sv].push_back({i, a});   // exit supernode: cost a
    }

    const int INF = 1e18;
    vector<int> dist(total + 1, INF);
    dist[src] = 0;
    priority_queue<pair<int,int>, vector<pair<int,int>>, greater<>> pq;
    pq.push({0, src});

    while (!pq.empty()) {
        auto [d, u] = pq.top(); pq.pop();
        if (d > dist[u]) continue;
        for (auto [v, w] : adj[u]) {
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
