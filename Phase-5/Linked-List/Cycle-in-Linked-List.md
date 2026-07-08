---
tags: [linked-list, two-pointer, floyd-cycle]
date: 2026-07-08
status: Solved
url: https://maang.in/problems/Cycle-in-Linked-List-527
---

# Cycle in Linked List

## Problem

Given a non-empty linked list, find the starting node of a cycle and the cycle's length. Return `nullptr` and `-1` if there's no cycle. Complete `pair<ListNode*,int> cycleInLinkedList(ListNode* head)`.

**Input Format**
- Line 1: n — length of the linked list
- Line 2: n space-separated node values
- Line 3: `LastLink` — 1-indexed position the last node connects back to; `0` means no cycle

**Output Format**
- Two space-separated integers: value of the cycle's starting node (`-1` if none), and cycle length (`-1` if none)

**Constraints**
- 1 ≤ Length of the linked list ≤ 10^6
- 0 ≤ Value in node ≤ 10^9
- 1 ≤ LastLink ≤ Length - 1 (or 0 for no cycle)

## Sample Test Cases

**Input 1:**
```
3
1 2 3
2
```
**Output 1:** `2 2`

**Input 2:**
```
3
1 2 3
0
```
**Output 2:** `-1 -1` (no cycle)

**Input 3:**
```
2
1 2
1
```
**Output 3:** `1 2`

## Key Insight ⚡

Classic Floyd's cycle detection, extended to also find the start and length:
1. **Detect**: fast/slow pointers meet somewhere inside the cycle if one exists.
2. **Find start**: reset one pointer to `head`; advance both one step at a time — they meet exactly at the cycle's start (standard Floyd proof: distance from head to cycle start equals distance from meeting point to cycle start, going around).
3. **Find length**: walk from the start node forward until returning to it, counting steps.

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

pair<ListNode*, int> cycleInLinkedList(ListNode* head) {
    ListNode* slow = head;
    ListNode* fast = head;
    bool hasCycle = false;
    while (fast && fast->getNext()) {
        slow = slow->getNext();
        fast = fast->getNext()->getNext();
        if (slow == fast) { hasCycle = true; break; }
    }
    if (!hasCycle) return {nullptr, -1};

    ListNode* ptr1 = head;
    ListNode* ptr2 = slow;
    while (ptr1 != ptr2) {
        ptr1 = ptr1->getNext();
        ptr2 = ptr2->getNext();
    }
    ListNode* cycleStart = ptr1;

    int length = 1;
    ListNode* temp = cycleStart->getNext();
    while (temp != cycleStart) {
        temp = temp->getNext();
        length++;
    }

    return {cycleStart, length};
}

ListNode* GetList(int n, vector<int>& num, int lastLink) {
    vector<ListNode*> nodes(n);
    for (int i = 0; i < n; i++) nodes[i] = new ListNode(num[i]);
    for (int i = 0; i + 1 < n; i++) nodes[i]->setNext(nodes[i + 1]);
    if (lastLink != 0) nodes[n - 1]->setNext(nodes[lastLink - 1]);
    return nodes[0];
}

int main() {
    int n;
    cin >> n;
    vector<int> num(n);
    for (auto &x : num) cin >> x;
    int lastLink;
    cin >> lastLink;

    ListNode* head = GetList(n, num, lastLink);
    auto [node, len] = cycleInLinkedList(head);

    cout << (node ? node->getVal() : -1) << " " << len << endl;
    return 0;
}
```
