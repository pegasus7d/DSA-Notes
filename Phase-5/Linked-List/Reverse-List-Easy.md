---
tags: [linked-list]
date: 2026-07-08
status: Solved
url: https://maang.in/problems/Reverse-List-easy-version-533
---

# Reverse List - easy version

## Problem

Given a singly linked list, reverse it and return the new head. Complete `ListNode* reverseList(ListNode* head)`.

**Input Format**
- Line 1: n — number of nodes
- Line 2: n space-separated node values

**Output Format**
- Reversed list's node values, space-separated

**Constraints**
- 0 ≤ Length of the linked list ≤ 10^6
- 0 ≤ Values stored in nodes ≤ 10^9

## Sample Test Cases

**Input 1:**
```
1
1
```
**Output 1:** `1`

**Input 2:**
```
5
1 2 3 4 5
```
**Output 2:** `5 4 3 2 1`

## Key Insight ⚡

Classic iterative pointer-reversal: keep a `prev` pointer (initially null), and for each node, save its `next`, point it backward to `prev`, then advance both `prev` and `head`. Single pass, no extra memory.

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

ListNode* reverseList(ListNode* head) {
    ListNode* prev = nullptr;
    while (head) {
        ListNode* nxt = head->getNext();
        head->setNext(prev);
        prev = head;
        head = nxt;
    }
    return prev;
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
    head = reverseList(head);

    while (head) {
        cout << head->getVal();
        if (head->getNext()) cout << " ";
        head = head->getNext();
    }
    cout << endl;
    return 0;
}
```
