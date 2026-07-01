---
tags: [graphs, bfs, simulation, grid]
date: 2026-06-28
status: Solved
url: https://maang.in/problems/Vertical-Grid-415
---

# Vertical Grid

## Problem

N×10 vertical grid with colored balls (1-9) and empty cells (0). Gravity means no 0 below a ball. Repeatedly: find all connected regions of same color with size ≥ K, remove them all simultaneously, apply gravity, repeat until stable. Output final grid.

**Input Format**
- First line: N K
- N lines of 10 characters (0-9)

**Output Format**
- N lines: final grid state

**Constraints**
- 1 ≤ N ≤ 100, 1 ≤ K ≤ 10N

## Sample Test Cases

**Input:** 6×10 grid with K=3 → balls of color 1 and 2 removed, then color 3 removed after falling → final stable grid

## Intuition

Simulation loop:
1. BFS each unvisited non-zero cell → collect full connected component
2. Mark all components with size ≥ K for removal
3. If nothing marked → break
4. Remove all marked cells simultaneously (set to '0')
5. Gravity: for each column, collect non-zero balls bottom-to-top, place back from bottom

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int n, k;
    cin >> n >> k;

    vector<string> grid(n);
    for (int i = 0; i < n; i++) cin >> grid[i];

    int dx[] = {0, 0, 1, -1};
    int dy[] = {1, -1, 0, 0};

    while (true) {
        vector<vector<bool>> visited(n, vector<bool>(10, false));
        vector<pair<int,int>> toRemove;
        bool changed = false;

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < 10; j++) {
                if (grid[i][j] != '0' && !visited[i][j]) {
                    char color = grid[i][j];
                    queue<pair<int,int>> q;
                    q.push({i, j});
                    visited[i][j] = true;
                    vector<pair<int,int>> comp;
                    comp.push_back({i, j});

                    while (!q.empty()) {
                        int r = q.front().first, c = q.front().second; q.pop();
                        for (int d = 0; d < 4; d++) {
                            int nr = r + dx[d], nc = c + dy[d];
                            if (nr >= 0 && nr < n && nc >= 0 && nc < 10 &&
                                !visited[nr][nc] && grid[nr][nc] == color) {
                                visited[nr][nc] = true;
                                q.push({nr, nc});
                                comp.push_back({nr, nc});
                            }
                        }
                    }

                    if ((int)comp.size() >= k) {
                        for (int x = 0; x < (int)comp.size(); x++) {
                            toRemove.push_back(comp[x]);
                        }
                        changed = true;
                    }
                }
            }
        }

        if (!changed) break;

        for (int x = 0; x < (int)toRemove.size(); x++) {
            grid[toRemove[x].first][toRemove[x].second] = '0';
        }

        for (int j = 0; j < 10; j++) {
            string balls = "";
            for (int i = 0; i < n; i++) {
                if (grid[i][j] != '0') balls += grid[i][j];
            }
            string newCol = string(n - balls.size(), '0') + balls;
            for (int i = 0; i < n; i++) grid[i][j] = newCol[i];
        }
    }

    for (int i = 0; i < n; i++) cout << grid[i] << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    solve();
}
```
