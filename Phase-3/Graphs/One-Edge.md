---
tags: [graphs, bfs, connected-components]
date: 2026-06-28
status: Solved
url: https://maang.in/problems/One-Edge-196
---

# One Edge

## Problem

Given an undirected graph with N nodes and M edges. Find the number of ways to add exactly one edge such that the total number of connected components decreases.

**Input Format**
- First line: N, M
- Next M lines: edges a b

**Output Format**
- Print the number of valid ways to add such an edge.

**Constraints**
- 1 ≤ N ≤ 10^5, 1 ≤ M ≤ 2×10^5

## Sample Test Cases

**Input:**
```
5 3
1 2
2 3
4 5
```

**Output:**
```
6
```

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
        int u, v; cin >> u >> v;
        adj[u].push_back(v);
        adj[v].push_back(u);
    }

    vector<bool> visited(n + 1, false);
    int sameComp = 0;  // pairs within the same component

    for (int i = 1; i <= n; i++) {
        if (visited[i]) continue;
        queue<int> bfs;
        bfs.push(i);
        visited[i] = true;
        int size = 0;
        while (!bfs.empty()) {
            int u = bfs.front(); bfs.pop();
            size++;
            for (int v : adj[u]) {
                if (!visited[v]) {
                    visited[v] = true;
                    bfs.push(v);
                }
            }
        }
        sameComp += size * (size - 1) / 2;
    }

    // total pairs - same component pairs = different component pairs
    int total = n * (n - 1) / 2;
    cout << total - sameComp << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    solve();
}
```
