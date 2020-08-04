## 基础算法-构造

### Pascal Walk[^1] (二进制分解)

#### 题目大意

从杨辉三角形的$(1,1)$出发，走出一条连续且不重复的路径，使路径上数字之和等于$N$，输出方案

其中$N\le 1e9$，并且路径长度不能超过500

<img src="C:/Users/小涛/AppData/Roaming/Typora/typora-user-images/image-20200414222115838.png" alt="image-20200414222115838" style="zoom:80%;" />

#### 题解

- **杨辉三角形每行数字之和等于2的n次幂**

根据这一性质，可以将**策略**制定为：从第1行开始，对于每一行，要么不取，要么就取完。如果选择不取，$N-=1$；如果选择取完，$N-=2^i$

如果路径不要求连续，只要把$N$对应二进制为1的所有位对应的行取完即可。比如$N=5_{10}=101_{2}$，只需要选择第一行和第三行。

由于$2^{30}>10^9$，所以30行之内一定能够走完。先假设一定有30行不取，让$N-=30$。接下来对$N$进行**二进制分解**，二进制为1的那一位所对应的行就选择取完。因为一开始是假设有30行不走的，现在前30行里面取了$x$行，就需要在30行之后补齐$30-x$行不取的。

#### 代码

```c++
#include <bits/stdc++.h>
using namespace std;
#define mem(a,b) memset(a,b,sizeof(a))
#define endl '\n'
typedef long long ll;
const int maxn = 40;
vector<ll>v[maxn];
void init() //预处理杨辉三角形
{
	v[1].push_back(1);
	v[2].push_back(1);
	v[2].push_back(1);
	for (int i = 3; i <= 30; i++) {
		for (int j = 1; j <= i; j++) {
			if (j == 1 || j == i) v[i].push_back(1);
			else v[i].push_back(v[i - 1][j - 1] + v[i - 1][j]);
		}
	}
}
void solve(int& n)
{
	if (n <= 30) { //如果小于等于30，每行都选择不取即可
		for (int i = 1; i <= n; i++) {
			cout << i << " " << 1 << endl;
		}
		return;
	}
	n -= 30;
	int vis[maxn] = {0}, r = 1;
	while (n) {	//找到所有要取的行
		if (n & 1) vis[r] = 1;
		r++;	//下一行
		n >>= 1;
	}
	int flag = 1, num = 0;
	for (int i = 1; i <= 30; i++) {
		if (vis[i]) {
			num++;
			if (flag) { //从左到右
				for (int j = 1; j <= i; j++) {
					cout << i << " " << j << endl;
				}
			}
			else {	//从右到左
				for (int j = i; j >= 1; j--) {
					cout << i << " " << j << endl;
				}
			}
			flag ^= 1;
		}
		else {
			if (flag) cout << i << " " << 1 << endl;
			else cout << i << " " << i << endl;
		}
	}
	for (int i = 1; i <= num; i++) { //取了多少行，就要补足多少行
		if (flag) cout << 30 + i << " " << 1 << endl;
		else cout << 30 + i << " " << 30 + i << endl;
	}
}
int main()
{
	ios::sync_with_stdio(0), cin.tie(0);
	init();
	int t;
	cin >> t;
	for (int T = 1; T <= t; T++) {
		cout << "Case #" << T << ":" << endl;
		int n;
		cin >> n;
		solve(n);
	}
}
```

[^1]:Code Jam 2020 Round 1A B