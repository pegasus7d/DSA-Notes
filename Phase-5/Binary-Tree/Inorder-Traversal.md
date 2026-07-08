---
tags: [binary-tree, dfs, traversal]
date: 2026-07-08
status: Solved
url: https://maang.in/problems/Inorder-Traversal-573
---

# Inorder Traversal

## Problem

Given a binary tree, return the inorder traversal (left, root, right) of its node values. `T` independent test cases; each tree given as a preorder serialization with `-1` marking a null child. Complete `vector<int> getInorderTraversal(Node* root)`.

**Input Format**
- Line 1: T — number of test cases
- Per test case: N (serialization length), then N space-separated integers (preorder serialization, `-1` = null)

**Output Format**
- Per test case: the inorder traversal, space-separated

**Constraints**
- 1 ≤ T ≤ 10^5
- 1 ≤ N ≤ 10^7
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
5222 6801 3819
5407 7891 1720 8901
```

## Key Insight ⚡

With N up to 10^7, a **recursive** inorder traversal risks stack overflow on a heavily skewed (near-linked-list-shaped) tree — confirmed locally: a plain recursive version segfaults on a ~150,000-deep skewed tree under an 8MB stack, well below the problem's scale. Used the standard **iterative** traversal instead: push left children onto an explicit (heap-allocated) stack until hitting null, then pop-visit-go-right, repeating. This has no recursion depth limit regardless of tree shape.

Note: the provided `getBinaryTree` parser itself is still recursive (not something this problem asks us to change) — worth knowing that's a separate, unavoidable risk if the actual judge doesn't have a generous stack size, but it passed here.

## Complexity

O(n) time, O(h) extra space where h is the tree height (O(n) worst case for a skewed tree, O(log n) for a balanced one).

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

vector<int> getInorderTraversal(Node* root) {
    vector<int> result;
    stack<Node*> st;
    Node* cur = root;
    while (cur || !st.empty()) {
        while (cur) {
            st.push(cur);
            cur = cur->getLeft();
        }
        cur = st.top(); st.pop();
        result.push_back(cur->getVal());
        cur = cur->getRight();
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
        vector<int> ans = getInorderTraversal(tree);
        for (auto v : ans) cout << v << " ";
        cout << endl;
    }
    return 0;
}
```
