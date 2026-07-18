---
tags: [live-session, contribution-technique, dp, kadane]
status: In Progress
source: https://maang.in/live-sessions/Contribution-Technique-1326
---

# Contribution Technique

Core idea for this whole session: instead of enumerating every combination/subarray directly and summing them, ask **"how much does each individual element (or pair) contribute to the final answer, and how many times?"** — then sum contributions. Several problems in this session also share a second, related lens: group subarrays **by the index they end at**, and build an `O(N)` DP where `dp[i] = combine(arr[i], dp[i-1])`, just swapping out what `combine` means (`×`, `+`, or `max`).

## 1. Sum of All Triplets

**Problem:** Given an array, sum the values of every triplet of elements (choose any 3 elements, sum them, add that sum across all `C(n,3)` triplets).

**Contribution idea:** instead of enumerating triplets, ask how many triplets each individual element `arr[i]` appears in. Fixing `arr[i]` as one member of the triplet, the other 2 members are chosen freely from the remaining `n-1` elements: `C(n-1, 2)` triplets. So `arr[i]` contributes `arr[i] × C(n-1, 2)` to the total.

```cpp
long long sumOfAllTriplets(vector<int>& arr) {
    int n = arr.size();
    long long totalSum = 0;
    for (int x : arr) totalSum += x;
    long long multiplier = (long long)(n - 1) * (n - 2) / 2;  // C(n-1, 2)
    return totalSum * multiplier;
}
```

**Verified:** `arr = [2, 3, 5, 1]`, `n = 4` → `C(3,2) = 3`, `totalSum = 11` → `11 × 3 = 33`.

Runs in `O(N)` — versus `O(N^3)` for brute-force enumeration of every triplet.

## 2. Sum of Inversions over All Subarrays

**Problem:** For every subarray of `arr`, count its inversions (pairs `i<j` within that subarray where `arr[i] > arr[j]`), and sum that inversion count across *all* subarrays.

**Contribution idea:** instead of re-counting inversions per subarray, ask: for a fixed inverted pair `(i, j)` with `i < j` and `arr[i] > arr[j]`, in how many subarrays does this pair *both* appear (with their relative order intact)? The subarray's start must be at or before `i` (`i+1` choices: indices `0..i`), and its end must be at or after `j` (`N-j` choices: indices `j..N-1`). So this pair contributes to `(i+1) × (N-j)` subarrays.

```cpp
long long sumOfInversionsOverAllSubarrays(vector<int>& arr) {
    int n = arr.size();
    long long total = 0;
    for (int i = 0; i < n; i++) {
        for (int j = i + 1; j < n; j++) {
            if (arr[i] > arr[j]) {
                total += (long long)(i + 1) * (n - j);
            }
        }
    }
    return total;
}
```

Runs in `O(N^2)` (one pass over all pairs) — versus enumerating every subarray *and* counting inversions within each, which would be much worse.

## 3. The "Ending at `i`" Family: Product / Sum / Max of All Subarrays

These three problems share the exact same skeleton: build `dp[i]` = "the answer restricted to subarrays ending exactly at index `i`", derived from `dp[i-1]` plus `arr[i]`, then combine all `dp[i]` into the final answer. The whiteboard derivation (using generic elements `a, b, c, d`) made this explicit by listing, column by column, every subarray ending at each letter:

- Ends at `a`: `a`
- Ends at `b`: `b`, `a·b` (or `a+b`)
- Ends at `c`: `c`, `b·c`, `a·b·c` (or `c`, `b+c`, `a+b+c`)
- Ends at `d`: `d`, `c·d`, `b·c·d`, `a·b·c·d` (or `d`, `c+d`, `b+c+d`, `a+b+c+d`)

### 3a. Sum of Products of All Subarrays

**Problem:** Sum the product of every contiguous subarray.

**Brute force — `O(N^2)`:**

```cpp
void solve(){
    int n; cin >> n;
    vector<int> arr(n);
    for (int i = 0; i < n; i++) cin >> arr[i];
    long long ans = 0;
    for (int i = 0; i < n; i++) {
        long long cur = 1;
        for (int j = i; j < n; j++) {
            cur *= arr[j];
            ans += cur;
        }
    }
    cout << ans << endl;
}
```

**Optimized — `O(N)`**, via `dp[i] = arr[i] × (1 + dp[i-1])`:

