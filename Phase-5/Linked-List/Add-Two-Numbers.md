---
tags: [linked-list]
date: 2026-07-08
status: Solved
url: https://maang.in/problems/Add-Two-Numbers-525
---

# Add Two Numbers

## Problem

A number is represented as a linked list storing its digits in reverse order — `head` points to the least significant digit. Given two non-empty such lists, add them and return the sum as a linked list. Complete `ListNode* addTwoNumbers(ListNode* n1, ListNode* n2)`.

**Input Format**
- Two space-separated strings representing the two non-negative integers (most significant digit first, no leading zeros).

**Output Format**
- The sum, printed as a plain number (no leading zeros).

**Constraints**
- Size of the linked list ≤ 10^5
- Each node stores a value from 0 to 9 (single digit, per the reverse-digit representation)

## Sample Test Cases

**Input 1:**
```
200 13421
```
**Output 1:** `13621`

**Input 2:**
```
12 0
```
**Output 2:** `12`

## Key Insight ⚡

Since `head` is the least significant digit for both lists, this is exactly elementary-school addition: walk both lists together, add corresponding digits plus carry, emit `sum % 10`, carry `sum / 10`. Continue while either list still has nodes or a carry remains (handles unequal lengths and a final leading carry, e.g. `999 + 1 = 1000`).

## Complexity

O(max(len1, len2)) time, O(max(len1, len2)) space for the result list.

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

ListNode* addTwoNumbers(ListNode* n1, ListNode* n2) {
    ListNode dummy;
    ListNode* cur = &dummy;
    int carry = 0;
    while (n1 || n2 || carry) {
        int sum = carry;
        if (n1) { sum += n1->getVal(); n1 = n1->getNext(); }
        if (n2) { sum += n2->getVal(); n2 = n2->getNext(); }
        carry = sum / 10;
        cur->setNext(new ListNode(sum % 10));
        cur = cur->getNext();
    }
    return dummy.getNext();
}

// Local test helper: string (most-sig-digit-first) -> list with head = least sig digit
ListNode* StringToList(const string& s) {
    ListNode dummy;
    ListNode* cur = &dummy;
    for (int i = (int)s.size() - 1; i >= 0; i--) {
        cur->setNext(new ListNode(s[i] - '0'));
        cur = cur->getNext();
    }
    return dummy.getNext();
}

int main() {
    string a, b;
    cin >> a >> b;

    ListNode* n1 = StringToList(a);
    ListNode* n2 = StringToList(b);
    ListNode* res = addTwoNumbers(n1, n2);

    vector<int> digits;
    while (res) { digits.push_back(res->getVal()); res = res->getNext(); }
    reverse(digits.begin(), digits.end());

    int start = 0;
    while (start < (int)digits.size() - 1 && digits[start] == 0) start++;
    for (int i = start; i < (int)digits.size(); i++) cout << digits[i];
    cout << endl;
    return 0;
}
```
