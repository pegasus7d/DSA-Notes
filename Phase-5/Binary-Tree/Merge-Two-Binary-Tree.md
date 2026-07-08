---
tags: [binary-tree, dfs]
date: 2026-07-08
status: Solved
url: https://maang.in/problems/Merge-Two-Binary-Tree-675
---

# Merge Two Binary Tree

## Problem

Merge two binary trees: where nodes overlap, sum their values; where only one has a node, use it directly. Complete `Node* getMergedTree(Node* t1, Node* t2)`.

**Input Format**
- Line 1: T — number of test cases
- Per test case: N (first tree's serialization length), then N integers (preorder, `-1` = null), then M (second tree's length), then M integers similarly

**Output Format**
- Per test case: the merged tree's preorder serialization, `-1` for nulls

**Constraints**
- 1 ≤ T ≤ 10^5
- 1 ≤ N, M ≤ 10^7 (sum of N and M across test cases ≤ 2×10^7)
- Node values in [-10^9, 10^9]

## Sample Test Cases

**Input 1:**
```
1
11
1496 5959 -1 -1 5566 1754 -1 -1 8678 -1 -1
9
1358 5601 8697 -1 -1 8984 -1 -1 -1
```
**Output 1:** `2854 11560 8697 -1 -1 8984 -1 -1 5566 1754 -1 -1 8678 -1 -1`

## Key Insight ⚡

Simple structural recursion, case by case: if either subtree is null, the result is just the other one (no new node needed — the surviving subtree is reused as-is); otherwise create a new node with the summed value and recursively merge both children pairs.

## Complexity

O(min(n, m)) time — recursion only continues where both trees have nodes; O(min(n, m)) extra space for the new merged nodes.

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

Node* getMergedTree(Node* t1, Node* t2) {
    if (!t1) return t2;
    if (!t2) return t1;
    Node* merged = new Node(t1->getVal() + t2->getVal());
    merged->setLeft(getMergedTree(t1->getLeft(), t2->getLeft()));
    merged->setRight(getMergedTree(t1->getRight(), t2->getRight()));
    return merged;
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

void generateArray(Node* root, vector<int>& ans) {
    if (root == nullptr) {
        ans.push_back(-1);
        return;
    }
    ans.push_back(root->getVal());
    generateArray(root->getLeft(), ans);
    generateArray(root->getRight(), ans);
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
        Node* tree1 = getBinaryTree(arr, &ind);
        arr.clear();
        cin >> n;
        arr.resize(n);
        for (int i = 0; i < n; i++) cin >> arr[i];
        ind = 0;
        Node* tree2 = getBinaryTree(arr, &ind);
        Node* mergedTree = getMergedTree(tree1, tree2);
        vector<int> ans;
        generateArray(mergedTree, ans);
        for (auto v : ans) cout << v << " ";
        cout << endl;
    }
    return 0;
}
```
