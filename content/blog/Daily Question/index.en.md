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
    cout << x << ' ';
    for(int i = 2; i <= n; i++) x -= d[i], cout << x << ' ';
    cout << '\n';

}
signed main(){
    std::ios::sync_with_stdio(false), cin.tie(0), cout.tie(0); int t; cin >> t; while (t--) solve();
}

```
## Robin Hood Archery - Rating 1900
Today is Friday, August 28, 2026. Problem link: [Robin Hood Archery](https://codeforces.com/contest/2014/problem/H).

First, let's consider under what circumstances the Sheriff will not lose. Since Robin Hood always goes first, if there is a choice between a larger and a smaller number, he will definitely pick the larger one. Therefore, the only scenario where the Sheriff does not lose is a tie. Because he is always picking second, the only way they can get the same score—and thus ensure the Sheriff doesn't lose—is if the numbers can be perfectly paired up (i.e., they are identical). 

Next, looking at the problem requirements, the number of queries is quite large. This means we need to frequently update and query within intervals. Furthermore, the queries do not need to be processed online (meaning we can compute the answers out of order and then output them in the original order of the questions). Therefore, we can consider using [Mo's Algorithm](https://www.hackerearth.com/practice/notes/mos-algorithm/), which allows us to perform range modifications and queries with a time complexity of $O(n \sqrt{n})$.

We maintain a variable $res$. When the frequencies of all numbers within the interval are even, $res = 0$. The specific code is as follows:
```cpp
// Retired?
#include <bits/stdc++.h>
#define int long long
using namespace std;
const int N = 1e6 + 10, mod = 1e9 + 7;
int pos[N], a[N], cnt[N], mp[N];
struct node{
    int l, r, now;
    bool operator< (const node &w) const{
        return (pos[l] ^ pos[w.l]) ? pos[l] < pos[w.l] : ((pos[l] & 1) ? r < w.r : r > w.r);
    }
}p[N];
void solve(){

    int n, m; cin >> n >> m;

    int len = sqrt(n);
    for(int i = 1; i <= n; i++) cin >> a[i], pos[i] = i / len;

    for(int i = 1; i <= m; i++){
        int l, r; cin >> l >> r;
        p[i] = {l, r, i};
    }

    sort(p + 1, p + 1 + m);

    int res = 0;
    auto add = [&](int x) -> void{
        mp[a[x]]++;
        if(mp[a[x]] % 2 == 0) res--;
        else res++;
    }; 

    auto del = [&](int x) -> void{
        mp[a[x]]--;
        if(mp[a[x]] % 2 == 0) res--;
        else res++;
    };

    int l = 1, r = 0;
    vector<int> ans(m + 1);
    for(int i = 1; i <= m; i++){
        int id = p[i].now;
        while(l < p[i].l) del(l++);
        while(l > p[i].l) add(--l);
        while(r < p[i].r) add(++r);
        while(r > p[i].r) del(r--);
        ans[id] = res;
    }

    for(int i = 1; i <= m; i++) cout << (ans[i] ? "NO" : "YES") << '\n';
    for(int i = 1; i <= n; i++) mp[a[i]] = 0;

}
signed main(){
    std::ios::sync_with_stdio(false), cin.tie(0), cout.tie(0);int t;cin>>t;while(t--)solve();
}
```