---
tags: [trees, bfs, diameter, center]
date: 2026-07-02
status: Solved
url: https://maang.in/problems/Center-of-Tree-429
---

# Center of Tree

## Problem

Find the center of a tree (node minimizing max distance to all others). If 2 centers exist, print -1.

## Key Insight ⚡

Center = middle node of the diameter path.
- If diameter is even → unique center at `path[diameter/2]`
- If diameter is odd → 2 centers → print -1

Find path: two-BFS to get diameter endpoints u→v, then reconstruct path via parent array from v back to u.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

vector<int> adj[200005];

pair<int,int> bfs(int src, int n, vector<int>& parent) {
    vector<int> dist(n + 1, -1);
    parent.assign(n + 1, -1);
    queue<int> q;
    q.push(src); dist[src] = 0;
    int farthest = src;
    while(!q.empty()) {
        int u = q.front(); q.pop();
        if(dist[u] > dist[farthest]) farthest = u;
        for(int v : adj[u])
            if(dist[v] == -1) { dist[v] = dist[u] + 1; parent[v] = u; q.push(v); }
    }
    return {farthest, dist[farthest]};
}

void solve() {
    int n; cin >> n;
    for(int i = 0; i < n - 1; i++) {
        int a, b; cin >> a >> b;
        adj[a].push_back(b); adj[b].push_back(a);
    }
    vector<int> par;
    auto [u, _] = bfs(1, n, par);
    auto [v, dia] = bfs(u, n, par);

    vector<int> path;
    int cur = v;
    while(cur != -1) { path.push_back(cur); cur = par[cur]; }

    if(dia % 2 == 1) cout << -1 << endl;
    else cout << path[dia / 2] << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    solve();
    return 0;
}
```
