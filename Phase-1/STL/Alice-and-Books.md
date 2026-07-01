---
tags: [stl, h-index, greedy]
date: 2026-07-01
status: Solved
url: https://maang.in/problems/Alice-and-Books-84
---

# Alice and Books

## Problem

After reading each book, find largest B such that at least B books have ≥ B pages (H-Index).

## Key Insight ⚡

H-index only increases (adding a book never decreases it). Maintain:
- `freq[x]` = count of books with exactly x pages
- `aboveB` = count of books with pages ≥ B+1

After adding book with `a` pages: if a ≥ B+1, increment aboveB. While aboveB ≥ B+1: B++, subtract freq[B] from aboveB.

O(N) total since B increases at most N times.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int n; cin >> n;
    vector<int> freq(100001, 0);
    int B = 0, aboveB = 0;

    for(int i = 0; i < n; i++) {
        int a; cin >> a;
        freq[a]++;
        if(a >= B + 1) aboveB++;
        while(aboveB >= B + 1) {
            B++;
            aboveB -= freq[B];
        }
        cout << B;
        if(i < n-1) cout << " ";
    }
    cout << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    int t; cin >> t;
    while(t--) solve();
    return 0;
}
```
