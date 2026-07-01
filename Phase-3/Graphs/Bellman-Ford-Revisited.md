---
tags: [graphs, bellman-ford, negative-cycle, shortest-path]
date: 2026-06-29
status: Solved
url: https://maang.in/problems/BellmanFord-Revisited-209
---

# Bellman-Ford Revisited

## Problem

Directed weighted graph (weights can be negative). Find maximum path weight from node 1 to node n. If there's a positive-weight cycle reachable from 1 that can reach n → print -1.

**Input Format**
- First line: N M
- M lines: a b x (directed edge a→b with weight x)

**Output Format**
- Maximum path weight, or -1

**Constraints**
- 1 ≤ N ≤ 2500, 1 ≤ M ≤ 5000, -10^9 ≤ x ≤ 10^9

## Intuition

Negate all weights → max path becomes min path → use Bellman-Ford.

1. Precompute `canReachN[]` via BFS on reverse graph from n
2. Run N-1 Bellman-Ford relaxation rounds from node 1
3. Nth round: if edge a→b still relaxes AND `canReachN[b]` → positive cycle affects path to n → print -1
4. Otherwise print `-dist[n]`

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int n, m;
    cin >> n >> m;

    vector<vector<int>> edges(m, vector<int>(3));
    vector<vector<int>> radj(n + 1);

    for (int i = 0; i < m; i++) {
        int a, b, x;
        cin >> a >> b >> x;
        edges[i][0] = a;
        edges[i][1] = b;
        edges[i][2] = -x;  // negate for max path
        radj[b].push_back(a);
    }

    // BFS on reverse graph from n → find nodes that can reach n
    vector<bool> canReachN(n + 1, false);
    queue<int> q;
    q.push(n);
    canReachN[n] = true;
    while (!q.empty()) {
        int u = q.front(); q.pop();
        for (int v : radj[u]) {
            if (!canReachN[v]) {
                canReachN[v] = true;
                q.push(v);
            }
        }
    }

    // Bellman-Ford with negated weights from node 1
    const int INF = 1e18;
    vector<int> dist(n + 1, INF);
    dist[1] = 0;

    for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < m; j++) {
            int a = edges[j][0], b = edges[j][1], x = edges[j][2];
            if (dist[a] != INF && dist[a] + x < dist[b]) {
                dist[b] = dist[a] + x;
            }
        }
    }

    // Nth round: detect negative cycle that can reach n
    for (int j = 0; j < m; j++) {
        int a = edges[j][0], b = edges[j][1], x = edges[j][2];
        if (dist[a] != INF && dist[a] + x < dist[b] && canReachN[b]) {
            cout << -1 << endl;
            return;
        }
    }

    cout << -dist[n] << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    solve();
}
```
