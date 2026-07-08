---
tags: [linked-list]
date: 2026-07-08
status: Solved
url: https://maang.in/problems/Copy-List-with-Random-Pointer-540
---

# Copy List with Random Pointer

## Problem

Given a linked list where each node has a `next` pointer and a `random` pointer (which can point to any node, itself, or `null`), create a deep copy — none of the new list's pointers may reference the original list's nodes. Complete `ListNode* copyRandomList(ListNode* head)`.

**Input Format**
- Line 1: N — number of nodes
- Next N lines: `value randomIndex` (randomIndex is `-1` if the random pointer is null)

**Output Format**
- For each node in the copied list (in order): its value and the value of the node its random pointer points to (or `-1`)

**Constraints**
- 0 ≤ N ≤ 10^5
- 0 ≤ Value of the Node ≤ 10^9

## Sample Test Cases

**Input 1:**
```
5
1 2
6 -1
3 -1
4 1
5 3
```
**Output 1:**
```
1 3
6 -1
3 -1
4 6
5 4
```

## Key Insight ⚡

O(1) extra space trick (no hash map needed): interleave a copy of each node right after its original (`A -> A' -> B -> B' -> ...`). Now each original node's copy is reachable via `cur->next`, so `cur->next->random = cur->random->next` correctly sets the copy's random pointer without any lookup table. Finally, unweave the two interleaved lists back into the original list (restored) and the standalone copy.

## Complexity

O(n) time, O(1) extra space (excluding the output copy itself).

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;

class ListNode {
private:
    int val;
    ListNode* next;
    ListNode* random;
public:
    ListNode() : val(0), next(nullptr), random(nullptr) {}
    ListNode(int x) : val(x), next(nullptr), random(nullptr) {}
    ListNode(int x, ListNode* nxt) : val(x), next(nxt), random(nullptr) {}
    ListNode(int x, ListNode* nxt, ListNode* rand) : val(x), next(nxt), random(rand) {}

    int getVal() const { return val; }
    ListNode* getNext() const { return next; }
    ListNode* getRandom() const { return random; }
    void setNext(ListNode* nxt) { next = nxt; }
    void setRandom(ListNode* r) { random = r; }
};

ListNode* copyRandomList(ListNode* head) {
    if (!head) return nullptr;

    ListNode* cur = head;
    while (cur) {
        ListNode* copy = new ListNode(cur->getVal());
        copy->setNext(cur->getNext());
        cur->setNext(copy);
        cur = copy->getNext();
    }

    cur = head;
    while (cur) {
        if (cur->getRandom()) cur->getNext()->setRandom(cur->getRandom()->getNext());
        cur = cur->getNext()->getNext();
    }

    cur = head;
    ListNode* copyHead = head->getNext();
    ListNode* copyCur = copyHead;
    while (cur) {
        cur->setNext(cur->getNext()->getNext());
        copyCur->setNext(copyCur->getNext() ? copyCur->getNext()->getNext() : nullptr);
        cur = cur->getNext();
        copyCur = copyCur->getNext();
    }

    return copyHead;
}

ListNode* GetList(vector<pair<int,int>>& num) {
    int n = (int)num.size();
    vector<ListNode*> copy(n);

    ListNode dummy;
    ListNode* cur = &dummy;
    for (int i = 0; i < n; i++) {
        ListNode* temp = new ListNode(num[i].first);
        copy[i] = temp;
        cur->setNext(temp);
        cur = temp;
    }

    for (int i = 0; i < n; i++) {
        if (num[i].second == -1) continue;
        copy[i]->setRandom(copy[num[i].second]);
    }

    return dummy.getNext();
}

int main() {
    int n;
    cin >> n;
    vector<pair<int,int>> num(n);
    for (int i = 0; i < n; i++) cin >> num[i].first >> num[i].second;

    ListNode* head = GetList(num);
    ListNode* ans = copyRandomList(head);

    while (ans) {
        cout << ans->getVal() << " ";
        if (ans->getRandom()) cout << ans->getRandom()->getVal() << endl;
        else cout << -1 << endl;
        ans = ans->getNext();
    }
    return 0;
}
```
