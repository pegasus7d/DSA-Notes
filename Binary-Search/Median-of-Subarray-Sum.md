---
tags: [binary-search, binary-search-on-answer, sliding-window]
date: 2026-06-28
status: Solved
url: https://maang.in/problems/Median-of-Subarray-Sum-45
---

# Median of Subarray Sum

## Problem

Given an array of N integers A. The score of a subarray is the sum of all elements in it. Find the median of all N*(N+1)/2 subarray sums. If the count is even, the median is the smaller of the two middle values.

**Input Format**
- First line: T — number of test cases
- Each test case: N on first line, then N space-separated integers
- Sum of N across all test cases ≤ 10^6

**Output Format**
- For each test case print the median subarray sum.

**Constraints**
- 1 ≤ T ≤ 10000
- 1 ≤ N ≤ 10^5
- 0 ≤ A[i] ≤ 10^9

## Sample Test Cases

**Input:**
```
3
3
1 2 3
1
5
2
1 4
```

**Output:**
```
3
5
5
```

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

int numElements, totalSubarrays, medianPosition;
vector<int> elements;

// Variable Window: count subarrays with sum <= mid in O(N)
bool hasEnoughSubarrays(int mid) {
    int head = -1, tail = 0;
    int currentSum = 0;
    int count = 0;

    while (tail < numElements) {
        while (head + 1 < numElements && currentSum + elements[head + 1] <= mid) {
            head++;
            currentSum += elements[head];
        }
        count += (head - tail + 1);
        if (tail <= head) {
            currentSum -= elements[tail];
            tail++;
        } else {
            tail++;
            head = tail - 1;
        }
    }
    return (count >= medianPosition);
}

void solve() {
    cin >> numElements;
    elements.assign(numElements, 0);

    totalSubarrays = (long long)numElements * (numElements + 1) / 2;
    medianPosition = (totalSubarrays + 1) / 2;

    int lo = LLONG_MAX, hi = 0;
    for (int i = 0; i < numElements; i++) {
        cin >> elements[i];
        lo = min(lo, elements[i]);
        hi += elements[i];
    }

    int ans = -1;
    while (lo <= hi) {
        int mid = lo + (hi - lo) / 2;
        if (hasEnoughSubarrays(mid)) {
            ans = mid;
            hi = mid - 1;
        } else {
            lo = mid + 1;
        }
    }
    cout << ans << endl;
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    int t; cin >> t;
    while (t--) solve();
}
```
