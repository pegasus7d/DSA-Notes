---
tags: [live-session, searching, sorting, frequency-mapping, counting-sort, anagram, palindrome]
date: 2026-06-07
status: Watched
duration: 01:04:08
source: https://maang.in/live-sessions/Searching-Sorting-and-Frequency-Mapping-1284
slides: https://docs.google.com/presentation/d/1gSkXcQ9aUeCyY4QKEFVWARygshNv7Hwhc65xQivIgkw
---

# Searching, Sorting and Frequency Mapping

Week 2, Session 2. Fundamental searching and sorting techniques along with frequency-mapping methods to efficiently process, organize, and analyze data in coding problems.

## 1. Min, First/Last Index, and Frequency of Min — Single Pass

**Problem:** Given an array, in **one loop only**, find:
- Min of the array
- Last index where the min is present
- First index where the min is present
- Count of the frequency of the min

**Example:** `N = 7`, `Arr = [3, 2, 5, 9, 2, 3, 2]` → min = `2`, last index = `6`, first index = `1`, frequency = `3`

**Approach:** Track `minSeen`, `minCnt`, `firstIdx`, `lastIdx` while scanning once. Whenever a new strictly-smaller element is found, reset the tracker (`minCnt = 1`, `firstIdx = lastIdx = i`); whenever an element equal to the current min is found, just bump `minCnt` and update `lastIdx`.

```cpp
int minSeen = INT_MAX, minCnt = 0;
int lastIdx = -1, firstIdx = -1;
int n; cin >> n;
int arr[n];
for (int i = 0; i < n; i++) {
    cin >> arr[i];
    if (arr[i] < minSeen) {
        minSeen = arr[i];
        minCnt = 1;
        lastIdx = firstIdx = i;
    } else if (arr[i] == minSeen) {
        minCnt++;
        lastIdx = i;
    }
}
```

## 2. Sorting Techniques — Reading

To-do: read through the classic sorting algorithms.
Reference: [Bubble Sort tutorial — HackerEarth](https://www.hackerearth.com/practice/algorithms/sorting/bubble-sort/tutorial/)

## 3. Stable vs Unstable Sorting

To-do: figure out which standard sorting algorithms are stable and which are unstable (equal elements preserve relative order or not).

## 4. `sort()` in C++

```cpp
int n = 5;
int arr[] = {3, 2, 1, 4, 5};
sort(arr, arr + n);
```

Open question raised in session: **which underlying sorting algorithm does `std::sort` actually use?** (worth looking up — typically introsort: quicksort + heapsort fallback + insertion sort for small ranges).

## 5. Practice Problems

- Solve: [Codeforces 632C](https://codeforces.com/problemset/problem/632/C)
- To-do: [LeetCode — Largest Number](https://leetcode.com/problems/largest-number/description/)

## 6. Counting Sort

**Problem:** Sort an array using counting sort.
**Constraints:** `N ≤ 10^6`, `1 ≤ arr[i] ≤ 5` (small value range — ideal for counting sort, O(N + K)).

## 7. Anagram Check

**Problem:** Given two strings, check whether they are anagrams.

- Example 1: `S = "abcdaba"`, `T = "cbdaaab"` → `True`
- Example 2: `S = "vivek"`, `T = "vevek"` → `False`

(Frequency-map both strings and compare — this is the "frequency mapping" technique the session title refers to.)

## 8. Can a String Be Made a Palindrome?

**Problem:** Given a string, determine if its characters can be rearranged into a palindrome.

- Example 1: `S = "abccaba"` → rearranged e.g. `"abcacba"` → `True`
- Example 2: `S = "vivek"` → `False`

(Key idea: at most one character can have an odd frequency count.)

## 9. Count Pairs with Equal Values

**Problem:** Find the number of pairs `(i, j)` such that `arr[i] == arr[j]` and `i < j`.
**Constraints:** `N ≤ 10^5`, `arr[i] ≤ 10^5`

(Frequency-map values, then sum `C(freq, 2)` per distinct value.)

## 10. Bonus Problem — Palindrome-Rearrangeable Substrings

**Problem:** Given a string `S` and `Q` queries of the form `(L, R)`, for each query determine whether the substring `S[L..R]` can be rearranged into a palindrome.

**Constraints (progressive difficulty):**
- Level 1: `N, Q ≤ 10^3`
- Level 2: `N, Q ≤ 10^5`
- Level 3: answer every query in `O(1)`

(Hinted approach for O(1): prefix-sum of per-character parity/frequency, likely via bitmask XOR prefix sums — ties into the "Learn prefix sum" to-do below.)

## Assignments / To-Do

1. Given assignments (see platform)
2. Learn different sorting algorithms
3. Figure out which sorting algorithms are stable vs unstable
4. Solve: [LeetCode — Largest Number](https://leetcode.com/problems/largest-number/description/)
5. Learn prefix sum technique to solve the bonus problem (palindrome-rearrangeable substrings)
