---
tags: [binary-tree, dfs]
date: 2026-07-09
status: Solved
url: https://maang.in/problems/Leaves-Removal-1049
---

# Leaves Removal

## Problem

Repeatedly strip the leaf nodes of a binary tree and print each round's leaves (left to right), until the tree is empty. No starter scaffold given — parse the level-order (comma-separated, `x` = null) input and print the result from scratch.

**Input Format**
- Line 1: T — number of test cases
- Per test case: one line, the level-order serialization (comma-separated, `x` for null)

**Output Format**
- Per test case: one line per removal round (space-separated values, trailing space), followed by a blank line

**Constraints**
- 1 ≤ T ≤ 10^4
- 1 ≤ n ≤ 10^5, value in [1, 10^5]
- Sum of n across test cases ≤ 10^6

## Sample Test Cases

**Input 1:**
```
1,2,3,4,5,x,x,x,x,x,x
```
Tree: `1` with children `2,3`; `2`'s children `4,5`.
**Output 1:**
```
4 5 3
2
1
```

## Key Insight ⚡

"Repeatedly remove leaves and print each round" is equivalent to grouping nodes by **height** (distance to the farthest leaf in their own subtree — leaves have height 0). A node is removed exactly on the round equal to its height. The left-to-right order within each round comes for free from a **post-order DFS**: since post-order always finishes an entire left subtree before visiting the right subtree, appending each node to `buckets[height]` right after computing its height (i.e., after both recursive calls return) naturally preserves left-to-right order within every height group — no separate sort or level-order bookkeeping needed.

Verified recursion is safe even for a fully skewed (right-chain) tree at N = 100,000 — no stack overflow locally, consistent with this judge's default stack tolerating recursion depths well past what an 8MB local stack usually allows for simple traversal problems.

## Complexity

O(n) time and space per test case.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;

class Node {
private:
    int val;
    Node* left;
    Node* right;
public:
    Node(int v) : val(v), left(nullptr), right(nullptr) {}

    int getVal() const { return val; }
    Node* getLeft() const { return left; }
    Node* getRight() const { return right; }
    void setLeft(Node* l) { left = l; }
    void setRight(Node* r) { right = r; }
};

Node* buildFromLevelOrder(vector<string>& tok) {
    if (tok.empty() || tok[0] == "x") return nullptr;
    Node* root = new Node(stoi(tok[0]));
    queue<Node*> q;
    q.push(root);
    size_t i = 1;
    while (!q.empty() && i < tok.size()) {
        Node* cur = q.front(); q.pop();
        if (i < tok.size()) {
            if (tok[i] != "x") {
                Node* l = new Node(stoi(tok[i]));
                cur->setLeft(l);
                q.push(l);
            }
            i++;
        }
        if (i < tok.size()) {
            if (tok[i] != "x") {
                Node* r = new Node(stoi(tok[i]));
                cur->setRight(r);
                q.push(r);
            }
            i++;
        }
    }
    return root;
}

int computeHeight(Node* node, vector<vector<int>>& buckets) {
    if (!node) return -1;
    int lh = computeHeight(node->getLeft(), buckets);
    int rh = computeHeight(node->getRight(), buckets);
    int h = 1 + max(lh, rh);
    if ((int)buckets.size() <= h) buckets.resize(h + 1);
    buckets[h].push_back(node->getVal());
    return h;
}

void solve() {
    string line;
    cin >> line;
    vector<string> tok;
    string cur;
    for (char c : line) {
        if (c == ',') { tok.push_back(cur); cur.clear(); }
        else cur.push_back(c);
    }
    tok.push_back(cur);

    Node* root = buildFromLevelOrder(tok);
    vector<vector<int>> buckets;
    computeHeight(root, buckets);
    for (auto& b : buckets) {
        for (int v : b) cout << v << " ";
        cout << endl;
    }
    cout << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    int t;
    cin >> t;
    while (t--) solve();
}
```
