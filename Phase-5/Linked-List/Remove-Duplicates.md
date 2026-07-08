---
tags: [linked-list]
date: 2026-07-08
status: Solved
url: https://maang.in/problems/Remove-Duplicates-532
---

# Remove Duplicates

## Problem

Given a sorted singly linked list, delete duplicates so each value appears only once.

## Sample Test Cases

**Input 1:**
```
3
1 1 2
```
**Output 1:** `1 2`

**Input 2:**
```
6
1 1 1 2 3 3
```
**Output 2:** `1 2 3`

## Key Insight ⚡

Since the list is sorted, duplicates are always adjacent. Walk the list with a single pointer `cur`: whenever `cur->val == cur->next->val`, unlink and delete the duplicate node without advancing; otherwise move `cur` forward.

## Complexity

O(n) time, O(1) extra space, single pass.

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

ListNode* deleteDuplicates(ListNode* head) {
    ListNode* cur = head;
    while (cur && cur->getNext()) {
        if (cur->getVal() == cur->getNext()->getVal()) {
            ListNode* dup = cur->getNext();
            cur->setNext(dup->getNext());
            delete dup;
        } else {
            cur = cur->getNext();
        }
    }
    return head;
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
    head = deleteDuplicates(head);

    while (head) {
        cout << head->getVal();
        if (head->getNext()) cout << " ";
        head = head->getNext();
    }
    cout << endl;
    return 0;
}
```
