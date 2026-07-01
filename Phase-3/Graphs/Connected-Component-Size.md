---
tags: [graphs, bfs, grid, flood-fill]
date: 2026-06-28
status: Solved
url: https://maang.in/problems/Connected-Component-Size-684
---

# Connected Component Size

## Problem

Given a 2D grid of 0s and 1s. 1s are walls. Replace each 0 with the size of its connected component (group of 0s sharing edges). If the component size is 1, leave it as 0.

**Input Format**
- First line: T (test cases)
- Each test case: N M, then N lines of M values

**Output Format**
- Print the modified grid for each test case.

**Constraints**
- 1 ≤ sum of (N×M) ≤ 10^5

## Sample Test Cases

**Input:**
```
2
2 2
0 1
1 0
6 5
1 0 0 1 0
0 1 0 0 0
0 0 1 1 0
0 1 1 0 1
1 1 1 1 1
0 1 0 0 0
```

**Output:**
```
0 1 
1 0 
1 7 7 1 7
4 1 7 7 7
4 4 1 1 7
4 1 1 0 1
1 1 1 1 1
0 1 3 3 3
```

## Intuition

BFS flood fill from each unvisited 0. Collect all cells in the component, then replace all of them with the component size (skip if size == 1).

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

    vector<vector<bool>> visited(n, vector<bool>(m, false));
    int dx[] = {0, 0, 1, -1};
    int dy[] = {1, -1, 0, 0};

    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (grid[i][j] == 0 && !visited[i][j]) {
                queue<pair<int,int>> q;
                vector<pair<int,int>> cells;
                q.push({i, j});
                visited[i][j] = true;
                while (!q.empty()) {
                    auto [r, c] = q.front(); q.pop();
                    cells.push_back({r, c});
                    for (int d = 0; d < 4; d++) {
                        int nr = r + dx[d], nc = c + dy[d];
                        if (nr >= 0 && nr < n && nc >= 0 && nc < m &&
                            !visited[nr][nc] && grid[nr][nc] == 0) {
                            visited[nr][nc] = true;
                            q.push({nr, nc});
                        }
                    }
                }
                int sz = cells.size();
                if (sz > 1) {
                    for (auto [r, c] : cells)
                        grid[r][c] = sz;
                }
            }
        }
    }

    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            cout << grid[i][j];
            if (j < m - 1) cout << " ";
        }
        cout << endl;
    }
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    int t; cin >> t;
    while (t--) solve();
}
```
