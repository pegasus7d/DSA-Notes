---
tags: [sliding-window, two-pointers, form-2-converging]
date: 2026-07-03
status: Solved
url: https://maang.in/problems/3-Sum-65?resourceUrl=cs120-cp545-pl5061-rs65
---

# 3 Sum

## Problem

Given array of N integers and a target, find three integers whose sum is closest to target. Print minimum |sum - target|.

**Constraints:** 1 ≤ T ≤ 100, 3 ≤ N ≤ 10^3, −10^18 ≤ target ≤ 10^18, −10^9 ≤ A[i] ≤ 10^9

## Key Insight ⚡

**Form 2 (Converging Two Pointers):** Sort array. For each `i`, use two pointers `l=i+1, r=n-1`. If `sum < target` → `l++`; if `sum > target` → `r--`; if equal → answer is 0.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int n, target; cin >> n >> target;
    vector<int> arr(n);
    for (int i = 0; i < n; i++) cin >> arr[i];

    sort(arr.begin(), arr.end());

    int ans = 2000000000000000000LL;

    for (int i = 0; i < n - 2; i++) {
        int l = i + 1, r = n - 1;
        while (l < r) {
            int s = arr[i] + arr[l] + arr[r];
            ans = min(ans, abs(s - target));
            if (s < target) l++;
            else if (s > target) r--;
            else { cout << 0 << endl; return; }
        }
    }

    cout << ans << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);
    int t; cin >> t;
    while(t--) solve();
    return 0;
}
```
