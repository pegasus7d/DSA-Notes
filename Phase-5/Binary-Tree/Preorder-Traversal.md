---
tags: [binary-tree, dfs, traversal]
date: 2026-07-08
status: Solved
url: https://maang.in/problems/Preorder-Traversal-574
---

# Preorder Traversal

## Problem

Given a binary tree, return the preorder traversal (root, left, right) of its node values. Same serialization/format as Inorder Traversal. Complete `vector<int> getPreorderTraversal(Node* root)`.

**Input Format**
- Line 1: T — number of test cases
- Per test case: N (serialization length), then N space-separated integers (preorder serialization, `-1` = null)

**Output Format**
- Per test case: the preorder traversal, space-separated

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
5222 3819 6801
8901 7891 5407 1720
```

## Key Insight ⚡

Same iterative-over-recursive reasoning as [[Inorder-Traversal]] (N up to 10^7 risks stack overflow on skewed trees). Iterative preorder is simpler than inorder: push the root, then repeatedly pop-and-visit, pushing right child before left child (so left is popped and visited first, since it's a stack).

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

vector<int> getPreorderTraversal(Node* root) {
    vector<int> result;
    if (!root) return result;
    stack<Node*> st;
    st.push(root);
    while (!st.empty()) {
        Node* cur = st.top(); st.pop();
        result.push_back(cur->getVal());
        if (cur->getRight()) st.push(cur->getRight());
        if (cur->getLeft()) st.push(cur->getLeft());
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
        vector<int> ans = getPreorderTraversal(tree);
        for (auto v : ans) cout << v << " ";
        cout << endl;
    }
    return 0;
}
```
