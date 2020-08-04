## 图论-最短路

### Bellman-Ford 模板

> - 题目：
>
>   N个点，M条路，从A点到B点耗时为C，求从1到N的最短时间
>
> - 输入：
>
>   ```
>   2 1		//N,M
>   1 2 3	//A,B,c
>   3 3
>   1 2 5
>   2 3 5
>   3 1 2
>   0 0
>   ```
>
> - 输出：
>
>   ```
>   3
>   2
>   ```

初始时假设所有的点到起点s的距离都为INF。
每次操作检查所有的m条边。对于每一个点，只考虑其相邻节点。如果该结点的邻居到s的距离不是INF，那么该点就可以通过邻居到达s，以此更新自己到s的最短距离

- 复杂度：$O(nm)$
- 存图：由于Bellman-Ford每一轮操作都要检查所有存在的m条边，所以直接选择**用数组来存边**
- **打印路径：**在连通图中，从起点s到任意一个结点t都存在一条最短路径。反之，从任意一个结点t往前追溯，沿着最短路径都能到达起点s，所以只要在每个结点上记录它的**前驱结点**就行。
  定义`pre[]`记录前驱结点，`pre[x]=y`表示最短路径上结点x的前一个结点是y
- **判断负圈：**当没有负圈时，最多只需要进行n轮就能结束，而如果**超过n轮**，就表示最短路径还在变化，所以肯定有负圈。

```c++
#include <bits/stdc++.h>
using namespace std;
#define mem(a,b) memset(a,b,sizeof(a))
#define endl '\n'
typedef long long ll;
const int INF = 1 << 30;
const int maxn = 100 + 10;
struct edge { int u, v, w; } e[maxn * maxn];
int n, m, cnt;
int pre[maxn];//记录前驱结点。pre[x]=y表示在最短路径上，结点x的前一个结点是y
void print_path(int s, int t) //打印从s到t的最短路径
{
	if (s == t) {//打印起点
		cout << s << " ";
		return;
	}
	print_path(s, pre[t]); //先打印前一个点
	cout << t << " ";//后打印当前点，最后打印的是终点t
}
void bellman()
{
	int s = 1;	//设置起点
	int d[maxn];//d[i]记录结点i到起点s的最短距离
	for (int i = 1; i <= n; i++) {
		d[i] = INF;
	}
	d[s] = 0;
	for (int k = 1; k <= n; k++) { //n轮操作
		for (int i = 0; i < cnt; i++) {
			int x = e[i].u, y = e[i].v;
			//x通过y到达起点s是否距离更近
			if (d[x] > d[y] + e[i].w) {
				d[x] = d[y] + e[i].w;
				pre[x] = y;
			}
		}
	}
	cout << d[n] << endl;
 	print_path(1,n); //打印路径
 	cout << endl;
}
int main()
{
	ios::sync_with_stdio(0), cin.tie(0);
	while (cin >> n >> m) {
		if (n == 0 && m == 0) break;
		//存图
		cnt = 0;
		while (m--) {
			int a, b, c;
			cin >> a >> b >> c;
			e[cnt].u = a, e[cnt].v = b, e[cnt].w = c, cnt++;
			e[cnt].u = b, e[cnt].v = a, e[cnt].w = c, cnt++;
		}
		bellman();
	}
	return 0;
}
```

### 判断是否存在负圈

```c++
void bellman()
{
	int s = 1;
	int d[maxn];//d[i]记录结点i到起点s的最短距离
	for (int i = 1; i <= n; i++) {
		d[i] = INF;
	}
	d[s] = 0;
	int k = 0;
	bool update = true;
	while (update) {
		k++;
		update = false;
		if (k > n) {
			cout << "有负圈" << endl;
			return;
		}
		for (int i = 0; i < cnt; i++) {
			int x = e[i].u, y = e[i].v;
			if (d[x] > d[y] + e[i].w) {
				d[x] = d[y] + e[i].w;
				pre[x] = y;
				update = true;
			}
		}
	}
	cout << d[n] << endl;
}
```

[^1]:hdu2544

