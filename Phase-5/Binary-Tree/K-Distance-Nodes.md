---
tags: [binary-tree, dfs]
date: 2026-07-09
status: Solved
url: https://maang.in/problems/K-Distance-Nodes-1030
---

# K Distance Nodes

## Problem

Given a binary tree, a target value, and `k`, find all node values at distance `k` from the target node (no parent pointers available). Return them sorted; empty vector if none exist. Complete `vector<int> KDistanceNodes(Node* root, int k, int tar)`, targeting O(N log N) time and O(H) space.

**Input Format**
- Line 1: T — number of test cases
- Per test case: `n k tar`, then the level-order serialization (comma-separated, `x` = null)

**Output Format**
- Per test case: the matching values, sorted, space-separated (empty line if none)

**Constraints**
- 1 ≤ n ≤ 10^4, 0 ≤ k ≤ 20, sum of n across test cases ≤ 10^5
- Node values and `tar` in [1, 10^4], all unique; `tar` is guaranteed to exist

## Sample Test Cases

**Input 1:**
```
10 2 5
1,5,3,2,4,10,6,7,x,9,8,x,x,x,x,x,x,x,x,x,x
```
**Output 1:** `3 7 8 9`

## Key Insight ⚡

Without parent pointers, find the target by recording the **root-to-target path** via DFS (`findPath`, backtracking with `pop_back` on the way out of dead ends). Then walk the path from the target back up to the root: for each ancestor at path-index `i`, the tree-distance from that ancestor down to level `k` (relative to target) is `d = k - distanceFromTarget`. If `d == 0`, that ancestor itself is an answer. If `d > 0`, explore its subtree(s) at depth `d - 1` via a plain DFS (`collectAtDistance`) — but **skip the child that's already on the path** (the one leading back toward the target), since that direction was already covered by earlier (smaller `i`) iterations. This uniformly handles the target itself (`i = m-1`, no "already visited" child to exclude, so both children are explored) and every ancestor above it.

Stress-tested against a brute-force BFS (via explicit parent pointers, treating the tree as an undirected graph) across 2000 random trees up to 30 nodes — 0 mismatches. Also hit a **red herring while stress-testing at scale**: a hand-written large skewed-chain test input crashed locally, but investigation showed the crash was due to my *test generator* producing a malformed/truncated serialization (missing the final null-child token for the deepest node), not a real bug in the solution or the given `deserialize` harness — once the test input was corrected to the proper `2n+1`-token format, N=10,000 in a fully skewed chain ran fine with no crash.

## Complexity

O(n) time (path-find is O(n); the "excluded subtree" DFS visits each non-path node at most once in total across all ancestors) and O(h) extra space for the path, satisfying the problem's own O(N log N) / O(H) target.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;

#define ll int64_t
#define endl '\n'

class Node {
private:
    int data;
    Node* left;
    Node* right;
public:
    Node(int x) : data(x), left(nullptr), right(nullptr) {}

    int getData() const { return data; }
    Node* getLeft() const { return left; }
    Node* getRight() const { return right; }
    void setLeft(Node* l) { left = l; }
    void setRight(Node* r) { right = r; }
};

bool findPath(Node* node, int tar, vector<Node*>& path) {
    if (!node) return false;
    path.push_back(node);
    if (node->getData() == tar) return true;
    if (findPath(node->getLeft(), tar, path) || findPath(node->getRight(), tar, path)) return true;
    path.pop_back();
    return false;
}

void collectAtDistance(Node* node, int dist, vector<int>& result) {
    if (!node) return;
    if (dist == 0) { result.push_back(node->getData()); return; }
    collectAtDistance(node->getLeft(), dist - 1, result);
    collectAtDistance(node->getRight(), dist - 1, result);
}

vector<int> KDistanceNodes(Node* root, int k, int tar) {
    vector<Node*> path;
    findPath(root, tar, path);
    vector<int> result;
    int m = path.size();
    for (int i = 0; i < m; i++) {
        int d = k - (m - 1 - i);
        if (d < 0) continue;
        if (d == 0) { result.push_back(path[i]->getData()); continue; }
        Node* exclude = (i + 1 < m) ? path[i + 1] : nullptr;
        if (path[i]->getLeft() != exclude) collectAtDistance(path[i]->getLeft(), d - 1, result);
        if (path[i]->getRight() != exclude) collectAtDistance(path[i]->getRight(), d - 1, result);
    }
    sort(result.begin(), result.end());
    return result;
}

Node* deserialize(string data) {
    if (data.size() == 0) return nullptr;
    vector<string> dat;
    string t;
    for (auto c : data) {
        if (c == ',') { dat.push_back(t); t.clear(); }
        else t.push_back(c);
    }
    dat.push_back(t);
    int i = 0;
    queue<Node*> q;
    auto root = new Node(stoll(dat[0]));
    q.push(root);
    i++;
    while (!q.empty()) {
        auto x = q.front();
        q.pop();
        if (dat[i] != "x") {
            Node* l = new Node(stoll(dat[i]));
            x->setLeft(l);
            q.push(l);
        }
        i++;
        if (dat[i] != "x") {
            Node* r = new Node(stoll(dat[i]));
            x->setRight(r);
            q.push(r);
        }
        i++;
    }
    return root;
}

void solve() {
    int n, k, tar;
    cin >> n >> k >> tar;
    string s;
    cin >> s;
    auto root = deserialize(s);
    auto ans = KDistanceNodes(root, k, tar);
    for (auto x : ans) cout << x << " ";
    cout << endl;
}

int main() {
    ios_base::sync_with_stdio(0);
    cin.tie(nullptr); cout.tie(nullptr);
    int t;
    cin >> t;
    while (t--) solve();
    return 0;
}
```
