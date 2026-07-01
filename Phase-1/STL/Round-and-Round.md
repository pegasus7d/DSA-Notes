---
tags: [stl, simulation, math]
date: 2026-07-01
status: Solved
url: https://maang.in/problems/Round-and-Round-90
---

# Round and Round

## Problem

Bob at (0,0) facing North. Instructions W (move), L (turn left), R (turn right), repeated forever. Goes in circle?

## Key Insight ⚡

After one pass: YES if direction ≠ North OR position = (0,0).
- If direction changes: after ≤4 passes Bob faces north again, and net displacement = 0 per 4 passes → circle.
- If direction stays North but net displacement ≠ 0: drifts forever → NO.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int n; cin >> n;
    string s; cin >> s;

    int dir = 0, x = 0, y = 0;
    int dx[] = {0, 1, 0, -1};  // N, E, S, W
    int dy[] = {1, 0, -1, 0};

    for(char c : s) {
        if(c == 'W') { x += dx[dir]; y += dy[dir]; }
        else if(c == 'L') dir = (dir + 3) % 4;
        else dir = (dir + 1) % 4;
    }

    if(dir != 0 || (x == 0 && y == 0)) cout << "YES" << endl;
    else cout << "NO" << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    int t; cin >> t;
    while(t--) solve();
    return 0;
}
```
