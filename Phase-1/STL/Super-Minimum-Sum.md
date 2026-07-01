---
tags: [stl, stack, monotonic-stack, contribution-technique]
date: 2026-07-01
status: Solved
url: https://maang.in/problems/Super-Minimum-Sum-78
---

# Super Minimum Sum

## Problem

Sum of minimums of all subarrays of array A (all elements distinct).

**Constraints:** 1 ≤ T, N ≤ 10^5, 1 ≤ Ai ≤ 10^6, sum of N ≤ 5×10^5

## Sample Test Cases

**Input:** `[1,2,3]→10`, `[3,1,2]→9`

## Intuition

Contribution technique: for each A[i], count how many subarrays have A[i] as minimum.

- `L[i]` = i - (index of nearest smaller to the left), or i+1 if none
- `R[i]` = (index of nearest smaller to the right) - i, or n-i if none
- Count = L[i] × R[i]
- Contribution = A[i] × L[i] × R[i]

Use two monotonic stacks (one forward, one backward) to compute leftSmall and rightSmall in O(N).

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int n; cin >> n;
    vector<int> a(n);
    for(int i = 0; i < n; i++) cin >> a[i];

    vector<int> leftSmall(n, -1), rightSmall(n, n);

    stack<int> st;
    for(int i = 0; i < n; i++) {
        while(!st.empty() && a[st.top()] > a[i]) st.pop();
        leftSmall[i] = st.empty() ? -1 : st.top();
        st.push(i);
    }

    while(!st.empty()) st.pop();

    for(int i = n-1; i >= 0; i--) {
        while(!st.empty() && a[st.top()] > a[i]) st.pop();
        rightSmall[i] = st.empty() ? n : st.top();
        st.push(i);
    }

    int total = 0;
    for(int i = 0; i < n; i++) {
        int L = i - leftSmall[i];
        int R = rightSmall[i] - i;
        total += a[i] * L * R;
    }

    cout << total << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    int t; cin >> t;
    while(t--) solve();
    return 0;
}
```
