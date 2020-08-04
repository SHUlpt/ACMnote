## 数学-数论

### [Acesrc and Good Numbers](http://acm.hdu.edu.cn/showproblem.php?pid=6659)【1-n中数字d出现次数】

定义$f(d,n)$表示在1到n中，数字$d$出现的次数（比如11中1的出现次数为2）。如果$f(d,k)=k$，就称$k$是一个“d-good”数。

现在给定$d(1\le d \le 9),x(0\le x\le 10^{18})$，找到一个最大的且不超过$x$的“d-good”数



### 题解

本题两个难点分别在于如何计算$f(d,n)$以及当$f(d,x)\not=x$时$x$的缩小范围

#### 1-n中数字d的出现次数





#### x缩小范围

当$f(d,x)=x$时，直接输出答案即可，否则就需要不断减小$x$以找到一个最大的满足的。

假设$f(d,x)=y$，不难发现，$f(d,x)$与$f(d,x-1)$之间最多相差18个$d$（比如$10^{18}-1$与$10^{18}-2$之间相差了18个9）

根据这一点，每次$x$至多可以缩小$\frac{|x-y|}{18}$。这时因为当$f(d,x)=y$时，$x$每缩小1，与$y$之间的差距最多能减小18，所以$x=x-abs((f(d,x)-x))/18$



### 代码

```c++
#include <bits/stdc++.h>
using namespace std;
#define mem(a,b) memset(a,b,sizeof(a))
#define endl '\n'
typedef long long ll;

ll f(ll d, ll n) {
	ll cnt = 0, k;
	for (ll i = 1; k = n / i; i *= 10) {
		cnt += (k / 10) * i;
		int cur = k % 10;
		if (cur > d) {
			cnt += i;
		}
		else if (cur == d) {
			cnt += n - k * i + 1;
		}
	}
	return cnt;
}


int main()
{
	ios::sync_with_stdio(0), cin.tie(0);
	int q; cin >> q;
	while (q--) {
		ll d, n;
		cin >> d >> n;
		while (1) {
			ll ans = f(d, n);
			if (ans == n) {
				cout << ans << endl;
				break;
			}
			else {
				n -= max(1ll, abs(ans - n) / 18);
			}
		}
	}
    return 0;
}
```

