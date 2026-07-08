---
tags: [binary-tree, dfs, traversal]
date: 2026-07-08
status: Solved
url: https://maang.in/problems/Postorder-Traversal-575
---

# Postorder Traversal

## Problem

Given a binary tree, return the postorder traversal (left, right, root) of its node values. Same serialization/format as Inorder/Preorder Traversal. Complete `vector<int> getPostorderTraversal(Node* root)`.

**Input Format**
- Line 1: T — number of test cases
- Per test case: N (serialization length), then N space-separated integers (preorder serialization, `-1` = null)

**Output Format**
- Per test case: the postorder traversal, space-separated

**Constraints**
- 1 ≤ T ≤ 10^5
- 1 ≤ N ≤ 10^7 (sum across test cases ≤ 10^7)

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
6801 3819 5222
5407 1720 7891 8901
```

## Key Insight ⚡

Two-stack iterative trick: postorder (L, R, root) is the reverse of "root, R, L". Do a preorder-like traversal that pushes **left before right** (so right is visited first, opposite of standard preorder), collecting nodes into a second stack as they're visited; then pop that second stack to get true postorder. Avoids the deep-recursion risk relevant here (N up to 10^7).

## Complexity

O(n) time, O(n) extra space (two stacks).

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

vector<int> getPostorderTraversal(Node* root) {
    vector<int> result;
    if (!root) return result;
    stack<Node*> s1, s2;
    s1.push(root);
    while (!s1.empty()) {
        Node* cur = s1.top(); s1.pop();
        s2.push(cur);
        if (cur->getLeft()) s1.push(cur->getLeft());
        if (cur->getRight()) s1.push(cur->getRight());
    }
    while (!s2.empty()) {
        result.push_back(s2.top()->getVal());
        s2.pop();
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
        vector<int> ans = getPostorderTraversal(tree);
        for (auto v : ans) cout << v << " ";
        cout << endl;
    }
    return 0;
}
```
