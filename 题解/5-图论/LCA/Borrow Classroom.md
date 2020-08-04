## 图论-LCA

### Borrow Classroom[^1] 

> 题目：
> 在一颗$n$个结点的树上，甲要从$B$结点出发，经过$C$结点后前往根节点（1号），而乙要从$A$结点出发，在甲到达根节点之前进行拦截。
> 甲乙速度相同，乙必须在甲到达根结点之前进行拦截
>
> 输入：
> 第一行一个正整数$T(\le 5)$，表示测试数据的组数
> 对于每组测试数据，第一行为两个整数$n,q(1\le n,q\le 1e5)$，分别表示结点数和询问数
> 接下来$n-1$行，每行两个整数$x,y(1\le x,y\le n)$，表示$x,y$之间有边相连
> 接下来$q$行，每行三个整数$A,B,C(1\le A,B,C\le n)$
>
> 输出：
> 对于每个询问，能成功拦截输出"YES"，否则"NO"
>
> 样例：
>
> ```
> #input
> 1
> 7 2
> 1 2
> 2 3
> 3 4
> 4 7
> 1 5
> 1 6
> 3 5 6
> 7 5 6
> 
> #output
> YES
> NO
> ```

需要求出$B\to C\to 1$的距离$dist1$和$A\to 1$的距离$dist2$

如果$dist1<dist2$，显然抓不到

如果$dist1>dist2$，乙可以去根节点等着甲

如果$dist1==dist2$，则需分类讨论：

1. $LCA(A,C)==1$，则他们最终会在根节点相遇，不满足要求
2. 否则乙可以在$LAC(A,C)$点进行拦截

```c++
#include <bits/stdc++.h>
using namespace std;
#define mem(a,b) memset(a,b,sizeof(a))
#define endl '\n'
typedef long long ll;
const int maxn = 1e5 + 10;
const int DEG = 20;
struct Edge {
	int to, next;
} edge[maxn << 1];
int head[maxn], cnt;
void addEdge(int u, int v)
{
	edge[cnt].to = v;
	edge[cnt].next = head[u]; //指向结点u上一次存的边的位置
	head[u] = cnt++;		//更新结点u最新边的存储位置
}
void init()
{
	cnt = 0;
	mem(head, -1);
}

int fa[maxn][DEG];
int deg[maxn];
void BFS(int root)
{
	queue<int>que;
	deg[root] = 0;
	fa[root][0] = root;
	que.push(root);
	while (!que.empty()) {
		int tmp = que.front();
		que.pop();
		for (int i = 1; i < DEG; i++) {
			fa[tmp][i] = fa[fa[tmp][i - 1]][i - 1];
		}
		for (int i = head[tmp]; i != -1; i = edge[i].next) {
			int v = edge[i].to;
			if (v == fa[tmp][0]) continue;
			deg[v] = deg[tmp] + 1;
			fa[v][0] = tmp;
			que.push(v);
		}
	}
}
int LCA(int u, int v)
{
	if (deg[u] > deg[v]) swap(u, v);
	int hu = deg[u], hv = deg[v];
	int tu = u, tv = v;
	for (int det = hv - hu, i = 0; det; det >>= 1, i++) {
		if (det & 1) tv = fa[tv][i];
	}
	if (tu == tv) return tu;
	for (int i = DEG - 1; i >= 0; i--) {
		if (fa[tu][i] == fa[tv][i]) continue;
		tu = fa[tu][i];
		tv = fa[tv][i];
	}
	return fa[tu][0];
}
int main()
{
	int t;
	cin >> t;
	while (t--) {
		init();
		int n, q;
		cin >> n >> q;
		for (int i = 1; i <= n - 1; i++) {
			int u, v;
			cin >> u >> v;
			addEdge(u, v);
			addEdge(v, u);
		}
		BFS(1);
		while (q--) {
			int a, b, c;
			cin >> a >> b >> c;
			if (b == c && b == 1) {
				cout << "NO" << endl;
				continue;
			}
			int dist1 = deg[b] + deg[c] - 2 * deg[LCA(b, c)] + deg[c];
			int dist2 = deg[a];
			if (dist1 < dist2) {
				cout << "NO" << endl;
			}
			else if (dist1 > dist2) {
				cout << "YES" << endl;
			}
			else {
				if (LCA(c, a) == 1) cout << "NO" << endl;
				else cout << "YES" << endl;
			}
		}
	}
	return 0;
}
```

[^1]: 牛客算法周周练1.C (https://ac.nowcoder.com/acm/contest/5086/C)

