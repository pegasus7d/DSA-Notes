---
tags: [stl, stack, monotonic-stack, histogram]
date: 2026-07-01
status: Solved
url: https://maang.in/problems/Largest-Rectangle-461
---

# Largest Rectangle

## Problem

Find the largest rectangular area in a histogram (contiguous bars, width=1 each).

**Constraints:** 1 ≤ T ≤ 100, 1 ≤ N ≤ 10^5, 1 ≤ Hi ≤ 10^5

## Sample Test Cases

**Input:** `[2,1,2] → 3`, `[1,2,3,4] → 6`, `[5,4,1,2] → 8`

## Intuition

Monotonic stack (increasing heights). Add sentinel 0 at both ends.

When popping a bar (because current bar is shorter):
- `height` = popped bar's height
- `width` = current index - new stack top - 1
- `area` = height × width

The "new stack top" after pop gives the left boundary (first bar shorter than popped bar).

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int n; cin >> n;
    vector<int> h(n+2);
    h[0] = 0;
    for(int i = 1; i <= n; i++) cin >> h[i];
    h[n+1] = 0;

    stack<int> st;
    int maxArea = 0;

    for(int i = 0; i <= n+1; i++) {
        while(!st.empty() && h[st.top()] > h[i]) {
            int height = h[st.top()]; st.pop();
            int width = i - st.top() - 1;
            maxArea = max(maxArea, height * width);
        }
        st.push(i);
    }

    cout << maxArea << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    int t; cin >> t;
    while(t--) solve();
    return 0;
}
```
