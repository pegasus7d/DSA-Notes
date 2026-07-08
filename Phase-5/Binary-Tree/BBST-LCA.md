---
tags: [binary-tree, bst]
date: 2026-07-08
status: Solved
url: https://maang.in/problems/BBST-LCA-674
---

# BBST LCA

## Problem

Given a balanced BST and `Q` queries, find the LCA of two given node values (both guaranteed to exist in the tree) for each query. Complete `Node* getLCANode(Node* root, int n1, int n2)`.

**Input Format**
- Line 1: T — number of test cases
- Per test case: `N Q`, then N space-separated integers (preorder serialization, `-1` = null), then Q lines each with two query values

**Output Format**
- Per test case: the Q LCA values, space-separated, on one line

**Constraints**
- 1 ≤ T ≤ 10^5
- Sum of N across test cases ≤ 10^7

## Sample Test Cases

**Input 1:**
```
1
19 9
677 260 180 -1 -1 386 -1 -1 848 680 -1 -1 897 890 -1 -1 913 -1 -1
897 890
890 386
180 386
```
**Output 1 (first 3 answers):** `897 677 260`

## Key Insight ⚡

Classic BST LCA — no traversal or precomputation needed. Starting at the root, if both query values are less than the current node, go left; if both are greater, go right; otherwise (one is ≤ current ≤ the other, or one equals the current) the current node **is** the LCA. Each query is answered independently in O(log n) since the tree is balanced.

## Complexity

O(log n) per query, O(1) extra space.

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

Node* getLCANode(Node* root, int n1, int n2) {
    Node* cur = root;
    while (cur) {
        if (n1 < cur->getVal() && n2 < cur->getVal()) cur = cur->getLeft();
        else if (n1 > cur->getVal() && n2 > cur->getVal()) cur = cur->getRight();
        else return cur;
    }
    return nullptr;
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
        int n, q;
        cin >> n >> q;
        vector<int> arr(n);
        for (int i = 0; i < n; i++) cin >> arr[i];
        int ind = 0;
        Node* tree = getBinaryTree(arr, &ind);
        for (int i = 0; i < q; i++) {
            int k1, k2;
            cin >> k1 >> k2;
            cout << getLCANode(tree, k1, k2)->getVal() << " ";
        }
        cout << endl;
    }
    return 0;
}
```
