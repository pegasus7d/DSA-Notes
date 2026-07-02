---
tags: [bit-manipulation, identity]
date: 2026-07-02
status: Solved
url: https://maang.in/problems/Bilkul-ricks-nhi-lena-ka-810
---

# Bilkul ricks nhi lena ka

## Problem

Given `a & b` and `a | b`, find `z = a + b`.

## Key Insight ⚡

`a + b = (a & b) + (a | b)` — always.

**Why:** `a + b = (a XOR b) + 2*(a AND b)` and `a | b = (a XOR b) + (a AND b)`, so `a + b = (a | b) + (a & b)`.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int x, y; cin >> x >> y;
    cout << x + y << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    solve();
    return 0;
}
```
