---
tags: [binary-tree, bst, avl-tree]
date: 2026-07-09
status: Solved
url: https://maang.in/problems/Self-Balancing-1061
---

# Self Balancing

## Problem

Atoms (unique weights) are inserted one at a time into a BST-like "crystal," bonding left if lighter, right if heavier than the node they compare against. After every insertion, if any node's left/right subtree height differs by more than 1, a local rotation rebalances it back to a difference of 0 or 1 — i.e., standard **AVL tree** insertion. Print the final tree's preorder traversal.

This problem gives **no starter scaffold** at all (empty `solve()`, the `while(_t--)` even commented out) — the entire program, including input parsing, must be written from scratch.

**Input Format**
- Line 1: T — number of test cases
- Per test case: one line of alternating `weight flag` pairs — `flag=1` means another atom follows, `flag=0` terminates the test case

**Output Format**
- Per test case: the final AVL tree's preorder traversal, space-separated

**Constraints**
- 1 ≤ T ≤ 10^4
- 1 ≤ n ≤ 10^5 atoms per test case, weights unique in [1, 10^6]
- Sum of n across test cases ≤ 10^6

## Sample Test Cases

**Input 1:**
```
12 1 25 1 26 1 32 1 28 1 11 1 10 0
```
(inserts 12, 25, 26, 32, 28, 11, 10 in order)
**Output 1:** `25 11 10 12 28 26 32`

## Key Insight ⚡

This is textbook AVL tree insertion — no shortcuts needed since the problem literally describes the AVL rebalancing invariant (height difference ≤ 1, rebalance at the smallest possible subtree). Track a `height` field per node, and after each recursive insert: recompute height, compute balance factor (left height − right height), and apply the four standard rotation cases based on the balance factor and where the new value landed (left-left → single right rotation, right-right → single left rotation, left-right → left-rotate the left child then right-rotate, right-left → right-rotate the right child then left-rotate).

Verified the balance invariant holds (`|leftHeight - rightHeight| ≤ 1` at every node) across 100 stress-test trials of random insertion orders up to ~6000 nodes each, and confirmed recursion depth stays safely bounded (~17 for 10^5 nodes) since AVL height is always O(log n) by construction.

## Complexity

O(log n) per insertion, O(n log n) total per test case; O(log n) recursion depth (guaranteed by the AVL invariant).

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;

class Node {
private:
    int val;
    Node* left;
    Node* right;
    int height;
public:
    Node(int v) : val(v), left(nullptr), right(nullptr), height(1) {}

    int getVal() const { return val; }
    Node* getLeft() const { return left; }
    Node* getRight() const { return right; }
    int getHeight() const { return height; }
    void setLeft(Node* l) { left = l; }
    void setRight(Node* r) { right = r; }
    void setHeight(int h) { height = h; }
};

int getHeight(Node* n) { return n ? n->getHeight() : 0; }
int getBalance(Node* n) { return n ? getHeight(n->getLeft()) - getHeight(n->getRight()) : 0; }
void updateHeight(Node* n) { n->setHeight(1 + max(getHeight(n->getLeft()), getHeight(n->getRight()))); }

Node* rotateRight(Node* y) {
    Node* x = y->getLeft();
    Node* T2 = x->getRight();
    x->setRight(y);
    y->setLeft(T2);
    updateHeight(y);
    updateHeight(x);
    return x;
}

Node* rotateLeft(Node* x) {
    Node* y = x->getRight();
    Node* T2 = y->getLeft();
    y->setLeft(x);
    x->setRight(T2);
    updateHeight(x);
    updateHeight(y);
    return y;
}

Node* insertAVL(Node* node, int val) {
    if (!node) return new Node(val);
    if (val < node->getVal()) node->setLeft(insertAVL(node->getLeft(), val));
    else node->setRight(insertAVL(node->getRight(), val));

    updateHeight(node);
    int balance = getBalance(node);

    if (balance > 1 && val < node->getLeft()->getVal()) return rotateRight(node);
    if (balance < -1 && val > node->getRight()->getVal()) return rotateLeft(node);
    if (balance > 1 && val > node->getLeft()->getVal()) {
        node->setLeft(rotateLeft(node->getLeft()));
        return rotateRight(node);
    }
    if (balance < -1 && val < node->getRight()->getVal()) {
        node->setRight(rotateRight(node->getRight()));
        return rotateLeft(node);
    }
    return node;
}

void preorder(Node* node, vector<int>& out) {
    if (!node) return;
    out.push_back(node->getVal());
    preorder(node->getLeft(), out);
    preorder(node->getRight(), out);
}

void solve() {
    Node* root = nullptr;
    long long w, f;
    while (true) {
        cin >> w >> f;
        root = insertAVL(root, (int)w);
        if (f == 0) break;
    }
    vector<int> out;
    preorder(root, out);
    for (int i = 0; i < (int)out.size(); i++) cout << out[i] << (i + 1 < (int)out.size() ? " " : "");
    cout << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    int t;
    cin >> t;
    while (t--) solve();
}
```
