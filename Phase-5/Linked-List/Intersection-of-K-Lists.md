---
tags: [linked-list, hash-map]
date: 2026-07-08
status: Solved
url: https://maang.in/problems/Intersection-of-K-Lists-539
---

# Intersection of K Lists

## Problem

Given the heads of `K` singly linked lists (guaranteed acyclic), return their common intersection node, or `nullptr` if none exists. Node values are globally unique — the same value always refers to the same physical node. Complete `ListNode* getKIntersectionNode(vector<ListNode*>& lists)`.

**Input Format**
- Line 1: K — number of linked lists
- For each list i: a line with `Ni` (its length), then a line with `Ni` space-separated values

**Output Format**
- Value of the common intersection node, or `-1` if none

**Constraints**
- 1 ≤ K ≤ 100
- 0 ≤ Ni ≤ 1000

## Sample Test Cases

**Input 1:**
```
3
7
1 2 3 5 6 8 9
6
4 3 5 6 8 9
4
7 6 8 9
```
**Output 1:** `6`

*(List 0 and list 1 share the suffix `5,6,8,9`, but list 2 only has `6,8,9` — so `5` isn't common to all three. The first node in list 0's own order that's present in every list is `6`.)*

## Key Insight ⚡

Since values uniquely identify physical nodes, count how many of the `K` lists each node pointer appears in (a single pass over every list into a hash map). Then walk **list 0 specifically** and return the first node whose count equals `K` — using list 0's own traversal order is what guarantees finding the *earliest* shared node, not just any node in the shared tail (a node further downstream would also have count `K` but isn't the true intersection start).

## Complexity

O(total nodes across all lists) time and space.

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

ListNode* getKIntersectionNode(vector<ListNode*>& lists) {
    int K = lists.size();
    if (K == 0) return nullptr;

    unordered_map<ListNode*, int> count;
    for (auto h : lists) {
        ListNode* cur = h;
        while (cur) {
            count[cur]++;
            cur = cur->getNext();
        }
    }

    ListNode* cur = lists[0];
    while (cur) {
        if (count[cur] == K) return cur;
        cur = cur->getNext();
    }
    return nullptr;
}

vector<ListNode*> GetList(vector<vector<int>>& num, int K) {
    unordered_map<int, ListNode*> nodeMap;
    vector<ListNode*> heads(K);
    for (int i = 0; i < K; i++) {
        ListNode dummy;
        ListNode* cur = &dummy;
        for (int v : num[i]) {
            ListNode* node;
            auto it = nodeMap.find(v);
            if (it != nodeMap.end()) node = it->second;
            else { node = new ListNode(v); nodeMap[v] = node; }
            cur->setNext(node);
            cur = node;
        }
        heads[i] = dummy.getNext();
    }
    return heads;
}

int main() {
    int K;
    cin >> K;
    vector<vector<int>> num(K);
    for (int i = 0; i < K; i++) {
        int n;
        cin >> n;
        num[i].resize(n);
        for (auto &x : num[i]) cin >> x;
    }

    vector<ListNode*> lists = GetList(num, K);
    ListNode* res = getKIntersectionNode(lists);

    cout << (res ? res->getVal() : -1) << endl;
    return 0;
}
```
