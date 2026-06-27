---
tags: [binary-search, fractional-programming]
date: 2026-06-28
status: Solved
url: https://maang.in/problems/Maximise-the-fraction-105
---

# Maximise the fraction

## Problem

Given two arrays A and B of size N and an integer K. Select K indexes such that `(A[i1]+...+A[iK]) / (B[i1]+...+B[iK])` is maximum. Print the result rounded to 6 decimal places.

**Input Format**
- First line: T (1 ≤ T ≤ 10000) — number of test cases
- Each test case: N, K on first line; N integers A on second line; N integers B on third line
- Sum of N across all test cases ≤ 10^5

**Output Format**
- For each test case print the maximum ratio rounded to 6 decimal places.

**Constraints**
- 1 ≤ T ≤ 10000, 1 ≤ N ≤ 10^4, 1 ≤ K ≤ N
- 1 ≤ A[i], B[i] ≤ 10^4

## Sample Test Cases

**Input:**
```
3
4 2
10 3 7 1
3 4 4 2
...
```

**Output:**
```
2.428571
...
```

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long double ld;

void solve() {
    int n, k;
    cin >> n >> k;
    int a[n], b[n];
    for (int i = 0; i < n; i++) cin >> a[i];
    for (int i = 0; i < n; i++) cin >> b[i];

    ld lo = 0.0, hi = 1e8, ans = 0.0;
    ld eps = 1e-9;  // stop when interval is tiny

    while (hi - lo > eps) {
        ld mid = (lo + hi) / 2.0;

        // keep top K values of a[i] - mid*b[i] using min-heap of size K
        priority_queue<ld, vector<ld>, greater<ld>> pq;
        for (int i = 0; i < n; i++) {
            pq.push(a[i] - mid * b[i]);
            if ((int)pq.size() > k) pq.pop();  // remove smallest, keep top K
        }

        // sum the K largest
        ld s = 0.0;
        while (!pq.empty()) { s += pq.top(); pq.pop(); }

        if (s >= 0.0) { ans = mid; lo = mid; }  // mid achievable, go higher
        else hi = mid;                            // too high, go lower
    }

    cout << fixed << setprecision(6) << ans << endl;
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);
    int t; cin >> t;
    while (t--) solve();
}
```
