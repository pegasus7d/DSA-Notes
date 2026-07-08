---
tags: [trees, bfs, diameter]
date: 2026-07-02
status: Solved
url: https://maang.in/problems/Tree-Diameter-I-427
---

# Tree Diameter - I

## Problem

Find the diameter (max distance between any two nodes) of an unweighted tree.

## Key Insight ⚡

Two-BFS trick:
1. BFS from any node → find farthest node `u`
2. BFS from `u` → farthest distance = diameter

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

vector<int> adj[200005];

pair<int,int> bfs(int src, int n) {
    vector<int> dist(n + 1, -1);
    queue<int> q;
    q.push(src); dist[src] = 0;
    int farthest = src;
    while(!q.empty()) {
        int u = q.front(); q.pop();
        if(dist[u] > dist[farthest]) farthest = u;
        for(int v : adj[u])
            if(dist[v] == -1) { dist[v] = dist[u] + 1; q.push(v); }
    }
    return {farthest, dist[farthest]};
}

void solve() {
    int n; cin >> n;
    for(int i = 0; i < n - 1; i++) {
        int a, b; cin >> a >> b;
        adj[a].push_back(b); adj[b].push_back(a);
    }
    auto [u, _] = bfs(1, n);
    auto [v, dia] = bfs(u, n);
    cout << dia << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    solve();
    return 0;
}
```
