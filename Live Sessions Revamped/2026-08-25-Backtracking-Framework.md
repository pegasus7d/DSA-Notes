---
tags: [live-session, backtracking, recursion, framework, subsets, permutations]
status: In Progress
date: 2026-08-25
duration: 01:51:22
source: https://maang.in/live-sessions/Backtracking-Framework-1394
---

# Backtracking Framework

## ⚡ Quick Revision (60 sec)

- **Backtracking = recursion-based brute force** — "explore every possibility": at each level **make a Choice → recurse → undo it**. The undo *is* the backtrack.
- **Framework = LCC MD:** **L**evel · **C**hoice · **C**heck · **M**ove *(change → recurse → revert)* · **D**ecide *(print/record at the leaf)*.
- **4 core problems = one skeleton.** Only the **Choice** and the **State** change:

| | **Distinct** | **Non-distinct** (duplicates) |
|---|---|---|
| **Subsets** `2ⁿ` | Take / Not-Take · state `multiset mt` | choose `0..freq` copies per value · state `valfreq` |
| **Permutations** `N!` | pick any **unused** · state `taken[]` | pick value with `count>0`, `second--/++` · state `valfreq` |

- **Non-distinct trick:** recurse over **distinct values + counts** (`valfreq`), not raw elements → duplicates never generated.
- **Revert cheatsheet:** subset → `mt.erase(mt.find(x))` (one copy, not all); perm → `taken[ch]=0`; non-distinct → restore the count.
- **Verified counts** on the worked examples: subsets{1,2,3}=8 · perms{1,2,3}=6 · subsets{1,2,2}=6 · perms{1,2,2}=3.

<sub>Full derivation, code, and I/O below.</sub>

---

The session builds one reusable mental model for *all* backtracking problems: backtracking is **recursion-implemented brute force** — you **explore every possibility** by, at each step, making a choice, recursing, and then undoing the choice. The instructor packages this into a 5-step mnemonic (**LCC MD**) and shows the four canonical problems it covers as a **2×2 matrix**, then codes the Subsets case end-to-end.

## 1. What Backtracking *Is*

From the opening board:

- **Backtracking → Recursion** — recursion is the *coding tool* used to implement it.
- **Backtracking → Brute force** — it systematically **"explores every possibility."**

So backtracking isn't a separate algorithm so much as a *disciplined way to brute-force* a search space using recursion, where you can **undo** each choice to try the next one.

## 2. The 2×2 Problem Matrix

The framework covers four fundamental problems, split along two axes:

|                          | **Distinct** elements | **Non-distinct** (duplicates present) |
|--------------------------|-----------------------|----------------------------------------|
| **Subsets**  (count `2^N`) | all subsets of the set | same, but **drop duplicate subsets** |
| **Permutations** (count `N!`) | all orderings          | same, but **skip duplicate orderings** |

Worked on `Arr = {2, 1, 3}`:

- **Subsets** → `{}`, `{1}`, `{2}`, `{3}`, `{1,2}`, `{1,3}`, `{2,3}`, `{1,2,3}` — total **2³ = 8**.
- **Permutations** → `123, 132, 213, 231, 312, 321` — total **N! = 6**.

The **distinct vs non-distinct** column is the whole reason the framework has a "Check" step (see §3): with duplicates you must prune the choices that would regenerate an identical subset/permutation.

## 3. The LCC MD Framework ⚡

The core mnemonic — five steps run at each recursion level:

| Step | Name | Meaning |
|------|------|---------|
| **L** | **Level**  | *where do you have choices* — each recursion level is one decision point |
| **C** | **Choice** | the options at this level (for subsets: **T / NT** = Take / Not-Take) |
| **C** | **Check**  | test the **restriction / constraint** before committing a choice |
| **M** | **Move**   | **try the choice and go into recursion** (change state → recurse → revert) |
| **D** | **Decide** | at the **last level**, decide whether the built candidate is valid / record it |

### Recursion tree (Take / Not-Take)

For `{2, 1, 3}`, each element becomes a **level**, and at each level you branch **T** (take) vs **NT** (not-take):

```
level 0 (elem 2):        T / NT
level 1 (elem 1):     T / NT   T / NT
level 2 (elem 3):   ... T/NT at each ...
```

Every root-to-leaf path is one subset — `2^N` leaves for `N` levels.

## 4. Code — Subsets via Backtracking

Built live from the empty skeleton up to a running program. Annotated with the LCC MD steps:

```cpp
#include <bits/stdc++.h>
using namespace std;

int n;
int arr[1010];
multiset<int> mt;                 // current subset

void rec(int level){              // L — Level
    if(level == n){               // D — Decide (all n elements decided)
        cout << mt.size() << ": ";
        for(int x : mt) cout << x << " ";
        cout << endl;
        return;
    }
    // C — Choice: Take / Not-Take   |   M — change → recurse → revert
    // TAKE
    mt.insert(arr[level]);            // CHANGE DS
    rec(level + 1);                   // recurse
    mt.erase(mt.find(arr[level]));    // REVERT DS (one copy — the "backtrack")
    // NOT-TAKE
    rec(level + 1);
}

void solve(){
    cin >> n;
    for(int i = 0; i < n; i++) cin >> arr[i];
    rec(0);                           // start at level 0, empty subset
}

int main(){
    ios_base::sync_with_stdio(0); cin.tie(0);
    solve();
    return 0;
}
```

