---
tags: [sliding-window, two-pointers, prefix-sums, multiset]
date: 2026-07-03
status: Solved
url: https://maang.in/problems/K-Odd-Number-51?resourceUrl=cs120-cp545-pl3713-rs51
---

# K Odd Number

## Problem

Given array of N integers, find subarray with at most K odd numbers and maximum sum ≤ D. Print IMPOSSIBLE if none exists.

**Constraints:** 1 ≤ T ≤ 10, 1 ≤ N ≤ 10^5, −10^9 ≤ D ≤ 10^9, −10^4 ≤ A[i] ≤ 10^4

## Key Insight ⚡

Two constraints (odd count + sum ≤ D) with negative elements — can't use simple Form 1.

**Sliding window + multiset:** For each right endpoint r, maintain valid l range [leftL, r] via odd-count sliding window. Then use multiset of prefix[l-1] values to find max sum ≤ D via lower_bound:
- sum[l,r] = pre[r] - pre[l-1] ≤ D → want min pre[l-1] ≥ pre[r] - D → `lower_bound(pre[r] - D)`

O(N log N) per test case.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int n, k, d; cin >> n >> k >> d;
    vector<int> a(n+1);
    for (int i = 1; i <= n; i++) cin >> a[i];

    vector<int> pre(n+1, 0);
    for (int i = 1; i <= n; i++) pre[i] = pre[i-1] + a[i];

    multiset<int> ms;
    int leftL = 1, oddCnt = 0;
    int ans = -2000000000000000000LL;
    bool found = false;

    for (int r = 1; r <= n; r++) {
        ms.insert(pre[r-1]);
        if (a[r] % 2 != 0) oddCnt++;
        while (oddCnt > k) {
            ms.erase(ms.find(pre[leftL-1]));
            if (a[leftL] % 2 != 0) oddCnt--;
            leftL++;
        }
        auto it = ms.lower_bound(pre[r] - d);
        if (it != ms.end()) {
            found = true;
            ans = max(ans, pre[r] - *it);
        }
    }

    if (found) cout << ans << endl;
    else cout << "IMPOSSIBLE" << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);
    int t; cin >> t;
    while(t--) solve();
    return 0;
}
```
