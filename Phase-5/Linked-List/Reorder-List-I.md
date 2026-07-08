---
tags: [linked-list, two-pointer]
date: 2026-07-08
status: Solved
url: https://maang.in/problems/Reorder-List-I-528
---

# Reorder List I

## Problem

Given the head of a singly linked list `L1 -> L2 -> ... -> Ln`, reorder it in place to `L1 -> Ln -> L2 -> Ln-1 -> L3 -> Ln-2 -> ...`. Only node links may change, not values. Complete `void reorderList(ListNode* head)`.

**Input Format**
- Line 1: N — number of nodes
- Line 2: N space-separated node values, head to tail

**Output Format**
- Reordered list's node values, space-separated

**Constraints**
- 0 ≤ Length of the linked list ≤ 10^6
- 0 ≤ Value stored in the node ≤ 10^9

## Sample Test Cases

**Input 1:**
```
1
3
```
**Output 1:** `3`

**Input 2:**
```
4
1 2 3 4
```
**Output 2:** `1 4 2 3`

## Key Insight ⚡

Three-step trick: (1) find the middle with fast/slow pointers, splitting the list into two halves; (2) reverse the second half; (3) merge the two halves by alternating nodes (`first, second, first->next, second->next, ...`). All in-place, no extra data structure.

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

    ListNode* slow = head;
    ListNode* fast = head;
    while (fast->getNext() && fast->getNext()->getNext()) {
        slow = slow->getNext();
        fast = fast->getNext()->getNext();
    }

    ListNode* second = slow->getNext();
    slow->setNext(nullptr);

    ListNode* prev = nullptr;
    while (second) {
        ListNode* nxt = second->getNext();
        second->setNext(prev);
        prev = second;
        second = nxt;
    }
    second = prev;

    ListNode* first = head;
    while (second) {
        ListNode* n1 = first->getNext();
        ListNode* n2 = second->getNext();
        first->setNext(second);
        second->setNext(n1);
        first = n1;
        second = n2;
    }
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
