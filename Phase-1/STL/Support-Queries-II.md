---
tags: [stl, multiset, two-sets, top-k]
date: 2026-07-01
status: Solved
url: https://maang.in/problems/Support-Queries-II-98
---

# Support Queries II

## Problem

Dynamic structure with insert/remove. Query: sum of top K largest elements (or sum of all if fewer than K).

**Constraints:** 1 ≤ Q, K ≤ 10^5, 0 ≤ x ≤ 10^9

## Sample Test Cases

**Input:** `17 3` then inserts/removes/queries → `14 13 11 11 6 6 0`

## Intuition

Two multisets: `top` (K largest) and `bot` (rest). Track `topSum`.

- **Insert x**: always insert into top, then if top.size() > K move its smallest to bot.
- **Remove x**: if x is in top, remove and refill from bot's max. Else remove from bot.
- **Query**: just print topSum.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int q, k; cin >> q >> k;

    multiset<int> top, bot;
    int topSum = 0;

    while(q--) {
        int type; cin >> type;

        if(type == 1) {
            int x; cin >> x;
            // Always insert into top, then move smallest to bot if oversized
            top.insert(x); topSum += x;
            if((int)top.size() > k) {
                int mn = *top.begin();
                top.erase(top.begin());
                topSum -= mn;
                bot.insert(mn);
            }

        } else if(type == 2) {
            int x; cin >> x;
            auto it = top.find(x);
            if(it != top.end()) {
                top.erase(it); topSum -= x;
                if(!bot.empty()) {
                    int mx = *bot.rbegin();
                    bot.erase(bot.find(mx));
                    top.insert(mx); topSum += mx;
                }
            } else {
                auto it2 = bot.find(x);
                if(it2 != bot.end()) bot.erase(it2);
            }

        } else {
            char c; cin >> c;
            cout << topSum << endl;
        }
    }
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    solve();
    return 0;
}
```
