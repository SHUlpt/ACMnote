## [HDU 6804 Contest of Rope Pulling](http://acm.hdu.edu.cn/showproblem.php?pid=6804)：01背包+概率优化         

### 题目

两个集合中各有$n,m$个人，其中每个人有一个力量值$w_i$和一个魅力值$v_i$。从两个集合中各取出若干个人，满足力量值之和相等($\sum_{i\in n} w_i=\sum_{j\in m}w_j$)，并且魅力值之和尽可能大($\sum (v_i+v_j)$)，输出最大的魅力值

> **数据范围：**
>
> $T$组测试，$1\le T\le 30$
>
> 每组测试中$1\le n,m\le 10^3,\ 1\le w_{i}\le 10^3,\ -10^9\le v_i\le 10^9$
>
> 所有测试数据中，$(n+m)\le 10^4$

### 题解

如果把其中一个集合中所有人的力量值取负数($-w_i$)，该问题可以转化为：有$n+m$个物品，每个物品用一对数$(w_i,v_i)(-10^3\le w_i\le 10^3,\ -10^9\le v_i\le 10^9)$描述。选择一个子集$S$，满足$\sum_{i\in S}w_i=0$，同时最大化$\sum_{i\in S}v_i$，是一个经典的**01背包问题**

令$dp[i]$表示：取完前$x$个物品后，当力量值为$i$时的最大魅力值。可以通过将DP数组的中间值($dp[mid]$)看作0点，最后的答案就是$dp[mid]$

虽然单个物品的$w_i$最大只有$10^3$，但在DP的过程中$|\sum w_i|$可能达到$10^6$，如果要把所有的可能取值都进行考虑，DP数组的大小会很大，复杂度是不允许的

为了减少$\sum w_i$的范围，可以考虑以一种随机的方式安排物品的加入顺序，如此一来最优解在每一阶段对应的状态都在0附近振荡（考虑如果每次都是一正一负的取，就可以尽可能保证已考虑的$\sum w_i$的值在0附近波动，从而减少DP数组的大小）。因此，只需要设一个足够大的上界，只考虑$\sum w_i$的取值范围落在$[-T,T]$内，进行DP即可

#### 数组随机化方法

```c++
#include <random>
mt19937 rnd(time(NULL));
shuffle(v.begin(), v.end(), rnd);
```

### 代码

```c++
#include <bits/stdc++.h>
#include <random>  // std::default_random_engine
using namespace std;
#define endl '\n'
typedef long long ll;
const int maxn = 1e6 + 10;
const ll INF = 1ll << 62;
mt19937 rnd(time(NULL));
struct node
{
    ll w, v;
};
int n, m;
vector<node> a;
ll dp[maxn];
int main()
{
    ios::sync_with_stdio(0), cin.tie(0);
    int T;
    cin >> T;
    while (T--) {
        a.clear();
        cin >> n >> m;
        for (int i = 1, w, v; i <= n + m; i++) {
            cin >> w >> v;
            if (i <= n)
                a.push_back({w, v});
            else
                a.push_back({-w, v});
        }
        shuffle(a.begin(), a.end(), rnd);  // 随机化数组
        fill_n(dp, maxn, -INF);  // 初始化DP
        ll ans = 0;
        int MID = 50000;
        dp[MID] = 0;  // 取中间点为0点
        for (int i = 0; i < n + m; i++) {
            if (a[i].w > 0) {  // w为正，从大到小更新01背包
                for (int j = 2 * MID; j >= a[i].w; j--) {
                    dp[j] = max(dp[j], dp[j - a[i].w] + a[i].v);
                }
            }
            else {  // w为负，同样是从“大”到“小”更新01背包，这里的大指的是离0点远
                for (int j = (a[i].w > 0 ? a[i].w : 0); j <= 2 * MID; j++) {
                    dp[j] = max(dp[j], dp[j - a[i].w] + a[i].v);
                }
            }
        }
        cout << dp[MID] << endl;
    }
    return 0;
}
```

