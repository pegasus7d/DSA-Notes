---
tags: [linked-list, two-pointer]
date: 2026-07-08
status: Solved
url: https://maang.in/problems/Intersection-of-Two-Lists-537
---

# Intersection of Two Lists

## Problem

Given the heads of two singly linked lists, return the node at which they intersect, or `NULL` if they don't. Complete `ListNode* getIntersectionNode(ListNode* headA, ListNode* headB)`.

**Input Format**
- Line 1: N — length of list A
- Line 2: N space-separated values for list A
- Line 3: M — length of list B
- Line 4: M space-separated values for list B
- Line 5: `skipA skipB` — if both are -1, no intersection; otherwise the lists share a common suffix starting at those offsets

**Output Format**
- Value of the intersection node, or `-1` if none

**Constraints**
- 1 ≤ N, M ≤ 10^5
- -1 ≤ skipA < N, -1 ≤ skipB < M
- 0 ≤ Value of the node ≤ 10^9

## Sample Test Cases

**Input 1:**
```
5
2 3 1 4 5
6
9 6 8 1 4 5
2 3
```
**Output 1:** `1`

**Input 2:**
```
4
1 2 5 3
3
6 5 3
2 1
```
**Output 2:** `5`

## Key Insight ⚡

Two-pointer trick: walk `a` down list A and `b` down list B simultaneously; when either pointer hits the end, redirect it to the *other* list's head instead of stopping. Both pointers then travel exactly `lenA + lenB` total steps, so they arrive at the intersection node at the same time (or both hit `nullptr` together if there's no intersection, since `a == b == nullptr` ends the loop correctly).

## Complexity

O(n + m) time, O(1) extra space.

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

ListNode* getIntersectionNode(ListNode* headA, ListNode* headB) {
    if (!headA || !headB) return nullptr;
    ListNode* a = headA;
    ListNode* b = headB;
    while (a != b) {
        a = a ? a->getNext() : headB;
        b = b ? b->getNext() : headA;
    }
    return a;
}

pair<ListNode*, ListNode*> GetList(vector<int>& listA, vector<int>& listB, int skipA, int skipB) {
    int n = listA.size();

    if (skipA == -1) {
        ListNode *headA = nullptr, *tailA = nullptr;
        for (int x : listA) {
            ListNode* node = new ListNode(x);
            if (!tailA) headA = tailA = node;
            else { tailA->setNext(node); tailA = node; }
        }
        ListNode *headB = nullptr, *tailB = nullptr;
        for (int x : listB) {
            ListNode* node = new ListNode(x);
            if (!tailB) headB = tailB = node;
            else { tailB->setNext(node); tailB = node; }
        }
        return {headA, headB};
    }

    ListNode *sharedHead = nullptr, *sharedTail = nullptr;
    for (int i = skipA; i < n; i++) {
        ListNode* node = new ListNode(listA[i]);
        if (!sharedTail) sharedHead = sharedTail = node;
        else { sharedTail->setNext(node); sharedTail = node; }
    }

    ListNode *headA = nullptr, *tailA = nullptr;
    for (int i = 0; i < skipA; i++) {
        ListNode* node = new ListNode(listA[i]);
        if (!tailA) headA = tailA = node;
        else { tailA->setNext(node); tailA = node; }
    }
    if (tailA) tailA->setNext(sharedHead); else headA = sharedHead;

    ListNode *headB = nullptr, *tailB = nullptr;
    for (int i = 0; i < skipB; i++) {
        ListNode* node = new ListNode(listB[i]);
        if (!tailB) headB = tailB = node;
        else { tailB->setNext(node); tailB = node; }
    }
    if (tailB) tailB->setNext(sharedHead); else headB = sharedHead;

    return {headA, headB};
}

int main() {
    int n;
    cin >> n;
    vector<int> listA(n);
    for (auto &x : listA) cin >> x;
    int m;
    cin >> m;
    vector<int> listB(m);
    for (auto &x : listB) cin >> x;
    int skipA, skipB;
    cin >> skipA >> skipB;

    auto [headA, headB] = GetList(listA, listB, skipA, skipB);
    ListNode* res = getIntersectionNode(headA, headB);

    if (res) cout << res->getVal() << endl;
    else cout << -1 << endl;
    return 0;
}
```
