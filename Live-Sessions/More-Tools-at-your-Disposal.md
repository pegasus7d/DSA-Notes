---
tags: [live-session, contribution-technique, hashing, prefix-sums, stl, two-pointers, upper-bound]
status: Watched
duration: 01:43:07
source: https://maang.in/live-sessions/More-Tools-at-your-Disposal-1328
---

# More Tools at your Disposal

Session outline (from the opening whiteboard): **Contribution technique → Atomic (single-element) contribution, Extending Ends**; **Pivot → Cumulative DS**. In practice this played out as a sequence of problems where the point each time was to replace an `O(N^2)` brute force with an `O(N)`/`O(N log N)` pass by *maintaining a data structure* (a frequency map or an ordered set) while scanning, instead of re-deriving everything from scratch per query.

## 1. Two Sum — Count Pairs Summing to X

**Problem:** Given array `arr` of size `N` and target `X`, count the number of pairs `(i,j)` with `i<j` such that `arr[i] + arr[j] == X`.

**Worked example:** `arr = [2, 3, 5, 2, 0]`, `N=5`, `X=5` → `Ans = 3` pairs.

**Brute force — `O(N^2)`:**

```cpp
int cnt = 0;
for (int i = 0; i < n-1; i++) {
    for (int j = i+1; j < n; j++) {
        if (arr[i] + arr[j] == x) cnt++;
    }
}
```

**Optimized — `O(N log N)`**, by maintaining a frequency map while scanning left to right:

```cpp
map<int,int> freq;
long long ans = 0;
for (int j = 0; j < n; j++) {
    ans += freq[x - arr[j]];   // how many earlier elements would complete a pair with arr[j]
    freq[arr[j]]++;            // record arr[j] itself for future j's
}
```

**Why this only counts `i<j`, never `i==j`:** the lookup `freq[x - arr[j]]` happens *before* `freq[arr[j]]++` in each iteration. So walking through it: at `j=0`, `freq` is still empty (nothing to pair with `arr[0]`), then `arr[0]` gets inserted. At `j=1`, `freq` contains only `{arr[0]}`, so the only pair checked is `(0,1)`. At `j=2`, `freq` contains `{arr[0], arr[1]}`, checking pairs `(0,2)` and `(1,2)`. In general, by the time index `j` is processed, `freq` holds exactly `arr[0..j-1]` — every element strictly before `j` — so every pair considered satisfies `i<j` by construction, and no element is ever paired with itself.

## 2. Best Time to Buy and Sell Stock (LeetCode 121)

**Problem:** Given `prices[i]` = price on day `i`, choose one day to buy and a later day to sell to maximize profit (`0` if no profit possible).

**Sample:** `prices=[7,1,5,3,6,4] → 5` (buy day 2 at price 1, sell day 5 at price 6). `prices=[7,6,4,3,1] → 0` (no profitable transaction). Constraints: `1 ≤ prices.length ≤ 10^5`, `0 ≤ prices[i] ≤ 10^4`.

**Intuition:** track the minimum price seen so far while scanning left to right; at each day `j`, the best possible profit selling on day `j` is `prices[j] - min_so_far`.

```cpp
int maxProfit(vector<int>& prices) {
    int best = 0;
    int minseen = 1e9;
    for (int j = 0; j < prices.size(); j++) {
        best = max(best, prices[j] - minseen);
        minseen = min(minseen, prices[j]);
    }
    return best;
}
```

Runs in `O(N)`, a single pass — no need to check every `(buy, sell)` pair explicitly.

## 3. Smallest Pair Sum `≥ X` ("Extending Ends")

**Problem:** Given array `arr` and target `X`, find the smallest value of `arr[i] + arr[j]` (for a valid pair `i<j`, going by the earlier pair-counting convention) that is `≥ X`. If no such pair exists, the answer is `-1`.

**Worked example:** `arr = [2, 3, 5, 7, 3, 2]`, `X = 5` → answer `6`.

**Key STL fact used — `upper_bound`:** `upper_bound(X)` on a sorted container/ordered `set` returns an iterator to the **first element strictly greater than `X`**. Equivalently, `upper_bound(X) - begin()` gives the count of elements `≤ X`.

**Optimized — `O(N log N)`**, maintaining an ordered `set` of elements seen so far and using `upper_bound` to find the best partner for each new element:

