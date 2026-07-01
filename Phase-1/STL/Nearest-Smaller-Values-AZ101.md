---
tags: [stl, stack, monotonic-stack]
date: 2026-07-01
status: Solved
url: https://maang.in/problems/Nearest-Smaller-Values-AZ101-378
---

# Nearest Smaller Values AZ101

## Problem

For each position i (1-based), find the nearest position to its left with a smaller value. Print 0 if none exists.

**Constraints:** 1 ≤ T, N ≤ 10^5, 1 ≤ Ai ≤ 10^9, sum of N ≤ 10^6

## Sample Test Cases

**Input:**
```
3
5
3 5 1 6 2
4
1 1 2 1
5
1 3 2 6 6
```
**Output:** `0 1 0 3 3 | 0 0 2 0 | 0 1 1 3 3`

## Intuition

Monotonic stack (increasing). For each element, pop all stack elements ≥ current (they can never be the "nearest smaller" for future elements). The answer is the index of what remains on top. Then push current.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int n; cin >> n;
    vector<int> a(n);
    for(int i = 0; i < n; i++) cin >> a[i];

    stack<pair<int,int>> st; // {value, 1-based index}
    vector<int> result(n);

    for(int i = 0; i < n; i++) {
        while(!st.empty() && st.top().first >= a[i]) st.pop();
        result[i] = st.empty() ? 0 : st.top().second;
        st.push({a[i], i+1});
    }

    for(int i = 0; i < n; i++) {
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
