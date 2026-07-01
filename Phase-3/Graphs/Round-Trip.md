---
tags: [graphs, dfs, cycle-detection]
date: 2026-06-28
status: Solved
url: https://maang.in/problems/Round-Trip-194
---

# Round Trip

## Problem

Given an undirected graph with N cities and M roads. Check if a cycle exists (a round trip through 2+ distinct intermediate cities back to start).

**Input Format**
- First line: N M
- Next M lines: edges a b

**Output Format**
- Print YES if cycle exists, else NO.

**Constraints**
- 1 ≤ N ≤ 10^5, 1 ≤ M ≤ 2×10^5

## Sample Test Cases

**Input 1:** 5 nodes, 6 edges → cycle exists → `YES`
**Input 2:** 4 nodes, 3 edges (tree) → no cycle → `NO`

## Intuition

DFS cycle detection in undirected graph. If during DFS we find a visited neighbor that is NOT the parent → back edge → cycle exists.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

bool dfs(int u, int parent, vector<vector<int>>& adj, vector<bool>& visited) {
    visited[u] = true;
    for (int v : adj[u]) {
        if (!visited[v]) {
            if (dfs(v, u, adj, visited)) return true;
        } else if (v != parent) {
            return true;
        }
    }
    return false;
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

    vector<bool> visited(n + 1, false);
    for (int i = 1; i <= n; i++) {
        if (!visited[i]) {
            if (dfs(i, -1, adj, visited)) {
                cout << "YES" << endl;
                return;
            }
        }
    }
    cout << "NO" << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    solve();
}
```
