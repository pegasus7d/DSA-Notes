---
tags: [graphs, bfs, bipartite]
date: 2026-06-28
status: Solved
url: https://maang.in/problems/Creating-Teams-193
---

# Creating Teams

## Problem

Given N students and M friendships. Divide into two teams such that no two friends are in the same team. Print YES if possible, else NO.

**Input Format**
- First line: N M
- Next M lines: edges a b

**Output Format**
- YES or NO

**Constraints**
- 1 ≤ N ≤ 10^5, 0 ≤ M ≤ 2×10^5

## Sample Test Cases

**Input 1:** Triangle-free graph → `YES`
**Input 2:** Triangle (1-2-3-1) → odd cycle → `NO`

## Intuition

Bipartite check using BFS 2-coloring. Color each node 0 or 1 alternately. If a neighbor already has the same color → odd cycle → not bipartite → NO.

A graph is bipartite if and only if it has no odd-length cycles.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

bool isBipartite(int start, vector<vector<int>>& adj, vector<int>& color) {
    queue<int> q;
    q.push(start);
    color[start] = 0;
    while (!q.empty()) {
        int u = q.front(); q.pop();
        for (int v : adj[u]) {
            if (color[v] == -1) {
                color[v] = 1 - color[u];
                q.push(v);
            } else if (color[v] == color[u]) {
                return false;
            }
        }
    }
    return true;
}

void solve() {
    int n, m;
    cin >> n >> m;

    vector<vector<int>> adj(n + 1);
    for (int i = 0; i < m; i++) {
        int a, b; cin >> a >> b;
        adj[a].push_back(b);
        adj[b].push_back(a);
    }

    vector<int> color(n + 1, -1);
    for (int i = 1; i <= n; i++) {
        if (color[i] == -1) {
            if (!isBipartite(i, adj, color)) {
                cout << "NO" << endl;
                return;
            }
        }
    }
    cout << "YES" << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    solve();
}
```