**Verified:** `input 3 / 1 2 3` → all 2³ = 8 subsets (`3: 1 2 3`, `2: 1 2`, `2: 1 3`, `1: 1`, `2: 2 3`, `1: 2`, `1: 3`, `0:`).

## 5. Permutations — the other *Distinct* cell

Same **LCC MD** skeleton, but the **Choice** and **Check** change. Now the **level is a *position* of the solution array** (`level = idx of sol`), and at each position you may place **any element not yet used**.

| Step | Permutation version |
|------|---------------------|
| **L — Level**  | index of `sol[]` being filled |
| **C — Choice** | `for(ch = 0..n-1)` — try **every** element |
| **C — Check**  | `if(!taken[ch])` — only if that element is **not already used** |
| **M — Move**   | mark `taken[ch]=1`, set `sol[level]=arr[ch]`, recurse, then **revert both** |
| **D — Decide** | at `level == n`, print `sol[]` |

**Extra state vs subsets:** a **`taken[]`** array (which elements are already used) and a **`sol[]`** array (the permutation being built).

```cpp
#include <bits/stdc++.h>
using namespace std;

int n;
int arr[1010];
int taken[1010];                  // taken[i]=1 if arr[i] already used
int sol[1010];

void rec(int level){              // L — Level = position of sol
    if(level == n){               // D — Decide
        for(int i = 0; i < n; i++) cout << sol[i] << " ";
        cout << endl;
        return;
    }
    for(int ch = 0; ch < n; ch++){    // C — Choice: every element
        if(!taken[ch]){               // C — Check: not already used
            taken[ch] = 1;            // M — mark
            sol[level] = arr[ch];     //     place
            rec(level + 1);           //     recurse
            sol[level] = 0;           //     revert sol
            taken[ch] = 0;            //     unmark (backtrack)
        }
    }
}

void solve(){
    cin >> n;
    for(int i = 0; i < n; i++) cin >> arr[i];
    rec(0);
}

int main(){
    ios_base::sync_with_stdio(0); cin.tie(0);
    solve();
    return 0;
}
```

> The live IDE frame was mid-writing — it had `sol[level]=arr[ch]; rec(level+1); sol[level]=0;` but was **missing the `taken[ch]=1/0` marking and the base case**. Without marking `taken[]`, the check never blocks and you'd get all `nⁿ` combinations instead of `N!` permutations. Completed above.

**Simple I/O:**

```
Input:            Output:
3                 1 2 3
1 2 3             1 3 2
                  2 1 3
                  2 3 1
                  3 1 2
                  3 2 1
```

All **N! = 3! = 6** permutations of `{1,2,3}` (compiled with `g++ -std=c++17`, output matches the whiteboard order).

## 6. Subsets × Non-distinct — count per value

