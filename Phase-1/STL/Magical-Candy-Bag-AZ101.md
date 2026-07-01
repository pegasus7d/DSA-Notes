---
tags: [stl, priority-queue, greedy]
date: 2026-07-01
status: Solved
url: https://maang.in/problems/The-Magical-Candy-Bag-AZ101-365
---

# The Magical Candy Bag AZ101

## Problem

N bags, K minutes. Each minute eat all candies from one bag, then it refills to floor(val/2). Maximize total candies eaten.

## Intuition

Greedy: always eat from the largest bag. Use a max-heap.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int n, k; cin >> n >> k;
    priority_queue<int> pq;
    for(int i = 0; i < n; i++) {
        int x; cin >> x;
        pq.push(x);
    }

    int total = 0;
    while(k-- && !pq.empty()) {
        int top = pq.top(); pq.pop();
        total += top;
        pq.push(top / 2);
    }
    cout << total << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    int t; cin >> t;
    while(t--) solve();
    return 0;
}
```
