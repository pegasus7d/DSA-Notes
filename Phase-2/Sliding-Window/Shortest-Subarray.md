---
tags: [sliding-window, two-pointers, form-1-variable-window]
date: 2026-07-03
status: Solved
url: https://maang.in/problems/Shortest-Subarray-57?resourceUrl=cs120-cp545-pl5060-rs57
---

# Shortest Subarray

## Problem

Given array of N integers, find the length of the smallest subarray that contains all distinct elements of the array.

**Constraints:** 1 ≤ T ≤ 10, 1 ≤ N ≤ 10^5, 0 ≤ A[i] ≤ 10^5

## Key Insight ⚡

Form 1 — precompute `total` distinct elements. Expand head until `distinct == total`, then record min length and shrink from left. Opposite of "Longest Distinct Subarray" — here we want minimum window covering all distinct values.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int n; cin >> n;
    vector<int> arr(n);
    for (int i = 0; i < n; i++) cin >> arr[i];

    int total = (int)unordered_set<int>(arr.begin(), arr.end()).size();

    unordered_map<int,int> freq;
    int head = -1, tail = 0;
    int distinct = 0, ans = n;

    while (tail < n) {
        while (head + 1 < n && distinct < total) {
            head++;
            if (freq[arr[head]]++ == 0) distinct++;
        }
        if (distinct == total)
            ans = min(ans, head - tail + 1);
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
