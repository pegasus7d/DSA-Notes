---
tags: [dp, recursion, memoization, grid-dp]
date: 2026-07-09
status: Solved
url: https://maang.in/problems/Maximum-Area-of-Square-494
---

# Maximum Area of Square

## Problem

Given an `n × m` grid of 0s and 1s, find the area of the largest square containing only 1s.

**Input Format**
- Line 1: T — number of test cases
- Per test case: `n m`, then `n` lines of `m` space-separated grid values

**Output Format**
- Per test case: the maximum area

**Constraints**
- 1 ≤ T ≤ 10^5
- 1 ≤ n, m ≤ 10^3
- Sum of `n × m` across test cases ≤ 10^7

## Sample Test Cases

**Input 1:** `2 3` grid `[[0,1,1],[0,0,0]]` → **Output:** `1`
**Input 2:** `3 3` grid `[[1,0,1],[0,1,1],[0,1,1]]` → **Output:** `4` (side-2 square)
**Input 3:** `2 2` grid of all zeros → **Output:** `0`

## Key Insight ⚡

Classic "maximal square" DP: `rec(r, c)` = side length of the largest square whose **bottom-right corner** is at `(r, c)`. If the cell itself is 1, that square can only be as large as `1 + min(rec(r-1,c), rec(r,c-1), rec(r-1,c-1))` — bounded by the smallest of its top, left, and top-left diagonal neighbors' square sizes, since all three must support a square one size smaller for the current cell to extend them into a bigger one.

Recursion depth is a non-issue here despite the grid being up to 1000×1000: the driver loop processes cells in row-major order (`i` outer, `j` inner), so by the time `rec(i, j)` is called, its three dependencies `rec(i-1,j)`, `rec(i,j-1)`, `rec(i-1,j-1)` are always already memoized from earlier driver iterations — each "recursive" call in practice just does an O(1) memo lookup, never a fresh multi-level recursive descent.

Global arrays sized `[1000][1000]` are reused across test cases; only the cells within the current test case's `n × m` bounds are reset to `-1` per test case (via the same loop that reads input), so stale data from a differently-sized previous test case never leaks in — the boundary check (`r/c` against the *current* `n, m`) keeps everything scoped correctly.

## Complexity

O(n·m) per test case (each cell computed once) — O(sum of n·m) ≤ O(10^7) total, comfortably within the 3s limit (verified locally: ~0.07s even for a single 1000×1000 all-ones grid).

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;

int n, m;
int a[1000][1000];
int dp[1000][1000];

int rec(int r, int c) {
    if (r < 0 || r >= n || c < 0 || c >= m) {
        return 0;
    }
    if (a[r][c] == 0) {
        return 0;
    }
    if (r == 0 && c == 0) {
        return 1;
    }
    if (dp[r][c] != -1) {
        return dp[r][c];
    }
    int ans = 1;
    if (a[r][c] == 1) {
        ans += min({rec(r - 1, c), rec(r, c - 1), rec(r - 1, c - 1)});
    }
    return dp[r][c] = ans;
}

void solve() {
    cin >> n >> m;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            cin >> a[i][j];
            dp[i][j] = -1;
        }
    }
    int ans = 0;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            ans = max(ans, rec(i, j));
        }
    }
    cout << (long long)ans * ans << endl;
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr); cout.tie(nullptr);
    int t;
    cin >> t;
    while (t--) solve();
    return 0;
}
```
