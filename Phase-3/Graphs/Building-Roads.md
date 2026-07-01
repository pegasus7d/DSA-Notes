---
tags: [graphs, bfs, connected-components]
date: 2026-06-28
status: Solved
url: https://maang.in/problems/Building-Roads-192
---

# Building Roads

## Problem

Zenithland has N cities and M roads. Find the minimum number of new roads needed so all cities are connected.

**Input Format**
- First line: N, M
- Next M lines: edges a b

**Output Format**
- Print minimum roads required.

**Constraints**
- 1 ≤ N ≤ 10^5, 1 ≤ M ≤ 2×10^5

## Sample Test Cases

**Input:**
```
4 2
1 2
3 4
```

**Output:**
```
1
```

## Intuition

Count connected components using BFS. To connect K components into one, you need exactly K-1 roads (like connecting nodes in a chain).

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
        int a, b; cin >> a >> b;
        adj[a].push_back(b);
        adj[b].push_back(a);
    }

    vector<bool> visited(n + 1, false);
    int components = 0;

    for (int i = 1; i <= n; i++) {
        if (visited[i]) continue;
        components++;
        queue<int> q;
        q.push(i);
        visited[i] = true;
        while (!q.empty()) {
            int u = q.front(); q.pop();
            for (int v : adj[u]) {
                if (!visited[v]) {
                    visited[v] = true;
                    q.push(v);
                }
            }
        }
    }

    cout << components - 1 << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    solve();
}
```
