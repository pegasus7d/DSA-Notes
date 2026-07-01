---
tags: [graphs, dsu, union-find]
date: 2026-06-29
status: Solved
url: https://maang.in/problems/Disjoint-Union-40
---

# Disjoint Union

## Problem

Implement a Disjoint Set Union (DSU) structure for n elements (0 to n-1). Process q queries: unite two sets or check if two elements are in the same set.

**Input Format**
- First line: n q
- Next q lines: com x y where com=0 means unite(x,y), com=1 means same(x,y)

**Output Format**
- For each same query, print 1 if in same set, 0 otherwise

**Constraints**
- 1 ≤ n ≤ 10^4, 1 ≤ q ≤ 10^5

## Sample Test Cases

**Input 1:**
```
5 12
0 1 4
0 2 3
1 1 2
1 3 4
1 1 4
1 3 2
0 1 3
1 2 4
1 3 0
0 0 4
1 0 2
1 3 0
```
**Output 1:**
```
0
0
1
1
1
0
1
1
```

## Intuition

DSU with path compression + union by size. `find()` flattens the tree as it traverses (path compression). `unite()` always attaches smaller tree under larger (union by size). Both together give nearly O(1) amortized per query.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

int find(vector<int>& parent, int x) {
    if(x == parent[x]) return x;
    return parent[x] = find(parent, parent[x]);  // path compression
}

void unite(vector<int>& parent, vector<int>& sz, int x, int y) {
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
}

void solve() {
    int n, q;
    cin >> n >> q;

    vector<int> parent(n);
    vector<int> sz(n, 1);
    for(int i = 0; i < n; i++) parent[i] = i;

    while(q--) {
        int com, x, y;
        cin >> com >> x >> y;
        if(com == 0) {
            unite(parent, sz, x, y);
        } else {
            if(find(parent, x) == find(parent, y)) cout << 1 << endl;
            else cout << 0 << endl;
        }
    }
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    solve();
    return 0;
}
```
