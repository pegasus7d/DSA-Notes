---
tags: [binary-search, binary-search-on-answer]
date: 2026-06-28
status: Solved
url: https://maang.in/problems/Famous-Painter-Partition-Problem-472
---

# Famous Painter Partition Problem

## Problem

We have to paint N boards of length A = {A1, A2, …, AN}. There are K painters available, and each painter takes 1 unit time to paint 1 unit of the board. Find the **minimum time** required to complete this task under the following constraints:

- Two painters cannot share a board.
- A board cannot be partially painted by one painter and partially by another.
- A painter can only paint **contiguous** boards.

**Input Format**
- The first line contains T (1 ≤ T ≤ 100000) — the number of test cases.
- The first line of each test case contains 2 space-separated integers N, K (1 ≤ N ≤ 100000, 1 ≤ K ≤ 100000).
- The second line contains N space-separated integers (0 ≤ xi ≤ 10^9) — the length of the boards.
- Sum of N across all test cases ≤ 10^6.

**Output Format**
- For each test case print the minimum time required in a new line.

**Constraints**
- 1 ≤ T ≤ 10^5
- 1 ≤ N, K ≤ 10^5
- 0 ≤ xi ≤ 10^9
- Sum of N ≤ 10^6

## Sample Test Cases

**Input:**
```
(from sample panel — N=5, K=2, boards [1,2,3,4,5] → answer 9)
```

**Output:**
```
9
```

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

// can k painters finish if each painter can paint at most "mid" units?
bool check(vector<int>& a, int k, int mid) {
    int painters = 1, cur = 0;
    for (int x : a) {
        if (cur + x > mid) {
            painters++;   // assign next painter
            cur = x;      // start fresh with this board
        } else {
            cur += x;
        }
        if (painters > k) return false;
    }
    return true;
}

void solve() {
    int n, k;
    cin >> n >> k;
    vector<int> a(n);
    for (auto& x : a) cin >> x;

    // minimum possible answer = max single board (one painter per board worst case)
    // maximum possible answer = sum of all boards (1 painter does everything)
    int lo = *max_element(a.begin(), a.end());
    int hi = 0;
    for (int x : a) hi += x;

    int ans = hi;
    while (lo <= hi) {
        int mid = lo + (hi - lo) / 2;
        if (check(a, k, mid)) {
            ans = mid;       // feasible, try smaller
            hi = mid - 1;
        } else {
            lo = mid + 1;    // not feasible, try larger
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
