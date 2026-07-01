---
tags: [graphs, dfs, grid, flood-fill]
date: 2026-06-28
status: Solved
url: https://maang.in/problems/Find-the-Number-of-Rooms-191
---

# Find the Number of Rooms

## Problem

Given an n×m grid where '.' is floor and '#' is wall. Count the number of rooms — maximal connected groups of floor cells connected via up/down/left/right moves.

**Input Format**
- First line: n, m
- Next n lines: m characters each ('.' or '#')

**Output Format**
- Print the number of rooms.

**Constraints**
- 1 ≤ n, m ≤ 1000

## Sample Test Cases

**Input:**
```
5 8
########
#..#...#
####.#.#
#..#...#
########
```

**Output:**
```
3
```

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;

void dfs(int r, int c, int n, int m, vector<string>& grid, vector<vector<bool>>& visited) {
    if (r < 0 || r >= n || c < 0 || c >= m) return;
    if (visited[r][c] || grid[r][c] == '#') return;
    visited[r][c] = true;
    dfs(r+1, c, n, m, grid, visited);
    dfs(r-1, c, n, m, grid, visited);
    dfs(r, c+1, n, m, grid, visited);
    dfs(r, c-1, n, m, grid, visited);
}

void solve() {
    int n, m;
    cin >> n >> m;
    vector<string> grid(n);
    vector<vector<bool>> visited(n, vector<bool>(m, false));
    for (int i = 0; i < n; i++) cin >> grid[i];

    int rooms = 0;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            if (!visited[i][j] && grid[i][j] == '.') {
                dfs(i, j, n, m, grid, visited);
                rooms++;
            }
        }
    }
    cout << rooms << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    solve();
}
```
