---
tags: [live-session, stl, pair, vector, stack, queue, deque, set, multiset]
status: Watched
duration: 01:50:45
source: https://maang.in/live-sessions/STL-Foundations-1-1322
---

# STL Foundations 1

Introductory session covering the core C++ STL containers used throughout the course.

## Topics Overview

**Stack** · **Queue** · **Priority Queue** — one loop-based group
**Vector** · **Pair** · **Deque** — indexable/sequence group
**Map** · **Set** · **Multiset** — associative/ordered group

## 1. Pair

```cpp
pair<int, int> a = {1, 2};
// a.first  -> 1
// a.second -> 2
```

Can be nested:

```cpp
pair<int, pair<int, int>> p;
// p.first          -> outer int
// p.second.first   -> inner first
// p.second.second  -> inner second
```

Also demonstrated: `pair<int, string>` and similar mixed-type pairs.

## 2. Vector

Contrasted with a static array `int arr[n]` (fixed size) — a vector is dynamically resizable.

```cpp
vector<int> v;
v.push_back(1);
v.push_back(3);
v.push_back(4);
v.pop_back();
v.clear();
v.resize(n);      // resize(5) demo: fills with 5 zero-initialized elements [0,0,0,0,0]
```

Indexing: `v[2]` — direct random access like an array.

Sorting: `sort(a.begin(), a.end())`

Iterators covered on a diagram:
- `v.begin()` → points to the first element
- `v.end()` → points just past the last element
- `v.rbegin()` → reverse-begin (last element)
- `v.rend()` → reverse-end (just before the first element)

## 3. Stack

```cpp
stack<int> st;
st.push(x);   // O(1)
st.pop();     // O(1)
st.empty();   // O(1)
st.size();    // O(1)
```

Diagram: single-ended box — push and pop both happen from the same (top) end.

## 4. Queue

```cpp
queue<int> q;
q.push(1);
q.push(2);
q.pop();
q.empty();
q.front();
```

Diagram: two-ended box — push happens at the back, pop/front operate at the front (FIFO).
A time-complexity comparison note was also on the board (partially visible — vector-related TC discussion, values shown as O(1)).

## 5. Deque

```cpp
deque<int> dq;
dq[1];             // random access, like a vector
dq.push_back(1);
dq.push_front(1);
dq.pop_back();     // O(1)
dq.pop_front();    // O(1)
dq.empty();
dq.size();
```

Diagram: double-ended box — push/pop supported at both front and back, plus direct indexing (unlike a plain queue).

## 6. Set

```cpp
set<int> s;
s.insert(1);   // O(log n)
s.insert(2);   // O(log n)
```

Flagged as covered later in the session (not captured in a whiteboard screenshot, but confirmed to be part of this topic): `s.erase()`, `s.begin()`, `s.end()`.

## 7. Multiset

Mentioned as a related container covered in this session — allows duplicate elements, unlike `set`. Same underlying balanced-BST structure as `set`, so all operations are `O(log n)`.

```cpp
multiset<int> ms;
ms.insert(1);        // O(log n) — duplicates allowed, unlike set
ms.insert(1);        // inserting 1 again is fine, count becomes 2
ms.erase(1);         // erases ALL occurrences of 1 — O(log n + count)
ms.erase(ms.find(1)); // erases only ONE occurrence (via iterator) — O(log n)
ms.count(1);          // O(log n + count) — number of occurrences of 1
ms.find(1);            // returns iterator to one occurrence, or ms.end()
ms.begin(); ms.end();  // iterators, same as set — smallest to largest
```

**Gotcha:** `ms.erase(value)` removes *every* copy of that value, while `ms.erase(iterator)` removes just the one at that position — a common source of bugs when you only meant to remove a single occurrence.

*(This section is standard STL reference material, not captured from a specific whiteboard frame — worth double-checking against the actual recording if exact wording/examples matter.)*

---

*Note compiled from whiteboard screenshots taken at intervals while watching the recording — captures written content, not verbal-only explanations. Session runs 1:50:45 total.*
