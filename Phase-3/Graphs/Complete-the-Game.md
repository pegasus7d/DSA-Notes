---
tags: [graphs, topological-sort, dp, dag]
date: 2026-06-28
status: Solved
url: https://maang.in/problems/Complete-the-Game-432
---

# Complete the Game

## Problem

DAG with N levels and M teleporters. Count the number of distinct paths from level 1 to level N. Answer modulo 10^9+7.

**Input Format**
- First line: N M
- M lines: a b (teleporter from a to b)

**Output Format**
- Number of paths mod 10^9+7

**Constraints**
- 1 ≤ N ≤ 10^5, 1 ≤ M ≤ 2×10^5

## Sample Test Cases

**Input:** 4 nodes, edges 1→2, 1→3, 1→4, 2→4, 3→4 → **Output:** `3` (paths: 1→4, 1→2→4, 1→3→4)

## Intuition

Topological sort (Kahn's) + DP on DAG. `dp[i]` = number of ways to reach node i from node 1.

- `dp[1] = 1`, all others 0
- Process nodes in topological order; for each edge u→v: `dp[v] += dp[u]`
- Answer = `dp[n]`

Topo order guarantees all predecessors of v are processed before v.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long
const int MOD = 1e9 + 7;

void solve() {
    int n, m;
    cin >> n >> m;

    vector<vector<int>> adj(n + 1);
    vector<int> indeg(n + 1, 0);

    for (int i = 0; i < m; i++) {
        int a, b;
        cin >> a >> b;
        adj[a].push_back(b);
        indeg[b]++;
    }

    vector<int> dp(n + 1, 0);
    dp[1] = 1;

    queue<int> q;
    for (int i = 1; i <= n; i++) {
        if (indeg[i] == 0) q.push(i);
    }

    while (!q.empty()) {
        int u = q.front(); q.pop();
        for (int v : adj[u]) {
            dp[v] = (dp[v] + dp[u]) % MOD;
            indeg[v]--;
            if (indeg[v] == 0) q.push(v);
        }
    }

    cout << dp[n] << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    solve();
}
```
