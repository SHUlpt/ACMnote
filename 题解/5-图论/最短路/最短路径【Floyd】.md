## 图论-最短路

### 最短路径[^1]（求最短路【Floyd】）

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

求两点`i,j`之间的最短距离，可以考虑经过图中某个`k`点的路径和不经过`k`点的路径，取二者中最短路径

- 可以一次性求得所有结点之间的最短距离

- **注意：**复杂度为$O(n^3)$，只能计算规模很小的图

```
#include <bits/stdc++.h>
using namespace std;
#define mem(a,b) memset(a,b,sizeof(a))
#define endl '\n'
typedef long long ll;
const int INF = 1 << 30;
const int maxn = 100 + 10;
int n, m, G[maxn][maxn];
void floyd()
{
	int s = 1; //定义起点
	for (int k = 1; k <= n; k++) { //考虑是否经过第k个点
		for (int i = 1; i <= n; i++) {
			//考虑从i到j是否要经过k点
			for (int j = 1; j <= n; j++) {
				if (G[i][k] == INF || G[k][j] == INF) continue;
				G[i][j] = min(G[i][j], G[i][k] + G[k][j]);
			}
		}
	}
	cout << G[s][n] << endl;
}
int main()
{
	ios::sync_with_stdio(0), cin.tie(0);
	while (cin >> n >> m) {
		if (n == 0 && m == 0) break;
		//初始化邻接表
		for (int i = 1; i <= n; i++) {
			for (int j = 1; j <= n; j++) {
				G[i][j] = INF;
			}
		}
		//存图
		while (m--) {
			int a, b, c;
			cin >> a >> b >> c;
			G[a][b] = G[b][a] = c;
		}
		floyd();
	}
	return 0;
}
```

[^1]:hdu2544

