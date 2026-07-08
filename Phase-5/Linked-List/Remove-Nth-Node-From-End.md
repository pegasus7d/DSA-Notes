---
tags: [linked-list, two-pointer]
date: 2026-07-08
status: Solved
url: https://maang.in/problems/Remove-Nth-Node-From-End-530
---

# Remove Nth Node From End

## Problem

Given a non-empty singly linked list, remove the `N`th node from the end and return the head, in one pass.

**Input Format**
- Line 1: n — number of nodes
- Line 2: n space-separated node values
- Line 3: N — which node from the end to remove

**Output Format**
- Resulting list's node values, space-separated

**Constraints**
- 1 ≤ Length of the linked list ≤ 10^6
- 1 ≤ N ≤ Length of the linked list
- 0 ≤ Value stored in nodes ≤ 10^9

## Sample Test Cases

**Input 1:**
```
3
1 2 3
1
```
**Output 1:** `1 2` (removes last node, value 3)

**Input 2:**
```
4
1 2 3 4
4
```
**Output 2:** `2 3 4` (removes head node, value 1)

**Input 3:**
```
4
1 2 3 4
3
```
**Output 3:** `1 3 4` (removes node with value 2)

## Key Insight ⚡

Two-pointer one-pass trick: advance a `fast` pointer `N` steps ahead of `slow` first, then move both together until `fast` hits the last node. At that point `slow` sits right before the node to remove. Using a **dummy node** before `head` avoids a special case for removing the head itself (when `N` equals the list length).

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

ListNode* removeNthFromEnd(ListNode* head, int N) {
    ListNode dummy(0, head);
    ListNode* fast = &dummy;
    ListNode* slow = &dummy;

    for (int i = 0; i < N; i++) fast = fast->getNext();

    while (fast->getNext()) {
        fast = fast->getNext();
        slow = slow->getNext();
    }

    ListNode* toDelete = slow->getNext();
    slow->setNext(toDelete->getNext());
    delete toDelete;
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
    int N;
    cin >> N;

    ListNode* head = GetList(num);
    head = removeNthFromEnd(head, N);

    while (head) {
        cout << head->getVal();
        if (head->getNext()) cout << " ";
        head = head->getNext();
    }
    cout << endl;
    return 0;
}
```
