---
tags: [dp, recursion, memoization, lis]
date: 2026-07-09
status: Solved
url: https://maang.in/problems/Count-LIS-919
---

# Count LIS

## Problem

Given an integer array, count the number of longest strictly increasing subsequences, modulo `10^9 + 7`.

**Input Format**
- Line 1: T — number of test cases
- Per test case: `n`, then `n` space-separated integers

**Output Format**
- Per test case: the count of LIS, mod `10^9 + 7`

**Constraints**
- 1 ≤ T ≤ 100
- 1 ≤ n ≤ 1000
- -10^6 ≤ arr[i] ≤ 10^6

## Sample Test Cases

**Input 1:** `1 2 2 3 6` → **Output:** `2` (`1,2,3,6` via either `2`)
**Input 2:** `1 3 5 4 7` → **Output:** `2` (`1,3,5,7` and `1,3,4,7`)
**Input 3:** `1 1 1 1 1 1` → **Output:** `6` (no pair is strictly increasing, so all 6 single-element subsequences tie as "longest")
**Input 4:** `3 1 1 2` → **Output:** `2` (`1,2` via either `1`)

## Key Insight ⚡

Top-down recursion over indices: `rec(level)` returns the length of the longest strictly increasing subsequence *ending at* `level`, while a parallel array `cnt[level]` tracks how many such subsequences achieve that length. Whenever a better (strictly longer) predecessor chain is found via some `prev_taken < level` with `arr[prev_taken] < arr[level]`, `cnt[level]` is **overwritten** (not accumulated) with `cnt[prev_taken]`, discarding any shorter-chain counts found earlier in the loop; when a predecessor *ties* the current best length, its count is added in. `cnt` is pre-filled with `1` per test case (the base case: every element is trivially a length-1 LIS ending at itself) before any `rec` calls run.

The final answer sums `cnt[i]` over every index `i` where `rec(i)` equals the global best length.

## Complexity

O(n²) per test case (each of the n `rec` calls scans all smaller indices once, memoized so each index's work happens exactly once) — fine given n ≤ 1000, T ≤ 100, and a generous 5s limit.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;

const int MOD = 1000000007;
int n;
int arr[1001];
int dp[1001];
int cnt[1001];

int rec(int level) {
    if (level < 0) return 0;
    if (dp[level] != -1) return dp[level];
    int ans = 1;
    for (int prev_taken = 0; prev_taken < level; prev_taken++) {
        if (arr[prev_taken] < arr[level]) {
            if (rec(prev_taken) + 1 > ans) {
                ans = rec(prev_taken) + 1;
                cnt[level] = cnt[prev_taken];
            } else if (rec(prev_taken) + 1 == ans) {
                cnt[level] += cnt[prev_taken];
                cnt[level] %= MOD;
            }
        }
    }
    return dp[level] = ans;
}

void solve() {
    cin >> n;
    for (int i = 0; i < n; i++) cin >> arr[i];

    memset(dp, -1, sizeof(dp));
    fill(cnt, cnt + 1001, 1);
    int best = 1;
    for (int i = 0; i < n; i++) {
        best = max(best, rec(i));
    }
    int ans = 0;
    for (int i = 0; i < n; i++) {
        if (rec(i) == best) {
            ans += cnt[i];
            ans %= MOD;
        }
    }
    cout << ans << endl;
}

int main() {
    ios::sync_with_stdio(0); cin.tie(0); cout.tie(0);
    int t; cin >> t;
    while (t--) solve();
    return 0;
}
```
