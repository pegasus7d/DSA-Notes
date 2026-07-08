---
tags: [binary-tree, dfs]
date: 2026-07-08
status: Solved
url: https://maang.in/problems/Root-to-leaf-path-sum-667
---

# Root to leaf path sum

## Problem

Given a binary tree and a target `S`, determine whether some root-to-leaf path's values sum to exactly `S`. Complete `bool hasPathSum(Node* node, int sum)`.

**Input Format**
- Line 1: T — number of test cases
- Per test case: `N S` on one line (serialization length, target sum), then N space-separated integers (preorder serialization, `-1` = null)

**Output Format**
- Per test case: `yes` if such a path exists, else `no`

**Constraints**
- 1 ≤ T ≤ 10^5
- 1 ≤ N ≤ 10^6 (sum across test cases ≤ 10^7)

## Sample Test Cases

**Input 1:**
```
4
5 10
1 9 -1 -1 -1
5 61
8 -1 10 -1 -1
3 2
2 -1 -1
13 13
3 5 3 8 9 -1 -1 -1 -1 -1 1 -1 -1
```
**Output 1:**
```
yes
no
yes
no
```

## Key Insight ⚡

DFS carrying the remaining sum needed down each path: at each node, subtract its value from the running target; if a **leaf** is reached with remaining sum exactly `0`, success. Done iteratively with an explicit `vector`-backed stack of `{node, remaining}` frames (a plain struct, not `tuple` — see [[IsBst]] for why: tuple + brace-init caused a real Compile Error on this judge).

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
    long long remaining;
};

bool hasPathSum(Node* node, int sum) {
    if (!node) return false;
    vector<Frame> st;
    Frame start; start.node = node; start.remaining = sum;
    st.push_back(start);
    while (!st.empty()) {
        Frame f = st.back(); st.pop_back();
        long long rem = f.remaining - f.node->getVal();
        bool isLeaf = !f.node->getLeft() && !f.node->getRight();
        if (isLeaf && rem == 0) return true;
        if (f.node->getLeft()) { Frame l; l.node = f.node->getLeft(); l.remaining = rem; st.push_back(l); }
        if (f.node->getRight()) { Frame r; r.node = f.node->getRight(); r.remaining = rem; st.push_back(r); }
    }
    return false;
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
        int n, sum;
        cin >> n >> sum;
        vector<int> arr(n);
        for (int i = 0; i < n; i++) cin >> arr[i];
        int ind = 0;
        Node* tree = getBinaryTree(arr, &ind);
        cout << (hasPathSum(tree, sum) ? "yes" : "no") << endl;
    }
    return 0;
}
```
