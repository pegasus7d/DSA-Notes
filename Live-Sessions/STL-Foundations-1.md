---
tags: [live-session, stl, pair, vector, stack, queue, deque, set, multiset, pointers, map, priority-queue]
status: Watched
duration: 01:50:45
source: https://maang.in/live-sessions/STL-Foundations-1-1322
source2: https://maang.in/live-sessions/STL-Foundations-2-1323
duration2: 02:06:09
---

# STL Foundations 1 & 2

Two-part introductory series covering the core C++ STL containers used throughout the course.

# Part 1: STL Foundations 1

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

# Part 2: STL Foundations 2

Continuation session — Pointers, then Map and Priority Queue.

## 1. Pointers

```cpp
int a = 5;
int* p = &a;      // p stores the address of a

cout << a << endl;   // 5           (value of a)
cout << p << endl;   // 0x...       (address of a, stored in p)
cout << *p << endl;  // 5           (dereference p -> value at that address)
```

Diagram: `a` lives at address `0x14`; `p` stores `0x14` (i.e. `&a`) and `p` itself lives at address `0x15`. Also demonstrated `*p = 6;` to show writing through a pointer mutates `a` directly.

The code demo used the same competitive-programming boilerplate/macro template style as this vault's own problem notes (`f(i,n)` loop macro, `pb` for `push_back`, `fi`/`se` for `.first`/`.second`, `all(x)`, `sz(a)`, `mem(a,n)`, `ndl` for `cout<<endl`, `see(x)` debug macro, `py`/`pn` for Yes/No output, etc.) — worth comparing directly against your own `#define` header if you keep one.

## 2. (Unclear frame — pairs/vector example)

One captured frame (timestamp ~1:26:54) showed heavy overlapping strokes from what look like multiple whiteboard passes layered together: points labeled `P1`, `P2`, a set of coordinate pairs (`{-1,2}`, `{-1,-3}`, `{-2,1}`, `{-3,-2}`), a grid/table of numbers, and a `vector<int> a` declaration starting underneath. Content wasn't legible enough to reconstruct precisely — likely a comparator/sorting-by-coordinate example. Worth re-screenshotting a few seconds before/after that timestamp if this section matters.

## 3. Map

Not captured from a whiteboard screenshot in this pass, but confirmed to have been covered in full. Standard reference:

```cpp
map<int, int> mp;
mp[key] = value;        // insert or update — O(log n)
mp.insert({key, value}); // insert only if key doesn't already exist — O(log n)
mp.find(key);            // returns iterator to key, or mp.end() if absent — O(log n)
mp.erase(key);           // O(log n)
mp.count(key);           // 0 or 1 — O(log n)

for (auto& [k, v] : mp) {
    // iterates in ascending key order (map is a balanced BST / red-black tree)
}
```

**Gotcha:** `mp[key]` auto-inserts the key with a default value (0 for int) if it doesn't exist yet — even a read like `if (mp[key] == 0)` silently creates the entry. Use `mp.find(key)` or `mp.count(key)` to check existence without inserting.

## 4. Priority Queue

Not captured from a whiteboard screenshot in this pass, but confirmed to have been covered in full. Standard reference:

```cpp
priority_queue<int> pq;              // max-heap by default
pq.push(x);    // O(log n)
pq.pop();      // removes the largest element — O(log n)
pq.top();      // largest element — O(1)
pq.empty();    // O(1)
pq.size();     // O(1)

// Min-heap variant:
priority_queue<int, vector<int>, greater<int>> minPq;
```

---

*Note compiled from whiteboard screenshots taken at intervals while watching the recordings — captures written content, not verbal-only explanations. Part 1 runs 1:50:45; Part 2 runs 2:06:09. Sections marked "not captured" are standard STL reference material added based on your confirmation that the topic was covered, not transcribed from the actual whiteboard — worth double-checking against the recording if exact wording/examples matter.*
