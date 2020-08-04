## 数学-数论

### [777](https://ac.nowcoder.com/acm/contest/5944/A)【1-n中数字d出现的次数】

给定一个数$n$，判断在$[0,n]$中数字7出现的次数，结果模上$1e9+7$

$n\le 10^{100000}$



### 题解

因为数字非常大，所以采取分析规律的方式按位处理

假设是一个5位数，**只考虑百位为7**的情况，可分3种情况讨论：

1. 百位数字$> 7\ ({\color{Red}318}56)$时，有以下情况满足（记$a=312,b=56$）

   ${\color{Red}7}00-{\color{Red}7}99\\1{\color{Red}7}00-1{\color{Red}7}99\\\cdots\\30{\color{Red}7}00-30{\color{Red}7}99\\31{\color{Red}7}00-31{\color{Red}7}99$

   因此，百位为8的5位数字，共有${\color{Red}(\frac{a+1}{10})*100}$个满足条件

2. 百位数字$=7({\color{Red}317}56)$时，有以下情况满足（记$a=317,b=56$）

   ${\color{Red}7}00-{\color{Red}7}99\\1{\color{Red}7}00-1{\color{Red}7}99\\\cdots\\30{\color{Red}7}00-30{\color{Red}7}99\\31{\color{Red}7}00-31{\color{Red}7}56$

   因此，百位为7的5位数字，共有${\color{Red}(\frac{a}{10})*100+(b+1)}$个

3. 百位数字$<7({\color{Red}310}56)$时，有以下情况满足（记$a=310,b=56$）

   ${\color{Red}7}00-{\color{Red}7}99\\1{\color{Red}7}00-1{\color{Red}7}99\\\cdots\\30{\color{Red}7}00-30{\color{Red}7}99$

   因此，百位为0的5位数字，共有${\color{Red}(\frac{a}{10})*100}$个满足条件

有了如上数字规律后，就可以从高到低对每一位进行统计即可



### 代码

```c++
#include <bits/stdc++.h>
using namespace std;
#define mem(a,b) memset(a,b,sizeof(a))
#define endl '\n'
typedef long long ll;
const int mod = 1e9 + 7;
const int maxn = 1e5 + 10;
int fac[maxn], num[maxn];

void init()
// 预处理10^n
{
	fac[0] = 1;
	for (int i = 1; i < maxn; i++) {
		fac[i] = 10ll * fac[i - 1] % mod;
	}
}

int solve(string &s)
{
	int n = s.size();
	int ans = 0;	// 最终结果
    int sum = 0;	// 字符串的高位部分（也就是题解中的a，因为循环从高位开始，所以一直更新a就可以）
	num[n] = 0;		// 字符串后i位对应的数值（低位部分，也就是题解中的b）
	for (int i = n - 1; i >= 0; i--) {	// 预处理b
		num[i] = (1ll * (s[i] - '0') * fac[n - i - 1] + num[i + 1]) % mod;
	}
	for (int i = 0; i < n; i++) {
		if (s[i] == '7') {
			ans = (ans + 1ll * sum * fac[n - i - 1]) % mod;
			ans = (ans + 1ll * (num[i + 1] + 1)) % mod;
		}
		else if (s[i] < '7') {
			ans = (ans + 1ll * sum * fac[n - i - 1]) % mod;
		}
		else {
			ans = (ans + 1ll * (sum + 1) * fac[n - i - 1]) % mod;
		}
		sum = (10ll * sum + s[i] - '0') % mod;
	}
	return ans;
}

int main()
{
	ios::sync_with_stdio(0), cin.tie(0);
	int q; cin >> q;
	init();
	while (q--) {
		string s; cin >> s;
		cout << solve(s) << endl;;
	}
	return 0;
}
```

