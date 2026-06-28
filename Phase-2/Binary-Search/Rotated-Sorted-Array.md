---
tags: [binary-search]
date: 2026-06-28
status: Solved
url: https://maang.in/problems/Rotated-Sorted-Array-108
---

# Rotated Sorted Array

## Problem

Given a rotated sorted array with all distinct elements, find the index (0-indexed) of the minimum element.

**Input Format**
- First line: T (1 ≤ T ≤ 10000) — number of test cases
- Each test case: N on first line, then N integers A[i]
- Sum of N across all test cases ≤ 10^6

**Output Format**
- For each test case print the 0-indexed position of the minimum element.

**Constraints**
- 1 ≤ N ≤ 10^5, -10^9 ≤ A[i] ≤ 10^9

## Sample Test Cases

**Input:**
```
5
6
4 5 6 1 2 3
5
3 4 5 1 2
4
2 3 1 4
5
1 2 3 4 5
6
3 4 5 6 1 2
```

**Output:**
```
3
3
2
0
4
```

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int n;
    cin >> n;
    vector<int> a(n);
    for (auto& x : a) cin >> x;

    int lo = 0, hi = n - 1, ans = 0;
    while (lo <= hi) {
        int mid = lo + (hi - lo) / 2;

        if (a[mid] < a[ans]) ans = mid;  // track index of minimum seen so far

        if (a[mid] > a[hi]) lo = mid + 1;  // min is in right half
        else hi = mid - 1;                  // min is in left half (mid already tracked)
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