```cpp
void solve(){
    int n, x;
    cin >> n >> x;
    int arr[n];
    for (int i = 0; i < n; i++) cin >> arr[i];

    int best = 1e9;
    set<int> st;
    for (int j = 0; j < n; j++) {
        auto it = st.upper_bound(x - arr[j]);
        if (it != st.end()) {
            best = min(best, arr[j] + (*it));
        }
        st.insert(arr[j]);
    }
    // best is 1e9 if no valid pair was found -> convert to -1 before printing
}
```

Same "maintain a data structure while scanning" pattern as the Two Sum problem above, except the data structure is an ordered `set` (so it supports `upper_bound`) instead of a plain frequency map, and the query is "find the smallest partner that still clears the threshold" rather than "count exact matches."

## 4. Prefix Sums / Partial Sums — Refresher

**Definition:** `P(i) = arr[0] + arr[1] + ... + arr[i]`, equivalently the recursive form `P(i) = P(i-1) + arr(i)`.

**Worked example:** `arr = [1, 3, 2, 5, 3, 6]` → `P = [1, 4, 6, 11, 14, 20]`.

**Range-sum formula:** for range `[L, R]`, `sum(L..R) = P[R] - P[L-1]`.

**Verified:** range `[1..4]` → `P[4] - P[0] = 14 - 1 = 13`, matching `arr[1]+arr[2]+arr[3]+arr[4] = 3+2+5+3 = 13`.

## 5. Number of Subarrays with Sum `= X` (LeetCode 560, "Subarray Sum Equals K")

**Problem:** Given `nums` and target `k`, return the total number of (contiguous, non-empty) subarrays whose sum equals `k`. Examples: `nums=[1,1,1], k=2 → 2`.

**Worked example from the whiteboard:** `arr = [0, 1, 0, -1, 3, -2]`, answer `6` for some `X` (walked through with prefix sums below).

**Derivation:** a subarray `[st, en]` sums to `X` iff `arr(st) + ... + arr(en) = X`, i.e. `P[en] - P[st-1] = X`, i.e. **`P[st-1] = P[en] - X`**. So for each right endpoint `en` (called `R` on the board), count how many earlier left-boundary prefixes `P[st-1]` (for `st-1 < en`) equal `P[en] - X` — the same "count exact matches via a frequency map while scanning" idea as Two Sum, just applied to prefix sums instead of raw array values.

**Worked prefix sums:** for `arr=[0,1,0,-1,3,-2]`, `P=[0,1,1,0,3,1]`.

**The `freq[0]=1` seeding trick:** to count subarrays that start at index `0` (where the "previous prefix" `P[-1]` doesn't literally exist), seed the frequency map with an implicit prefix sum of `0` *before* the scan starts. Concretely: with `X=0` and an all-zero array, an implicit "index `-1`" slot holding prefix sum `0` is what makes `P(R)=0` at `R=0` matchable — without the seed, that first subarray would be silently undercounted.

```cpp
int n, x;
cin >> n >> x;
int arr[n];
int p[n];
for (int i = 0; i < n; i++) {
    cin >> arr[i];
    p[i] = arr[i];
    if (i > 0) p[i] += p[i-1];
}

int ans = 0;
map<int,int> freq;
freq[0] = 1;   // seeds the "empty prefix" / subarrays starting at index 0
for (int r = 0; r < n; r++) {
    ans += freq[p[r] - x];
    freq[p[r]]++;
}

cout << ans << endl;
```

Runs in `O(N log N)` (map lookups), turning what would otherwise be an `O(N^2)` "check every subarray" brute force into a single pass with a running frequency map — the same recurring theme as sections 1 and 3 above.

---

*Note compiled from whiteboard/IDE screenshots taken at intervals while watching the recording. One transition frame (overlapping strokes introducing a hashmap/frequency-map idea between sections 1 and 2) was only partially legible and is not separately captured above; a later frame showing circled values `1,2,3,4` merging into pairwise sums alongside the recursive `P(i)=P(i-1)+arr(i)` definition also wasn't fully connected to surrounding context — worth a re-check against the recording if either matters. Session runs 1:43:07; the LeetCode 560 "Subarray Sum Equals K" tab was reached around 1:32:04, near the end of the recording.*
