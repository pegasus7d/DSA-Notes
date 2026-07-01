---
tags: [graphs, dijkstra, graph-formulation, shortest-path]
date: 2026-06-29
status: Solved
url: https://maang.in/problems/Budget-Travelling-615
---

# Budget Travelling

## Problem

N cities, M bidirectional roads. Car with tank capacity C (starts empty). Each city i has petrol price P[i]. Traveling 1 unit distance costs 1 liter. Find minimum cost to travel from city A to city B.

**Input Format**
- N (cities), M (roads)
- M lines: u v d (road between u and v of length d)
- N prices P[1..N]
- A B C (start, end, tank capacity)

**Output Format**
- Minimum cost to reach B from A.

**Constraints**
- 1 ≤ N ≤ 10000, 1 ≤ M ≤ 100000, 1 ≤ C ≤ 100, 1 ≤ d ≤ C

## Sample Test Cases

**Input 1:**
```
5
5
1 2 1
2 3 1
3 4 1
4 5 1
1 4 6
1 10 10 10 1
1 5 8
```
**Output 1:** `4`

**Input 2:**
```
6
6
1 2 1
2 3 1
3 4 1
4 5 1
1 6 1
6 5 5
10 10 10 10 1 1
1 5 8
```
**Output 2:** `15`

## Intuition

Dijkstra on state `(city, fuel)`. Two transitions:
- Buy 1 liter: `(city, fuel) → (city, fuel+1)` at cost `P[city]`
- Travel edge `(city→v, d)`: `(city, fuel) → (v, fuel-d)` at cost 0, if `fuel >= d`

State space: N × (C+1) ≈ 1M nodes. First time B is popped from priority queue = minimum cost answer.

## Code

```cpp
#include <bits/stdc++.h>
using namespace std;
#define int long long

void solve() {
    int n; cin >> n;
    int m; cin >> m;

    vector<vector<pair<int,int>>> adj(n+1);
    for(int i = 0; i < m; i++) {
        int u, v, d; cin >> u >> v >> d;
        adj[u].push_back({v, d});
        adj[v].push_back({u, d});
    }

    vector<int> P(n+1);
    for(int i = 1; i <= n; i++) cin >> P[i];

    int A, B, C; cin >> A >> B >> C;

    const int INF = 1e18;
    vector<vector<int>> dist(n+1, vector<int>(C+1, INF));

    // {cost, city, fuel}
    priority_queue<tuple<int,int,int>, vector<tuple<int,int,int>>, greater<>> pq;
    dist[A][0] = 0;
    pq.push({0, A, 0});

    while(!pq.empty()) {
        auto [cost, city, fuel] = pq.top(); pq.pop();

        if(cost > dist[city][fuel]) continue;
        if(city == B) {
            cout << cost << endl;
            return;
        }

        // Buy 1 liter at current city
        if(fuel < C) {
            int nc = cost + P[city];
            if(nc < dist[city][fuel+1]) {
                dist[city][fuel+1] = nc;
                pq.push({nc, city, fuel+1});
            }
        }

        // Travel edges
        for(auto [v, d] : adj[city]) {
            if(fuel >= d && cost < dist[v][fuel-d]) {
                dist[v][fuel-d] = cost;
                pq.push({cost, v, fuel-d});
            }
        }
    }
}

signed main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL); cout.tie(NULL);
    solve();
    return 0;
}
```
