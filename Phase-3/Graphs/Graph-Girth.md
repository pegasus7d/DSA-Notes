---
tags: [graphs, bfs, cycle-detection]
date: 2026-06-28
status: Solved
url: https://maang.in/problems/Graph-Girth-895
---

# Graph Girth

## Problem

Given an undirected graph, find its girth — the length of the shortest cycle. Print -1 if no cycle exists.

**Input Format**
- First line: N M
- M lines: a b (undirected edge)

**Output Format**
- Shortest cycle length, or -1

**Constraints**
- 1 ≤ N ≤ 2500, 1 ≤ M ≤ 5000

## Sample Test Cases

**Input 1:** Graph with triangle (nodes 2,5,4) → **Output:** `3`  
**Input 2:** Acyclic graph → **Output:** `-1`

## Intuition

BFS from every node. When processing node u, if a neighbor v is already visited and v is not u's BFS parent → cycle found with length `dist[u] + dist[v] + 1`. Take minimum across all BFS runs.

O(N × (N+M)) = 2500 × 7500 ≈ 18M ops — fits in 1 sec.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int n, m;
    cin >> n >> m;

    vector<vector<int>> adj(n + 1);
    for (int i = 0; i < m; i++) {
        int a, b;
        cin >> a >> b;
        adj[a].push_back(b);
        adj[b].push_back(a);
    }

    const int INF = 1e9;
    int girth = INF;

    for (int src = 1; src <= n; src++) {
        vector<int> dist(n + 1, -1);
        vector<int> par(n + 1, -1);
        queue<int> q;
        dist[src] = 0;
        q.push(src);

        while (!q.empty()) {
            int u = q.front(); q.pop();
            for (int v : adj[u]) {
                if (dist[v] == -1) {
                    dist[v] = dist[u] + 1;
                    par[v] = u;
                    q.push(v);
                } else if (v != par[u]) {
                    girth = min(girth, dist[u] + dist[v] + 1);
                }
            }
        }
    }

    if (girth == INF) cout << -1 << endl;
    else cout << girth << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    solve();
}
```
