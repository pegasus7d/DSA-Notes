---
tags: [stl, stack, map, parsing]
date: 2026-07-01
status: Solved
url: https://maang.in/problems/Molecular-Formula-460
---

# Molecular Formula

## Problem

Parse a chemical formula with nested parentheses. Print element counts sorted alphabetically.

## Intuition

Stack of `map<string, int>`. Push new map on `(`, pop and multiply counts on `)`, add to parent map.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);
    string s; cin >> s;
    int n = s.size(), i = 0;
    stack<map<string,int>> st;
    st.push({});

    while(i < n) {
        if(s[i] == '(') {
            st.push({}); i++;
        } else if(s[i] == ')') {
            i++;
            int num = 0;
            while(i < n && isdigit(s[i])) { num = num*10 + (s[i]-'0'); i++; }
            if(num == 0) num = 1;
            auto top = st.top(); st.pop();
            for(auto& [el, cnt] : top) st.top()[el] += cnt * num;
        } else if(isupper(s[i])) {
            string el = ""; el += s[i++];
            while(i < n && islower(s[i])) el += s[i++];
            int num = 0;
            while(i < n && isdigit(s[i])) { num = num*10 + (s[i]-'0'); i++; }
            if(num == 0) num = 1;
            st.top()[el] += num;
        }
    }

    for(auto& [el, cnt] : st.top()) {
        cout << el;
        if(cnt > 1) cout << cnt;
    }
    cout << endl;
    return 0;
}
```
