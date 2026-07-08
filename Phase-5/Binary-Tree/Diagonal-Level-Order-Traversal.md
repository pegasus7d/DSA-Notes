---
tags: [binary-tree, bfs]
date: 2026-07-08
status: Solved
url: https://maang.in/problems/Diagonal-Level-Order-Traversal-585
---

# Diagonal Level Order Traversal

## Problem

Given a binary tree, group nodes lying on the same "\"-slope diagonal (a right child continues its parent's diagonal; a left child starts a new, deeper diagonal). Complete `vector<vector<int>> getDiagonalLevelorderTraversal(Node* root)`.

**Input Format**
- Line 1: T — number of test cases
- Per test case: N (serialization length), then N space-separated integers (preorder serialization, `-1` = null)

**Output Format**
- Per test case: each diagonal's values on its own line, followed by a blank line

**Constraints**
- 1 ≤ T ≤ 10^5
- Sum of N across test cases ≤ 10^7

## Sample Test Cases

**Input 1** (first of 11 test cases):
```
9
5 1 7 6 -1 -1 -1 -1 -1
```
**Output 1:**
```
6
7
1
5
```

## Key Insight ⚡

BFS one diagonal at a time: for each node popped off the queue, walk its entire right-child chain in a `while` loop, collecting every value along the way and enqueueing any left children encountered (each left child starts the *next* diagonal group). This is the standard diagonal-traversal trick.

⚠️ **Gotcha**: the problem's own text says diagonals are "printed from the top-most diagonal downward," but the actual expected output orders them **bottom-most first**. Confirmed empirically against all 11 sample test cases — e.g. for input `5 1 7 6 -1 -1 -1 -1 -1` (a pure left-skewed chain `5→1→7→6`), the natural top-down order would be `5/1/7/6`, but the accepted output is the reverse, `6/7/1/5`. Fixed by reversing the result vector before returning. Worth trusting the sample I/O over the prose description when they conflict.

## Complexity

O(n) time, O(w) extra space where w is the number of distinct diagonals (O(n) worst case).

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

vector<vector<int>> getDiagonalLevelorderTraversal(Node* root) {
    vector<vector<int>> result;
    if (!root) return result;
    queue<Node*> q;
    q.push(root);
    while (!q.empty()) {
        int sz = q.size();
        vector<int> diagonal;
        for (int i = 0; i < sz; i++) {
            Node* node = q.front(); q.pop();
            while (node) {
                diagonal.push_back(node->getVal());
                if (node->getLeft()) q.push(node->getLeft());
                node = node->getRight();
            }
        }
        result.push_back(diagonal);
    }
    reverse(result.begin(), result.end());
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
        vector<vector<int>> ans = getDiagonalLevelorderTraversal(tree);
        for (auto& u : ans) {
            for (auto v : u) cout << v << " ";
            cout << endl;
        }
        cout << endl;
    }
    return 0;
}
```
