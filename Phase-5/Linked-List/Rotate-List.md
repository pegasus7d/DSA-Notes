---
tags: [linked-list]
date: 2026-07-08
status: Solved
url: https://maang.in/problems/Rotate-List-529
---

# Rotate List

## Problem

Given a singly linked list, rotate the list to the right by `K` places. Complete `ListNode* rotateList(ListNode* head, int K)`.

**Input Format**
- Line 1: n — length of the linked list
- Line 2: n space-separated node values
- Line 3: K

**Output Format**
- The rotated list's node values, space-separated

**Constraints**
- 0 ≤ Length of the linked list ≤ 10^6
- 0 ≤ K ≤ 10^9
- 0 ≤ Value stored in node ≤ 10^9

## Sample Test Cases

**Input 1:**
```
3
1 2 3
0
```
**Output 1:** `1 2 3`

**Input 2:**
```
3
1 2 3
6
```
**Output 2:** `1 2 3` (K mod n = 0, list unchanged)

**Input 3:**
```
3
1 2 3
1
```
**Output 3:** `3 1 2`

## Key Insight ⚡

Rotating by `n` (the list length) is a no-op, so reduce `K %= n` first — essential since `K` can be up to `1e9`. Link `tail->next = head` to make the list circular, walk `n - K - 1` steps from `head` to find the new tail, then the new head is `newTail->next`; break the circle by setting `newTail->next = nullptr`.

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

ListNode* rotateList(ListNode* head, int K) {
    if (!head || !head->getNext()) return head;

    int n = 1;
    ListNode* tail = head;
    while (tail->getNext()) {
        tail = tail->getNext();
        n++;
    }

    K %= n;
    if (K == 0) return head;

    tail->setNext(head); // make circular

    ListNode* newTail = head;
    for (int i = 0; i < n - K - 1; i++) newTail = newTail->getNext();

    ListNode* newHead = newTail->getNext();
    newTail->setNext(nullptr);

    return newHead;
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
    head = rotateList(head, K);

    while (head) {
        cout << head->getVal();
        if (head->getNext()) cout << " ";
        head = head->getNext();
    }
    cout << endl;
    return 0;
}
```
