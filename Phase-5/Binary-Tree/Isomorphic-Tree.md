---
tags: [binary-tree, dfs]
date: 2026-07-08
status: Solved
url: https://maang.in/problems/Isomorphic-Tree-676
---

# Isomorphic Tree

## Problem

Two binary trees are isomorphic if one can be obtained from the other by swapping the children of any number of nodes (at any level). Complete `bool isIsomorphic(Node* t1, Node* t2)`. Two empty trees are isomorphic.

**Input Format**
- Line 1: T — number of test cases
- Per test case: N (first tree's length), then N integers (preorder, `-1` = null), then M (second tree's length), then M integers similarly

**Output Format**
- Per test case: `yes` or `no`

**Constraints**
- 1 ≤ T ≤ 10^5
- Sum of N across test cases ≤ 10^7

## Sample Test Cases

**Input 1** (first of 3 test cases):
```
11
35111 -1 38067 -1 11935 -1 42935 -1 83239 -1 -1
11
35111 38067 11935 -1 42935 -1 83239 -1 -1 -1 -1
```
**Output 1:** `yes`

## Key Insight ⚡

Both trees are isomorphic at a pair of nodes if their values match and, recursively, **either** (both left subtrees match and both right subtrees match) **or** (t1's left matches t2's right and t1's right matches t2's left) — the second branch of the `||` accounts for a possible child-swap at this node. Two null nodes are trivially isomorphic; one null and one non-null are not.

## Complexity

O(min(n, m)) time in the worst case (recursion short-circuits once trees diverge), O(h) recursion stack space.

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

bool isIsomorphic(Node* t1, Node* t2) {
    if (!t1 && !t2) return true;
    if (!t1 || !t2) return false;
    if (t1->getVal() != t2->getVal()) return false;
    return (isIsomorphic(t1->getLeft(), t2->getLeft()) && isIsomorphic(t1->getRight(), t2->getRight()))
        || (isIsomorphic(t1->getLeft(), t2->getRight()) && isIsomorphic(t1->getRight(), t2->getLeft()));
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
        Node* tree1 = getBinaryTree(arr, &ind);
        arr.clear();
        cin >> n;
        arr.resize(n);
        for (int i = 0; i < n; i++) cin >> arr[i];
        ind = 0;
        Node* tree2 = getBinaryTree(arr, &ind);
        cout << (isIsomorphic(tree1, tree2) ? "yes" : "no") << endl;
    }
    return 0;
}
```
