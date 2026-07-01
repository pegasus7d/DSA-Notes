---
tags: [stl, priority-queue, greedy, math]
date: 2026-07-01
status: Solved
url: https://maang.in/problems/Diversify-the-Array-97
---

# Diversify the Array

## Problem

Diversity = pairs (i,j) with A[i]≠A[j]. Maximize with at most K operations (change any element to any value).

## Intuition

Diversity = N*(N-1)/2 - same_pairs, where same_pairs = Σ C(freq[v], 2).

Minimize same_pairs with K ops. Each op reduces the freq of one value by 1 (adding a new unique value). Greedy: always reduce the highest-freq value using a max-heap.

**Key:** Cap K at total excess (N - distinct_count), since beyond that all elements are distinct.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int n, k; cin >> n >> k;
    map<int,int> freq;
    for(int i = 0; i < n; i++) { int x; cin >> x; freq[x]++; }

    int samePairs = 0, excess = 0;
    priority_queue<int> pq;
    for(auto& [v, f] : freq) {
        samePairs += f * (f-1) / 2;
        excess += f - 1;
        if(f > 1) pq.push(f);
    }

    k = min(k, excess);

    while(k > 0) {
        int f = pq.top(); pq.pop();
        samePairs -= (f - 1);
        k--;
        if(f - 1 > 1) pq.push(f - 1);
    }

    cout << n * (n-1) / 2 - samePairs << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    int t; cin >> t;
    while(t--) solve();
    return 0;
}
```
