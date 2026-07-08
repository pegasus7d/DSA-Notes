---
tags: [binary-tree, sliding-window, bfs]
date: 2026-07-09
status: Solved
url: https://maang.in/problems/Shortest-Range-in-a-BST-1062
---

# Shortest Range in a BST

## Problem

Find the shortest range `[x, y]` such that every level of the BST has at least one node value within the range. Ties (equal `y - x`) are broken by smallest `x`. Complete `pair<int, int> shortestRangeBST(TreeNode* root)`.

**Input Format**
- Line 1: T — number of test cases
- Per test case: n (node count), then the level-order serialization as a comma-separated string, `x` = null

**Output Format**
- Per test case: the range as two space-separated integers

**Constraints**
- 1 ≤ T ≤ 10^4
- 1 ≤ n ≤ 10^5, values in [-10^6, 10^6], all unique
- Sum of n across test cases ≤ 10^6

## Sample Test Cases

**Input 1:**
```
8,3,10,2,6,x,14,x,x,4,7,12,x,x,x,x,x,11,13,x,x,x,x
```
Levels: `[8]`, `[3,10]`, `[2,6,14]`, `[4,7,12]`, `[11,13]`
**Output 1:** `6 11`

## Key Insight ⚡

This is "smallest range covering an element from each of K lists" (LeetCode 632) with each BST *level* acting as one list. BFS to collect each level's values, tag every value with its level index, flatten and sort all `(value, level)` pairs together, then slide a two-pointer window over the sorted list, tracking how many distinct levels are currently covered via a count array. Whenever all levels are covered, try shrinking from the left and record the best `[x, y]` seen.

⚠️ **Real bug hit and fixed**: initialized `bestX = INT_MIN, bestY = INT_MAX` and compared new candidates via `curY - curX < bestY - bestX`. `INT_MAX - INT_MIN` overflows a signed 32-bit int, so that very first comparison silently evaluated to false (undefined-behavior wraparound), and the sliding window's internally-correct best candidate never got written to `bestX`/`bestY` — the function always returned the untouched sentinel values. Debug printing showed the correct window `[6, 11]` being found and printed *inside* the loop, yet the final returned pair was still the sentinels — the comparison itself was the broken link. Fixed by tracking the best gap as a separate `long long bestRange` (initialized to `LLONG_MAX`) instead of deriving it by subtracting two `int` sentinels.

## Complexity

O(n log n) per test case (dominated by sorting all `(value, level)` pairs), O(n) space.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;

#define ll int64_t
#define endl '\n'

class TreeNode {
private:
    int data;
    TreeNode* left;
    TreeNode* right;
public:
    TreeNode(int x) : data(x), left(nullptr), right(nullptr) {}

    int getData() const { return data; }
    TreeNode* getLeft() const { return left; }
    TreeNode* getRight() const { return right; }
    void setLeft(TreeNode* l) { left = l; }
    void setRight(TreeNode* r) { right = r; }
};

class Solution {
public:
    pair<int, int> shortestRangeBST(TreeNode* root) {
        vector<vector<int>> levels;
        queue<TreeNode*> q;
        if (root) q.push(root);
        while (!q.empty()) {
            int sz = q.size();
            vector<int> level;
            for (int i = 0; i < sz; i++) {
                TreeNode* node = q.front(); q.pop();
                level.push_back(node->getData());
                if (node->getLeft()) q.push(node->getLeft());
                if (node->getRight()) q.push(node->getRight());
            }
            levels.push_back(level);
        }
        int numLevels = levels.size();
        vector<pair<int,int>> all;
        for (int i = 0; i < numLevels; i++)
            for (int v : levels[i]) all.push_back({v, i});
        sort(all.begin(), all.end());

        vector<int> cnt(numLevels, 0);
        int covered = 0;
        int left = 0;
        long long bestRange = LLONG_MAX;
        int bestX = 0, bestY = 0;
        for (int right = 0; right < (int)all.size(); right++) {
            int lvl = all[right].second;
            if (cnt[lvl] == 0) covered++;
            cnt[lvl]++;
            while (covered == numLevels) {
                int curX = all[left].first, curY = all[right].first;
                long long curRange = (long long)curY - curX;
                if (curRange < bestRange || (curRange == bestRange && curX < bestX)) {
                    bestRange = curRange;
                    bestX = curX; bestY = curY;
                }
                int llvl = all[left].second;
                cnt[llvl]--;
                if (cnt[llvl] == 0) covered--;
                left++;
            }
        }
        return {bestX, bestY};
    }
};

TreeNode* deserialize(string data) {
    if (data.size() == 0) return nullptr;
    vector<string> dat;
    string t;
    for (auto c : data) {
        if (c == ',') { dat.push_back(t); t.clear(); }
        else t.push_back(c);
    }
    dat.push_back(t);
    int i = 0;
    queue<TreeNode*> q;
    TreeNode* root = new TreeNode(stoll(dat[0]));
    q.push(root);
    i++;
    while (!q.empty()) {
        auto x = q.front();
        q.pop();
        if (dat[i] != "x") {
            TreeNode* l = new TreeNode(stoll(dat[i]));
            x->setLeft(l);
            q.push(l);
        }
        i++;
        if (dat[i] != "x") {
            TreeNode* r = new TreeNode(stoll(dat[i]));
            x->setRight(r);
            q.push(r);
        }
        i++;
    }
    return root;
}

void solve() {
    int n;
    cin >> n;
    string s;
    cin >> s;
    auto root = deserialize(s);
    Solution sol;
    auto ans = sol.shortestRangeBST(root);
    cout << ans.first << " " << ans.second << endl;
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(nullptr); cout.tie(nullptr);

    int t;
    cin >> t;
    while (t--) solve();
    return 0;
}
```
