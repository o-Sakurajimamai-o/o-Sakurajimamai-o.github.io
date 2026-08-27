---
title: "Codeforces 每日一题"
date: 2026-08-27
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