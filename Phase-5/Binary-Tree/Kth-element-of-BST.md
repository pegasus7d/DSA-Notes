---
tags: [binary-tree, bst]
date: 2026-07-08
status: Solved
url: https://maang.in/problems/Kth-element-of-BST-672
---

# Kth element of BST

## Problem

Given a balanced BST and `Q` queries, find the `K`th smallest element for each query. Complete `int getKthElement(Node* node, int k)` — called once per query with the same tree root.

**Input Format**
- Line 1: T — number of test cases
- Per test case: `N Q` (serialization length, query count), then N space-separated integers (preorder serialization, `-1` = null), then Q query values of K

**Output Format**
- Per test case: the Q answers, space-separated, on one line

**Constraints**
- 1 ≤ T ≤ 10^5
- Sum of N across test cases ≤ 10^7

## Sample Test Cases

**Input 1:**
```
1
21 10
605 192 87 -1 -1 308 -1 -1 758 637 624 -1 -1 668 -1 -1 792 -1 954 -1 -1
1 2 3 4 5 6 7 8 9 10
```
**Output 1:** `87 192 308 605 624 637 668 758 792 954`

## Key Insight ⚡

`getKthElement` is called **once per query** with the same tree root — recomputing the full inorder traversal on every call would be wasteful (O(N·Q) total). Instead, cache the sorted inorder list keyed by the root pointer: only recompute (iteratively, given N up to 10^7) when the root pointer changes (i.e., a new test case's tree), then answer every query for that tree in O(1) via direct indexing.

## Complexity

O(n) per unique tree (amortized across all its queries) + O(1) per query, using O(n) cache space.

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

int getKthElement(Node* node, int k) {
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
    return cachedVals[k - 1];
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
            cout << getKthElement(tree, k) << " ";
        }
        cout << endl;
    }
    return 0;
}
```
