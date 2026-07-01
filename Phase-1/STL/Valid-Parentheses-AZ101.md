---
tags: [stl, stack, greedy, parentheses]
date: 2026-07-01
status: Solved
url: https://maang.in/problems/Valid-Parentheses-AZ101-348
---

# Valid Parentheses AZ101

## Problem

Given a string of `(` and `)`, find the minimum number of parentheses to add to make it valid.

**Constraints:** 1 ≤ T ≤ 2×10^5, 1 ≤ |S| ≤ 10^5, sum of |S| ≤ 10^6

## Sample Test Cases

**Input:**
```
3
(())))
()()(()
))((
```
**Output:** `2 1 4`

## Intuition

Track two counters:
- `open` = unmatched `(` so far
- `close` = unmatched `)` so far

For each `(`: open++. For each `)`: if open > 0 then open-- (matched), else close++ (unmatched).

Answer = open + close (each needs one bracket added to fix it).

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    string s; cin >> s;
    int open = 0, close = 0;
    for(char c : s) {
        if(c == '(') open++;
        else {
            if(open > 0) open--;
            else close++;
        }
    }
    cout << open + close << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    int t; cin >> t;
    while(t--) solve();
    return 0;
}
```
