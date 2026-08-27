---
title: "Daily Codeforces Problem"
date: 2026-08-27
description: "Starting Monday, solve one Codeforces problem each day, in order of increasing difficulty"
---

## Array Replacement - Rating 1700
Today is Thursday, August 27, 2026. Problem link: [Array Replacement](https://codeforces.com/contest/2252/problem/D).

The approach to this problem is somewhat subtle, so we can only approach it from the perspective of the operation itself. This is actually a classic swap-difference problem. Consider a triplet ($a_{i-1}$, $a_i$, $a_{i+1}$). After performing one operation on it, we obtain ($a_i$, $a_{i - 1} - a_i + a_{i + 1}$, $a_{i + 1}$).

Let’s consider under what conditions replacing $a_i$ with $a_{i - 1} - a_i + a_{i + 1}$ is optimal. Clearly, the replacement should be made when $a_i < a_{i - 1} - a_i + a_{i + 1}$. We can rearrange this expression to $a_i - a_{i + 1} < a_{i - 1} - a_i$. Noting that this is very similar to the difference array, we consider defining an array $D$ such that $D_i = a_{i - 1} - a_i$.
Let’s examine how operations on $a_i$ affect the array $D$:
- For $D_i$, it becomes $a_{i - 1} - (a_{i - 1} - a_i + a_{i + 1}) = a_{i} - a_{i + 1} = D_{i + 1}$.
- For $D_{i + 1}$, it becomes $(a_{i - 1} - a_i + a_{i + 1}) - a_{i + 1} = a_{i - 1} - a_i = D_{i}$.

That is, in a single operation, we swap $D_i$ and $D_{i + 1}$, but only if they have the same parity. Therefore, we maintain a segment of $D$ values with the same parity, sort them by maximum value within that segment, and subtract the largest difference value from the head each time to ensure the lexicographical order is minimized.
The code is as follows:

```cpp
// Retired?
#include <bits/stdc++.h>
#define int long long
using namespace std;
const int N = 1e6 + 10, mod = 1e9 + 7;
void solve(){

    int n; cin >> n;

    vector<int> a(n + 1), d(n + 1);
    for(int i = 1; i <= n; i++) cin >> a[i], d[i] = a[i - 1] - a[i];

    int l = 2;
    while (l <= n) {
        int r = l;
        while (r <= n && abs(d[l] % 2) == abs(d[r] % 2)) r++;
        sort(d.begin() + l, d.begin() + r);
        reverse(d.begin() + l, d.begin() + r);
        l = r;
    }

    int x = a[1];
    cout << x << ‘ ’;
    for(int i = 2; i <= n; i++) x -= d[i], cout << x << ‘ ’;
    cout << ‘\n’;

}
signed main(){
    std::ios::sync_with_stdio(false), cin.tie(0), cout.tie(0); int t; cin >> t; while (t--) solve();
}

```
