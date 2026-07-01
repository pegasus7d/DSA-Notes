---
tags: [stl, deque, sliding-window, monotonic-deque]
date: 2026-07-01
status: Solved
url: https://maang.in/problems/Maximum-in-Window-77
---

# Maximum in Window

## Problem

Given array A of size N and window size K, find the maximum in every window of size K.

**Output:** B[i] = max(A[i..i+K-1]) for all valid i, space-separated.

**Constraints:** 1 ≤ T ≤ 10^5, 1 ≤ K ≤ N ≤ 10^5, -10^9 ≤ Ai ≤ 10^9, sum of N ≤ 5×10^5

## Sample Test Cases

**Input:**
```
4
9 3
1 2 3 1 4 5 2 3 6
5 5
1 -4 3 -3 -9
4 1
-3 1 -8 3
5 2
-1 -1 -1 -1 -1
```
**Output:**
```
3 3 4 5 5 5 6
3
-3 1 -8 3
-1 -1 -1 -1
```

## Intuition

Monotonic deque — store indices in decreasing order of their values. For each new element:
1. Pop front if it's out of the window (index < i - k + 1)
2. Pop back while back element ≤ current (current is a better candidate)
3. Push current index
4. Front of deque = index of max in current window

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int n, k; cin >> n >> k;
    vector<int> a(n);
    for(int i = 0; i < n; i++) cin >> a[i];

    deque<int> dq;
    vector<int> result;
    for(int i = 0; i < n; i++) {
        while(!dq.empty() && dq.front() < i - k + 1) dq.pop_front();
        while(!dq.empty() && a[dq.back()] <= a[i]) dq.pop_back();
        dq.push_back(i);
        if(i >= k - 1) result.push_back(a[dq.front()]);
    }
    for(int i = 0; i < (int)result.size(); i++) {
        if(i) cout << " ";
        cout << result[i];
    }
    cout << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    int t; cin >> t;
    while(t--) solve();
    return 0;
}
```
