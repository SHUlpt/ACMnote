## 数学-组合数学

### Count The Blocks[^1]（容斥定理应用）

> - 题目
>
>   写出所有0到$10^n-1$的整数，包含前导0。如$n=3$是，所有的整数是$000,001,\dots,998,999$。
>   一个整数中最长的相同部分被看作一块，对于从1到$n$的所有长度的块，输出这个块在所有整数中出现了多少次？
>   比如$00027734000$中，有3个长度为1的块，1个长度为2的块，2个长度为3的块
>
> - 输入
>
>   ```
>   2		// n (n<=2e5)
>   ```
>
> - 输出
>
>   ```
>   180 10	// ans[i] (%998244353)
>   ```

考虑在长度为$n$的整数里放入一个长度为$p$的块，那么这个块的插入位置就有$n-p+1$种。
先不考虑重复的情况，插入长度为$p$的块后，剩余位置的元素进行任意填值，有$(n-p+1)*10*10^{n-p}$种方案。

接着考虑去掉重复的部分，重复的情况一定是因为在$p$的左右进行任意填充时，导致块$p$的长度发生了变化，因此可以利用之前计算的结果。
假设左右两部分填充之后导致$p$的长度变为$p+1$，那么可以看作是把长度为$p$的块插入到长度为$p+1$的块中，插入位置有$(p+1)-p+1$种，所以重复的情况就是$2*ans[p+1]$，$p+2,p+3,\dots,n$都同理

因此可以得到递推公式：$ans[i]=(n-i+1)*10^{n-i+1}-\sum_{j=i+1}^n (j-i+1)*ans[j]$

公式前半部分可以通过预处理10的次方，后半部分采用前缀和的方法。
计算$ans[n-1]$时减去的部分是$2*ans[n]$；
计算$ans[n-2]$时减去的部分是$2*ans[n-1]+3*ans[n]$；
计算$ans[n-3]$时减去的部分是$2*ans[n-2]+3*ans[n-1]+4*ans[n]$
用$sum$记录$ans[n]+ans[n-1]+\dots$，用$cur$记录包含系数的$ans[n],ans[n-1,\dots]$
如：
计算$ans[n-1]$时，$sum=ans[n],\,cur=ans[n]$；
计算$ans[n-2]$时，$sum=ans[n]+ans[n-1],\,cur=2ans[n]+ans[n-1]$
重复的结果就是$sum+cur$的值

```c++
#include <bits/stdc++.h>
using namespace std;
#define mem(a,b) memset(a,b,sizeof(a))
#define endl '\n'
typedef long long ll;
const int maxn = 2e5 + 10;
const ll MOD = 998244353;
ll ans[maxn], fac[maxn];
int main()
{
	ios::sync_with_stdio(0), cin.tie(0);
	int n;
	cin >> n;
	fac[0] = 1;
	for (int i = 1; i <= n; i++) fac[i] = fac[i - 1] * 10 % MOD;
	ans[n] = 10;
	ll sum = 10, cur = 10;
	for (int i = n - 1; i >= 1; i--) {
		ans[i] = (n - i + 1) * fac[n - i + 1] % MOD;
		ans[i] = (ans[i] - cur + MOD - sum + MOD) % MOD;
		sum = (sum + ans[i]) % MOD;
		cur = (cur + sum) % MOD;
	}
	for (int i = 1; i <= n; i++) {
		cout << ans[i] << " ";
	}
	return 0;
}
```



[^1]:[Educational Round 84: E](https://codeforces.com/contest/1327/problem/D)