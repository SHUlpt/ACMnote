## DP-基础DP

### [Subset of Five](https://ac.nowcoder.com/acm/contest/5944/B)

集合$A$中有$n$个元素$\{a_1,a_2,\cdots,a_n\}$，取出一个子集$B$，要求$B$中元素之和最大且$\% 5=0$



### 题解

显然，这是一个多段决策问题。

**定义$sum[i][5]$：前$i$个数在余数为$j$时的最大和**

假设第$i$个元素$a[i]\%5=tmp$，分别对$sum[i-1][j]$共5个状态进行转移，每个状态转移到的新状态就是$sum[i][(j+tmp)\%5]$

```c++
for (int j = 0; j < 5; j++) {	// 处理第i个数
    if (sum[i - 1][j]){	
        sum[i][(j + tmp) % 5] = max(sum[i - 1][j] + a[i], sum[i][(j + tmp) % 5]);
    }
}
for(int j = 0; j < 5; j++){	// 第i个数处理完后，全部更新
    sum[i][j] = max(sum[i - 1][j], sum[i][j]);
}
```

需要注意的一点是，转移之前要确保$sum[i-1][j]\not=0$，也就是前$i-1$个元素之和的余数为$j$的情况是存在的，这有这样才能进行转移

最终只需要输出$sum[n][0]$的值即可



### 代码

```c++
#define ONLINE_JUDGE
#include <bits/stdc++.h>
using namespace std;
#define mem(a,b) memset(a,b,sizeof(a))
#define endl '\n'
typedef long long ll;
const int maxn = 1e6 + 10;
ll sum[maxn][5];	// 前i个数在余数为j时的最大和
int main()
{
	ios::sync_with_stdio(0), cin.tie(0);
	int n; cin >> n;
	vector<int>a(n);
	for (int i = 0; i < n; i++) {
		cin >> a[i];
	}
	sum[0][a[0] % 5] = a[0];	// 初始化
	for (int i = 1; i < n; i++) {
		int tmp = a[i] % 5;
		if (sum[i - 1][tmp] == 0) sum[i][tmp] = a[i];	// 前i-1个数之和的余数为tmp的情况还不存在，进行初始化
		for (int j = 0; j < 5; j++) {
			if (sum[i - 1][j]){
                sum[i][(tmp + j) % 5] = max(sum[i - 1][j] + a[i], sum[i][(tmp + j) % 5]);
            }
		}
        for(int j = 0; j < 5; j++){
    		sum[i][j] = max(sum[i - 1][j], sum[i][j]);
		}
	}
	cout << sum[n - 1][0] << endl;
	return 0;
}
```



