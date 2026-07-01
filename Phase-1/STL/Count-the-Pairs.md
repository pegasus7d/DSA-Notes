---
tags: [stl, two-pointers, sorting]
date: 2026-07-01
status: Solved
url: https://maang.in/problems/Count-the-Pairs-396
---

# Count the Pairs

## Problem

Count ordered pairs (i,j), i≠j such that A[i]+A[j] ≤ X. Both (i,j) and (j,i) count.

**Constraints:** 1 ≤ T, N ≤ 10^5, 1 ≤ Ai, X ≤ 10^6, sum of N ≤ 10^5

## Sample Test Cases

**Input:**
```
2
5 4
1 2 2 3 4
3 10
5 5 5
```
**Output:** `8  6`

## Intuition

Sort array. Two pointers (lo, hi). If a[lo]+a[hi] ≤ X, then a[lo] pairs with all (hi-lo) elements from lo+1 to hi — add (hi-lo) unordered pairs, then lo++. Else hi--. Multiply final count by 2 for ordered pairs.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int n, x; cin >> n >> x;
    vector<int> a(n);
    for(int i = 0; i < n; i++) cin >> a[i];
    sort(a.begin(), a.end());

    int lo = 0, hi = n - 1, pairs = 0;
    while(lo < hi) {
        if(a[lo] + a[hi] <= x) {
            pairs += hi - lo;
            lo++;
        } else {
            hi--;
        }
    }
    cout << pairs * 2 << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    int t; cin >> t;
    while(t--) solve();
    return 0;
}
```