```cpp
long long sumOfProductsOfAllSubarrays(vector<int>& arr) {
    int n = arr.size();
    long long total = 0, prev = 0;
    for (int i = 0; i < n; i++) {
        prev = arr[i] * (1 + prev);   // dp[i] = arr[i]*(1+dp[i-1])
        total += prev;
    }
    return total;
}
```

**Verified:** `arr = [1, 3, 2]` → all 6 subarray products: `[1]→1, [3]→3, [2]→2, [1,3]→3, [3,2]→6, [1,3,2]→6` → total `21`. Both the brute force and the `O(N)` DP were run on the platform with input `1 3 2` and produced `21`.

### 3b. Sum of All Subarrays

**Problem:** Sum every element of every contiguous subarray (equivalently, sum the sums of all subarrays).

**Contribution formula:** `arr[i]` appears in every subarray whose start is at or before `i` (`i+1` choices) and whose end is at or after `i` (`n-i` choices):

**`answer = Σ arr[i] × (i+1) × (n-i)`**

Equivalently, as a running `dp`, matching the board's `dp[i] = dp[i-1] + arr[i]×(i+1)` framing:

```cpp
long long sumOfAllSubarrays(vector<int>& arr) {
    int n = arr.size();
    long long ans = 0, prev = 0;
    for (int i = 0; i < n; i++) {
        prev = prev + (long long)(i + 1) * arr[i];
        ans += prev;
    }
    return ans;
}
```

**Verified:** `arr = [1, 3, 2]` → all 6 subarray sums: `[1]→1, [3]→3, [2]→2, [1,3]→4, [3,2]→5, [1,3,2]→6` → total `21`. Contribution formula check: `1×(1)(3) + 3×(2)(2) + 2×(3)(1) = 3 + 12 + 6 = 21`. Both the manual enumeration on the whiteboard and the running-`prev` code (tested on the platform with input `1 3 2`) produced `21`.

**Live debugging note:** the instructor's first attempt at this code wrote `prev = prev + (i*1)*arr[i]` (i.e. multiplying by `i` instead of `i+1`), which produced the wrong output `10` — a good example of how easy it is to be off-by-one on the "how many subarrays include index `i`" count. Fixing `(i*1)` → `(i+1)` corrected it to `21`.

### 3c. Maximum Subarray Sum (Kadane's Algorithm)

**Problem:** Find the contiguous subarray with the largest sum (this is the classic [LeetCode 53 — Maximum Subarray](https://leetcode.com/problems/maximum-subarray/)).

Same "ending at `i`" idea, but the combine operation is `max` instead of `×`/`+`: `dp[i]` = best sum of a subarray ending exactly at `i`, either extending the previous best-ending-at-`(i-1)` or restarting fresh at `i`.

```cpp
int maxSubArray(vector<int>& nums) {
    int best = INT_MIN;   // see note below on INT_MIN vs 0
    int cur = 0;
    for (int i = 0; i < nums.size(); i++) {
        cur = max(cur + nums[i], nums[i]);   // dp[i] = max(dp[i-1]+arr[i], arr[i])
        best = max(best, cur);
    }
    return best;
}
```

**Verified on LeetCode:** accepted submission, 210/210 test cases passed, 0ms runtime (100th percentile).

**Gotcha — initializing `best`:** whether `best` should start at `0` or `INT_MIN` (`-1e9`) depends on whether an *empty* subarray is a valid answer:
- If the empty subarray is allowed (sum `0` is a valid floor), initialize `best = 0`.
- If only non-empty subarrays are allowed (as in the LeetCode version), initialize `best = INT_MIN`. Otherwise, an all-negative array (e.g. `[-5, -3, -1]`, true answer `-1`) would incorrectly return `0` from the "empty subarray."

**Divide & Conquer variant (mentioned, not fully captured):** the session also started sketching an `O(N log N)` divide-and-conquer approach — split the array at the midpoint, recursively solve each half, then combine by considering the best subarray *ending at* the midpoint from the left half and the best subarray *starting at* the midpoint from the right half (their sum gives the best "crossing" subarray). The exact combine-step derivation on the whiteboard wasn't fully legible/complete in the captured frames — worth re-checking the recording (~around the point right after the LeetCode submission) if the D&C version is needed in detail. The `O(N)` Kadane's solution above is the confirmed, fully-derived, and platform-verified takeaway from this session.

---

*Note compiled from whiteboard screenshots and IDE code taken at intervals while watching the recording. The Divide & Conquer approach for Maximum Subarray was introduced but not fully captured — may need a follow-up pass.*
