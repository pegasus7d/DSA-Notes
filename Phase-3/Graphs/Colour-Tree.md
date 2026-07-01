---
tags: [graphs, tree, greedy]
date: 2026-06-28
status: Solved
url: https://maang.in/problems/Colour-Tree-416
---

# Colour Tree

## Problem

Given a tree with N nodes. Colour each node such that no two adjacent nodes AND no two nodes sharing a common neighbor have the same colour. Find the minimum number of colours required.

**Input Format**
- First line: N
- Next N-1 lines: edges u v

**Output Format**
- Print minimum number of colours.

**Constraints**
- 1 ≤ N ≤ 10^5

## Sample Test Cases

**Input:**
```
4
1 2
2 3
3 4
```

**Output:**
```
3
```

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int n;
    cin >> n;

    vector<int> degree(n + 1, 0);
    for (int i = 0; i < n - 1; i++) {
        int u, v; cin >> u >> v;
        degree[u]++;
        degree[v]++;
    }

    // a node with degree d needs d+1 colors (itself + all neighbors distinct)
    // so answer = max degree + 1
    int maxDeg = *max_element(degree.begin() + 1, degree.end());
    cout << maxDeg + 1 << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    solve();
}
```
