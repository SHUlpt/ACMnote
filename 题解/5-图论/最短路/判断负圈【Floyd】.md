## 图论-最短路

### Wormholes[^1]（判断负圈【Floyd】）

> - 题目：
>
>   有N个点，M条双向边，W条单向边且权值为负，问图中是否存在负环
>
> - 输入：
>
>   ```
>   1		//T组数据
>   3 3 1	//N,M,W
>   1 2 2	//从u到v权值为t
>   1 3 4
>   2 3 1
>   3 1 3
>   ```
>
> - 输出：
>
>   ```
>   NO
>   ```

结点`i`到自己的距离初始化为`INF`，在计算结束后，$G[i][i]=G[i][u]+\dots+G[v][i]$，即到外面绕一圈回来的最小路径。

- **如果`G[i][i]<0`，则存在负圈**
- 可以让`G[i][j]`的初值为0，从而加快判断过程

```c++
// #include <bits/stdc++.h>
#include <stdio.h>
#include <iostream>
#include <cmath>
#include <string.h>
using namespace std;
#define mem(a,b) memset(a,b,sizeof(a))
#define endl '\n'
typedef long long ll;
const int INF = 1 << 30;
const int maxn = 500 + 10;
int n, m, w, G[maxn][maxn];
bool floyd() //判断是否存在负环
{
	for (int k = 1; k <= n; k++) {
		for (int i = 1; i <= n; i++) {
			if (G[i][k] != INF) {
				for (int j = 1; j <= n; j++) {
					if (G[i][j] > G[i][k] + G[k][j]) {
						G[i][j] = G[i][k] + G[k][j];
					}
				}
			}
			if (G[i][i] < 0) {
				return true;
			}
		}
	}
	return false;
}
int main()
{
	ios::sync_with_stdio(0), cin.tie(0);
	int t;
	cin >> t;
	while (t--) {
		cin >> n >> m >> w;
		for (int i = 1; i <= n; i++) {
			for (int j = 1; j <= n; j++) {
				G[i][j] = INF;
			}
		}
		int s, e, t;
		while (m--) {
			cin >> s >> e >> t;
			if (t < G[s][e]) {
				G[s][e] = G[e][s] = t;
			}
		}
		while (w--) {
			cin >> s >> e >> t;
			G[s][e] = -t;
		}
		cout << (floyd() == true ? "YES" : "NO") << endl;
	}
}
```

[^1]:poj3259