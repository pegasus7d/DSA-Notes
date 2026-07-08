---
tags: [dp, recursion, subset-sum, memoization]
date: 2026-07-09
status: Solved
url: https://maang.in/problems/Subset-Sum-Queries-760
---

# Subset Sum Queries

## Problem

Given an array of size `N` and `Q` queries, for each query return the 0-based indices of a subset of the array whose sum equals the queried value, or `{-1}` if no such subset exists. Complete `vector<vector<int>> subset_queries(vector<int>& arr, vector<int>& queries)`.

**Input Format**
- Line 1: `N Q`
- Line 2: N space-separated array elements
- Line 3: Q space-separated queries

**Output Format**
- The platform harness validates the returned index set against each queried sum internally (re-summing the returned indices) — the actual contract is just: return a valid subset of indices summing to the target, or `{-1}` if impossible.

**Constraints**
- 1 ≤ N ≤ 100
- 1 ≤ Q ≤ 10^5
- 1 ≤ arr[i] ≤ 10^5
- 1 ≤ sum_i ≤ 10^5

## Sample Test Cases

**Input 1:**
```
5 3
1 2 3 4 5
7 16 3
```
For query 7: `{2, 5}` sums to 7 (exists). For query 16: no subset sums to 16 (max possible is 15). For query 3: `{3}` sums to 3 (exists).

## Key Insight ⚡

Standard subset-sum via **top-down recursion with memoization**, shared across all `Q` queries: `canForm(i, sumLeft)` returns whether some subset of `arr[i..]` sums to `sumLeft`, memoized on `dp[i][sumLeft]` (sized `N × (maxQuery+1)`, built once and reused for every query — no need to rebuild per query since the array doesn't change).

Path reconstruction (`reconstruct`) walks the same recursion: at each index, greedily check via `canForm` whether the *remaining* suffix can still hit the target after taking `arr[i]` — if yes, take it and recurse into `sumLeft - arr[i]`; otherwise skip and recurse into the same `sumLeft`. Because `canForm` is already memoized, these reconstruction-time calls are O(1) lookups, not recomputation — the whole reconstruction is O(N).

## Complexity

O(N · maxSum) to build the memo table once, then O(N) per query for reconstruction (the `canForm` checks during reconstruction are O(1) memoized lookups) — O(N · maxSum + Q · N) total.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;

int canForm(int i, int sumLeft, vector<int>& arr, vector<vector<int>>& dp) {
    if (sumLeft == 0) return 1;
    if (i == (int)arr.size() || sumLeft < 0) return 0;
    if (dp[i][sumLeft] != -1) return dp[i][sumLeft];
    int take = canForm(i + 1, sumLeft - arr[i], arr, dp);
    int skip = canForm(i + 1, sumLeft, arr, dp);
    return dp[i][sumLeft] = (take || skip);
}

void reconstruct(int i, int sumLeft, vector<int>& arr, vector<vector<int>>& dp, vector<int>& path) {
    if (sumLeft == 0 || i == (int)arr.size()) return;
    if (sumLeft - arr[i] >= 0 && canForm(i + 1, sumLeft - arr[i], arr, dp)) {
        path.push_back(i);
        reconstruct(i + 1, sumLeft - arr[i], arr, dp, path);
    } else {
        reconstruct(i + 1, sumLeft, arr, dp, path);
    }
}

vector<vector<int>> subset_queries(vector<int>& arr, vector<int>& queries) {
    int n = arr.size();
    int maxSum = *max_element(queries.begin(), queries.end());
    vector<vector<int>> dp(n, vector<int>(maxSum + 1, -1));

    vector<vector<int>> ans;
    for (int target : queries) {
        if (!canForm(0, target, arr, dp)) {
            ans.push_back({-1});
        } else {
            vector<int> path;
            reconstruct(0, target, arr, dp, path);
            ans.push_back(path);
        }
    }
    return ans;
}

void solve() {
    int n, q;
    cin >> n >> q;
    vector<int> arr(n);
    for (int i = 0; i < n; i++) cin >> arr[i];
    vector<int> queries(q);
    for (int i = 0; i < q; i++) cin >> queries[i];

    auto ans = subset_queries(arr, queries);
    for (auto& indices : ans) {
        for (int idx : indices) cout << idx << " ";
        cout << endl;
    }
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr); cout.tie(nullptr);
    solve();
    return 0;
}
```
