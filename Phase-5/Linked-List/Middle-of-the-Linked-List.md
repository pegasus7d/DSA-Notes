---
tags: [linked-list, two-pointer]
date: 2026-07-08
status: Solved
url: https://maang.in/problems/Middle-of-the-Linked-List-526
---

# Middle of the Linked List

## Problem

Given a non-empty singly linked list, find its middle node. If there are two middle nodes (even length), return the second one. Complete `ListNode* middleNode(ListNode* head)`.

**Input Format**
- Line 1: n — number of nodes
- Line 2: n space-separated node values, head to tail

**Output Format**
- Value of the middle node

**Constraints**
- Size of linked list ≤ 10^5
- 0 ≤ Value stored in nodes ≤ 10^9

## Sample Test Cases

**Input 1:**
```
1
5
```
**Output 1:** `5`

**Input 2:**
```
2
1 8
```
**Output 2:** `8` (two middle nodes, return the second)

## Key Insight ⚡

Classic fast/slow (tortoise-and-hare) pointer: `fast` advances 2 nodes per step, `slow` advances 1. When `fast` reaches the end, `slow` is exactly at the middle — and this naturally lands on the *second* middle node when the length is even.

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

ListNode* middleNode(ListNode* head) {
    ListNode* slow = head;
    ListNode* fast = head;
    while (fast && fast->getNext()) {
        slow = slow->getNext();
        fast = fast->getNext()->getNext();
    }
    return slow;
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
    ListNode* mid = middleNode(head);
    cout << mid->getVal() << endl;
    return 0;
}
```
