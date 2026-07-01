---
tags: [graphs, 0-1-bfs, grid, shortest-path]
date: 2026-06-28
status: Solved
url: https://maang.in/problems/One-Piece-902
---

# One Piece

## Problem

N×M grid where each cell has a wind direction (1=right, 2=left, 3=down, 4=up). Move from (1,1) to (N,M). Moving along wind costs 0; changing a cell's direction costs 1. Find minimum cost.

**Input Format**
- First line: N M
- N lines of M integers (wind directions)

**Output Format**
- Minimum cost

**Constraints**
- 2 ≤ N, M ≤ 1000

## Intuition

0-1 BFS on grid. From each cell, try all 4 moves:
- If cell's wind already points that direction → cost 0 → push_front
- Otherwise → cost 1 (need to change wind) → push_back

`dirs[] = {1,2,3,4}` maps move index to wind value for easy comparison.

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

    // 1=right, 2=left, 3=down, 4=up
    int dx[] = {0, 0, 1, -1};
    int dy[] = {1, -1, 0, 0};
    int dirs[] = {1, 2, 3, 4};  // direction value for each move

    const int INF = 1e18;
    vector<vector<int>> dist(n, vector<int>(m, INF));
    dist[0][0] = 0;
    deque<pair<int,int>> dq;
    dq.push_back({0, 0});

    while (!dq.empty()) {
        int r = dq.front().first, c = dq.front().second;
        dq.pop_front();

        for (int d = 0; d < 4; d++) {
            int nr = r + dx[d], nc = c + dy[d];
            if (nr < 0 || nr >= n || nc < 0 || nc >= m) continue;

            // cost 0 if wind already points this way, else cost 1
            int w = (grid[r][c] == dirs[d]) ? 0 : 1;

            if (dist[r][c] + w < dist[nr][nc]) {
                dist[nr][nc] = dist[r][c] + w;
                if (w == 0) dq.push_front({nr, nc});
                else        dq.push_back({nr, nc});
            }
        }
    }

    cout << dist[n-1][m-1] << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    solve();
}
```
