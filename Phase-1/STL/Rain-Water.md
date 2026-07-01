---
tags: [stl, two-pointers, stack]
date: 2026-07-01
status: Solved
url: https://maang.in/problems/Rain-Water-462
---

# Rain Water

## Problem

Given elevation array, compute total water trapped after rain.

**Constraints:** 1 ≤ T, N ≤ 10^5, 0 ≤ Ai ≤ 10^5

## Sample Test Cases

**Input:** `[3,0,2]→2`, `[2,0,0,4,3]→4`, `[1,2,3,1,5]→2`

## Intuition

Two pointers (l, r). Track leftMax and rightMax. Water at position i = min(leftMax, rightMax) - height[i].

- If a[l] ≤ a[r]: rightMax is guaranteed ≥ leftMax, so water at l = leftMax - a[l]. Move l right.
- Else: water at r = rightMax - a[r]. Move r left.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int n; cin >> n;
    vector<int> a(n);
    for(int i = 0; i < n; i++) cin >> a[i];

    int l = 0, r = n - 1;
    int leftMax = 0, rightMax = 0, water = 0;

    while(l < r) {
        if(a[l] <= a[r]) {
            leftMax = max(leftMax, a[l]);
            water += leftMax - a[l];
            l++;
        } else {
            rightMax = max(rightMax, a[r]);
            water += rightMax - a[r];
            r--;
        }
    }

    cout << water << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    int t; cin >> t;
    while(t--) solve();
    return 0;
}
```
