---
tags: [sliding-window, two-pointers, form-1-variable-window]
date: 2026-07-03
status: Solved
url: https://maang.in/problems/Number-of-Subarray-with-sum-at-most-K-59?resourceUrl=cs120-cp545-pl5060-rs59
---

# Number of Subarray with sum at most K

## Problem

Given array of N integers, count subarrays with sum ≤ K.

**Constraints:** 1 ≤ T ≤ 10, 1 ≤ N ≤ 10^5, 0 ≤ K ≤ 10^9, 0 ≤ A[i] ≤ 10^4

## Key Insight ⚡

**Form 1 (Variable Window):** Since A[i] ≥ 0, window sum is monotone — expanding head never decreases sum. For each `tail`, expand `head` as far right as possible while `currSum ≤ K`. All subarrays starting at `tail` ending in `[tail..head]` are valid → `ans += (head - tail + 1)`.

Edge case: single element > K → `tail` surpasses `head`, reset `head = tail - 1`.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int n, k; cin >> n >> k;
    vector<int> arr(n);
    for (int i = 0; i < n; i++) cin >> arr[i];

    int head = -1, tail = 0;
    int currSum = 0, ans = 0;

    while (tail < n) {
        while (head + 1 < n && currSum + arr[head + 1] <= k) {
            head++;
            currSum += arr[head];
        }
        ans += (head - tail + 1);
        if (tail <= head) {
            currSum -= arr[tail];
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
