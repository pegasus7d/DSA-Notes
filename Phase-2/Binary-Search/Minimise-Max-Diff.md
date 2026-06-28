---
tags: [binary-search]
date: 2026-06-28
status: Solved
url: https://maang.in/problems/Minimise-Max-Diff-46
---

# Minimise Max Diff

## Problem

You are given N distinct points on the number line in a sorted array A. You can place at most K more points on the line (integer coordinates only). You have to make the maximum separation between any two consecutive points in the final configuration as minimum as possible. Output this minimal value.

Note: You can place the points anywhere you like, but you cannot place more than one point at the same position on the line.

**Input Format**
- The first line contains an integer T, the number of test cases (1 ≤ T ≤ 10000).
- The first line of each test case contains two space-separated integers N, K where 2 ≤ N ≤ 10^5, 0 ≤ K ≤ 10^9.
- The next line contains N space-separated distinct integers Ai (0 ≤ Ai ≤ 1e9).
- The sum of N across all test cases does not exceed 10^6.

**Output Format**
For each test case, output the minimal maximum separation between any two consecutive points possible, in a new line.

**Constraints**
- 1 ≤ T ≤ 10000
- 2 ≤ N ≤ 10^5
- 0 ≤ K ≤ 10^9
- 0 ≤ Ai ≤ 1e9
- Sum of N across all test cases ≤ 10^6

## Sample Test Cases

**Input:**
```
5
5 5
1 2 3 4 5
5 0
1 2 3 4 5
3 2
100 200 230
11 10
1 3 5 7 9 11 13 15 17 19 21
2 5
7 8 10
```

**Output:**
```
1
3
34
1
4
```

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

// Can we make every gap <= mid by adding at most K points?
bool check(vector<int>& A, int K, int mid) {
    int points_needed = 0;
    for (int i = 0; i + 1 < (int)A.size(); i++) {
        int gap = A[i + 1] - A[i];
        // to keep gap <= mid, we need ceil(gap/mid) - 1 extra points
        points_needed += (gap + mid - 1) / mid - 1;
        if (points_needed > K) return false;
    }
    return true;
}

void solve() {
    int n, k;
    cin >> n >> k;
    vector<int> a(n);
    for (auto& x : a) cin >> x;

    int lo = 1;
    int hi = a[n-1] - a[0];
    int ans = hi;

    while (lo <= hi) {
        int mid = lo + (hi - lo) / 2;
        if (check(a, k, mid)) {
            ans = mid;
            hi = mid - 1;
        } else {
            lo = mid + 1;
        }
    }

    cout << ans << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    int t; cin >> t;
    while (t--) solve();
}
```
