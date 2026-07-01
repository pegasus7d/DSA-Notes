---
tags: [graphs, dfs, cycle-detection, directed]
date: 2026-06-28
status: Solved
url: https://maang.in/problems/Round-Trip-II-388
---

# Round Trip II

## Problem

Given a directed graph with N cities and M one-way flights. Check if a directed cycle exists (round trip possible). T test cases.

**Input Format**
- First line: T
- Each test case: N M, then M directed edges a→b

**Output Format**
- Print "Yes" or "No" for each test case.

**Constraints**
- 1 ≤ T ≤ 10, 1 ≤ N ≤ 10^5, 1 ≤ M ≤ 2×10^5

## Sample Test Cases

**Input:** 4 nodes, cycle 1→3→2→1 exists → `Yes`

## Intuition

DFS with 3 states for directed cycle detection:
- `0` = unvisited
- `1` = currently on DFS stack (being explored)
- `2` = fully done

If we reach a neighbor with state `1` → back edge in directed graph → cycle exists.

State `2` means already explored in a previous branch — not a cycle, just a cross edge.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

bool dfs(int u, vector<vector<int>>& adj, vector<int>& state) {
    state[u] = 1;
    for (int v : adj[u]) {
        if (state[v] == 1) return true;
        if (state[v] == 0) {
            if (dfs(v, adj, state)) return true;
        }
    }
    state[u] = 2;
    return false;
}

void solve() {
    int n, m;
    cin >> n >> m;

    vector<vector<int>> adj(n + 1);
    for (int i = 0; i < m; i++) {
        int a, b; cin >> a >> b;
        adj[a].push_back(b);
    }

    vector<int> state(n + 1, 0);
    for (int i = 1; i <= n; i++) {
        if (state[i] == 0) {
            if (dfs(i, adj, state)) {
                cout << "Yes" << endl;
                return;
            }
        }
    }
    cout << "No" << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    int t; cin >> t;
    while (t--) solve();
}
```
