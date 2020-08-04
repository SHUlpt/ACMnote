##  [hdu4632](http://acm.hdu.edu.cn/showproblem.php?pid=4632)：计算回文子序列

> - 题目：给定一个字符串，求这个字符串的所有子序列中有多少个回文串
>
> - Input
>
>   ```
>   4
>   a
>   aaaaa
>   goodafternooneveryone
>   welcometoooxxourproblems
>   ```
>
> - Output
>
>   ```
>   Case 1: 1
>   Case 2: 31
>   Case 3: 421
>   Case 4: 960
>   ```

### 题解

用$dp[i][j]$表示从第$i$个到$j$个字符中有多少个回文子序列，注意要将$dp[i][i]$初始化为1，其余为0

状态转移：
$dp[i][j]$是根据$dp[i+1][j]$和$dp[i][j-1]$这两个状态推导来的，应该相加。
但是$dp[i+1][j]$和$dp[i][j-1]$都包含了$dp[i+1][j-1]$的状态，相当于多加了一个，应该减去。
对于当前状态，除了从之前转移来的，还有就是当$s[i]=s[j]$时，会和$dp[i+1][j-1]$状态的回文组成新的回文序列，所以$dp[i][j]=dp[i][j]+do[i+1][j-1]+1$

- **注意**：第一个状态转移方程涉及减法，直接取模可能会成为负数，所以应该先加`mod`，然后再取模

```c++
const int maxn = 1e3 + 10;
const int mod = 10007;
int len;
string s;
int dp[maxn][maxn]; //区间i到j的回文子序列数目
void init()
{
    mem(dp, 0);
    len = s.length();
    for (int i = 0; i < len; i++) {
        dp[i][i] = 1;
    }
}
void solve()
{
    for (int i = len - 1; i >= 0; i--) {
        for (int j = i + 1; j < len; j++) {
            dp[i][j] = (dp[i][j - 1] + dp[i + 1][j] - dp[i + 1][j - 1] + mod) % mod;
            if (s[i] == s[j]) {
                dp[i][j] = (dp[i][j] + dp[i + 1][j - 1] + 1) % mod;
            }
        }
    }

}
int main()
{
    int t;
    cin >> t;
    for(int i=1;i<=t;i++)
    {
        cout << "Case "<< i <<": ";
        cin >> s;
        init();
        solve();
        cout << dp[0][len - 1] << endl;
    }
}
```



## :question:[hdu2476](http://acm.hdu.edu.cn/showproblem.php?pid=2476)：将s1变为s2的最小花费

> - 题目：给两个字符串s1,s2，每次操作可以将s1的任意子串全部变为同一种字母，问最少几次操作能将s1变为s2
>
> - Input
>
>   ```
>   zzzzzfzzzzz
>   abcdefedcba
>   abababababab
>   cdcdcdcdcdcd
>   ```
>
> - Output
>
>   ```
>   6
>   7
>   ```

### 题解

[参考](https://www.cnblogs.com/zsben991126/p/10319332.html)

```c++
int solve(string a, string b)
{
	int dp[maxn][maxn], ans[maxn];
	int len = a.length();
	mem(dp, 0);
	for (int r = 0; r < len; r++) { //右边界
		for (int l = r; l >= 0; l--) { //左边界
			dp[l][r] = dp[l + 1][r] + 1;
			for (int k = l + 1; k <= r; k++) {
				if (b[l] == b[k]) {
					dp[l][r] = min(dp[l][r], dp[l + 1][k] + dp[k + 1][r]);
				}
			}
		}
	}
	for (int i = 0; i < len; i++) {
		ans[i] = dp[0][i];
	}
	for (int i = 0; i < len; i++) {
		if (a[i] == b[i]) ans[i] = ans[i - 1];
		else {
			for (int j = 0; j < i; j++) {
				ans[i] = min(ans[i], ans[j] + dp[j + 1][i]);
			}
		}
	}
	return ans[len-1];
}
```

