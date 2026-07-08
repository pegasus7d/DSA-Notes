---
tags: [math, gcd, euclidean-algorithm]
date: 2026-07-08
status: Solved
url: https://maang.in/problems/Piles-of-stones-211
---

# Piles of stones

## Problem

Two piles of stones, `a` and `b`. Repeatedly: pick the pile with more stones, and remove from it a number of stones equal to the count in the *other* pile. Stop when one pile is empty. Find the number of stones left in the non-empty pile. `T` independent test cases.

**Input Format**
- Line 1: T — number of test cases
- Each of the next T lines: two integers `a b`

**Output Format**
- For each test case, the final non-empty pile's stone count

**Constraints**
- 1 ≤ T ≤ 10^5
- 1 ≤ a, b ≤ 10^18

## Sample Test Cases

**Input 1:**
```
2
1 3
4 6
```
**Output 1:**
```
1
2
```

## Key Insight ⚡

This operation is exactly the **subtractive Euclidean algorithm**: repeatedly subtracting the smaller value from the larger is the textbook (slow) way to compute `gcd(a, b)`. The final non-empty pile's value is simply `gcd(a, b)` — no need to simulate the subtraction at all (which would be too slow given values up to 10^18), just use the standard fast GCD (`__gcd`, which internally uses the modulo-based Euclidean algorithm).

## Complexity

O(log(min(a, b))) per test case, O(T log(max value)) overall.

## Code

```cpp
#include <bits/stdc++.h>

using namespace std;
#define int long long
#define endl '\n'


void solve(){
    int a, b;
    cin >> a >> b;
    cout << __gcd(a, b) << endl;
}

signed main(){
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);cout.tie(NULL);
    int _t;cin>>_t;while(_t--)
    solve();
}
```
