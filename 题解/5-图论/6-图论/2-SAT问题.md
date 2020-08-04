## 2-SAT

### 判断是否有合法解

根据限制条件建图，如果所有的SCC内部不存在违规的边，则存在合法解

#### Party

> - 题目
>
>   有n对夫妻被邀请，每对夫妻中只有1人可以列席。在这2n个人中，某些人之间有矛盾，不能同时参加聚会。问有没有可能让n个人同时列席

首先，把矛盾关系用图来表示。如果`A,B`有矛盾，则有`A->~B`，表示有A必有非B，以及`B->~A`

最终生成的图中形成了多个强连通分量SCC。一个SCC内部的点都是相互依赖的，如果其中有一人出席，那么这个SCC内部的所有人都要出席。所以只要所有的SCC内部都没有夫妻，就会有合法的出席组合。

- Tarjan缩图之后，只需要随便找到n个人就可，所以一定会存在合法解

```c++
//hdu3062
#include <bits/stdc++.h>
using namespace std;
#define rush() int MYTESTNUM;cin>>MYTESTNUM;while(MYTESTNUM--)
#define mem(a,b) memset(a,b,sizeof(a))
#define endl '\n'
#define pi (acos(-1.0))
#define eps (1e-8)
#define inf (1<<30)
typedef long long ll;
const int maxn = 2e3 + 10;
int n, m;
int low[maxn], num[maxn], dfn, cnt;
int sccno[maxn];
vector<int>G[maxn];
stack<int>S;
void init()
{
	for (int i = 0; i < maxn; i++) {
		G[i].clear();
		low[i] = 0;
		num[i] = 0;
		sccno[i] = 0;
	}
	while (!S.empty()) {
		S.pop();
	}
	dfn = 0;
	cnt = 0;
}
void tarjan(int u)
{
	num[u] = low[u] = ++dfn;
	S.push(u);
	for (int i = 0; i < G[u].size(); i++) {
		int v = G[u][i];
		if (!num[v]) {
			tarjan(v);
			low[u] = min(low[u], low[v]);
		}
		else if (!sccno[v]) {
			low[u] = min(low[u], num[v]);
		}
	}
	if (low[u] == num[u]) {
		cnt++;
		while (1) {
			int v = S.top();
			S.pop();
			sccno[v] = cnt;
			if (u == v) break;
		}
	}
}
int main()
{
	while (cin >> n >> m) {
		init();
		for (int i = 1; i <= m; i++) {
			int a1, a2, c1, c2;
			cin >> a1 >> a2 >> c1 >> c2;
			G[2 * a1 + c1].push_back(2 * a2 + 1 - c2);
			G[2 * a2 + c2].push_back(2 * a1 + 1 - c1);
		}
		for (int i = 0; i < 2 * n; i++) {
			if (!num[i]) {
				tarjan(i);
			}
		}
		bool flag = true;
		for (int i = 0; i < n; i++) {
			if (sccno[i << 1] == sccno[i << 1 | 1]) {
				flag = false;
			}
		}
		cout << (flag == true ? "YES" : "NO") << endl;
	}
}
```

