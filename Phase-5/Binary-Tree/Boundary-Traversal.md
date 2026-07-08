---
tags: [binary-tree, dfs]
date: 2026-07-08
status: Solved
url: https://maang.in/problems/Boundary-Traversal-668
---

# Boundary Traversal

## Problem

Print the boundary nodes of a binary tree anti-clockwise from the root: root (if not a leaf), left boundary (excluding leaves), all leaves left-to-right, right boundary (excluding leaves, bottom-up) — no duplicate nodes. Complete `void printBoundary(Node* root)` (prints directly, doesn't return).

**Input Format**
- Line 1: T — number of test cases
- Per test case: N (serialization length), then N space-separated integers (preorder serialization, `-1` = null)

**Output Format**
- Per test case: the boundary traversal on its own line

**Constraints**
- 1 ≤ T ≤ 10^5
- Sum of N across test cases ≤ 10^7

## Sample Test Cases

**Input 1** (first of 9 test cases):
```
9
8249 -1 609 -1 5681 8332 -1 -1 -1
```
**Output 1:** `8249 8332 5681 609`

## Key Insight ⚡

Standard boundary-traversal algorithm: add root if it's not a leaf; walk the left boundary (`left` if present else `right`), adding only non-leaf nodes; collect all leaves via DFS (left before right); walk the right boundary similarly (`right` if present else `left`) into a temp list, then append it **reversed** (to get bottom-up order). No duplicates arise because both boundary walks explicitly skip leaves, and root is added exactly once up front.

⚠️ **Real bug hit and fixed**: for a **null/empty tree**, the correct behavior is to print **nothing at all** — my first attempt printed a blank `"\n"` instead, which visually "matched" the Run on Sample preview (whose one empty-tree sample case happened to be second-to-last) but caused 3 failed full submissions, because strict grading shifts every subsequent test case's output by one line once a spurious blank line is inserted mid-output. Extensive algorithm stress-testing (700k+ random trees against an independently-implemented reference, zero mismatches) didn't catch this, because the bug was in the base-case *output formatting*, not the traversal logic itself. Lesson: for `void print(...)`-style functions, explicitly verify what the null/base case should literally emit — don't assume "print nothing" and "print an empty line" are interchangeable, and don't fully trust the sample preview for edge cases the given samples don't clearly stress.

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

bool isLeaf(Node* node) { return !node->getLeft() && !node->getRight(); }

void addLeftBoundary(Node* node) {
    Node* cur = node->getLeft();
    while (cur) {
        if (!isLeaf(cur)) cout << cur->getVal() << " ";
        cur = cur->getLeft() ? cur->getLeft() : cur->getRight();
    }
}

void addLeaves(Node* node) {
    if (!node) return;
    vector<Node*> st;
    st.push_back(node);
    while (!st.empty()) {
        Node* cur = st.back(); st.pop_back();
        if (isLeaf(cur)) { cout << cur->getVal() << " "; continue; }
        if (cur->getRight()) st.push_back(cur->getRight());
        if (cur->getLeft()) st.push_back(cur->getLeft());
    }
}

void addRightBoundary(Node* node) {
    Node* cur = node->getRight();
    vector<int> tmp;
    while (cur) {
        if (!isLeaf(cur)) tmp.push_back(cur->getVal());
        cur = cur->getRight() ? cur->getRight() : cur->getLeft();
    }
    for (int i = (int)tmp.size() - 1; i >= 0; i--) cout << tmp[i] << " ";
}

void printBoundary(Node* root) {
    if (!root) return;
    if (!isLeaf(root)) cout << root->getVal() << " ";
    addLeftBoundary(root);
    addLeaves(root);
    addRightBoundary(root);
    cout << endl;
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
        printBoundary(tree);
    }
    return 0;
}
```
