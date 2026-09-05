---
tags: [live-session, drill, recursion, backtracking, permutations, subsets]
status: In Progress
date: 2026-09-04
duration: 01:46:23
source: https://maang.in/live-sessions/Drill-Session-Recursion-Backtracking-1405
---

# Drill Session — Recursion & Backtracking

Practice problems applying the [[2026-08-25-Backtracking-Framework|LCC MD backtracking framework]]. Each problem: **Question → Example → Logic → Solution** (full runnable code, verified with `g++-15`).

## ⚡ Quick Revision (problems in this drill)

1. **Letter Tile Possibilities** (LC 1079) — *count "permutations of all subset sizes"* → build arrangements tile-by-tile, **count every non-empty node**.
2. **Path with Maximum Gold** (LC 1219) — grid backtracking, **Level = `(x,y)`**, Choice = 4 directions, Move = block cell (`=0`) → recurse → restore, **Decide = `max` at every cell** (you may stop anywhere).
3. **Max Length of Concatenated String with Unique Chars** (LC 1239) — **Take/Not-Take subsets** + a "no repeated letter" check via a `set<char> used`, **Decide = `max` length at every node**.
4. **Max Score Words Formed by Letters** (LC 1255, Hard) — **Take/Not-Take subsets** over words + a **letter-count budget** `cnt[26]`, **Decide = `max` score at every node**.
5. **Verbal Arithmetic Puzzle** (LC 1307, Hard) — it's just **§5 permutations**: assign each letter a **distinct digit** (`usedDigit[10]` = `taken[]`), **Decide at the leaf = check `Σwords == result`**. Prune: >10 letters → impossible; `bool` return stops at first solution.

**Shared idea across all:** *you may stop anytime → Decide at **every** node* (P1 counts, P2/P3/P4 take max; P5 is a leaf-decision — assign all then verify). **Check gates the Move:** validate the choice, then apply → recurse → revert.

