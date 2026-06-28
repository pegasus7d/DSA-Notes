---
tags: [graphs, bfs, connected-components]
date: 2026-06-28
status: Solved
url: https://maang.in/problems/Easy-Graph-Queries-400
---

# Easy Graph Queries

## Problem

Given an undirected graph with N nodes and M edges, answer Q queries:
- Type 1: `1 X` — print size of connected component containing node X
- Type 2: `2 X Y` — print YES if X and Y are in the same component, else NO

**Input Format**
- First line: N, M, Q
- Next M lines: edges u v
- Next Q lines: queries

**Constraints**
- 1 ≤ N, M, Q ≤ 10^5

## Sample Test Cases

**Input:**
```
6 6 5
1 2
2 3
4 4
5 6
1 2
2 3
1 2
1 4
2 3 4
1 5
2 5 6
```

**Output:**
```
3
1
NO
2
YES
```

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;

void solve() {
    int n, m, q;
    cin >> n >> m >> q;

    vector<vector<int>> adj(n + 1);
    for (int i = 0; i < m; i++) {
        int u, v; cin >> u >> v;
        if (u != v) {          // skip self-loops
            adj[u].push_back(v);
            adj[v].push_back(u);
        }
    }

    vector<int> comp(n + 1, -1);   // comp[i] = component id of node i
    vector<int> compSize;           // size of each component

    int id = 0;
    for (int i = 1; i <= n; i++) {
        if (comp[i] != -1) continue;

        // BFS to label all nodes in this component
        queue<int> bfs;
        bfs.push(i);
        comp[i] = id;
        int size = 0;
        while (!bfs.empty()) {
            int u = bfs.front(); bfs.pop();
            size++;
            for (int v : adj[u]) {
                if (comp[v] == -1) {
                    comp[v] = id;
                    bfs.push(v);
                }
            }
        }
        compSize.push_back(size);
        id++;
    }

    while (q--) {
        int type; cin >> type;
        if (type == 1) {
            int x; cin >> x;
            cout << compSize[comp[x]] << endl;
        } else {
            int x, y; cin >> x >> y;
            cout << (comp[x] == comp[y] ? "YES" : "NO") << endl;
        }
    }
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    solve();
}
```
