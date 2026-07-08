---
tags: [linked-list, two-pointer]
date: 2026-07-08
status: Solved
url: https://maang.in/problems/Reorder-List-II-538
---

# Reorder List II

## Problem

Given the head of a singly linked list `L1 -> L2 -> ... -> Ln`, reorder it in place to `L1 -> L3 -> L5 -> ... -> L6 -> L4 -> L2` (odd positions in order, then even positions in reverse order). Only node links may change, not values. Complete `void reorderList(ListNode* head)`.

**Input Format**
- Line 1: n — number of nodes
- Line 2: n space-separated node values, head to tail

**Output Format**
- Reordered list's node values, space-separated

**Constraints**
- 0 ≤ Length of the linked list ≤ 10^6
- 0 ≤ Value stored in a node ≤ 10^9

## Sample Test Cases

**Input 1:**
```
1
3
```
**Output 1:** `3`

**Input 2:**
```
5
1 2 3 4 5
```
**Output 2:** `1 3 5 4 2`

## Key Insight ⚡

Different split than "Reorder List I": here we split by **alternating position** (odd-indexed nodes vs even-indexed nodes) rather than by first-half/second-half. Walk the list splitting into two interleaved sublists (`odd`: 1st, 3rd, 5th... and `even`: 2nd, 4th, 6th...), reverse the even sublist, then attach the reversed even list to the end of the odd list. Odd positions naturally stay in original relative order; reversing evens gives the required "high to low" tail.

## Complexity

O(n) time, O(1) extra space.

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

void reorderList(ListNode* head) {
    if (!head || !head->getNext()) return;

    ListNode* odd = head;
    ListNode* even = head->getNext();
    ListNode* evenHead = even;

    while (even && even->getNext()) {
        odd->setNext(even->getNext());
        odd = odd->getNext();
        even->setNext(odd->getNext());
        even = even->getNext();
    }
    odd->setNext(nullptr);

    ListNode* prev = nullptr;
    ListNode* cur = evenHead;
    while (cur) {
        ListNode* nxt = cur->getNext();
        cur->setNext(prev);
        prev = cur;
        cur = nxt;
    }

    odd->setNext(prev);
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
    reorderList(head);

    while (head) {
        cout << head->getVal();
        if (head->getNext()) cout << " ";
        head = head->getNext();
    }
    cout << endl;
    return 0;
}
```
