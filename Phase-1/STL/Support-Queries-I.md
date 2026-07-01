---
tags: [stl, multiset, data-structure]
date: 2026-07-01
status: Solved
url: https://maang.in/problems/Support-Queries-I-83
---

# Support Queries I

## Problem

Design a data structure supporting:
1. `1 x` — add x
2. `2 x` — remove one occurrence of x (if present)
3. `3 ?` — print minimum (-1 if empty)
4. `4 ?` — print maximum (-1 if empty)
5. `5 ?` — print sum (0 if empty)

**Constraints:** 1 ≤ Q ≤ 10^5, 0 ≤ x ≤ 10^9

## Sample Test Cases

**Input:**
```
18
1 5
1 4
1 4
3 ?
4 ?
5 ?
2 4
...
```
**Output:** `4 5 13 4 5 9 5 5 5 -1 -1 0`

## Intuition

Use `multiset<int>` — it keeps elements sorted, supports duplicate insertion, and erases exactly one occurrence with `erase(find(x))`. Track sum separately with a variable.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int q; cin >> q;
    multiset<int> ms;
    int sum = 0;

    while(q--) {
        int type; cin >> type;
        if(type == 1) {
            int x; cin >> x;
            ms.insert(x);
            sum += x;
        } else if(type == 2) {
            int x; cin >> x;
            auto it = ms.find(x);
            if(it != ms.end()) { ms.erase(it); sum -= x; }
        } else {
            char c; cin >> c;
            if(type == 3) cout << (ms.empty() ? -1 : *ms.begin()) << endl;
            else if(type == 4) cout << (ms.empty() ? -1 : *ms.rbegin()) << endl;
            else cout << sum << endl;
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
