---
tags: [binary-tree, bfs, traversal]
date: 2026-07-08
status: Solved
url: https://maang.in/problems/Level-Order-Traversal-576
---

# Level Order Traversal

## Problem

Given the root of a binary tree, return its level order traversal — nodes grouped level by level, left to right. Same serialization/format as the other traversal problems. Complete `vector<vector<int>> getLevelorderTraversal(Node* root)`.

**Input Format**
- Line 1: T — number of test cases
- Per test case: N (serialization length), then N space-separated integers (preorder serialization, `-1` = null)

**Output Format**
- Per test case: each level's values on its own line, followed by a blank line

**Constraints**
- 1 ≤ T ≤ 10^5
- Sum of N across all test cases ≤ 10^7

## Sample Test Cases

**Input 1:**
```
3
3
6004 -1 -1
7
5222 -1 3819 6801 -1 -1 -1
9
8901 7891 5407 -1 -1 1720 -1 -1 -1
```
**Output 1:**
```
6004

5222
3819
6801

8901
7891
5407 1720
```

## Key Insight ⚡

Standard BFS with a queue: process the queue one full level at a time by snapshotting its current size before the inner loop (`sz = q.size()`), so each iteration of the outer loop consumes exactly one level's worth of nodes and pushes the next level's children.

## Complexity

O(n) time, O(w) extra space where w is the tree's maximum width (O(n) worst case).

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
    Node(int x) : val(x), left(nullptr), right(nullptr) {}

    int getVal() const { return val; }
    Node* getLeft() const { return left; }
    Node* getRight() const { return right; }
    void setLeft(Node* l) { left = l; }
    void setRight(Node* r) { right = r; }
};

vector<vector<int>> getLevelorderTraversal(Node* root) {
    vector<vector<int>> result;
    if (!root) return result;
    queue<Node*> q;
    q.push(root);
    while (!q.empty()) {
        int sz = q.size();
        vector<int> level;
        for (int i = 0; i < sz; i++) {
            Node* cur = q.front(); q.pop();
            level.push_back(cur->getVal());
            if (cur->getLeft()) q.push(cur->getLeft());
            if (cur->getRight()) q.push(cur->getRight());
        }
        result.push_back(level);
    }
    return result;
}

Node* getBinaryTree(vector<int>& num, int* ind) {
    if (num[*ind] == -1) return nullptr;
    Node* node = new Node(num[*ind]);
    (*ind)++;
    node->setLeft(getBinaryTree(num, ind));
    (*ind)++;
    node->setRight(getBinaryTree(num, ind));
    return node;
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(0); cout.tie(0);

    int t;
    cin >> t;
    while (t--) {
        int n;
        cin >> n;
        vector<int> arr(n);
        for (int i = 0; i < n; i++) cin >> arr[i];
        int ind = 0;
        Node* tree = getBinaryTree(arr, &ind);
        vector<vector<int>> ans = getLevelorderTraversal(tree);
        for (auto& u : ans) {
            for (auto v : u) cout << v << " ";
            cout << endl;
        }
        cout << endl;
    }
    return 0;
}
```
