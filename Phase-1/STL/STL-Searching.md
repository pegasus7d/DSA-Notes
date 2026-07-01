---
tags: [stl, binary-search, lower-bound, upper-bound]
date: 2026-07-01
status: Solved
url: https://maang.in/problems/STL-Searching-395
---

# STL Searching

## Problem

Array of N integers. Answer Q queries:
1. `1 x` — smallest element ≥ x
2. `2 x` — smallest element > x
3. `3 x` — count of elements ≤ x
4. `4 x` — count of elements < x

Print -1 if answer doesn't exist.

**Input:** T test cases, each with N Q, array, then Q queries  
**Output:** All query answers for a test case on one line (space separated)

**Constraints:** 1 ≤ T, N, Q ≤ 10^5, 1 ≤ Ai, x ≤ 10^6

## Sample Test Cases

**Input:**
```
2
5 4
1 2 2 3 4
1 2
2 2
3 4
3 2
3 2
5 5 5
3 5
4 5
```
**Output:**
```
2 3 5 3
3 0
```

## Intuition

Sort the array. Use STL binary search functions directly:
- `lower_bound(x)` → first element ≥ x (query 1)
- `upper_bound(x)` → first element > x (query 2)
- `upper_bound(x) - begin` → count of elements ≤ x (query 3)
- `lower_bound(x) - begin` → count of elements < x (query 4)

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int n, q;
    cin >> n >> q;
    vector<int> a(n);
    for(int i = 0; i < n; i++) cin >> a[i];
    sort(a.begin(), a.end());

    bool first = true;
    while(q--) {
        int type, x; cin >> type >> x;
        if(!first) cout << " ";
        first = false;

        if(type == 1) {
            auto it = lower_bound(a.begin(), a.end(), x);
            cout << (it == a.end() ? -1 : *it);
        } else if(type == 2) {
            auto it = upper_bound(a.begin(), a.end(), x);
            cout << (it == a.end() ? -1 : *it);
        } else if(type == 3) {
            cout << (upper_bound(a.begin(), a.end(), x) - a.begin());
        } else {
            cout << (lower_bound(a.begin(), a.end(), x) - a.begin());
        }
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
