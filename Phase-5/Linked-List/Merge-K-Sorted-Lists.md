---
tags: [linked-list, merge-sort, divide-and-conquer]
date: 2026-07-08
status: Solved
url: https://maang.in/problems/Merge-K-Sorted-Lists-535
---

# Merge K Sorted Lists

## Problem

Given an array of `K` ascending-sorted singly linked lists, merge them into one sorted list, in O(1) extra space (beyond recursion). Complete `ListNode* mergeKLists(vector<ListNode*> head)`.

**Input Format**
- Line 1: K — number of linked lists
- For each list i: a line with `Ni` (its length), then a line with `Ni` space-separated ascending values

**Output Format**
- Merged sorted list's node values, space-separated

**Constraints**
- 1 ≤ K ≤ 100
- 0 ≤ Ni ≤ 1000
- 0 ≤ Values of the node ≤ 10^9

## Sample Test Cases

**Input 1:**
```
3
3
3 5 6
3
1 2 9
3
4 7 8
```
**Output 1:** `1 2 3 4 5 6 7 8 9`

**Input 2:**
```
3
2
1 5
0

1
4
```
**Output 2:** `1 4 5`

## Key Insight ⚡

Divide and conquer, pairwise: recursively split the array of `K` lists into two halves, merge each half (recursively down to single lists), then merge the two resulting sorted lists with the standard two-pointer merge (reusing existing nodes, no extra allocation). This is O(N log K) time (N = total nodes across all lists) and avoids the O(K) heap a priority-queue approach would need.

## Complexity

O(N log K) time where N is the total number of nodes; O(log K) recursion stack space (no heap/array of size K needed).

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;

class ListNode {
private:
    int val;
    ListNode* next;
public:
    ListNode() : val(0), next(nullptr) {}
    ListNode(int x) : val(x), next(nullptr) {}
    ListNode(int x, ListNode* nxt) : val(x), next(nxt) {}

    int getVal() const { return val; }
    ListNode* getNext() const { return next; }
    void setNext(ListNode* nxt) { next = nxt; }
};

ListNode* mergeTwoLists(ListNode* a, ListNode* b) {
    ListNode dummy;
    ListNode* tail = &dummy;
    while (a && b) {
        if (a->getVal() <= b->getVal()) { tail->setNext(a); a = a->getNext(); }
        else { tail->setNext(b); b = b->getNext(); }
        tail = tail->getNext();
    }
    tail->setNext(a ? a : b);
    return dummy.getNext();
}

ListNode* mergeKListsHelper(vector<ListNode*>& lists, int lo, int hi) {
    if (lo > hi) return nullptr;
    if (lo == hi) return lists[lo];
    int mid = (lo + hi) / 2;
    ListNode* left = mergeKListsHelper(lists, lo, mid);
    ListNode* right = mergeKListsHelper(lists, mid + 1, hi);
    return mergeTwoLists(left, right);
}

ListNode* mergeKLists(vector<ListNode*> head) {
    if (head.empty()) return nullptr;
    return mergeKListsHelper(head, 0, (int)head.size() - 1);
}

vector<ListNode*> GetList(int K, vector<vector<int>>& num) {
    vector<ListNode*> heads(K);
    for (int i = 0; i < K; i++) {
        ListNode dummy;
        ListNode* cur = &dummy;
        for (int x : num[i]) {
            cur->setNext(new ListNode(x));
            cur = cur->getNext();
        }
        heads[i] = dummy.getNext();
    }
    return heads;
}

int main() {
    int K;
    cin >> K;
    vector<vector<int>> num(K);
    for (int i = 0; i < K; i++) {
        int n;
        cin >> n;
        num[i].resize(n);
        for (auto &x : num[i]) cin >> x;
    }

    vector<ListNode*> lists = GetList(K, num);
    ListNode* head = mergeKLists(lists);

    while (head) {
        cout << head->getVal();
        if (head->getNext()) cout << " ";
        head = head->getNext();
    }
    cout << endl;
    return 0;
}
```
