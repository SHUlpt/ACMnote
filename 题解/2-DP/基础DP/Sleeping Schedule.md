## DP-基础DP

### :star:[Sleeping Schedule](https://codeforces.com/contest/1324/problem/E)

> - 题目：
>
>   给定一个有n个数的数组`a[]`，每次可以选择加上`a[i]`或者`a[i]-1`，将每次所得到的和模h后，如果落在区间l到r之间，则贡献值+1
>   问每次应该如何选择，才能让总贡献值最大？
>
> - 输入：
>
>   ```
>   7 24 21 23				//n,h,l,r
>   16 17 14 20 20 11 22	//a[]
>   ```
>
> - 输出：
>
>   ```
>   3
>   ```

用`dp[i][j]`表示在选完第`i`个数后，并且选择了`j`次`a[i]-1`情况下的最大贡献值，那么答案就是$\max_{j=0}^{n}dp[n][j]$

用$s=\sum_{k=0}^{i}a[k]$表示当前已选择的`a`数组的元素之和，那么对于下一次，即`i+1`次：
如果选择`a[i]`，则$dp[i+1][j]=max(dp[i+1][j],dp[i][j]+|(s-j)\%h\in[l,r]|)$
如果选择`a[i]-1`，则$dp[i+1][j+1]=max(dp[i+1][j+1],dp[i][j]+|(s-j-1)\%h\in[l,r]|)$

```c++
#include <bits/stdc++.h>
using namespace std;
bool in(int x, int l, int r) {
	return l <= x && x <= r;
}
int main() {
	int n, h, l, r;
	cin >> n >> h >> l >> r;
	vector<int> a(n);
	for (auto &it : a) cin >> it;
	vector<vector<int>> dp(n + 1, vector<int>(n + 1, INT_MIN));
	dp[0][0] = 0;
	int sum = 0;
	for (int i = 0; i < n; i++) {
		sum += a[i];
		for (int j = 0; j <= i; j++) {
			dp[i + 1][j] = max(dp[i + 1][j], dp[i][j] + in((sum - j) % h, l, r));
			dp[i + 1][j + 1] = max(dp[i + 1][j + 1], dp[i][j] + in((sum - j - 1) % h, l, r));
		}
	}
	cout << *max_element(dp[n].begin(), dp[n].end()) << endl;
	return 0;
}
```

反思：本题看似有$2^n$种情况，实际上在选择了前`n`个数的情况下，选择`a[i]-1`的次数也被限制在了`0~n`之间。
无需在意之前要怎么选，只用考虑从第`i`次向`i+1`次转移时候能够取得的最大贡献值。