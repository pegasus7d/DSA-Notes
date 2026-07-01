---
tags: [graphs, floyd-warshall, all-pairs-shortest-path]
date: 2026-06-29
status: Solved
url: https://maang.in/problems/All-Pairs-Shortest-Path-431
---

# All Pairs Shortest Path

## Problem

Directed complete weighted graph (N×N adjacency matrix). Vertices are removed one by one in a given order. Before each removal, print the sum of all-pairs shortest paths among currently active vertices.

**Input Format**
- Line 1: N
- N lines of N integers: adjacency matrix (a[i][i]=0, a[i][j]=edge weight)
- Last line: removal order x[1], x[2], ..., x[N]

**Output Format**
- N space-separated sums (sum before each removal)

**Constraints**
- 1 ≤ N ≤ 500

## Intuition

Removal is hard → reverse it: **add vertices** in reverse removal order, run Floyd-Warshall incrementally.

When vertex v is activated, relax all pairs using v as intermediate:
`A[j][k] = min(A[j][k], A[j][v] + A[v][k])`

Key insight: FW relaxation runs over ALL n vertices (not just active ones), so `A[i][v]` and `A[v][j]` are already correctly updated by previously added vertices when v is activated.

After activating x[i]..x[n-1], active nodes are exactly indices i..n-1 in the order array → sum directly via `A[x[j]][x[k]]` for j,k ∈ [i, n-1].

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long
#define N 510

void solve() {
    int n;
    cin >> n;

    int A[N][N];
    for (int i = 0; i < n; i++)
        for (int j = 0; j < n; j++)
            cin >> A[i][j];

    int x[N];
    for (int i = 0; i < n; i++) { cin >> x[i]; x[i]--; }  // 0-indexed

    vector<int> ans;

    for (int i = n-1; i >= 0; i--) {
        // FW relaxation using x[i] as new intermediate (over all vertices)
        for (int j = 0; j < n; j++)
            for (int k = 0; k < n; k++)
                A[j][k] = min(A[j][k], A[j][x[i]] + A[x[i]][k]);

        // Active nodes are x[i..n-1] — sum directly via order array
        int temp = 0;
        for (int j = i; j < n; j++)
            for (int k = i; k < n; k++)
                temp += A[x[j]][x[k]];
        ans.push_back(temp);
    }

    // Built back-to-front → print in reverse
    for (int i = (int)ans.size()-1; i >= 0; i--)
        cout << ans[i] << " ";
    cout << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    solve();
}
```

## Complexity
- Time: O(N³)
- Space: O(N²)

## Key Insight ⚡
FW relaxation over ALL vertices (not just active) means A[i][v] and A[v][j] are pre-warmed by the time v is activated. No separate initialization step needed.

## Pattern Tag
`floyd-warshall` `reverse-deletion` `incremental-shortest-path`
