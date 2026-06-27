---
tags: [binary-search]
date: 2026-06-28
status: Solved
url: https://maang.in/problems/Bitonic-Array-107
---

# Bitonic Array

## Problem

Given a bitonic array A of N integers and Q queries. For each query integer K (guaranteed to exist in A), find all positions of K in A. Positions are 1-indexed.

A bitonic array: A[1] < A[2] < ... < A[peak] > A[peak+1] > ... > A[N]

**Input Format**
- First line: T — number of test cases
- Each test case: N, Q on first line; N integers on second line; Q lines each with integer K
- Sum of N and Q across all test cases ≤ 10^6

**Output Format**
- For each query, print positions of K in sorted order on one line.

**Constraints**
- 1 ≤ N ≤ 10^5, 1 ≤ Q ≤ 10^6, -10^9 ≤ A[i], K ≤ 10^9

## Sample Test Cases

**Input:**
```
1
5 3
1 3 5 3 1
3
1
5
```

**Output:**
```
2 4 
1 5 
3 
```

## Code

```cpp
#include<bits/stdc++.h>
using namespace std;

int n, q;
vector<int> arr;

bool check(int i) {
    if (arr[i] > arr[i-1]) return 1;
    else return 0;
}

void solve() {
    cin >> n >> q;
    arr.resize(n);
    for (int i = 0; i < n; i++) cin >> arr[i];

    // find peak: largest index where arr[i] > arr[i-1]
    int lo = 1, hi = n - 1, peak = 0;
    while (lo <= hi) {
        int mid = (lo + hi) / 2;
        if (check(mid)) {
            peak = mid;
            lo = mid + 1;
        } else {
            hi = mid - 1;
        }
    }

    while (q--) {
        int k;
        cin >> k;

        vector<int> res;

        // binary search on ascending left part [0, peak-1]
        lo = 0; hi = peak - 1;
        while (lo <= hi) {
            int mid = (lo + hi) / 2;
            if (arr[mid] == k) { res.push_back(mid + 1); break; }
            else if (arr[mid] > k) hi = mid - 1;
            else lo = mid + 1;
        }

        // binary search on descending right part [peak, n-1]
        lo = peak; hi = n - 1;
        while (lo <= hi) {
            int mid = (lo + hi) / 2;
            if (arr[mid] == k) { res.push_back(mid + 1); break; }
            else if (arr[mid] > k) lo = mid + 1;
            else hi = mid - 1;
        }

        for (auto v : res) cout << v << " ";
        cout << endl;
    }
}

signed main() {
    ios_base::sync_with_stdio(0); cin.tie(0); cout.tie(0);
    int _t; cin >> _t;
    while (_t--) solve();
}
```
