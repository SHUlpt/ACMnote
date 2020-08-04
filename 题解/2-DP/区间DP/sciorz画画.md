## DP-区间DP

### sciorz画画（凸多边形最优三角型剖分）

> - 题目
>
>   多边形的每个顶点都有一个权值`a[i]`，要用`n-3`条不相交的线将这个`n`边形分割成`n-2`个三角形，每个三角形的价值等于三个顶点权值的乘积。
>   问怎么分隔才能使得`n-2`个三角形的价值和最大
>
> - 输入
>
>   ```
>   2		1<=t<=100
>   3		1<=n<=100
>   1 2 3 	1<=a[i]<=100
>   4
>   1 2 3 4
>   ```
>
> - 输出
>
>   ```
>   Case #1: 6
>   Case #2: 32
>   ```

用`dp[i][j]`表示从第`i`个到第`j`个点的最优剖分答案

1. 当`j=i`或`j=i+1`时，无法构成三角形，`dp[i][j]=0`
2. 当`j>=i+2`时，在`i`与`j`之间取一个分割点`k`。此时多边形被划分为3部分：`i`到`k`部分，`k`到`j`部分，三角形`i,j,k`。其中`i`到`k`部分和`k`到`j`部分都是子问题，可以利用之前求出的结果

$$
dp[i][j]=\max_{k=i+1}^{j-1}\lbrace dp[i][j],dp[i][k]+dp[k][j]+cost(i,j,k)\rbrace
$$

注意：因为区间DP需要用到子区间的结果，所以区间需要由小到大，因而区间起点`i`是从`n-1`到1

```c++
#include <bits/stdc++.h>
using namespace std;
#define mem(a,b) memset(a,b,sizeof(a))
#define endl '\n'
typedef long long ll;
const int maxn = 100 + 10;
int n, a[maxn];
int dp[maxn][maxn];
void solve()
{
	mem(dp, 0);
	for (int i = n-1; i >=1; i--) {	//由小到大
		for (int j = i + 2; j <= n; j++) {
			for (int k = i + 1; k <= j - 1; k++) {
				dp[i][j] = max(dp[i][j], dp[i][k] + dp[k][j] + a[i] * a[j] * a[k]);
			}
		}
	}
	cout << dp[1][n] << endl;
}
int main()
{
	ios::sync_with_stdio(0), cin.tie(0);
	int t;
	cin >> t;
	for (int i = 1; i <= t; i++) {
		cin >> n; 
		cout << "Case #" << i << ": ";
		for (int j = 1; j <= n; j++) {
			cin >> a[j];
		}
		solve();
	}
}
```

