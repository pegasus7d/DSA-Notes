---
tags: [stl, permutation, next_permutation]
date: 2026-07-01
status: Solved
url: https://maang.in/problems/Generating-Permutations-AZ101-366
---

# Generating Permutations AZ101

## Problem

Print all permutations of [1..N] in lexicographic order.

## Key Idea

Use `next_permutation` from STL on sorted array.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    int n; cin >> n;
    vector<int> a(n);
    for(int i = 0; i < n; i++) a[i] = i + 1;

    do {
        for(int i = 0; i < n; i++) {
            if(i > 0) cout << " ";
            cout << a[i];
        }
        cout << endl;
    } while(next_permutation(a.begin(), a.end()));

    return 0;
}
```
