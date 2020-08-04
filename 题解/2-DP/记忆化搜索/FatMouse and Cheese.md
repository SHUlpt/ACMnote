## DP-记忆化搜索

### [FatMouse and Cheese](http://acm.hdu.edu.cn/showproblem.php?pid=1078)

> - 题目
>
>   一个老鼠在一个$n*n$的矩阵中，矩阵的每一个方格中都有一块奶酪。老鼠每次可以垂直或着水平前进最多k步，并且要保证下一个格子的奶酪的重量要大于当前格子，问老鼠最多能吃到多少奶酪？
>
> - 输入
>
>   ```
>   3 1
>   1 2 5
>   10 11 6
>   12 12 7
>   ```
>
> - 输出
>
>   ```
>   37
>   ```

在DFS回溯的过程中，更新从当前格子出发能够得到的最大价值，以避免重复的计算。
因为回溯时，上一个格子的所有可能情况都已经考虑过了，也就是说上一个格子的状态已经是最优的了，所以直接用上一个格子的值来更新当前格子。在当前格子的所有方向都回溯完时，当前格子也就达到了最优值，继续更新之后的。

```c++
#include <bits/stdc++.h>
using namespace std;
const int maxn = 100 + 10;
int n, k, mat[maxn][maxn], vis[maxn][maxn];
int dp[maxn][maxn];
int d[][4] = { 1, -1, 0, 0, 0, 0, 1, -1 };
inline bool check(int x, int y)
{
	if (x < 1 || x > n || y < 1 || y > n || vis[x][y]) return false;
	return true;
}
int dfs(int x, int y)
{
	int maxx = 0, ans;
	if (!dp[x][y]) {
		for (int i = 0; i < 4; i++) {
			for (int j = 1; j <= k; j++) {	//考虑不同步长
				int nx = x + d[0][i] * j, ny = y + d[1][i] * j;
				if (check(nx, ny) && mat[nx][ny] > mat[x][y]) {
					ans = dfs(nx, ny);
					maxx = max(maxx, ans);	//更新最优解
				}
			}
		}
        //用最优解更新当前格子，表示从当前格子出发能够获得的最大价值
		dp[x][y]=maxx+mat[x][y]; 
    }
	return dp[x][y];
}
int main()
{
    ios::sync_with_stdio(0), cin.tie(0);
	while (cin >> n >> k)
	{
		if (n == -1 && k == -1) {
			break;
		}
		mem(dp, 0);
		for (int i = 1; i <= n; i++) {
			for (int j = 1; j <= n; j++) {
				cin >> mat[i][j];
			}
		}
		cout << dfs(1, 1) << endl;
	}
}
```