*(LC 1238 Circular Permutation was in the drill but skipped here — it's a Gray-code formula `start ^ i ^ (i>>1)`, not a backtracking problem.)*

---

## 1. Letter Tile Possibilities — LeetCode 1079 (Medium)

### Question
You have `n` tiles, each with one letter `tiles[i]`. Return the number of **possible non-empty sequences of letters** you can make using the tiles.

### Examples
| Input | Output |
|-------|--------|
| `"AAB"` | `8` → `A, B, AA, AB, BA, AAB, ABA, BAA` |
| `"AAABBC"` | `188` |
| `"V"` | `1` |

### Logic — "permutations of all subset sizes"

Build strings by **adding one tile at a time**, and **count every non-empty string** you pass through.

- **Order matters** (`AB` and `BA` are different) → it's arrangements = **permutations**.
- You may **stop at any length** (use only some tiles) → so you count **partial** strings too, not just full-length ones. That's what makes it *"permutations of all subset sizes"* rather than plain full permutations.
- **Repeated tiles:** work with each distinct letter's **remaining count** (not tile positions), so "add an A" is one choice no matter which A — this avoids duplicate sequences.

**Tree for `"AB"`** (each node = one sequence; count every node except the empty root):

```
              ""            (level 0 — empty, don't count)
            /     \
         add A    add B
         "A" ✓    "B" ✓     (level 1)
          |         |
         add B    add A
         "AB" ✓   "BA" ✓    (level 2)
```
→ `A, B, AB, BA` = **4**.

Two knobs do all the work:
- **`level > 0`** → count only **non-empty** strings (skip the empty root).
- **`count > 0`** → can only add a tile that still has copies left (handles repeats).

> This is exactly the framework's [[2026-08-25-Backtracking-Framework|§7 Permutations × Non-distinct]] (count-decrement), with one change: **count at *every* node** instead of only at the leaf. Leaf-only would give the *full* permutations (`AAB`→3); every-node gives all subset sizes (`AAB`→8).

### Solution

```cpp
#include <bits/stdc++.h>
using namespace std;

map<char,int> freq;                 // letter -> how many tiles of it
vector<pair<char,int>> valfreq;     // {letter, remaining count}
int ans;
string cur;                         // the sequence built so far

// Each call represents ONE sequence: the string currently in `cur`.
void rec(int level){                // level = length of cur
    if(level > 0){                  // skip the empty string (level 0)
        ans++;                      // count this non-empty sequence
        // cout << cur << "\n";     // <-- UNCOMMENT to print each sequence
    }
    for(int ch = 0; ch < (int)valfreq.size(); ch++){   // try every DISTINCT letter
        if(valfreq[ch].second > 0){                    // only if tiles of it remain
            cur.push_back(valfreq[ch].first);          // place the letter
            valfreq[ch].second--;                      // use one tile
            rec(level + 1);                            // extend the sequence
            valfreq[ch].second++;                      // put the tile back (backtrack)
            cur.pop_back();                            // remove the letter (backtrack)
        }
    }
}

int numTilePossibilities(string tiles){
    freq.clear(); valfreq.clear(); ans = 0; cur.clear();
    for(char c : tiles) freq[c]++;              // count tiles per letter
    for(auto v : freq) valfreq.push_back(v);    // -> [{letter, count}, ...]
    rec(0);
    return ans;
}

int main(){
    ios_base::sync_with_stdio(0); cin.tie(0);
    string tiles; cin >> tiles;
    cout << numTilePossibilities(tiles) << "\n";
    return 0;
}
```

**Verified** (`g++-15 -std=gnu++17 -O2 -Wall`, clean): `AAB → 8`, `AAABBC → 188`, `V → 1`.
Uncomment the `cout` to see the sequences — for `AAB`: `A, AA, AAB, AB, ABA, B, BA, BAA`.

*(We deliberately do **not** use `next_permutation` or a lambda — the count-decrement recursion is cleaner and reuses the framework.)*

---

## 2. Path with Maximum Gold — LeetCode 1219 (Medium)

### Question
In a gold mine `grid` (`m x n`), each cell holds gold (`0` = empty). Return the **maximum gold you can collect**:
- standing on a cell collects **all** its gold;
- move one step **up / down / left / right**;
- **can't revisit** a cell; **never** step on a `0` cell;
- you may **start and stop on any cell that has gold**.

### Examples
| Input | Output |
|-------|--------|
| `[[0,6,0],[5,8,7],[0,9,0]]` | `24` (path `9→8→7`) |
| `[[1,0,7],[2,0,6],[3,4,5],[0,3,0],[9,0,20]]` | `28` |

### Logic — grid backtracking (same LCC MD, `Level = (x, y)`)

The state you recurse on is now a **cell**, so the "level" is a coordinate pair `(x, y)` instead of a single index.

| LCC MD | Here |
|--------|------|
| **L — Level** | the cell `(x, y)` you're standing on |
| **C — Choice** | the **4 directions** (`dx/dy`) |
| **C — Check** | neighbor in bounds, gold `> 0`, not visited |
| **M — Move** | block cell + add gold (`g=0`, `collected += gold`) → recurse → revert both (`collected -= gold`, `g=gold`) |
| **D — Decide** | `ans = max(ans, collected)` at **every** cell — you may stop anywhere |

**Same "Decide at every node" as Problem 1:** because you can **stop on any cell**, every cell you stand on is a candidate answer — so you update `ans` at every node (here `max`, there `count`), not only at a leaf. Visited-tracking is done by temporarily setting the cell to `0` (and restoring it) — the grid version of `taken[]` / count-decrement. You also **start from every gold cell**.

### Solution

```cpp
#include <bits/stdc++.h>
using namespace std;

int m, n;
vector<vector<int>> g;
int ans, collected;             // collected = running gold on the current path
int dx[4] = {-1, 1, 0, 0};      // up, down, left, right
int dy[4] = { 0, 0,-1, 1};

void rec(int x, int y){                       // L — Level = cell (x, y)
    // D — Decide: you may STOP on any cell, so this path total is a candidate
    ans = max(ans, collected);

    for(int dir = 0; dir < 4; dir++){                 // C — Choice: the 4 directions
        int nx = x + dx[dir], ny = y + dy[dir];
        if(nx >= 0 && nx < m && ny >= 0 && ny < n && g[nx][ny] > 0){   // C — Check
            int gold = g[nx][ny];
            g[nx][ny] = 0;                            // M — mark visited (change)
            collected += gold;                        //     add gold
            rec(nx, ny);                              //     recurse
            collected -= gold;                        //     revert gold  (backtrack)
            g[nx][ny] = gold;                         //     restore cell (backtrack)
        }
    }
}

int getMaximumGold(){
    ans = 0;
    for(int x = 0; x < m; x++)
        for(int y = 0; y < n; y++)
            if(g[x][y] > 0){                          // you may START on any gold cell
                int gold = g[x][y];
                g[x][y] = 0;
                collected = gold;                     // start the path with this cell's gold
                rec(x, y);
                g[x][y] = gold;
            }
    return ans;
}

int main(){
    ios_base::sync_with_stdio(0); cin.tie(0);
    cin >> m >> n;                                    // grid dimensions
    g.assign(m, vector<int>(n));
    for(int i = 0; i < m; i++)
        for(int j = 0; j < n; j++) cin >> g[i][j];
    cout << getMaximumGold() << "\n";
    return 0;
}
```

**Verified** (`g++-15 -std=gnu++17 -O2 -Wall`, clean): `[[0,6,0],[5,8,7],[0,9,0]] → 24`, and `[[1,0,7],[2,0,6],[3,4,5],[0,3,0],[9,0,20]] → 28`.
*(Input format: first line `m n`, then the grid rows.)*

---

## 3. Maximum Length of a Concatenated String with Unique Characters — LeetCode 1239 (Medium)

### Question
Given an array of strings `arr`. A string `s` is the **concatenation of a subsequence** of `arr` that has **all unique characters**. Return the **maximum possible length** of `s`.

### Examples
| Input | Output |
|-------|--------|
| `["un","iq","ue"]` | `4` — `"uniq"` or `"ique"` (`"unique"` invalid, `u` repeats) |
| `["cha","r","act","ers"]` | `6` — `"acters"` / `"cheers"` |
| `["abcdefghijklmnopqrstuvwxyz"]` | `26` |

### Logic — Take/Not-Take subsets + uniqueness check

It's **§4 Subsets (Take / Not-Take)** with a validity check and maximize:
- **L — Level** = index into `arr` (which string to decide)
- **C — Choice** = **Take** or **Not-Take** `arr[level]`
- **C — Check** = if taking, the string has **no internal duplicate** letter **and** shares **no letter with those already used**
- **M — Move** = add its letters to a `set<char> used`, recurse, then remove them (change→recurse→revert)
- **D — Decide** = `ans = max(ans, curLen)` at **every** node (any valid subset counts — stop anytime)

**Two sets, two jobs:** `used` (global) = letters from strings already taken on this path (checks cross-string clashes; reverted on backtrack); `seen` (local in `canTake`) = letters inside the *current* string (checks its own internal duplicate).

### Solution

```cpp
#include <bits/stdc++.h>
using namespace std;

int n;
vector<string> arr;
set<char> used;      // the letters used so far  (readable alt to a 26-bit mask)
int curLen, ans;

// can we TAKE this string?  (no internal dup, and no letter already in `used`)
bool canTake(const string& s){          // C — Check
    set<char> seen;
    for(char c : s){
        if(seen.count(c)) return false;   // internal duplicate letter
        if(used.count(c)) return false;   // clashes with letters already used
        seen.insert(c);
    }
    return true;
}

void rec(int level){                    // L — Level = index into arr
    ans = max(ans, curLen);             // D — Decide: update max at every node
    if(level == n) return;              // base

    // C — Choice: Take / Not-Take
    if(canTake(arr[level])){            // TAKE
        for(char c : arr[level]) used.insert(c);   // M — add letters to the set
        curLen += (int)arr[level].size();
        rec(level + 1);                            //     recurse
        for(char c : arr[level]) used.erase(c);    //     revert (remove them)
        curLen -= (int)arr[level].size();
    }
    rec(level + 1);                     // NOT-TAKE
}

int maxLength(vector<string>& a){
    arr = a; n = a.size(); used.clear(); curLen = 0; ans = 0;
    rec(0);
    return ans;
}

int main(){
    ios_base::sync_with_stdio(0); cin.tie(0);
    int k; cin >> k;                    // number of strings
    vector<string> a(k);
    for(auto &s : a) cin >> s;
    cout << maxLength(a) << "\n";
    return 0;
}
```

**Verified** (`g++-15 -std=gnu++17 -O2 -Wall`, clean): `["un","iq","ue"] → 4`, `["cha","r","act","ers"] → 6`, `[a…z] → 26`, `["aa","bb"] → 0`.
*(Input: first line `k` = number of strings, then the `k` strings.)*

**Faster alternative:** replace the `set<char>` with a **26-bit `int` mask** — `used & mask` for the clash check, `used |= mask` / `used ^= mask` to add/remove, and precompute each string's mask (marking `-1` if it has an internal duplicate). Same logic, `O(1)` checks.

---

## 4. Maximum Score Words Formed by Letters — LeetCode 1255 (Hard)

### Question
Given `words`, a multiset of `letters` (may repeat), and a `score[0..25]` per character. Return the **maximum total score of any valid set of words**, where:
- each `words[i]` is used **at most once**,
- each **letter** is used **at most once** (you needn't use all),
- a word's score = sum of its letters' scores.

### Examples
| Input | Output |
|-------|--------|
| `words=["dog","cat","dad","good"]`, `letters=[a,a,c,d,d,d,g,o,o]`, `a=1,c=9,d=5,g=3,o=2` | `23` (`"dad"`=11 + `"good"`=12) |
| `words=["xxxz","ax","bx","cx"]`, `letters=[z,a,b,c,x,x,x]`, `a=b=c=4,x=5,z=10` | `27` (`ax+bx+cx`) |

### Logic — Take/Not-Take subsets + a letter-count budget

Same **Take/Not-Take** shape as Problem 3, but the "used set" is now a **count budget** `cnt[26]` (a letter can appear several times), and we maximize **score**.

- **L — Level** = index into `words`
- **C — Choice** = **Take / Not-Take** `words[level]`
- **C — Check** = `canForm` — enough of each letter left (`need[i] <= cnt[i]`). *Check gates the Move.*
- **M — Move** = spend letters + add score (`cnt--`, `curScore += add`) → recurse → **revert both** (`curScore -= add`, `cnt++`)
- **D — Decide** = `ans = max(ans, curScore)` at **every** node (any subset of words is valid)

> `curScore` is kept as a **global accumulator** (not a function parameter): add it on the way in, subtract it on the way out — the same change→recurse→revert as the letters. (Passing it as `rec(level, curScore+add)` also works — the value auto-restores when the call returns — but the global+revert form is symmetric with the rest of the state.)

### Solution

```cpp
#include <bits/stdc++.h>
using namespace std;

int n;
vector<string> words;
int sc[26];        // score of each letter
int cnt[26];       // remaining available letters (the budget)
int curScore, ans; // running score along the current path

bool canForm(const string& w){        // C — Check: enough letters?
    int need[26] = {0};
    for(char c : w) need[c - 'a']++;
    for(int i = 0; i < 26; i++) if(need[i] > cnt[i]) return false;
    return true;
}
int wordScore(const string& w){ int s = 0; for(char c : w) s += sc[c - 'a']; return s; }

void rec(int level){                  // L — Level = index into words
    ans = max(ans, curScore);         // D — max score at every node
    if(level == n) return;            // base

    // C — Choice: Take / Not-Take words[level]
    if(canForm(words[level])){                          // CHECK first (gates the Move)
        int add = wordScore(words[level]);              // this word's score (compute once)
        for(char c : words[level]) cnt[c - 'a']--;      // M — spend its letters
        curScore += add;                                //     add its score
        rec(level + 1);                                 //     recurse
        curScore -= add;                                //     revert score   (backtrack)
        for(char c : words[level]) cnt[c - 'a']++;      //     restore letters (backtrack)
    }
    rec(level + 1);                   // NOT-TAKE
}

int maxScoreWords(vector<string>& w, vector<char>& letters, vector<int>& score){
    words = w; n = w.size();
    for(int i = 0; i < 26; i++){ sc[i] = score[i]; cnt[i] = 0; }
    for(char c : letters) cnt[c - 'a']++;      // build the letter budget
    curScore = 0; ans = 0; rec(0);
    return ans;
}

int main(){
    ios_base::sync_with_stdio(0); cin.tie(0);
    int nw; cin >> nw;
    vector<string> w(nw); for(auto& s : w) cin >> s;
    int nl; cin >> nl;
    vector<char> letters(nl); for(auto& c : letters) cin >> c;
    vector<int> score(26); for(auto& x : score) cin >> x;
    cout << maxScoreWords(w, letters, score) << "\n";
    return 0;
}
```

**Verified** (`g++-15 -std=gnu++17 -O2 -Wall`, clean): Example 1 → `23`, Example 2 → `27`.
*(Input: `nw` + words, then `nl` + letters, then 26 scores.)*

---

## 5. Verbal Arithmetic Puzzle — LeetCode 1307 (Hard)

### Question
Given `words` (left side) and `result` (right side). Return `true` if the equation is solvable where: each **character → a digit** (0–9), **distinct characters get distinct digits**, numbers have **no leading zeros**, and **Σ(words) == result**.

### Examples
| Input | Output |
|-------|--------|
| `words=["SEND","MORE"]`, `result="MONEY"` | `true` (`SEND+MORE=MONEY`) |
| `words=["SIX","SEVEN","SEVEN"]`, `result="TWENTY"` | `true` |
| `words=["LEET","CODE"]`, `result="POINT"` | `false` |

### Logic — it's just §5 permutations (assign each letter a distinct digit)

Assigning **distinct digits to distinct letters is a permutation** — so this is the **[[2026-08-25-Backtracking-Framework|§5 Permutations × Distinct]]** cell, with letters getting digits:

| §5 Permutations | Verbal Arithmetic |
|---|---|
| pick any **unused element** | pick any **unused digit** |
| `taken[]` | `usedDigit[10]` |
| Level = position in `sol` | Level = which letter |
| Decide at leaf: *print* | Decide at leaf: **check `Σwords == result`** |

- **L — Level** = which distinct letter to assign
- **C — Choice** = a digit `0–9`
- **C — Check** = `!usedDigit[d]` (each digit once) **+** no leading letter gets `0`
- **M — Move** = **do** (assign) → **try** (recurse) → **undo** (revert)
- **D — Decide** = at the leaf (all letters assigned), verify the sum

**Two prunings (both in the code):**
1. **Decline early** — more than **10 distinct letters** can't fit 10 digits → `return false` before recursing.
2. **`bool` short-circuit** — it's a *yes/no* problem, so `rec` returns `bool`; the first valid assignment `return true`s and abandons the rest.

### Solution

```cpp
#include <bits/stdc++.h>
using namespace std;

vector<string> words;
string result;
vector<char> letters;     // distinct letters to assign
set<char> leading;        // first letter of a multi-char word/result -> can't be 0
int mp[128];              // letter -> digit
bool usedDigit[10];       // which digits are taken  (= the taken[] from §5)

long long val(const string& s){          // decode a word using current assignment
    long long v = 0;
    for(char c : s) v = v * 10 + mp[(int)c];
    return v;
}
bool check(){                            // D — Σ words == result ?
    long long sum = 0;
    for(auto& w : words) sum += val(w);
    return sum == val(result);
}

bool rec(int idx){                       // L — Level = which distinct letter
    if(idx == (int)letters.size()) return check();   // all assigned -> verify
    char c = letters[idx];
    for(int d = 0; d <= 9; d++){         // C — Choice: a digit
        if(usedDigit[d]) continue;                    // Check: digit unused (= taken[])
        if(d == 0 && leading.count(c)) continue;      // Check: no leading zero
        usedDigit[d] = true; mp[(int)c] = d;          // DO  — assign digit d to letter c
        if(rec(idx + 1)) return true;                 // TRY — recurse; stop at first solution
        usedDigit[d] = false;                         // undo — revert (backtrack)
    }
    return false;
}

bool isSolvable(vector<string>& w, string r){
    words = w; result = r;
    set<char> s;
    for(auto& x : w) s.insert(x.begin(), x.end());
    s.insert(r.begin(), r.end());
    if((int)s.size() > 10) return false;              // PRUNE: >10 letters -> impossible
    letters.assign(s.begin(), s.end());
    leading.clear();
    for(auto& x : w) if(x.size() > 1) leading.insert(x[0]);
    if(r.size() > 1) leading.insert(r[0]);
    memset(usedDigit, 0, sizeof(usedDigit));
    return rec(0);
}

int main(){
    ios_base::sync_with_stdio(0); cin.tie(0);
    int nw; cin >> nw;
    vector<string> w(nw); for(auto& x : w) cin >> x;
    string r; cin >> r;
    cout << (isSolvable(w, r) ? "true" : "false") << "\n";
}
```

**Verified** (`g++-15 -std=gnu++17 -O2 -Wall`, clean): `SEND+MORE=MONEY → true`, `SIX+SEVEN+SEVEN=TWENTY → true`, `LEET+CODE=POINT → false`.
*(Input: `nw` + words, then `result`.)*

**Faster alternative (not needed here):** process the addition **column-by-column with carry** (assign letters as you sweep right-to-left, check each column's arithmetic, backtrack the instant one fails). More pruning, but more code — the plain "permutations + verify at the leaf" is clear and fast enough for these constraints.

---

*Note compiled from whiteboard/IDE screenshots while watching the recording (Problems 1–5 captured from the 1:46:23 session; LC 1238 skipped as non-backtracking). More problems to be appended as captured. Every solution is a full runnable program, compiled clean and output-verified with `g++-15 -std=gnu++17 -O2 -Wall` using `#include <bits/stdc++.h>`.*
