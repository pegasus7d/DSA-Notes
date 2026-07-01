---
tags: [stl, priority-queue, greedy, huffman]
date: 2026-07-01
status: Solved
url: https://maang.in/problems/Reduce-the-Array-AZ101-376
---

# Reduce the Array AZ101

## Problem

Merge any two elements (cost = their sum) until 1 element remains. Find minimum total cost.

## Intuition

Always merge the two smallest elements — classic Huffman coding greedy. Use a min-heap.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int n; cin >> n;
    priority_queue<int, vector<int>, greater<int>> pq;
    for(int i = 0; i < n; i++) {
        int x; cin >> x;
        pq.push(x);
    }

    int cost = 0;
    while(pq.size() > 1) {
        int a = pq.top(); pq.pop();
        int b = pq.top(); pq.pop();
        cost += a + b;
        pq.push(a + b);
    }
    cout << cost << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    int t; cin >> t;
    while(t--) solve();
    return 0;
}
```
