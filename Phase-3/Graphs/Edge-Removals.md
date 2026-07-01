---
tags: [graphs, dsu, union-find, offline, reverse-queries]
date: 2026-06-29
status: Solved
url: https://maang.in/problems/Edge-Removals-41
---

# Edge Removals

## Problem

Given an undirected graph with N nodes and M edges. Process Q queries: remove an edge or print the number of connected components. If the same edge is removed multiple times, only the last removal counts.

**Input Format**
- First line: N M Q
- Next M lines: u v (edge between u and v, 1-indexed)
- Next Q lines: "1 X" (remove edge X) or "2" (print components)

**Output Format**
- For each type-2 query, print the number of connected components

**Constraints**
- 1 ≤ N, M, Q ≤ 100000

## Sample Test Cases

**Input 1:**
```
3 3 5
1 2
2 3
3 1
2
1 2
2
1 1
2
```
**Output 1:**
```
1
1
2
```

## Intuition

DSU doesn't support edge deletion, but supports addition. Process queries **offline in reverse** — deletions become additions. Start with edges that are never removed, then replay queries backwards: type-2 records the current component count, type-1 adds the edge back. Reverse the collected answers at the end.

Key: if the same edge is removed multiple times, only the **last** removal query actually matters — track `lastRemoval[edge]` and only add the edge back when processing that specific query index.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

int find(vector<int>& parent, int x) {
    if(x == parent[x]) return x;
    return parent[x] = find(parent, parent[x]);
}

void unite(vector<int>& parent, vector<int>& sz, int x, int y, int& components) {
    int px = find(parent, x);
    int py = find(parent, y);
    if(px == py) return;
    if(sz[px] >= sz[py]) {
        parent[py] = px;
        sz[px] += sz[py];
    } else {
        parent[px] = py;
        sz[py] += sz[px];
    }
    components--;
}

void solve() {
    int n, m, q;
    cin >> n >> m >> q;

    vector<pair<int,int>> edges(m+1);
    for(int i = 1; i <= m; i++) cin >> edges[i].first >> edges[i].second;

    vector<pair<int,int>> queries(q);
    vector<int> lastRemoval(m+1, -1);

    for(int i = 0; i < q; i++) {
        int type; cin >> type;
        if(type == 1) {
            int x; cin >> x;
            queries[i] = {1, x};
            lastRemoval[x] = i;
        } else {
            queries[i] = {2, 0};
        }
    }

    vector<int> parent(n+1);
    vector<int> sz(n+1, 1);
    for(int i = 1; i <= n; i++) parent[i] = i;
    int components = n;

    for(int i = 1; i <= m; i++) {
        if(lastRemoval[i] == -1) {
            unite(parent, sz, edges[i].first, edges[i].second, components);
        }
    }

    vector<int> answers;
    for(int i = q-1; i >= 0; i--) {
        if(queries[i].first == 2) {
            answers.push_back(components);
        } else {
            int x = queries[i].second;
            if(lastRemoval[x] == i) {
                unite(parent, sz, edges[x].first, edges[x].second, components);
            }
        }
    }

    reverse(answers.begin(), answers.end());
    for(int ans : answers) cout << ans << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    solve();
    return 0;
}
```
