---
title: "Codeforces 每日一题"
date: 2026-08-28
description: "从周一开始，每天按难度递增解决 Codeforces 上的一道题"
---

## Array Replacement - Rating 1700
今天是 2026 年 8月 27 日，星期四，问题链接：[Array Replacement](https://codeforces.com/contest/2252/problem/D)。

此题考察方式比较隐晦，我们只能从操作方法入手，这其实是一个典型的交换差分，考虑一个三元组($a_{i-1}$, $a_i$, $a_{i+1}$)，我们对它进行一次操作后，得到($a_i$, $a_{i - 1} - a_i + a_{i + 1}$, $a_{i + 1}$)。

我们思考什么情况下将 $a_i$ 替换为 $a_{i - 1}-a_i+a_{i + 1}$ 是优的。显然是在 $a_i < a_{i - 1}-a_i+a_{i + 1}$ 时进行替换。我们可以把这个式子移向变为 $a_i-a_{i + 1} < a_{i - 1}-a_i$，注意到与差分数组十分相似，于是考虑定义数组 $D$，满足 $D_i=a_{i-1}-a_{i}$。
我们观察一下，如果对 $a_i$ 进行操作时对 $D$ 数组的影响:
- 对于 $D_{i}$ 来说，它会变成 $a_{i - 1} - (a_{i - 1} - a_i + a_{i + 1}) = a_{i} - a_{i + 1} = D_{i + 1}$。
- 对于 $D_{i + 1}$ 来说，它会变成 $(a_{i - 1} - a_i + a_{i + 1}) - a_{i + 1} = a_{i - 1} - a_i = D_{i}$。

即一次操作，我们会交换 $D_i$ 和 $D_{i + 1}$，且只有两者奇偶性相同时才可交换。于是我们维护每段奇偶性相同的 $D$，在这段数组内按最大值排序，从头部每次减去最大的差分值来保证字典序最小即可。
代码如下：

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
    while(l <= n){
        int r = l;
        while(r <= n && abs(d[l] % 2) == abs(d[r] % 2)) r++;
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
    std::ios::sync_with_stdio(false), cin.tie(0), cout.tie(0);int t;cin>>t;while(t--)solve();
}

```

## Robin Hood Archery - Rating 1900
今天是 2026 年 8月 28 日，星期五，问题链接：[Robin Hood Archery](https://codeforces.com/contest/2014/problem/H)。

首先考虑什么情况下 Sheriff 才不会输，由于 Robin Hood 总是先手出，那么在一大一小的情况下，他肯定选择拿大的，那么 Sheriff 不输的局面只有平局，因为他总是落后的，所以当两个数相等的局面，两者得到的分数才会相同，这样就不会输。

再看题意，题目的查询次数较大，因此需要频繁的在区间内进行统计和查询，且不具备实时性（即可以先乱序得出答案，再按问题顺序回答），因此我们考虑[莫队](https://www.hackerearth.com/practice/notes/mos-algorithm/)，它可以使得我们在 $O(n \sqrt{n})$ 复杂度内进行区间内的修改查询。

我们维护一个变量 $res$，当区间内的数频均为偶数次时 $res=0$，具体代码如下：
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