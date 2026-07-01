---
tags: [graphs, topological-sort, bfs, greedy]
date: 2026-06-28
status: Solved
url: https://maang.in/problems/Smallest-Permutation-898
---

# Smallest Permutation

## Problem

Given N elements and M constraints (Ai must appear before Bi), find the lexicographically smallest permutation of (1..N) satisfying all constraints. Print -1 if no valid permutation exists (cycle).

**Input Format**
- First line: N M
- M lines: Ai Bi

**Output Format**
- Space-separated permutation, or -1

**Constraints**
- 2 ≤ N ≤ 2×10⁵, 1 ≤ M ≤ 2×10⁵

## Sample Test Cases

**Input 1:** N=4, constraints: 2→1, 1→3 → **Output:** `2 1 3 4`  
**Input 2:** cycle → **Output:** `-1`

## Intuition

Kahn's algorithm (BFS topological sort) with a **min-heap** instead of a regular queue. Always pick the smallest available node (in-degree 0). If result size ≠ N → cycle exists → -1.

Key: greedy min-heap ensures lexicographically smallest valid order.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

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

    priority_queue<int, vector<int>, greater<int>> pq;
    for (int i = 1; i <= n; i++) {
        if (indeg[i] == 0) pq.push(i);
    }

    vector<int> result;
    while (!pq.empty()) {
        int u = pq.top(); pq.pop();
        result.push_back(u);
        for (int v : adj[u]) {
            indeg[v]--;
            if (indeg[v] == 0) pq.push(v);
        }
    }

    if ((int)result.size() != n) {
        cout << -1 << endl;
        return;
    }

    for (int i = 0; i < n; i++) {
        cout << result[i];
        if (i < n - 1) cout << " ";
    }
    cout << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    solve();
}
```
