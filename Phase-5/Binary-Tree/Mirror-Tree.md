---
tags: [binary-tree]
date: 2026-07-08
status: Solved
url: https://maang.in/problems/Mirror-Tree-646
---

# Mirror Tree

## Problem

Convert a binary tree into its mirror — swap the left and right children of every node. Complete `Node* getMirrorTree(Node* root)`.

**Input Format**
- Line 1: T — number of test cases
- Per test case: N (serialization length), then N space-separated integers (preorder serialization, `-1` = null)

**Output Format**
- Per test case: the mirrored tree's preorder serialization (`-1` for nulls)

**Constraints**
- 1 ≤ T ≤ 10^5
- 1 ≤ N (sum across test cases ≤ 10^7)

## Sample Test Cases

**Input 1:**
```
3
3
2067 -1 -1
13
8365 1448 5240 -1 9909 -1 -1 5843 -1 -1 5682 -1 -1
15
7841 883 -1 -1 4929 6791 -1 -1 9159 2122 -1 -1 4179 -1 -1
```
**Output 1:**
```
2067 -1 -1
8365 5682 -1 -1 1448 5843 -1 -1 5240 9909 -1 -1 -1
7841 4929 9159 4179 -1 -1 2122 -1 -1 6791 -1 -1 883 -1 -1
```

## Key Insight ⚡

Visit every node (order doesn't matter — used an iterative DFS with an explicit stack rather than recursion, given N up to 10^7) and swap its `left`/`right` pointers. No new nodes needed; it's purely a link-rewiring operation.

## Complexity

O(n) time, O(h) extra space (O(n) worst case for a skewed tree).

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

Node* getMirrorTree(Node* root) {
    if (!root) return root;
    stack<Node*> st;
    st.push(root);
    while (!st.empty()) {
        Node* cur = st.top(); st.pop();
        Node* tmp = cur->getLeft();
        cur->setLeft(cur->getRight());
        cur->setRight(tmp);
        if (cur->getLeft()) st.push(cur->getLeft());
        if (cur->getRight()) st.push(cur->getRight());
    }
    return root;
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
        Node* tree = getBinaryTree(arr, &ind);
        tree = getMirrorTree(tree);
        vector<int> ans;
        generateArray(tree, ans);
        for (auto v : ans) cout << v << " ";
        cout << endl;
    }
    return 0;
}
```
