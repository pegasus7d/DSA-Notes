---
tags: [binary-tree, dfs, tree-construction]
date: 2026-07-08
status: Solved
url: https://maang.in/problems/Tree-from-inorder-and-preorder-651
---

# Tree from inorder and preorder

## Problem

Reconstruct a binary tree from its inorder and preorder traversals (all node values distinct); output the reconstructed tree's preorder serialization (`-1` for nulls). Complete `Node* getBinaryTree(vector<int>& inorder, vector<int>& preorder)`.

**Input Format**
- Line 1: T — number of test cases
- Per test case: N, then the inorder array, then the preorder array (each N space-separated distinct integers)

**Output Format**
- Per test case: the reconstructed tree's preorder serialization, `-1` for nulls

**Constraints**
- 1 ≤ T ≤ 10^5
- 1 ≤ N (sum across test cases ≤ 10^7)

## Sample Test Cases

**Input 1** (first of 3 test cases):
```
4
8766 2202 6058 4965
4965 6058 2202 8766
```
**Output 1:** `4965 6058 2202 8766 -1 -1 -1 -1 -1`

## Key Insight ⚡

Mirror image of [[Tree-from-inorder-and-postorder]]: the **first** element of preorder is always the root (consumed forward this time, not from the end). Find its position in inorder (hash map for O(1) lookup) to split into left/right ranges, then build **left subtree before right** — the natural order matching how preorder is consumed front-to-back (opposite of the postorder version, which must build right-before-left since that one consumes from the back).

## Complexity

O(n) time (hash map avoids O(n) linear search per lookup), O(n) extra space.

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

Node* buildHelper(vector<int>& preorder, int inStart, int inEnd, int& preIdx, unordered_map<int,int>& inMap) {
    if (inStart > inEnd) return nullptr;
    int rootVal = preorder[preIdx++];
    Node* root = new Node(rootVal);
    if (inStart == inEnd) return root;
    int inRootIdx = inMap[rootVal];
    root->setLeft(buildHelper(preorder, inStart, inRootIdx - 1, preIdx, inMap));
    root->setRight(buildHelper(preorder, inRootIdx + 1, inEnd, preIdx, inMap));
    return root;
}

Node* getBinaryTree(vector<int>& inorder, vector<int>& preorder) {
    int n = inorder.size();
    unordered_map<int,int> inMap;
    for (int i = 0; i < n; i++) inMap[inorder[i]] = i;
    int preIdx = 0;
    return buildHelper(preorder, 0, n - 1, preIdx, inMap);
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
        vector<int> arr1(n);
        for (int i = 0; i < n; i++) cin >> arr1[i];
        vector<int> arr2(n);
        for (int i = 0; i < n; i++) cin >> arr2[i];
        Node* tree = getBinaryTree(arr1, arr2);
        vector<int> ans;
        generateArray(tree, ans);
        for (auto v : ans) cout << v << " ";
        cout << endl;
    }
    return 0;
}
```
