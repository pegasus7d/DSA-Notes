---
tags: [binary-search, binary-search-on-answer]
date: 2026-06-28
status: Solved
url: https://maang.in/problems/Color-Balls-104
---

# Color Balls

## Problem

There are N students, each belonging to a section represented by array A. Form the maximum number of valid study groups where each group must contain at least K students from **different** sections. Each student can join only one group.

**Input Format**
- First line: T (1 ≤ T ≤ 100000) — number of test cases
- Each test case: N and K on first line, then N integers A[i] on second line
- Sum of N across all test cases ≤ 10^6

**Output Format**
- For each test case, print the maximum number of valid groups.

**Constraints**
- 1 ≤ T ≤ 10^5
- 1 ≤ N ≤ 10^5
- 1 ≤ K ≤ N
- 0 ≤ A[i] ≤ 10^9

## Sample Test Cases

**Input:**
```
3
5 5
1 2 3 4 5
5 2
1 2 3 4 5
6 3
1 2 2 1 3 3
```

**Output:**
```
1
2
2
```

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

// can we form m groups, each needing K students from different sections?
// each section with freq f can contribute at most min(f, m) students total
bool check(vector<int>& freq, int k, int m) {
    int total = 0;
    for (int f : freq) total += min(f, m);
    return total >= m * k;  // need at least m*k students across m groups
}

void solve() {
    int n, k;
    cin >> n >> k;

    map<int,int> cnt;
    for (int i = 0; i < n; i++) {
        int x; cin >> x;
        cnt[x]++;
    }

    vector<int> freq;
    for (auto& [val, c] : cnt) freq.push_back(c);

    // binary search on the number of groups
    int lo = 0, hi = n / k, ans = 0;
    while (lo <= hi) {
        int mid = lo + (hi - lo) / 2;
        if (check(freq, k, mid)) {
            ans = mid;       // feasible, try more groups
            lo = mid + 1;
        } else {
            hi = mid - 1;   // too many groups, try fewer
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
