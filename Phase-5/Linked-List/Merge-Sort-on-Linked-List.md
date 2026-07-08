---
tags: [linked-list, merge-sort, divide-and-conquer]
date: 2026-07-08
status: Solved
url: https://maang.in/problems/Merge-Sort-on-Linked-List-531
---

# Merge Sort on Linked List

## Problem

Sort a singly linked list in O(N log N) using merge sort. Complete `ListNode* mergesort(ListNode* head)`.

**Input Format**
- Line 1: N — number of nodes
- Line 2: N space-separated node values

**Output Format**
- Sorted (non-decreasing) node values, space-separated

**Constraints**
- 0 ≤ N ≤ 10^5
- 0 ≤ Values stored in nodes ≤ 10^9

## Sample Test Cases

**Input 1:**
```
1
2
```
**Output 1:** `2`

**Input 2:**
```
5
1 3 6 1 7
```
**Output 2:** `1 1 3 6 7`

## Key Insight ⚡

Standard divide-and-conquer merge sort, adapted for lists (no random access needed, unlike arrays): find the middle with fast/slow pointers (`fast` starts one node ahead so a 2-node list splits into two singletons), split into two halves, recursively sort each half, then merge two sorted lists linearly. No extra array needed — merging is done by relinking nodes.

## Complexity

O(n log n) time, O(log n) recursion stack space (no extra array allocation).

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

ListNode* findMid(ListNode* head) {
    ListNode* slow = head;
    ListNode* fast = head->getNext();
    while (fast && fast->getNext()) {
        slow = slow->getNext();
        fast = fast->getNext()->getNext();
    }
    return slow;
}

ListNode* mergeTwo(ListNode* a, ListNode* b) {
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

ListNode* mergesort(ListNode* head) {
    if (!head || !head->getNext()) return head;
    ListNode* mid = findMid(head);
    ListNode* right = mid->getNext();
    mid->setNext(nullptr);
    ListNode* leftSorted = mergesort(head);
    ListNode* rightSorted = mergesort(right);
    return mergeTwo(leftSorted, rightSorted);
}

ListNode* GetList(vector<int>& num) {
    ListNode dummy;
    ListNode* cur = &dummy;
    for (int x : num) {
        cur->setNext(new ListNode(x));
        cur = cur->getNext();
    }
    return dummy.getNext();
}

int main() {
    int n;
    cin >> n;
    vector<int> num(n);
    for (auto &x : num) cin >> x;

    ListNode* head = GetList(num);
    head = mergesort(head);

    while (head) {
        cout << head->getVal();
        if (head->getNext()) cout << " ";
        head = head->getNext();
    }
    cout << endl;
    return 0;
}
```
