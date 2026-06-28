---
tags: [binary-search, binary-search-on-answer]
date: 2026-06-28
status: Solved
url: https://maang.in/problems/Kth-Sum-Value-102
---

# Kth Sum Value

## Problem

Given two arrays A of size N and B of size M and an integer K. Create a new array C of size N×M consisting of A[i]+B[j] for 1 ≤ i ≤ N, 1 ≤ j ≤ M. Find the Kth smallest element in the array C.

**Input Format**
- The first line contains T, the number of test cases (1 ≤ T ≤ 10000).
- Each test case begins with three space-separated integers N, M, K where 1 ≤ N ≤ 10^6, 1 ≤ M ≤ 10^6, 1 ≤ K ≤ N×M.
- The next line contains N space-separated integers representing A1, A2, …, AN where 0 ≤ Ai ≤ 10^4.
- The next line contains M space-separated integers representing B1, B2, …, BM where 0 ≤ Bi ≤ 10^4.
- Sum of min(N, M) across all test cases ≤ 10^5.

**Output Format**
- For each test case print the Kth smallest element in array C.

**Constraints**
- 1 ≤ T ≤ 10000
- 1 ≤ N, M ≤ 10^6
- 1 ≤ K ≤ N×M
- 0 ≤ Ai, Bi ≤ 10^4
- Sum of min(N, M) ≤ 10^5

## Sample Test Cases

**Input:**
```
1
3 3 6
1 2 3
4 5 6
```

**Output:**
```
7
```

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

// count pairs where A[i]+B[j] <= mid
long long countPairs(vector<int>& A, vector<int>& B, int mid) {
    long long cnt = 0;
    for (int x : A)
        cnt += upper_bound(B.begin(), B.end(), mid - x) - B.begin();
    return cnt;
}

void solve() {
    int n, m, k;
    cin >> n >> m >> k;
    vector<int> a(n), b(m);
    for (auto& x : a) cin >> x;
    for (auto& x : b) cin >> x;
    sort(a.begin(), a.end());
    sort(b.begin(), b.end());
    if (a.size() > b.size()) swap(a, b); // iterate over smaller array

    int lo = a[0] + b[0];
    int hi = a.back() + b.back();
    int ans = hi;

    while (lo <= hi) {
        int mid = lo + (hi - lo) / 2;
        if (countPairs(a, b, mid) >= k) {
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
