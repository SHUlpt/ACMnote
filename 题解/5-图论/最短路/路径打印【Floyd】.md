## 图论-最短路

### Minimum Transport Cost[^1]（路径打印【Floyd】）

> - 题目：
>
>   给定一个n*n矩阵，表示两点间距离。有若干组询问，打印两点间最短路，如果存在多解，打印字典序最小的。
>
> - 输入：
>
>   ```
>   5				//矩阵大小
>   0 3 22 -1 4		//矩阵
>   3 0 5 -1 -1
>   22 5 0 9 20
>   -1 -1 9 0 4
>   4 -1 20 4 0
>   5 17 8 3 1
>   1 3				//询问
>   3 5
>   2 4
>   -1 -1
>   0
>   ```
>
> - 输出：
>
>   ```
>   From 1 to 3 :
>   Path: 1-->5-->4-->3
>   Total cost : 21
>   
>   From 3 to 5 :
>   Path: 3-->4-->5
>   Total cost : 16
>   
>   From 2 to 4 :
>   Path: 2-->1-->5-->4
>   Total cost : 17
>   ```

- 用`path[i][j]`表示从`i`到`j`的最短路中，从`i`出发，下一个要经过的点。

初始化：`path[i][j]=j`，假设都是直接到达，后续再更新

```c++
#include <bits/stdc++.h>
using namespace std;
#define mem(a,b) memset(a,b,sizeof(a))
#define endl '\n'
typedef long long ll;
const int INF = 1 << 30;
const int maxn = 500 + 10;
int n, G[maxn][maxn], b[maxn], path[maxn][maxn];
void Floyd()
{
	for (int k = 1; k <= n; k++) {
		for (int i = 1; i <= n; i++) {
			for (int j = 1; j <= n; j++) {
				if (G[i][k] == INF || G[k][j] == INF) continue;
				if (G[i][j] > G[i][k] + G[k][j] + b[k]) {
					G[i][j] = G[i][k] + G[k][j] + b[k];
					path[i][j] = path[i][k];
				} 
                //更新字典序最小的路径
                else if (G[i][j] == G[i][k] + G[k][j] + b[k] 
                           && path[i][j] > path[i][k]) {
					path[i][j] = path[i][k];
				}
			}
		}
	}
}
void Path(int x, int y)
{
	cout << "From " << x << " to " << y << " :" << endl;
	cout << "Path: " << x;
	int tempx = x;
	while (tempx != y) {
		tempx = path[tempx][y];
		cout << "-->" << tempx;
	}
	cout << endl;
	cout << "Total cost : " << G[x][y] << endl;
}

int main()
{
	ios::sync_with_stdio(0), cin.tie(0);
	while (cin >> n) {
		if (n == 0) break;
		for (int i = 1; i <= n; i++) {
			for (int j = 1; j <= n; j++) {
				cin >> G[i][j];
				if (G[i][j] == -1) G[i][j] = INF;
				path[i][j] = j;
			}
		}
		for (int i = 1; i <= n; i++) {
			cin >> b[i];
		}
		Floyd();
		int x, y;
		while (cin >> x >> y) {
			if (x == -1 && y == -1) {
				break;
			}
			Path(x, y);
			cout << endl;
		}
	}
}
```

[^1]:hdu1385