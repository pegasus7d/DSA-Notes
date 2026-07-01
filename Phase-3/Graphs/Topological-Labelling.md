---
tags: [graphs, topological-sort, greedy, reverse-graph]
date: 2026-06-28
status: Solved
url: https://maang.in/problems/Topological-Labelling-392
---

# Topological Labelling

## Problem

DAG with N vertices, M edges. Assign labels 1..N such that edge v→u means label[v] < label[u]. Find lexicographically smallest labeling.

**Input Format**
- First line: N M
- M lines: v u (directed edge v→u)

**Output Format**
- N space-separated labels (label[1] label[2] ... label[N])

**Constraints**
- 2 ≤ N ≤ 10^5, 1 ≤ M ≤ 10^5

## Sample Test Cases

**Input:** 3 nodes, edges 1→2, 3→2 → **Output:** `1 3 2`

## Intuition

Reverse of Kahn's algorithm. Assign labels N down to 1. At each step, among zero-outdegree vertices pick the **largest-indexed** one → assign it the current largest label. This saves small labels for small-indexed vertices → lex smallest.

Use **reverse adjacency list** (`radj[u]` = predecessors of u) to efficiently decrement out-degrees when u is processed.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int n, m;
    cin >> n >> m;

    vector<vector<int>> radj(n + 1);  // reverse adj: who points TO this node
    vector<int> indeg(n + 1, 0);      // in-degree of reverse graph = out-degree of original

    for (int i = 0; i < m; i++) {
        int v, u;
        cin >> v >> u;
        indeg[v]++;              // v has one more outgoing edge (out-degree of original)
        radj[u].push_back(v);   // v points TO u → store v as predecessor of u
    }

    // Push all nodes with indeg=0 (sinks in original = sources in reverse graph)
    // Max-heap → always pick largest-indexed sink first
    // → gives large labels to large nodes → small labels left for small nodes → lex smallest
    priority_queue<int> pq;
    for (int i = 1; i <= n; i++) {
        if (indeg[i] == 0) pq.push(i);
    }

    vector<int> label(n + 1);
    int cur = n;  // assign labels from n down to 1

    while (!pq.empty()) {
        int u = pq.top(); pq.pop();
        label[u] = cur--;        // give current largest label to largest-index sink

        // Remove u from graph: decrement indeg of all predecessors of u
        // When their indeg hits 0, they become new sinks → push to heap
        for (int v : radj[u]) {
            indeg[v]--;
            if (indeg[v] == 0) pq.push(v);
        }
    }

    for (int i = 1; i <= n; i++) {
        cout << label[i];
        if (i < n) cout << " ";
    }
    cout << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    solve();
}
```
