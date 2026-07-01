---
tags: [stl, multiset, map, modular-inverse, data-structure]
date: 2026-07-01
status: Solved
url: https://maang.in/problems/Running-Mean-Median-and-Mode-AZ101-391
---

# Running Mean, Median and Mode AZ101

## Problem

Dynamic array with insert/remove. Answer getMean, getMedian, getMode queries. Fractions returned as P * Q^(-1) mod 10^9+7.

**Constraints:** 1 ≤ T, Q ≤ 10^5, sum of Q ≤ 10^5

## Sample Test Cases

**Input:** `insert 4,3,5 → getMean=4, getMedian=4, getMode=3; then insert 4, remove 3, insert 5 → getMean=500000008 (4.5), getMedian=500000008, getMode=4`

## Intuition

- **Mean**: track running sum mod MOD, divide by count using modular inverse
- **Median**: two multisets (lo = lower half, hi = upper half). Keep lo.size() == hi.size() or lo.size() == hi.size()+1. Median = lo.back() if odd, (lo.back()+hi.front())/2 if even
- **Mode**: `map<freq, set<element>>` — mode = smallest element in highest-frequency bucket

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
        base = base * base % mod;
        exp >>= 1;
    }
    return result;
}

struct MedianDS {
    multiset<int> lo, hi;
    void rebalance() {
        while(lo.size() > hi.size() + 1) {
            hi.insert(*lo.rbegin());
            lo.erase(lo.find(*lo.rbegin()));
        }
        while(hi.size() > lo.size()) {
            lo.insert(*hi.begin());
            hi.erase(hi.begin());
        }
    }
    void insert(int x) {
        if(lo.empty() || x <= *lo.rbegin()) lo.insert(x);
        else hi.insert(x);
        rebalance();
    }
    void remove(int x) {
        auto it = lo.find(x);
        if(it != lo.end()) lo.erase(it);
        else hi.erase(hi.find(x));
        rebalance();
    }
    int get() {
        if(lo.empty()) return -1;
        int n = lo.size() + hi.size();
        if(n % 2 == 1) return *lo.rbegin() % MOD;
        int a = *lo.rbegin(), b = *hi.begin();
        return (a + b) % MOD * power(2, MOD-2, MOD) % MOD;
    }
};

void solve() {
    int q; cin >> q;
    MedianDS md;
    map<int,int> freq;
    map<int,set<int>> byFreq;
    int total = 0, sum = 0;

    while(q--) {
        string op; cin >> op;
        if(op == "insert") {
            int x; cin >> x;
            if(freq[x] > 0) byFreq[freq[x]].erase(x);
            freq[x]++; byFreq[freq[x]].insert(x);
            md.insert(x);
            sum = (sum + x % MOD) % MOD; total++;
        } else if(op == "remove") {
            int x; cin >> x;
            byFreq[freq[x]].erase(x);
            if(byFreq[freq[x]].empty()) byFreq.erase(freq[x]);
            freq[x]--; if(freq[x] > 0) byFreq[freq[x]].insert(x);
            md.remove(x);
            sum = (sum - x % MOD + MOD) % MOD; total--;
        } else if(op == "getMean") {
            if(total == 0) cout << -1 << endl;
            else cout << sum % MOD * power(total, MOD-2, MOD) % MOD << endl;
        } else if(op == "getMedian") {
            cout << md.get() << endl;
        } else {
            if(total == 0) cout << -1 << endl;
            else cout << *byFreq.rbegin()->second.begin() << endl;
        }
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
