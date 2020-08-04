## 数学-线性基

[hdu 3949 : XOR](http://acm.hdu.edu.cn/showproblem.php?pid=3949)【第K大】

### 题目

给出$n$个数，多次询问这$n$个数异或和的第$k$大

### 题解



### 代码

```c++
#include <bits/stdc++.h>
using namespace std;
#define mem(a,b) memset(a,b,sizeof(a))
#define endl '\n'
typedef long long ll;
const int maxn = 1e4 + 10;
const int Bit = 63;
int n, q;
ll a[maxn], p[Bit + 10];
void init()
{
	fill_n(p, Bit + 10, 0);
}
void Insert(ll x)
{
	for (int i = Bit; i >= 0; i--) {
		if ((x >> i) & 1) {
			if (p[i]) x ^= p[i];
			else { //更新第i位，并化简为对角矩阵
				p[i] = x;
				for (int j = i - 1; j >= 0; j--) {
					if (p[j] && ((p[i] >> j) & 1)) p[i] ^= p[j];
				}
				for (int j = i + 1; j <= Bit; j++) {
					if ((p[j] >> i) & 1) p[j] ^= p[i];
				}
				break;
			}
		}
	}
}

int main()
{
	ios::sync_with_stdio(0), cin.tie(0);
	int t; cin >> t;
	int k = 1;
	while (t--) {
		fill_n(p, Bit + 10, 0);
		cin >> n;
		for (int i = 1; i <= n; i++) {
			cin >> a[i];
			Insert(a[i]);
		}
		cout << "Case #" << k++ << ":" << endl;
		int cnt = 0;
		for (int i = 0; i <= Bit; i++) if (p[i]) cnt++;	//最多组成1<<cnt个不同的数
		cin >> q;
		for (int i = 1; i <= q; i++) {
			ll x;
			cin >> x;
			if (cnt == n) x++;	//0不可以到达，就从1开始
			if (x > (1ll << cnt)) {	//超过最大范围
				cout << -1 << endl;
				continue;
			}
			ll ans = 0;
			int temp = cnt;
			for (int i = Bit; i >= 0; i--) {
				if (p[i]) {
					ll now = 1ll << (temp - 1);
					if (x > now) {
						x -= now;
						ans ^= p[i];
					}
					temp--;
				}
			}
			cout << ans << endl;
		}
	}
}

```

