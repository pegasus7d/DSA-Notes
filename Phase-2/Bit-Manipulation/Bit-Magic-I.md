---
tags: [bit-manipulation, bitset]
date: 2026-07-02
status: Solved
url: https://maang.in/problems/Bit-Magic-I-884
---

# Bit Magic I

## Problem

60-flag bitset. Perform test/set/clear/flip/all/any/none/count/val operations.

## Key Insight ⚡

60 flags fit in a single `long long`. All operations are O(1) with bitwise ops.

| Op | Code |
|----|------|
| test(i) | `(state >> i) & 1` |
| set(i) | `state \|= (1LL << i)` |
| clear(i) | `state &= ~(1LL << i)` |
| flip(i) | `state ^= (1LL << i)` |
| all | `state == (1LL<<60)-1` |
| any | `state != 0` |
| none | `state == 0` |
| count | `__builtin_popcountll(state)` |
| val | `state` |

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int q; cin >> q;
    long long state = 0;
    long long allOn = (1LL << 60) - 1;

    while(q--) {
        int t; cin >> t;
        if(t == 1)      { int i; cin >> i; cout << ((state >> i) & 1) << "\n"; }
        else if(t == 2) { int i; cin >> i; state |= (1LL << i); }
        else if(t == 3) { int i; cin >> i; state &= ~(1LL << i); }
        else if(t == 4) { int i; cin >> i; state ^= (1LL << i); }
        else if(t == 5) { cout << (state == allOn ? 1 : 0) << "\n"; }
        else if(t == 6) { cout << (state != 0 ? 1 : 0) << "\n"; }
        else if(t == 7) { cout << (state == 0 ? 1 : 0) << "\n"; }
        else if(t == 8) { cout << __builtin_popcountll(state) << "\n"; }
        else if(t == 9) { cout << state << "\n"; }
    }
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);
    solve();
    return 0;
}
```
