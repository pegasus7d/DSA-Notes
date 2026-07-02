---
tags: [bit-manipulation, contribution-technique]
date: 2026-07-02
status: Solved
url: https://maang.in/problems/XOR-and-AND-49
---

# XOR and AND

## Problem

Given array of N integers. Find (mod 1e9+7):
1. Sum of XOR of all pairs
2. Sum of XOR of all subsets
3. Sum of AND of all pairs
4. Sum of AND of all subsets

## Key Insight ⚡

Process bit by bit. For bit b, let `cnt` = elements with bit b set.

| Answer | Formula |
|--------|---------|
| XOR pairs | `2^b * cnt * (N-cnt)` |
| XOR subsets | `2^b * 2^(N-1)` if cnt > 0 |
| AND pairs | `2^b * cnt*(cnt-1)/2` |
| AND subsets | `2^b * (2^cnt - 1)` |

**Why XOR subsets = 2^(N-1):** Among all 2^N subsets, exactly half have an odd count of bit-b elements when cnt ≥ 1.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long
const int MOD = 1e9 + 7;

int power(int base, int exp, int mod) {
    int result = 1; base %= mod;
    while(exp > 0) {
        if(exp & 1) result = result * base % mod;
        base = base * base % mod; exp >>= 1;
    }
    return result;
}

void solve() {
    int n; cin >> n;
    vector<int> a(n);
    for(int i = 0; i < n; i++) cin >> a[i];

    int xorPairs = 0, xorSubs = 0, andPairs = 0, andSubs = 0;
    int pow2n1 = power(2, n - 1, MOD);
    int inv2 = power(2, MOD - 2, MOD);

    for(int b = 0; b < 30; b++) {
        int cnt = 0;
        for(int i = 0; i < n; i++) if((a[i] >> b) & 1) cnt++;
        int bitVal = power(2, b, MOD);
        xorPairs = (xorPairs + bitVal * (cnt % MOD) % MOD * ((n - cnt) % MOD)) % MOD;
        if(cnt > 0) xorSubs = (xorSubs + bitVal * pow2n1) % MOD;
        andPairs = (andPairs + bitVal * (cnt % MOD * ((cnt - 1) % MOD) % MOD * inv2 % MOD)) % MOD;
        andSubs = (andSubs + bitVal * ((power(2, cnt, MOD) - 1 + MOD) % MOD)) % MOD;
    }
    cout << xorPairs << " " << xorSubs << " " << andPairs << " " << andSubs << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    int t; cin >> t;
    while(t--) solve();
    return 0;
}
```
