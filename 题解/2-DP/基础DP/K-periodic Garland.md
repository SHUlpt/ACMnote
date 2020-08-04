## DP-基础DP

### [K-periodic Garland](https://codeforces.com/contest/1353/problem/E)

给定一个由`0,1`组成的字符串，要求字符串中任意两个`1`之间相距为`k`，求最小操作次数

例如：`k=3`时，`010010,1001001,0010,0`都满足条件

- 字符串长度$n\le 10^6$；模$k\le n$

### 题解

因为最终得到的字符串中，`1`所在的位置模k都是相同的，因此首先根据模$k$对原字符串进行划分$(i,i+k,i+2k,\dots)$，得到一系列模$k$相等的子序列

对于每一个子序列，将不属于该子序列的`1`全部变为`0`，并且将该子序列转换为符合条件的序列（其形式类似于`001100`这样，一段连续的0、一段连续的1、再一段连续的0，如此可以保证任意两个`1`之间的距离一定为$k$），并更新答案。  

因此问题就转化为：对于一个字符串，选取一个区间，使得区间里的元素都为1，而区间外的元素都为0  
令$dp[i]$表示：假设第$i$位为1的条件下，字符串前$i$位满足条件的最小操作数，其转移方程为

```c++
for (int i = 0; i < n; i++) {
    int cur = s[i] - '0';		// 第i位的元素
    pref += cur;				// 记录字符串前i位中有多少个1
    dp[i] = (cur == 0 ? 1 : 0);	// 因为dp[i]是假设第i位是1，所以如果是0，则操作次数需要+1
    // 转移过程：要么根据前一个状态进行转移，要么选择将第i位之前的所有元素全部变为0
    if (i > 0) dp[i] += min(dp[i - 1], pref - cur);
    ans = min(ans, dp[i] + num - pref); //第i位之后的所有1也都要变为0
}
```



### 代码

```c++
#include <bits/stdc++.h>
using namespace std;
#define mem(a,b) memset(a,b,sizeof(a))
#define endl '\n'
typedef long long ll;
const int maxn = 1e6 + 10;
const int INF = 1 << 30;
int solve(const string &s)
// 选取一个连续的区间，进行操作让区间里的值都成为 1，区间外的值都为 0，并且保证操作次数最小
{
	int n = s.size();
	int num = count(s.begin(), s.end(), '1');
	int ans = num;
	vector<int> dp(n);	// 假设第i位为1的条件下，字符串前i位满足条件的最小操作数
	int pref = 0;
	for (int i = 0; i < n; i++) {
    int cur = s[i] - '0';		// 第i位的元素
    pref += cur;				// 记录字符串前i位中有多少个1
    dp[i] = (cur == 0 ? 1 : 0);	// 因为dp[i]是假设第i位是1，所以如果是0，则操作次数需要+1
    if (i > 0) dp[i] += min(dp[i - 1], pref - cur); // 转移过程
    ans = min(ans, dp[i] + num - pref); //第i位之后的所有1也都要变为0
}
	return ans;
}
int main()
{
	ios::sync_with_stdio(0), cin.tie(0);
	int t;
	cin >> t;
	while (t--) {
		int n, k;
		string s;
		cin >> n >> k >> s;
		vector<string> v(k);	//模k相同的子串
		for (int i = 0; i < n; i++) {
			v[i % k] += s[i];
		}
		int cnt = count(s.begin(), s.end(), '1');// 字符串中1的个数
		int ans = INF;
		for (auto &it : v) {
			// 除本字符串外其他1都要变为0，再加上当前字符串的操作
			int temp = solve(it) + (cnt - count(it.begin(), it.end(), '1'));
			ans = min(ans, temp);
		}
		cout << ans << endl;
	}
	return 0;
}

```

