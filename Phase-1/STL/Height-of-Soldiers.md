---
tags: [stl, stack, monotonic-stack, sliding-window]
date: 2026-07-01
status: Solved
url: https://maang.in/problems/Height-of-Soldiers-88
---

# Height of Soldiers

## Problem

For each window size x (1 to N), find the maximum of the minimums of all windows of size x.

**Constraints:** 1 ≤ T, N ≤ 10^5, sum of N ≤ 5×10^5

## Sample Test Cases

**Input:** `[1,5,3] → 5 3 1`, `[8,6,1,1] → 8 6 1 1`

## Intuition

For each H[i], find:
- L = distance to nearest smaller element on left (using `>=` to handle duplicates)
- R = distance to nearest smaller element on right (using `>`)
- H[i] is minimum of windows up to size L+R-1

Set `ans[L+R-1] = max(ans[L+R-1], H[i])`. Then propagate: `ans[s] = max(ans[s], ans[s+1])` from right to left (a minimum for size s is also valid for all smaller sizes).

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int n; cin >> n;
    vector<int> h(n);
    for(int i = 0; i < n; i++) cin >> h[i];

    vector<int> leftSmall(n, -1), rightSmall(n, n);

    stack<int> st;
    for(int i = 0; i < n; i++) {
        while(!st.empty() && h[st.top()] >= h[i]) st.pop();
        leftSmall[i] = st.empty() ? -1 : st.top();
        st.push(i);
    }

    while(!st.empty()) st.pop();

    for(int i = n-1; i >= 0; i--) {
        while(!st.empty() && h[st.top()] > h[i]) st.pop();
        rightSmall[i] = st.empty() ? n : st.top();
        st.push(i);
    }

    vector<int> ans(n+2, 0);
    for(int i = 0; i < n; i++) {
        int L = i - leftSmall[i];
        int R = rightSmall[i] - i;
        int maxWindow = L + R - 1;
        ans[maxWindow] = max(ans[maxWindow], h[i]);
    }

    for(int s = n-1; s >= 1; s--)
        ans[s] = max(ans[s], ans[s+1]);

    for(int s = 1; s <= n; s++) {
        if(s > 1) cout << " ";
        cout << ans[s];
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
