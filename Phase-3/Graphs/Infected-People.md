---
tags: [graphs, bfs, multisource-bfs, grid]
date: 2026-06-28
status: Solved
url: https://maang.in/problems/Infected-People-199
---

# Infected People

## Problem

Grid with 0 (empty), 1 (healthy person), 2 (infected). Each unit time, infected cells spread to adjacent 1s (not through 0s). Find minimum time to infect all people, or -1 if impossible.

**Input Format**
- First line: N M
- N lines of M integers (0, 1, or 2)

**Output Format**
- Minimum time, or -1

**Constraints**
- 1 ≤ N, M ≤ 1000

## Sample Test Cases

**Input 1:** All 1s reachable from 2s → `2`
**Input 2:** A 1 is blocked by 0s → `-1`

## Intuition

Multisource BFS from all infected cells (2s) simultaneously. Spread only through 1s (skip 0s). Answer = max BFS distance among all 1s. If any 1 has dist = INF → -1.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int n, m;
    cin >> n >> m;

    vector<vector<int>> grid(n, vector<int>(m));
    for (int i = 0; i < n; i++)
        for (int j = 0; j < m; j++)
            cin >> grid[i][j];

    int dx[] = {0, 0, 1, -1};
    int dy[] = {1, -1, 0, 0};
    const int INF = 1e18;

    vector<vector<int>> dist(n, vector<int>(m, INF));
    queue<pair<int,int>> q;

    for (int i = 0; i < n; i++)
        for (int j = 0; j < m; j++)
            if (grid[i][j] == 2) {
                dist[i][j] = 0;
                q.push({i, j});
            }

    while (!q.empty()) {
        int r = q.front().first, c = q.front().second; q.pop();
        for (int d = 0; d < 4; d++) {
            int nr = r + dx[d], nc = c + dy[d];
            if (nr >= 0 && nr < n && nc >= 0 && nc < m &&
                grid[nr][nc] == 1 && dist[nr][nc] == INF) {
                dist[nr][nc] = dist[r][c] + 1;
                q.push({nr, nc});
            }
        }
    }

    int ans = 0;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (grid[i][j] == 1) {
                if (dist[i][j] == INF) { cout << -1 << endl; return; }
                ans = max(ans, dist[i][j]);
            }
        }
    }
    cout << ans << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    solve();
}
```
