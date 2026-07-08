---
tags: [binary-tree, bst]
date: 2026-07-08
status: Solved
url: https://maang.in/problems/IsBst-671
---

# IsBst

## Problem

Check whether a binary tree is a valid BST (each node's value strictly between the max of its left ancestors' lower bound and the min of its right ancestors' upper bound). Complete `bool isBst(Node* root)`.

**Input Format**
- Line 1: T — number of test cases
- Per test case: N (serialization length), then N space-separated integers (preorder serialization, `-1` = null)

**Output Format**
- Per test case: `yes` if valid BST, else `no`

**Constraints**
- 1 ≤ T ≤ 10^5
- 1 ≤ N < 10^7 (sum across test cases ≤ 10^7)

## Sample Test Cases

**Input 1:**
```
6
17
113169 111387 -1 -1 209114 -1 957726 786099 643981 -1 -1 821914 -1 -1 990181 -1 -1
7
566098 413153 -1 -1 797870 -1 -1
3
964901 -1 -1
19
14136 -1 35825 -1 37940 -1 264128 -1 370052 -1 545817 -1 659383 -1 678544 -1 682492 -1 -1
13
7813 6718 -1 5816 -1 -1 5249 -1 3912 8623 -1 -1 -1
15
8359 8305 9109 6276 -1 -1 6626 -1 -1 2942 -1 -1 8575 -1 -1
```
**Output 1:**
```
yes
yes
yes
yes
no
no
```

## Key Insight ⚡

Track a valid `(lo, hi)` range for each node as it's visited: a node's value must satisfy `lo < val < hi`; its left subtree inherits `(lo, val)`, its right subtree inherits `(val, hi)`. Done **iteratively** (not recursively) given N up to 10^7 risks stack overflow on a skewed tree — same reasoning as the traversal problems.

⚠️ **Gotcha hit while solving this**: an initial version carried `(node, lo, hi)` per stack frame as a `std::tuple` pushed via a brace-init-list (`stack<tuple<Node*,ll,ll>> st; st.push({node, lo, hi});`). That compiled fine locally under both `-std=gnu++14` and `-std=gnu++17`, but the actual platform judge rejected it with a **Compile Error** (an explicit-constructor ambiguity for `tuple` when packing a raw pointer alongside int literals needing conversion to `long long`). Fixed by switching to a plain aggregate `struct Frame { Node* node; long long lo, hi; };` pushed into a `vector` used as a stack — no ambiguity, and it was Accepted immediately.

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

struct Frame {
    Node* node;
    long long lo, hi;
};

bool isBst(Node* root) {
    if (!root) return true;
    vector<Frame> st;
    Frame start;
    start.node = root; start.lo = LLONG_MIN; start.hi = LLONG_MAX;
    st.push_back(start);
    while (!st.empty()) {
        Frame f = st.back(); st.pop_back();
        if (!f.node) continue;
        if (f.node->getVal() <= f.lo || f.node->getVal() >= f.hi) return false;
        Frame leftF; leftF.node = f.node->getLeft(); leftF.lo = f.lo; leftF.hi = (long long)f.node->getVal();
        Frame rightF; rightF.node = f.node->getRight(); rightF.lo = (long long)f.node->getVal(); rightF.hi = f.hi;
        st.push_back(leftF);
        st.push_back(rightF);
    }
    return true;
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
        cout << (isBst(tree) ? "yes" : "no") << endl;
    }
    return 0;
}
```
