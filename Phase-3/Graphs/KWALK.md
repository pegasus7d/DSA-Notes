---
tags: [graphs, bfs, shortest-path]
date: 2026-06-28
status: Solved
url: https://maang.in/problems/KWALK-607
---

# KWALK

## Problem

Given an N×N chessboard and a knight at (Sx, Sy). Find minimum moves to reach (Fx, Fy). Print -1 if impossible.

**Input Format**
- First line: T (test cases)
- Each test case: N Sx Sy Fx Fy

**Output Format**
- Print minimum moves, or -1 if not possible.

**Constraints**
- 1 ≤ T ≤ 20, 1 ≤ N ≤ 1000

## Sample Test Cases

**Input:**
```
3
6 4 5 1 1
6 3 3 6 6
6 6 1 1 6
```

**Output:**
```
3
2
4
```

## Intuition

BFS from (Sx, Sy) using all 8 knight moves. BFS guarantees shortest path. Return distance when (Fx, Fy) is reached.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int n, sx, sy, fx, fy;
    cin >> n >> sx >> sy >> fx >> fy;

    if (sx == fx && sy == fy) { cout << 0 << endl; return; }

    int dx[] = {-2, -2, -1, -1, 1, 1, 2, 2};
    int dy[] = {-1, 1, -2, 2, -2, 2, -1, 1};

    vector<vector<int>> dist(n + 1, vector<int>(n + 1, -1));
    queue<pair<int,int>> q;
    q.push({sx, sy});
    dist[sx][sy] = 0;

    while (!q.empty()) {
        int r = q.front().first, c = q.front().second;
        q.pop();
        for (int d = 0; d < 8; d++) {
            int nr = r + dx[d], nc = c + dy[d];
            if (nr >= 1 && nr <= n && nc >= 1 && nc <= n && dist[nr][nc] == -1) {
                dist[nr][nc] = dist[r][c] + 1;
                if (nr == fx && nc == fy) { cout << dist[nr][nc] << endl; return; }
                q.push({nr, nc});
            }
        }
    }
    cout << -1 << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    int t; cin >> t;
    while (t--) solve();
}
```
