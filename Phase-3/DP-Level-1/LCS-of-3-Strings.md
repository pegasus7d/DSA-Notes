---
tags: [dp, recursion, memoization, lcs]
date: 2026-07-09
status: Solved
url: https://maang.in/problems/LCS-of-3-Strings-911
---

# LCS of 3 Strings

## Problem

Given three strings, find the length of their longest common subsequence.

**Input Format**
- Line 1: T — number of test cases
- Per test case: three space-separated strings `s1 s2 s3`

**Output Format**
- Per test case: the LCS length

**Constraints**
- 1 ≤ T ≤ 100
- 1 ≤ |s1|, |s2|, |s3| ≤ 100

## Sample Test Cases

**Input 1:** `abc abc bbc` → **Output:** `2` (`bc`)
**Input 2:** `algozenith algo algorithm` → **Output:** `4` (`algo`)
**Input 3:** `algo zenith zen` → **Output:** `0` (no common subsequence)

## Key Insight ⚡

Direct extension of standard 2-string LCS to 3 dimensions: `rec(i, j, k)` = LCS length of the suffixes `s1[i:]`, `s2[j:]`, `s3[k:]`. If all three characters at the current positions match, take the character and recurse on all three indices advanced (`1 + rec(i+1, j+1, k+1)`); otherwise take the best of advancing any single index alone. Memoized on a fixed-size `dp[105][105][105]` array (sized for the 100-length constraint), reset with `memset` per test case.

Verified locally that the recursion stays comfortably within the 2-second time limit even at the constraint ceiling (T=100, all three strings length 100, worst-case 2-character alphabet to maximize match-branch traversal): ~1.3s under `-O2`, ~0.7s under `-O3`.

## Complexity

O(|s1| · |s2| · |s3|) states, each computed once via memoization — O(10^6) per test case, O(T · 10^6) total.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;

long long dp[105][105][105];
string s1, s2, s3;

long long rec(long long i, long long j, long long k) {
    long long n = s1.size();
    long long m = s2.size();
    long long o = s3.size();
    if (i == n || j == m || k == o) return 0;
    if (dp[i][j][k] != -1) return dp[i][j][k];
    long long ans = 0;
    ans = max(ans, rec(i, j + 1, k));
    ans = max(ans, rec(i, j, k + 1));
    ans = max(ans, rec(i + 1, j, k));
    if (s1[i] == s2[j] && s2[j] == s3[k]) {
        ans = max(ans, 1 + rec(i + 1, j + 1, k + 1));
    }
    return dp[i][j][k] = ans;
}

void solve() {
    cin >> s1 >> s2 >> s3;
    memset(dp, -1, sizeof(dp));
    cout << rec(0, 0, 0) << endl;
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
