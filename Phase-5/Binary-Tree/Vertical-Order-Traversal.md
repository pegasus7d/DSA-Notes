---
tags: [binary-tree, bfs, sorting]
date: 2026-07-08
status: Solved
url: https://maang.in/problems/Vertical-Order-Traversal-1048
---

# Vertical Order Traversal

## Problem

Given a binary tree, return its vertical order traversal. Each node has a `(row, col)` position: root at `(0,0)`, left child at `(row+1, col-1)`, right child at `(row+1, col+1)`. Group nodes by column (left to right); within a column, order top-to-bottom by row, and for ties (same row and column), sort by value. Complete `vector<vector<int>> verticalTraversal(TreeNode* root)` (a method of the given `Solution` class).

Note: this problem uses a **different tree representation** than the others in this course — level-order serialization with commas and `x` for null (`"1,2,3,4,6,5,7,x,x,..."`), parsed by a provided `deserialize` function, not the `-1`-marked preorder format used elsewhere.

**Input Format**
- Line 1: T — number of test cases
- Per test case: one comma-separated level-order serialization string (`x` = null)

**Output Format**
- Per test case: each column's values (top-to-bottom order) on its own line, columns left to right, followed by a blank line

**Constraints**
- 1 ≤ T ≤ 10^4
- 1 ≤ n ≤ 10^5 nodes per tree (sum of n across test cases ≤ 10^6)

## Sample Test Cases

**Input 1:**
```
2
1,2,3,4,6,5,7,x,x,x,x,x,8,x,9,x,x,x,x
3,1,4,7,2,2,x,x,x,x,x,x,x
```
**Output 1:**
```
4
2
1 5 6
3 8
7
9

7
1
3 2 2
4
```

## Key Insight ⚡

BFS while tracking each node's `(row, col)`, bucketing values into a `map<int, vector<...>>` keyed by column (a `map` keeps columns sorted ascending automatically). Within each column's bucket, sort by `(row, value)` — this naturally produces top-to-bottom order, with value as the tiebreaker for nodes sharing the same row and column.

State carried through the BFS queue and per-column buckets uses plain structs (`QItem`, `ColEntry`) rather than `pair`/`tuple`, consistent with avoiding the tuple/brace-init compile-error risk noted in [[IsBst]].

## Complexity

O(n log n) time (dominated by sorting within columns), O(n) extra space.

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

struct QItem { TreeNode* node; int row; int col; };
struct ColEntry { int row; int val; };

class Solution {
public:
    vector<vector<int>> verticalTraversal(TreeNode* root) {
        map<int, vector<ColEntry>> cols;
        if (!root) return {};
        queue<QItem> q;
        QItem start; start.node = root; start.row = 0; start.col = 0;
        q.push(start);
        while (!q.empty()) {
            QItem cur = q.front(); q.pop();
            ColEntry e; e.row = cur.row; e.val = cur.node->getData();
            cols[cur.col].push_back(e);
            if (cur.node->getLeft()) {
                QItem l; l.node = cur.node->getLeft(); l.row = cur.row + 1; l.col = cur.col - 1;
                q.push(l);
            }
            if (cur.node->getRight()) {
                QItem r; r.node = cur.node->getRight(); r.row = cur.row + 1; r.col = cur.col + 1;
                q.push(r);
            }
        }
        vector<vector<int>> result;
        for (auto& kv : cols) {
            vector<ColEntry>& vec = kv.second;
            sort(vec.begin(), vec.end(), [](const ColEntry& a, const ColEntry& b) {
                if (a.row != b.row) return a.row < b.row;
                return a.val < b.val;
            });
            vector<int> col;
            for (auto& e : vec) col.push_back(e.val);
            result.push_back(col);
        }
        return result;
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
    string tr;
    cin >> tr;
    Solution sol;
    auto root = deserialize(tr);
    auto ans = sol.verticalTraversal(root);
    for (auto& x : ans) {
        for (auto y : x) cout << y << " ";
        cout << endl;
    }
    cout << endl;
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
