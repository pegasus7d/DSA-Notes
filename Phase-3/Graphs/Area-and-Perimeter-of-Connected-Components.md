---
tags: [graphs, bfs, grid, connected-components]
date: 2026-06-28
status: Solved
url: https://maang.in/problems/Area-and-Perimeter-of-Connected-Components-422
---

# Area and Perimeter of Connected Components

## Problem

N×N grid of `.` (empty) and `#` (occupied). Find the largest connected component of `#` cells — output its area and perimeter. If tie on area, pick smallest perimeter.

**Perimeter:** number of edges of `#` cells that touch either the grid boundary or an empty cell.

**Input Format**
- First line: N
- N lines of the grid

**Output Format**
- area perimeter

**Constraints**
- 1 ≤ N ≤ 1000

## Sample Test Cases

**Input:** 6×6 grid → largest component has area 13, perimeter 22 → `13 22`

## Intuition

BFS from each unvisited `#`. For each `#` cell popped from queue, check all 4 neighbors:
- If out of bounds or `.` → perimeter += 1
- If unvisited `#` → add to queue

Track best (max area, min perimeter on tie).

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int n;
    cin >> n;

    vector<string> grid(n);
    for (int i = 0; i < n; i++) cin >> grid[i];

    int dx[] = {0, 0, 1, -1};
    int dy[] = {1, -1, 0, 0};

    vector<vector<bool>> visited(n, vector<bool>(n, false));
    int bestArea = -1, bestPerim = -1;

    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            if (grid[i][j] == '#' && !visited[i][j]) {
                queue<pair<int,int>> q;
                q.push({i, j});
                visited[i][j] = true;
                int area = 0, perim = 0;

                while (!q.empty()) {
                    int r = q.front().first, c = q.front().second; q.pop();
                    area++;
                    for (int d = 0; d < 4; d++) {
                        int nr = r + dx[d], nc = c + dy[d];
                        if (nr < 0 || nr >= n || nc < 0 || nc >= n || grid[nr][nc] == '.') {
                            perim++;
                        } else if (!visited[nr][nc]) {
                            visited[nr][nc] = true;
                            q.push({nr, nc});
                        }
                    }
                }

                if (area > bestArea || (area == bestArea && perim < bestPerim)) {
                    bestArea = area;
                    bestPerim = perim;
                }
            }
        }
    }

    cout << bestArea << " " << bestPerim << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    solve();
}
```
