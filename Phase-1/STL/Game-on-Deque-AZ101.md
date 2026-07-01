---
tags: [stl, deque, cycle-detection]
date: 2026-07-01
status: Solved
url: https://maang.in/problems/Game-on-Deque-AZ101-385
---

# Game on Deque AZ101

## Problem

N-element deque. Op: take first two, push max to front and min to back. Q queries: what's the pair taken on op X (X up to 10^18)?

## Key Insight ⚡

Once the global maximum reaches position 0, it stays there forever (since max(maxVal, x) = maxVal). After that, period = N-1.

**Algorithm:**
1. Simulate until global max is at front (at most N-1 ops). Let that be step `cycleStart`.
2. Simulate N-1 more steps for one full period.
3. For query X: if X < cycleStart → direct lookup; else → `cycleStart + (X - cycleStart) % (N-1)`.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int n, q; cin >> n >> q;
    deque<int> dq(n);
    for(int i = 0; i < n; i++) cin >> dq[i];

    int maxVal = *max_element(dq.begin(), dq.end());

    vector<pair<int,int>> pairs;
    int cycleStart = -1;

    while(true) {
        int a = dq[0], b = dq[1];
        pairs.push_back({a, b});
        dq.pop_front(); dq.pop_front();
        dq.push_front(max(a, b));
        dq.push_back(min(a, b));

        if(cycleStart == -1 && dq[0] == maxVal)
            cycleStart = (int)pairs.size();
        if(cycleStart != -1 && (int)pairs.size() == cycleStart + (n - 1))
            break;
        if(cycleStart == -1 && (int)pairs.size() >= 2 * n) break;
    }

    int periodLen = (cycleStart == -1) ? (int)pairs.size() : (n - 1);

    while(q--) {
        int x; cin >> x; x--;
        int idx;
        if(x < cycleStart || cycleStart == -1)
            idx = min(x, (int)pairs.size() - 1);
        else
            idx = cycleStart + (x - cycleStart) % periodLen;
        cout << pairs[idx].first << " " << pairs[idx].second << endl;
    }
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    int t; cin >> t;
    while(t--) solve();
    return 0;
}
```
