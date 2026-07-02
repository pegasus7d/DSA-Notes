---
tags: [bit-manipulation, prefix-sums, range-queries]
date: 2026-07-02
status: Solved
url: https://maang.in/problems/XOR-AND-OR-Query-50
---

# XOR AND OR Query

## Problem

For each query [L, R], find smallest X1, X2, X3 (each < 2^31) that maximize:
1. Sum of (A[i] XOR X1) over [L,R]
2. Sum of (A[i] OR X2) over [L,R]
3. Sum of (A[i] AND X3) over [L,R]

Output X1 + X2 + X3.

## Key Insight ⚡

**Bits are sum-independent.** Decide each bit independently using prefix counts.

For bit b, let `cnt` = count of elements in [L,R] with bit b set, `len = R-L+1`:

| X | Rule | Reason |
|---|------|--------|
| X1 (XOR) | set bit if `cnt*2 < len` | XOR flips bit; more zeros→flip gains more |
| X2 (OR) | set bit if `cnt < len` | OR sets bit for all; helps when not all already set |
| X3 (AND) | set bit if `cnt > 0` | AND keeps bit; worth setting if at least one has it |

When equal (XOR case: `cnt*2 == len`), don't set — minimizes X1.

## Complexity

Precompute `pre[i][b]` (prefix count of bit b in [1..i]): O(N × 31).  
Each query: O(31). Total: O((N+Q) × 31).

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int n; cin >> n;
    vector<array<int, 31>> pre(n + 1);
    pre[0].fill(0);

    for(int i = 1; i <= n; i++) {
        int a; cin >> a;
        pre[i] = pre[i-1];
        for(int b = 0; b < 31; b++)
            if((a >> b) & 1) pre[i][b]++;
    }

    int q; cin >> q;
    while(q--) {
        int l, r; cin >> l >> r;
        int len = r - l + 1;
        int x1 = 0, x2 = 0, x3 = 0;

        for(int b = 0; b < 31; b++) {
            int cnt = pre[r][b] - pre[l-1][b];
            if(cnt * 2 < len)  x1 |= (1LL << b);
            if(cnt < len)      x2 |= (1LL << b);
            if(cnt > 0)        x3 |= (1LL << b);
        }
        cout << x1 + x2 + x3 << "\n";
    }
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);
    int t; cin >> t;
    while(t--) solve();
    return 0;
}
```

> Use `"\n"` not `endl` when output count is large — `endl` flushes on every line and causes TLE.
