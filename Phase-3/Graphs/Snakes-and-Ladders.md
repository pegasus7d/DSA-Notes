---
tags: [graphs, bfs, shortest-path, graph-formulation]
date: 2026-06-29
status: Solved
url: https://maang.in/problems/Snakes-and-Ladders-927
---

# Snakes and Ladders

## Problem

Abhishek plays Snakes and Ladders on a 10×10 board (cells 1–100). He can choose any die roll from 1–6 optimally. Find the minimum number of rolls to reach cell 100 from cell 1.

- If a roll would go beyond 100, no move is made.
- Landing at a ladder base → climb to top.
- Landing at a snake mouth → slide to tail.

**Input Format**
- First line: t (test cases)
- For each test case: n (ladders), then n lines of (start, end); m (snakes), then m lines of (start, end)

**Output Format**
- Minimum rolls per test case, or -1 if impossible.

**Constraints**
- 1 ≤ t ≤ 10, 1 ≤ n, m ≤ 15
- Board: 1 to 100; cell 1 and 100 are never snake/ladder starts

## Sample Test Cases

**Input:**
```
2
3
32 62
42 68
12 98
7
95 13
97 25
93 37
79 27
75 19
49 47
67 17
4
8 52
6 80
26 42
2 72
9
51 19
39 11
37 29
81 3
59 5
79 23
53 7
43 33
77 21
```

**Output:**
```
3
5
```

## Intuition

BFS on cells 1–100. From each cell, try all 6 die rolls. Apply snake/ladder teleportation via a `board[]` array. BFS guarantees minimum rolls since each edge (roll) has cost 1.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int board[101];
    for(int i = 1; i <= 100; i++) board[i] = i;

    int n;
    cin >> n;
    while(n--) {
        int a, b; cin >> a >> b;
        board[a] = b;
    }

    int m;
    cin >> m;
    while(m--) {
        int a, b; cin >> a >> b;
        board[a] = b;
    }

    vector<bool> visited(101, false);
    queue<pair<int,int>> q;
    q.push({1, 0});
    visited[1] = true;

    while(!q.empty()) {
        auto [cell, rolls] = q.front(); q.pop();

        for(int die = 1; die <= 6; die++) {
            int next = cell + die;
            if(next > 100) continue;
            next = board[next];

            if(next == 100) {
                cout << rolls + 1 << endl;
                return;
            }

            if(!visited[next]) {
                visited[next] = true;
                q.push({next, rolls + 1});
            }
        }
    }

    cout << -1 << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);

    int t;
    cin >> t;
    while(t--) solve();

    return 0;
}
```
