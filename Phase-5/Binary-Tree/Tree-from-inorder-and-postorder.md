---
tags: [binary-tree, dfs, tree-construction]
date: 2026-07-08
status: Solved
url: https://maang.in/problems/Tree-from-inorder-and-postoder-663
---

# Tree from inorder and postorder

## Problem

Reconstruct a binary tree from its inorder and postorder traversals (all node values distinct), and output the reconstructed tree's preorder serialization (`-1` for nulls). Complete `Node* getBinaryTree(vector<int>& inorder, vector<int>& postorder)`.

**Input Format**
- Line 1: T — number of test cases
- Per test case: N, then the inorder array, then the postorder array (each N space-separated distinct integers)

**Output Format**
- Per test case: the reconstructed tree's preorder serialization, `-1` for nulls

**Constraints**
- 1 ≤ T ≤ 10^5
- 1 ≤ N (sum across test cases ≤ 10^7)

## Sample Test Cases

**Input 1** (first of 7 test cases):
```
4
5096 3880 9002 8750
8750 9002 3880 5096
```
**Output 1:** `5096 -1 3880 -1 9002 -1 8750 -1 -1`

## Key Insight ⚡

The **last** element of postorder is always the root. Find its position in inorder (via a value→index hash map for O(1) lookup) to split inorder into left/right subtree ranges. Since postorder is consumed from the **end** backward, the **right** subtree must be built *before* the left subtree at each step (so the "next root to pop" lines up correctly) — a common trick that's easy to get backwards.

## Complexity

O(n) time (hash map gives O(1) index lookups instead of O(n) linear search), O(n) extra space for the map and recursion stack.

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

Node* buildHelper(vector<int>& postorder, int inStart, int inEnd, int& postIdx, unordered_map<int,int>& inMap) {
    if (inStart > inEnd) return nullptr;
    int rootVal = postorder[postIdx--];
    Node* root = new Node(rootVal);
    if (inStart == inEnd) return root;
    int inRootIdx = inMap[rootVal];
    root->setRight(buildHelper(postorder, inRootIdx + 1, inEnd, postIdx, inMap));
    root->setLeft(buildHelper(postorder, inStart, inRootIdx - 1, postIdx, inMap));
    return root;
}

Node* getBinaryTree(vector<int>& inorder, vector<int>& postorder) {
    int n = inorder.size();
    unordered_map<int,int> inMap;
    for (int i = 0; i < n; i++) inMap[inorder[i]] = i;
    int postIdx = n - 1;
    return buildHelper(postorder, 0, n - 1, postIdx, inMap);
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
