---
tags: [binary-search, binary-search-on-answer]
date: 2026-06-28
status: Solved
url: https://maang.in/problems/Number-Sum-of-Digit-26
---

# Number & Sum of Digit

## Problem

You are given two integers N and S. Find the number of positive integers X less than or equal to N, such that the difference between X and the sum of its digits (in decimal representation) is not less than S.

i.e. count X in [1..N] where X - sumDigits(X) >= S

**Input Format**
- The first line contains T (1 ≤ T ≤ 10^4) — the number of test cases.
- Each test case contains two space-separated integers N, S where 1 ≤ N ≤ 10^18, 0 ≤ S ≤ 10^18.

**Output Format**
- For each test case print the count of valid X in a new line.

**Constraints**
- 1 ≤ T ≤ 10^4
- 1 ≤ N ≤ 10^18
- 0 ≤ S ≤ 10^18

## Sample Test Cases

**Input:**
```
2
5 4
100 4
```

**Output:**
```
0
91
```

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

// returns sum of digits of x
// e.g. sumDigits(123) = 1+2+3 = 6
int sumDigits(int x) {
    int total = 0;
    while (x > 0) {
        total += x % 10;  // take last digit
        x /= 10;          // remove last digit
    }
    return total;
}

// f(x) = x - sumDigits(x)
// This function is always non-decreasing as x increases
// So we can binary search on it!

void solve() {
    int n, s;
    cin >> n >> s;

    // We want: count of x in [1..N] where x - sumDigits(x) >= S
    // Since f(x) is non-decreasing, all such x form a suffix [first..N]
    // So find the FIRST x where f(x) >= S, then answer = N - first + 1

    int lo = 1, hi = n, first = -1;
    while (lo <= hi) {
        int mid = lo + (hi - lo) / 2;
        if (mid - sumDigits(mid) >= s) {
            first = mid;      // this works, try smaller
            hi = mid - 1;
        } else {
            lo = mid + 1;     // need bigger x
        }
    }

    if (first == -1) cout << 0 << endl;       // no x satisfies condition
    else cout << n - first + 1 << endl;        // all x from first to n
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    int t; cin >> t;
    while (t--) solve();
}
```
