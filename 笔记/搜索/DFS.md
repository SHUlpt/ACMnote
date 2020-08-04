## DFS

### 优先搜索分支数少的

#### 例题：[Lead of Wisdom](http://acm.hdu.edu.cn/showproblem.php?pid=6772)

##### 题目

总共有$k$类装备，每类装备中都有若干个装备，每个装备分别有4个属性$a_i,b_i,c_i,d_i$，每类装备中至多只能选择一个装备。设取出的装备为集合$S$，要求最终的$DMG$最大，求出最大值
$$
DMG=(100+\sum_{i\in S}a_i)(100+\sum_{i\in S}b_i)(100+\sum_{i\in S}c_i)(100+\sum_{i\in S}d_i)
$$

##### 题解

共有$n$种装备，分为$k$类。最坏情况下，当每类装备中的装备数相同时，方案数为$x^{\frac{n}{x}}$。当$n=50$时，$x$在3处取到极大值，因为不是很大，可以直接暴搜所有方案得到最优解。

在搜索的过程中，应该先对$k$类物品按照装备数目从小到大排序，以保证先搜索分支数少的，后搜索分支数多的

##### 代码

```c++
#include <bits/stdc++.h>
using namespace std;
#define endl '\n'
typedef long long ll;
const int maxk = 50 + 10;
struct node
{
    int t, a, b, c, d;
    node(int T, int A, int B, int C, int D)
        : t(T), a(A), b(B), c(C), d(D) {}
};
vector<node> v[maxk];
int n, k;
ll ans;
inline ll cal(int a, int b, int c, int d)
{
    return 1ll * (100 + a) * (100 + b) * (100 + c) * (100 + d);
}
void dfs(int p, int a, int b, int c, int d)
{
    if (p > k) {
        ll res = cal(a, b, c, d);
        if (res > ans) ans = res;
        return;
    }
    if (v[p].size() == 0) dfs(p + 1, a, b, c, d);
    for (int i = 0; i < v[p].size(); i++) {
        dfs(p + 1, a + v[p][i].a, b + v[p][i].b, c + v[p][i].c, d + v[p][i].d);
    }
}
int main()
{
    ios::sync_with_stdio(0), cin.tie(0);
    int t;
    cin >> t;
    while (t--) {
        cin >> n >> k;
        for (int i = 0; i < maxk; i++) v[i].clear();
        for (int i = 1; i <= n; i++) {
            int t, a, b, c, d;
            cin >> t >> a >> b >> c >> d;
            v[t].push_back(node(t, a, b, c, d));
        }
        sort(v + 1, v + 1 + k, [](vector<node> a, vector<node> b) { return a.size() < b.size(); });
        ans = 0;
        dfs(1, 0, 0, 0, 0);
        cout << ans << endl;
    }
    return 0;
}
```



### 双向DFS

##### 代码

```c++
#include<bits/stdc++.h>
using namespace std;
using ll = long long;
using pii = pair<int, int>;
using db = double;
using vi = vector<int>;
#define rep(i,a,n) for (int i=a;i<n;i++)
#define per(i,a,n) for (int i=n-1;i>=a;i--)
#define pb push_back
#define mp make_pair
#define all(x) (x).begin(),(x).end()
#define fi first
#define se second
#define SZ(x) ((int)(x).size())
#define debug(x) cout << #x <<"\t" << x <<endl
const int inf = 0x3f3f3f3f;
const db eps = 1e-8;
const int mod = 1e9+7;
ll qpow(ll a, ll b){
    ll ret = 1;
    while(b){
        if(b&1)ret = ret*a%mod;
        a = a*a%mod;
        b>>=1;
    }
    return ret;
}
const int maxn = 60;
struct node{
    int a, b, c, d;
};
ll ans = 0;
vector<node>v[maxn];
void dfs(int l, int r, int a, int b, int c, int d){
    if(l>r){
        ans = max(ans, 1ll*(100+a)*(100+b)*(100+c)*(100+d));
        return;
    }
    if(l==r){
        if(v[l].size()==0){
            dfs(l+1, r, a, b, c, d);
            return;
        }
        for(int i=0; i<v[l].size(); ++i){
            dfs(l+1, r, a+v[l][i].a, b+v[l][i].b, c+v[l][i].c, d+v[l][i].d);
        }
        return;
    }
    if(v[l].size()==0){
        dfs(l+1, r, a, b, c, d);
        return;
    }
    if(v[r].size()==0){
        dfs(l, r-1, a, b, c, d);
        return;
    }
    for(int i=0; i<v[l].size(); ++i){
        for(int j=0; j<v[r].size(); ++j){
            dfs(l+1, r-1, a+v[l][i].a+v[r][j].a, b+v[l][i].b+v[r][j].b, c+v[l][i].c+v[r][j].c, d+v[l][i].d+v[r][j].d);
        }
    }
}

int main()
{
    ios::sync_with_stdio(false);
    cin.tie(0);
    int t;
    cin >> t;
    while(t--){
        int n, k;
        cin >> n >> k;
        for(int i=0; i<=k; ++i)v[i].clear();
        for(int i=0; i<n; ++i){
            int t, a, b, c, d;
            cin >> t >> a >> b >> c >> d;
            v[t].pb({a, b, c, d});
        }
        ans = 0;
        dfs(1, k, 0, 0, 0, 0);
        cout << ans <<"\n";
    }
}
```

