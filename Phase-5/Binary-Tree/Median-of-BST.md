---
tags: [binary-tree, bst]
date: 2026-07-08
status: Solved
url: https://maang.in/problems/Median-of-BST-669
---

# Median of BST

## Problem

Given a BST, find the median. For odd node count, it's the `(n+1)/2`th value in sorted (inorder) order; for even, it's the integer average of the `n/2`th and `n/2+1`th values. Print `0` for an empty tree. Complete `int getMedian(Node* root)`.

**Input Format**
- Line 1: T — number of test cases
- Per test case: N (serialization length), then N space-separated integers (preorder serialization, `-1` = null)

**Output Format**
- Per test case: the median (or `0` if empty)

**Constraints**
- 1 ≤ T ≤ 10^5
- Sum of N across test cases ≤ 10^7

## Sample Test Cases

**Input 1** (first of 6 test cases):
```
11
7456 1022 -1 3404 -1 6680 -1 -1 9344 -1 -1
```
**Output 1:** `6680`

## Key Insight ⚡

Inorder traversal of a BST visits nodes in sorted order — so run an iterative inorder (stack-based, given N up to 10^7), collect all values, then index directly into the sorted array by parity: `n/2` for odd `n` (0-indexed), or the average of `n/2 - 1` and `n/2` for even `n`.

Unlike [[Boundary-Traversal]], this function *returns* an int and `main` unconditionally prints it (`cout << getMedian(...) << endl`), so the empty-tree case has no ambiguity — just `return 0`.

## Complexity

O(n) time, O(n) extra space (storing the full sorted sequence).

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

int getMedian(Node* root) {
    if (!root) return 0;
    vector<int> vals;
    stack<Node*> st;
    Node* cur = root;
    while (cur || !st.empty()) {
        while (cur) { st.push(cur); cur = cur->getLeft(); }
        cur = st.top(); st.pop();
        vals.push_back(cur->getVal());
        cur = cur->getRight();
    }
    int n = vals.size();
    if (n % 2 == 1) return vals[n / 2];
    else return (vals[n / 2 - 1] + vals[n / 2]) / 2;
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
        cout << getMedian(getBinaryTree(arr, &ind)) << endl;
    }
    return 0;
}
```
