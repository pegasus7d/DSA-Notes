---
tags: [sliding-window]
date: 2026-06-28
status: Solved
url: https://maang.in/problems/Consecutive-one-44
---

# Consecutive one

## Problem

Given a binary array of length N and an integer K, the **score** of an array is defined as the length of the **longest continuous subsegment** consisting only of 1's.

Determine the **maximum score** possible if you are allowed to change (flip) **at most** K elements of the array.

**Input Format**
- The first line contains an integer T, the number of test cases.
- The first line of each test case contains two space-separated integers N, K.
- The next line contains N space-separated integers which are either 0 or 1.

**Output Format**
- For each test case, print the maximum score possible if you can change at most K elements of the array.

**Constraints**
- 1 ≤ T ≤ 10^4
- 1 ≤ N ≤ 10^5
- 0 ≤ K ≤ N
- Sum of N across all test cases ≤ 10^6

## Sample Test Cases

**Input:**
```
5
10 2
1 0 1 1 0 1 1 0 0 1
10 1
1 1 0 1 0 0 0 1 0 0
10 3
1 0 0 1 1 0 1 1 0 1
10 3
1 1 1 0 0 0 1 1 1 1
10 3
1 1 0 0 1 1 0 0 1 1
```

**Output:**
```
7
4
8
10
7
```

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int n, k;
    cin >> n >> k;
    vector<int> a(n);
    for (auto& x : a) cin >> x;

    int l = 0, zeros = 0, ans = 0;
    for (int r = 0; r < n; r++) {
        if (a[r] == 0) zeros++;       // expand window right
        while (zeros > k) {           // too many zeros — shrink from left
            if (a[l] == 0) zeros--;
            l++;
        }
        ans = max(ans, r - l + 1);    // window is valid, update answer
    }
    cout << ans << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    int t; cin >> t;
    while (t--) solve();
}
```
