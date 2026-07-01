---
tags: [graphs, dsu, mst, kruskal, minimum-spanning-tree]
date: 2026-06-29
status: Solved
url: https://maang.in/problems/Road-Reparation-206
---

# Road Reparation

## Problem

N cities, M roads. Each road has a repair cost. Find the minimum total cost to repair roads so every city is reachable from every other. If impossible, print "IMPOSSIBLE".

**Input Format**
- First line: n m
- Next m lines: a b c (road between a and b with cost c)

**Output Format**
- Minimum total cost, or "IMPOSSIBLE" if graph cannot be connected

**Constraints**
- 1 ≤ n ≤ 10^5, 1 ≤ m ≤ 2×10^5, 1 ≤ c ≤ 10^9

## Sample Test Cases

**Input 1:**
```
5 6
1 2 3
2 3 5
2 4 2
3 4 8
5 1 7
5 4 4
```
**Output 1:** `14`

## Intuition

Minimum Spanning Tree — Kruskal's algorithm. Sort edges by cost, greedily add cheapest edge that connects two different components (using DSU). After processing all edges, if more than 1 component remains → IMPOSSIBLE.

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
    int n, m;
    cin >> n >> m;

    vector<tuple<int,int,int>> edges(m);
    for(int i = 0; i < m; i++) {
        int a, b, c; cin >> a >> b >> c;
        edges[i] = {c, a, b};
    }
    sort(edges.begin(), edges.end());

    vector<int> parent(n+1);
    vector<int> sz(n+1, 1);
    for(int i = 1; i <= n; i++) parent[i] = i;
    int components = n;

    int totalCost = 0;
    for(auto [c, a, b] : edges) {
        if(find(parent, a) != find(parent, b)) {
            unite(parent, sz, a, b, components);
            totalCost += c;
        }
    }

    if(components > 1) cout << "IMPOSSIBLE" << endl;
    else cout << totalCost << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    solve();
    return 0;
}
```
