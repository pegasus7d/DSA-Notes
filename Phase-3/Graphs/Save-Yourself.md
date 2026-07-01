---
tags: [graphs, bfs, multisource-bfs, grid]
date: 2026-06-28
status: Solved
url: https://maang.in/problems/Save-Yourself-195
---

# Save Yourself

## Problem

You and monsters are on a grid. Each step you take, monsters also take one step. Reach a boundary cell before any monster reaches the same cell. Find shortest path, or NO if impossible.

**Input Format**
- First line: N M
- N lines of grid: `.` floor, `#` wall, `A` player, `M` monster

**Output Format**
- YES + shortest path length, or NO

**Constraints**
- 1 ≤ N, M ≤ 1000

## Intuition

**Multisource BFS:**
1. BFS from ALL monsters simultaneously → `mdist[i][j]` = min steps any monster takes to reach cell
2. BFS from player → only expand to `(nr,nc)` if `player_dist + 1 < mdist[nr][nc]` (player arrives strictly before any monster)
3. Answer = minimum `pdist` among all reachable boundary cells

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int n, m;
    cin >> n >> m;

    vector<string> grid(n);
    for (int i = 0; i < n; i++) cin >> grid[i];

    int dx[] = {0, 0, 1, -1};
    int dy[] = {1, -1, 0, 0};
    const int INF = 1e18;

    // Multi-source BFS from all monsters
    vector<vector<int>> mdist(n, vector<int>(m, INF));
    queue<pair<int,int>> q;
    int sx = -1, sy = -1;

    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (grid[i][j] == 'M') {
                mdist[i][j] = 0;
                q.push({i, j});
            }
            if (grid[i][j] == 'A') { sx = i; sy = j; }
        }
    }

    while (!q.empty()) {
        int r = q.front().first, c = q.front().second; q.pop();
        for (int d = 0; d < 4; d++) {
            int nr = r + dx[d], nc = c + dy[d];
            if (nr >= 0 && nr < n && nc >= 0 && nc < m && grid[nr][nc] != '#' && mdist[nr][nc] == INF) {
                mdist[nr][nc] = mdist[r][c] + 1;
                q.push({nr, nc});
            }
        }
    }

    // BFS from player - only expand if player arrives before monster
    vector<vector<int>> pdist(n, vector<int>(m, INF));
    pdist[sx][sy] = 0;
    q.push({sx, sy});

    while (!q.empty()) {
        int r = q.front().first, c = q.front().second; q.pop();
        for (int d = 0; d < 4; d++) {
            int nr = r + dx[d], nc = c + dy[d];
            if (nr >= 0 && nr < n && nc >= 0 && nc < m && grid[nr][nc] != '#' && pdist[nr][nc] == INF) {
                int nd = pdist[r][c] + 1;
                if (nd < mdist[nr][nc]) {
                    pdist[nr][nc] = nd;
                    q.push({nr, nc});
                }
            }
        }
    }

    // Check boundary cells
    int ans = INF;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (i == 0 || i == n-1 || j == 0 || j == m-1) {
                if (pdist[i][j] < INF) ans = min(ans, pdist[i][j]);
            }
        }
    }

    if (ans == INF) cout << "NO" << endl;
    else cout << "YES" << endl << ans << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    solve();
}
```
