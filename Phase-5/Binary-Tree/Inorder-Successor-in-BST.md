---
tags: [binary-tree, bst, binary-search]
date: 2026-07-08
status: Solved
url: https://maang.in/problems/Inorder-Successor-in-BST-673
---

# Inorder Successor in BST

## Problem

Given a balanced BST and `Q` queries, for each query find the smallest tree value strictly greater than the queried key (`-1` if none exists). Complete `int getInorderSuccessor(Node* node, int k)`.

**Input Format**
- Line 1: T — number of test cases
- Per test case: `N Q`, then N space-separated integers (preorder serialization, `-1` = null), then Q query keys

**Output Format**
- Per test case: the Q answers, space-separated, on one line

**Constraints**
- 1 ≤ T ≤ 10^5
- Sum of N across test cases ≤ 10^7

## Sample Test Cases

**Input 1:**
```
1
15 7
333 218 71 -1 -1 323 -1 -1 657 545 -1 -1 715 -1 -1
813 58 148 224 851 594 880
```
**Output 1:** `-1 71 218 323 -1 657 -1`

## Key Insight ⚡

Despite the description framing it as "the successor of a node," the sample queries include values that **aren't actually in the tree** (e.g. 813, 58, 224) — so this is really an `upper_bound`-style query: the smallest tree value strictly greater than `k`, whether or not `k` itself is a node. Same caching trick as [[Kth-element-of-BST]] — build the sorted inorder array once per unique tree (keyed by root pointer), then answer each query via `std::upper_bound` in O(log n).

## Complexity

O(n) per unique tree (amortized) + O(log n) per query.

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

Node* cachedRoot = nullptr;
vector<int> cachedVals;

int getInorderSuccessor(Node* node, int k) {
    if (node != cachedRoot) {
        cachedRoot = node;
        cachedVals.clear();
        stack<Node*> st;
        Node* cur = node;
        while (cur || !st.empty()) {
            while (cur) { st.push(cur); cur = cur->getLeft(); }
            cur = st.top(); st.pop();
            cachedVals.push_back(cur->getVal());
            cur = cur->getRight();
        }
    }
    auto it = upper_bound(cachedVals.begin(), cachedVals.end(), k);
    if (it == cachedVals.end()) return -1;
    return *it;
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
            int k;
            cin >> k;
            cout << getInorderSuccessor(tree, k) << " ";
        }
        cout << endl;
    }
    return 0;
}
```
