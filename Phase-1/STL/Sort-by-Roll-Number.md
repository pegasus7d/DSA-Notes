---
tags: [stl, sorting, pair]
date: 2026-07-01
status: Solved
url: https://maang.in/problems/Sort-by-Roll-Number-352
---

# Sort by Roll Number

## Problem

Given N students with name and roll number, sort by roll number ascending.

**Constraints:** 1 ≤ T, N ≤ 10^5, sum of N ≤ 10^5

## Sample Test Cases

**Input:**
```
2
5
amelia 21
sophia 12
...
```
**Output:** sorted by roll number ascending

## Intuition

Store as `pair<int,string>` (roll, name) so default sort orders by roll. Structured binding for clean output.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int n; cin >> n;
    vector<pair<int,string>> students(n);
    for(int i = 0; i < n; i++) {
        string name; int roll;
        cin >> name >> roll;
        students[i] = {roll, name};
    }
    sort(students.begin(), students.end());
    for(auto& [roll, name] : students)
        cout << name << " " << roll << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    int t; cin >> t;
    while(t--) solve();
    return 0;
}
```
