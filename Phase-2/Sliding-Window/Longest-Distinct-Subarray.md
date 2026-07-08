---
tags: [sliding-window, two-pointers, form-1-variable-window]
date: 2026-07-03
status: Solved
url: https://maang.in/problems/Longest-Distinct-Subarray-371?resourceUrl=cs120-cp545-pl5060-rs371
---

# Longest Distinct Subarray

## Problem

Given array of N integers, find the length of the longest subarray with all distinct elements.

**Constraints:** 1 ≤ T ≤ 10^5, 1 ≤ N ≤ 10^5, 1 ≤ A[i] ≤ 10^9, sum of N ≤ 10^5

## Key Insight ⚡

Form 1 — expand head while `freq[arr[head+1]] == 0` (no duplicate). Track `ans = max(ans, head - tail + 1)`.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int n; cin >> n;
    vector<int> arr(n);
    for (int i = 0; i < n; i++) cin >> arr[i];

    unordered_map<int,int> freq;
    int head = -1, tail = 0, ans = 0;

    while (tail < n) {
        while (head + 1 < n && freq[arr[head + 1]] == 0) {
            head++;
            freq[arr[head]]++;
        }
        ans = max(ans, head - tail + 1);
        if (tail <= head) {
            freq[arr[tail]]--;
            tail++;
        } else {
            tail++;
            head = tail - 1;
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
