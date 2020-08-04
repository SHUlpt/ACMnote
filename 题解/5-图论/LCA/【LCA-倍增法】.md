## 图论-LCA

### LCA倍增法

用$fa[x][y]$表示结点$x$的$2^y$个祖先

首先让$u,v$中相对较深的结点爬到与另外一个结点相同的深度。假设$depth(u)>depth(v)$，那么$u$要上爬$h=depth(u)-depth(v)$个祖先到达与$v$相同的深度

考虑对$h$进行二进制拆分，假设$h=(13)_{10}=(1101)_2$，那么从低位开始，依次是$u=fa[u][0],u=fa[u,2],u=fa[u][3]$

当$u,v$达到同一高度后，分两种情况：

1. $u,v$相等，等价于$v$是$u$的祖先，那么$LAC(u,v)=v$
2. 因为不知道要上跳多少次才能让$u,v$相遇，所以选择从高位开始枚举，上跳的条件是$fa[u][0]\not=fa[v][0]$

```c++
const int maxn = 1e5 + 10;
const int DEG = 20;			//树的深度
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

int fa[maxn][DEG];	//结点i的第2^j个祖先
int deg[maxn];		//结点深度
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
	for (int det = hv - hu, i = 0; det; det >>= 1, i++) { //使u,v深度相同
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
```

