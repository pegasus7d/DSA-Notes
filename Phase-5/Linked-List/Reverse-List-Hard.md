---
tags: [linked-list]
date: 2026-07-08
status: Solved
url: https://maang.in/problems/Reverse-List-hard-version-534
---

# Reverse List - hard version

## Problem

Given a singly linked list of length `N` and an integer `K`, reverse the nodes in groups of size `K` and return the head. `K` is guaranteed to divide `N` exactly. Complete `ListNode* reverseList(ListNode* head, int K)`.

**Input Format**
- Line 1: N — length of the linked list
- Line 2: N space-separated node values
- Line 3: K

**Output Format**
- Modified list's node values, space-separated

**Constraints**
- 1 ≤ N ≤ 10^6
- 1 ≤ K ≤ N (K divides N exactly)
- 0 ≤ Values stored in nodes ≤ 10^9

## Sample Test Cases

**Input 1:**
```
6
1 2 3 4 5 6
3
```
**Output 1:** `3 2 1 6 5 4`

**Input 2:**
```
3
1 3 2
3
```
**Output 2:** `2 3 1`

## Key Insight ⚡

Extend the single-group reversal trick to repeat every `K` nodes. Use a dummy node before `head`, and track `prevGroupTail` (the last node of the previous — already reversed — group). For each group: find the `K`th node ahead (`kth`), reverse just that sub-list in place (bounded by `nextGroupStart = kth->next`), then relink `prevGroupTail->next = kth` (the new group head) and set `prevGroupTail = groupStart` (which is now the group's tail after reversal, ready for the next iteration). Since `K` divides `N` exactly, the loop terminates cleanly when fewer than `K` nodes remain (never happens here) — `kth` becomes `nullptr` right at the list's end.

## Complexity

O(n) time (each node visited a constant number of times), O(1) extra space.

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

ListNode* reverseList(ListNode* head, int K) {
    ListNode dummy(0, head);
    ListNode* prevGroupTail = &dummy;

    while (true) {
        ListNode* kth = prevGroupTail;
        for (int i = 0; i < K && kth; i++) kth = kth->getNext();
        if (!kth) break;

        ListNode* groupStart = prevGroupTail->getNext();
        ListNode* nextGroupStart = kth->getNext();

        ListNode* prev = nextGroupStart;
        ListNode* cur = groupStart;
        while (cur != nextGroupStart) {
            ListNode* nxt = cur->getNext();
            cur->setNext(prev);
            prev = cur;
            cur = nxt;
        }

        prevGroupTail->setNext(kth);
        prevGroupTail = groupStart;
    }

    return dummy.getNext();
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
    int K;
    cin >> K;

    ListNode* head = GetList(num);
    head = reverseList(head, K);

    while (head) {
        cout << head->getVal();
        if (head->getNext()) cout << " ";
        head = head->getNext();
    }
    cout << endl;
    return 0;
}
```