With duplicate values, naive Take/Not-Take on each element **double-counts** subsets (e.g. `{2}` from either of the two 2's). The fix reframes the **Choice**: **group duplicates and, per distinct value, pick how many copies to take (`0 .. frequency`).**

**Whiteboard example — `Arr = {1, 2, 2}`:** distinct values `1` (×1) and `2` (×2).
- value **1** → choose `0 | 1` copies
- value **2** → choose `0 | 1 | 2` copies
- distinct subsets = `(1+1) × (2+1) = 6` (not `2³ = 8`): `{}`, `{1}`, `{2}`, `{1,2}`, `{2,2}`, `{1,2,2}`.

**Setup — build a frequency structure so recursion runs over *distinct values*, not raw elements:**

```cpp
#include <bits/stdc++.h>
using namespace std;

int n;
int arr[1010];
map<int,int> freq;                 // value -> count
vector<pair<int,int>> valfreq;     // {value, count}, indexable by level
multiset<int> sol;                 // current subset

void rec(int level){              // L — Level = distinct-value index
    if(level == (int)valfreq.size()){   // D — Decide
        cout << sol.size() << ": ";
        for(int x : sol) cout << x << " ";
        cout << endl;
        return;
    }
    int val = valfreq[level].first, cnt = valfreq[level].second;
    for(int ch = 0; ch <= cnt; ch++){                     // C — Choice: take 0..cnt copies
        for(int i = 0; i < ch; i++) sol.insert(val);      // M — insert ch copies
        rec(level + 1);                                   //     recurse
        for(int i = 0; i < ch; i++) sol.erase(sol.find(val)); // revert ch copies via find
    }
}

void solve(){
    cin >> n;
    for(int i = 0; i < n; i++){ cin >> arr[i]; freq[arr[i]]++; }
    for(auto v : freq) valfreq.push_back(v);
    rec(0);
}

int main(){
    ios_base::sync_with_stdio(0); cin.tie(0);
    solve();
    return 0;
}
```

> The live IDE version was mid-debugging — it indexed `valfreq[i]` (with `i` the inner copy-counter) instead of `valfreq[level]`, and reverted with a single `sol.erase(valfreq[i].first)`. Two fixes: (1) index by **`level`**, and (2) **the revert must be a `for` loop erasing `ch` copies one at a time via `sol.erase(sol.find(val))`** — because `multiset::erase(value)` would wipe **all** copies, over-reverting. Corrected above.

**Simple I/O:**

```
Input:            Output (size: elements):
3                 0:
1 2 2             1: 2
                  2: 2 2
                  1: 1
                  2: 1 2
                  3: 1 2 2
```

All **6** distinct subsets of `{1,2,2}` — the two duplicates that naive Take/Not-Take would produce are structurally avoided (compiled with `g++ -std=c++17`).

## 7. Permutations × Non-distinct — count-decrement

The cleanest of the four cells. Combine the **permutation** structure (fill positions of `sol`) with the **count tracking** from §6: at each position, choose a **distinct value that still has copies left**, place it, **decrement its count**, recurse, then **restore**.

**Whiteboard example — `Arr = {1, 2, 2}`:** grouped as `1` (×1), `2` (×2). Distinct permutations = `3! / 2! = 3` — the two identical 2's never become separate choices, so no duplicate orderings.

```cpp
#include <bits/stdc++.h>
using namespace std;

int n;
int arr[1010];
map<int,int> freq;                 // value -> count
vector<pair<int,int>> valfreq;     // {value, count}
int sol[1010];

void rec(int level){              // L — Level = position of sol
    if(level == n){               // D — Decide
        for(int i = 0; i < n; i++) cout << sol[i] << " ";
        cout << endl;
        return;
    }
    for(int ch = 0; ch < (int)valfreq.size(); ch++){   // C — Choice: over DISTINCT values
        if(valfreq[ch].second > 0){                    // C — Check: copies still left
            sol[level] = valfreq[ch].first;            // M — place the value
            valfreq[ch].second--;                      //     consume one copy
            rec(level + 1);                            //     recurse
            valfreq[ch].second++;                      //     restore count (backtrack)
            sol[level] = 0;                            //     revert sol
        }
    }
}

void solve(){
    cin >> n;
    for(int i = 0; i < n; i++){ cin >> arr[i]; freq[arr[i]]++; }
    for(auto v : freq) valfreq.push_back(v);
    rec(0);
}

int main(){
    ios_base::sync_with_stdio(0); cin.tie(0);
    solve();
    return 0;
}
```

The **only change from distinct permutations (§5)** is the *state*: instead of a `taken[]` boolean per element, you keep a **remaining-count per distinct value** (`valfreq[ch].second`) and gate the choice on `count > 0`. Iterating over distinct values (not raw elements) is what removes the duplicate orderings.

> The live IDE frame had this exact recursive logic but the **base case was still empty** (no print), so its output panel was stale. Base-case print added above.

**Simple I/O:**

```
Input:            Output:
3                 1 2 2
1 2 2             2 1 2
                  2 2 1
```

All **3 = 3!/2!** distinct permutations of `{1,2,2}` — duplicate orderings avoided (compiled with `g++ -std=c++17`).

---

### The 2×2 matrix — complete ✅

| | **Distinct** | **Non-distinct** |
|---|---|---|
| **Subsets** (2ⁿ) | §4 — Take / Not-Take | §6 — count per value (`0..freq`) |
| **Permutations** (N!) | §5 — pick any unused (`taken[]`) | §7 — count-decrement (`count>0`, `second--/++`) |

## Key Insight ⚡

Every backtracking problem = **pick a Level → enumerate Choices → Check constraints → Move (change / recurse / revert) → Decide at the leaf.** Change *what a "choice" is* (Take/Not-Take for subsets; pick-any-unused for permutations) and *what "Check" prunes* (nothing for distinct; duplicate-skips for non-distinct) and the same `LCC MD` skeleton solves all four cells of the matrix.

## Pattern Tag

`#backtracking` · `#recursion` · framework: **LCC MD** (Level · Choice · Check · Move · Decide)

---

*Note compiled from whiteboard/IDE screenshots taken while watching the recording (captured up to ~1:27 / 1:51:22). Covers **all 4** matrix cells: **Subsets × Distinct** (§4), **Permutations × Distinct** (§5), **Subsets × Non-distinct** (§6), **Permutations × Non-distinct** (§7). Several code frames were captured mid-writing and completed/corrected here (subsets base-case print + `mt` decl reconstructed; permutations `taken[]` marking + base case added; non-distinct subsets `valfreq[i]`→`valfreq[level]` index fix + `for`-loop `erase(find())` revert; non-distinct permutations base-case print added). Every cell's code is a full runnable program (with `main`) and was independently compiled clean — no warnings — with `g++-15 -std=gnu++17 -O2 -Wall` (Homebrew GCC 15.2.0) using `#include <bits/stdc++.h>`, and its output verified.*
