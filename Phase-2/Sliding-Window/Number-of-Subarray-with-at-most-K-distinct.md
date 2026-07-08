---
tags: [sliding-window, two-pointers, form-1-variable-window]
date: 2026-07-03
status: Solved
url: https://maang.in/problems/Number-of-Subarray-with-at-most-K-distinct-60?resourceUrl=cs120-cp545-pl5060-rs60
---

# Number of Subarray with at most K distinct

## Problem

Given array of N integers, count subarrays with at most K distinct elements.

**Constraints:** 1 ≤ T ≤ 10, 1 ≤ N ≤ 10^5, 0 ≤ K ≤ 10^5, 0 ≤ A[i] ≤ 10^6

## Key Insight ⚡

Same Form 1 template as "sum at most K" — just replace `currSum` with `distinct` count tracked via frequency map. Expand head while adding next element won't push distinct above K.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int n, k; cin >> n >> k;
    vector<int> arr(n);
    for (int i = 0; i < n; i++) cin >> arr[i];

    unordered_map<int,int> freq;
    int head = -1, tail = 0;
    int distinct = 0, ans = 0;

    while (tail < n) {
        while (head + 1 < n) {
            int val = arr[head + 1];
            if (freq[val] == 0 && distinct == k) break;
            head++;
            if (freq[arr[head]]++ == 0) distinct++;
        }
        ans += (head - tail + 1);
        if (tail <= head) {
            if (--freq[arr[tail]] == 0) distinct--;
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
