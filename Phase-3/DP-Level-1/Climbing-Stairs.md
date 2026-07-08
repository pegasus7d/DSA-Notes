---
tags: [dp, recursion, greedy]
date: 2026-07-09
status: Solved
url: https://maang.in/problems/Climbing-Stairs-1051
---

# Climbing Stairs

## Problem

Climbing a staircase of `N` steps, where each climb is either 1 step or `M` steps. Find the minimum number of climbs to reach the top.

**Input Format**
- Line 1: T — number of test cases
- Per test case: two integers `N M`

**Output Format**
- Per test case: the minimum number of climbs

**Constraints**
- 1 ≤ T ≤ 10^6
- 1 ≤ M ≤ N ≤ 10^9

## Sample Test Cases

**Input 1:** `5 1` → **Output:** `5`
**Input 2:** `6 4` → **Output:** `3`

## Key Insight ⚡

Although this sits under "Moving from Recursion to DP," the intended top-down recursion doesn't need memoization at all: taking an `M`-step is always at least as good as any combination of smaller climbs (it strictly reduces the remaining distance faster for the same "cost" of 1 climb), so the optimal recursion always takes the `M`-jump whenever possible, never branching to compare against the 1-step alternative.

The naive version of that recursion (`f(n) = 1 + f(n - M)` decrementing by `M` one call at a time) still has O(N/M) depth — for `M = 1` that's up to 10^9, way too deep/slow for `T` up to 10^6 test cases. The fix: reduce by **mod** instead of repeated subtraction — `f(n) = n/M + f(n % M)` — since `n % M < M` always hits the base case on the very next call, this collapses the recursion to O(1) depth regardless of how large `N` is.

## Complexity

O(1) per test case (single recursive call, immediately hits base case via the mod reduction).

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;

long long minClimbs(long long n, long long m) {
    if (n < m) return n;
    return (n / m) + minClimbs(n % m, m);
}

void solve() {
    long long n, m;
    cin >> n >> m;
    cout << minClimbs(n, m) << endl;
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
