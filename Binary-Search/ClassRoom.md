---
tags: [binary-search, binary-search-on-answer]
date: 2026-06-28
status: Solved
url: https://maang.in/problems/ClassRoom-471
---

# ClassRoom

## Problem

Vivek has built a new classroom with N seats. The seats are located along a straight line at positions x1, x2, …, xN. Vivek has to assign seats to K students such that a seat can be assigned to at most 1 student and the minimum distance between any two students is as large as possible. Find the largest minimum distance possible.

**Input Format**
- The first line contains a single integer T (1 ≤ T ≤ 100000) — the number of test cases.
- The first line of each test case contains 2 space-separated integers N, K (2 ≤ N ≤ 100000, 2 ≤ K ≤ N).
- The second line contains N space-separated integers (0 ≤ xi ≤ 10^9) — the position of the seats. All positions are distinct.
- Sum of N across all test cases ≤ 10^6.

**Output Format**
- For each test case print the largest minimum distance possible in a new line.

**Constraints**
- 2 ≤ N ≤ 10^5
- 2 ≤ K ≤ N
- 0 ≤ xi ≤ 10^9
- Sum of N ≤ 10^6

## Sample Test Cases

**Input:**
```
14
13 3
6048 2794 6123 1643 6907 6993 2382 6961 1094 488 7424 6469 6009
10 6
469 706 278 545 711 386 298 242 385 316
...
```

**Output:**
```
(as shown in sample test panel)
```

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

// can we place k students with minimum gap >= mid?
bool check(vector<int>& x, int k, int mid) {
    int count = 1, last = x[0];
    for (int i = 1; i < (int)x.size(); i++) {
        if (x[i] - last >= mid) {
            count++;
            last = x[i];
            if (count >= k) return true;
        }
    }
    return count >= k;
}

void solve() {
    int n, k;
    cin >> n >> k;
    vector<int> x(n);
    for (auto& v : x) cin >> v;
    sort(x.begin(), x.end());

    int lo = 1, hi = x[n-1] - x[0], ans = 0;
    while (lo <= hi) {
        int mid = lo + (hi - lo) / 2;
        if (check(x, k, mid)) {
            ans = mid;
            lo = mid + 1;  // feasible, try larger
        } else {
            hi = mid - 1;  // not feasible, try smaller
        }
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
