---
title: "Codeforces 每日一题"
date: 2026-09-02
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

## Stepan and Permutation - Rating 1100
今天是 2026 年 8月 31 日，星期一，问题链接：[Stepan and Permutation](https://codeforces.com/contest/2244/problem/C)。

我们可以根据交换条件得出可以随意交换的位置为分成组的，组内可以随便交换，组外不可交换。考虑通过并查集来分组，对于 $a_i$ 来说，它与 $a_{i + j * x}$ 和 $a_{i + j * y}$ 一组, 其中 $j \in {0,1,2...}$，那么我们在组内就可以直接排序，然后记录一下每一组所占的位置，按顺序安置。这样只需要检查最终结果是否为增序的排列即可。

代码如下：
```cpp
// Retired?
#include <bits/stdc++.h>
#define int long long
using namespace std;
const int N = 1e6 + 10, mod = 1e9 + 7;
void solve(){

    int n, x, y; cin >> n >> x >> y;
    
    vector<int> a(n + 1), p(n + 1);
    for(int i = 1; i <= n; i++) cin >> a[i], p[i] = i;

    function<int(int)> find = [&](int x) -> int{
        if(x != p[x]) p[x] = find(p[x]);
        return p[x];
    };

    for(int i = 1; i <= n; i++){
        if(i + x <= n){
            if(find(a[i]) != find(a[i + x])){
                p[find(a[i + x])] = find(a[i]);
            }
        }
        if(i + y <= n){
            if(find(a[i]) != find(a[i + y])){
                p[find(a[i + y])] = find(a[i]);
            }
        }
    }

    vector<int> b(n + 1);
    vector<vector<int>> v(n + 1), pos(n + 1);
    for(int i = 1; i <= n; i++) v[find(a[i])].push_back(a[i]), pos[find(a[i])].push_back(i);
    for(int i = 1; i <= n; i++) sort(v[i].begin(), v[i].end());
    for(int i = 1; i <= n; i++){
        for(int j = 0; j < v[i].size(); j++){
            b[pos[i][j]] = v[i][j];
        }
    }

    for(int i = 1; i <= n; i++){
        if(b[i] != i){
            cout << "NO" << '\n';
            return;
        } 
    }

    cout << "YES" << '\n';

}
signed main(){
    std::ios::sync_with_stdio(false), cin.tie(0), cout.tie(0);int t;cin>>t;while(t--)solve();
}
```

## Risky Tower - Rating 1400
今天是 2026 年 9 月 1 日，星期二，问题链接：[Risky Tower](https://codeforces.com/problemset/problem/2252/C)。

首先考虑贪心策略：要摧毁第 $i$ 行，我们需要选取若干个木块使得总攻击力 $\geq v_i$，且每行最多选 $m$ 个木块。为了使操作次数最少，显然应优先选最大的木块。

考虑塔从上到下依次摧毁，摧毁第 $i$ 行时，第 $i$ 行及其下方（第 $i, i+1, \ldots, n$ 行）的所有木块都可以使用。因此，我们从第 $1$ 行开始枚举，每处理完一行就将该行的木块从可用集合中移除。

我们用一个计数器 $mp$ 统计所有木块的出现次数，并将所有不同的木块值存入数组 $que$ 后按降序排列。对于当前枚举的第 $i$ 行，我们从 $que$ 头部依次取最大的木块，将其值累加至 $sum$，直到 $sum \geq v_i$ 或已取满 $m$ 个。若当前最大值 $top$ 的剩余次数 $mp[top]$ 为 $0$，则跳过。处理完第 $i$ 行后，将该行所有木块的计数各减一。答案为所有行所需最小操作次数的最小值，且不超过 $m$。

代码如下：
```cpp
// Retired?
#include <bits/stdc++.h>
#define int long long
using namespace std;
const int N = 1e6 + 10, mod = 1e9 + 7;
void solve(){

    int n, m; cin >> n >> m;

    vector<int> v(n + 1);
    for(int i = 1; i <= n; i++) cin >> v[i];

    map<int, int> mp;
    vector<int> que;
    vector<vector<int>> a(n + 1, vector<int>(m + 1));
    for(int i = 1; i <= n; i++){
        for(int j = 1; j <= m; j++){
            cin >> a[i][j];
            if(!mp.count(a[i][j])) que.push_back(a[i][j]);
            mp[a[i][j]]++;
        }
    }

    sort(que.begin(), que.end());
    reverse(que.begin(), que.end());

    int res = LLONG_MAX;
    for(int i = 1; i <= n; i++){
        int sum = 0, cnt = 0, pos = 0;
        while(sum < v[i] && cnt < m){
            int top = que[pos];
            while(mp[top] == 0) top = que[++pos];
            if(sum + top * mp[top] >= v[i]){
                int need = (v[i] - sum + top - 1) / top;
                sum += top * need, cnt += need;
            } else sum += top * mp[top], cnt += mp[top];
            pos++;
        }
        res = min(res, cnt);
        for(int j = 1; j <= m; j++) mp[a[i][j]]--;
    }

    cout << min(res, m) << '\n';

}
signed main(){
    std::ios::sync_with_stdio(false), cin.tie(0), cout.tie(0);int t;cin>>t;while(t--)solve();
}
```


今天是2026年9月2日星期三。题目链接：[Prefix Equality](https://atcoder.jp/contests/abc250/tasks/abc250_e)。

这是一道有趣的题目。由于每次查询都涉及数组$a$中索引$1$到$i$以及$1$到$j$的元素，因此这些操作具有某种前缀般的性质。

我们先以数组 $a$ 为例。

 - 对于数组 $a$ 中的特定位置 $i$，我们将数组 $b$ 中与集合 $\{a_1, \dots, a_i\}$ 匹配的最近和最远位置分别记为 $La$ 和 $Ra$。然后，我们维护两个集合 `set_a` 和 `set_b`，用于记录 $a$ 和 $b$ 前缀中的不同元素。

 - 如果 $a_i$ 不会对 `set_a` 产生影响，则初始化 $La_i = La_{i - 1}$ 和 $Ra_i = Ra_{i - 1}$。只要 $b_j$ 属于 `set_a`，我们就可以将 $j$ 递增，并将 $Ra_i$ 更新为 $j$。此外，当首次遇到 $b_j$ 时，应将 $j$ 记录为 $La_i$，因为 $<j$ 的集合无法包含集合 $\{a_1, \dots, a_i\}$ 中所需的所有元素。

因此我们可以计算 $La$、$Ra$、$Lb$ 和 $Rb$。如果 $x$ 和 $y$ 是一对正确的组合，则应满足 `la[x] <= y && y <= ra[x]` and `lb[y] <= x && x <= rb[y]`。时间复杂度为 $O(N \log N + Q)$。

代码如下：
```cpp
// Retired?
#include <bits/stdc++.h>
#define int long long
using namespace std;
const int N = 1e6 + 10, mod = 1e9 + 7;
signed main(){
    std::ios::sync_with_stdio(false), cin.tie(0), cout.tie(0);

    int n; cin >> n;

    vector<int> a(n + 1), b(n + 1);
    for(int i = 1; i <= n; i++) cin >> a[i];
    for(int i = 1; i <= n; i++) cin >> b[i];

    auto work = [&](const vector<int>& A, const vector<int>& B) {
        vector<int> l(n + 1), r(n + 1);
        set<int> seA, seB;
        for(int i = 1, j = 1; i <= n; i++){
            if(seA.count(A[i])) l[i] = l[i - 1], r[i] = r[i - 1];
            else seA.insert(A[i]);
            while(j <= n && seA.count(B[j])){
                r[i] = j;
                if(!seB.count(B[j])) l[i] = j;
                seB.insert(B[j++]);
            }
        }
        return make_pair(l, r);
    };

    auto [la, ra] = work(a, b);
    auto [lb, rb] = work(b, a);

    int Q; cin >> Q;
    for(int i = 1; i <= Q; i++){
        int x, y; cin >> x >> y;
        if((la[x] <= y && y <= ra[x]) && (lb[y] <= x && x <= rb[y])){
            cout << "Yes" << '\n';
        } else cout << "No" << '\n';
    }

    return 0;
}
```