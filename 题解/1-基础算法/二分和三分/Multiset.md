## 基础算法-二分

### [Multiset](https://codeforces.com/contest/1354/problem/D)【二分答案】

给定一个具有$n$个元素的集合，有$q$次操作，每次操作给定一个整数$k$，规则如下：

1. 如果$k\ge 1$，则把$k$加入集合中；
2. 如果$k<0$，则从集合中按升序排序后，移除第$|k|$个元素

如果最终集合非空，输出任一集合中元素即可

- Memory limit: 28mb

### 题解

本题可以选择用线段树之类的数据结构来模拟题目中的操作，但需要进行一定的优化，因为本题空间限制非常严格。

实际上，考虑到最终只需要输出集合中的任意一个元素即可，不妨假设要输出最小的一个，那么可以用**二分答案**的方法来查找。

二分查找的思路为：  
定义函数`count(x)`表示：对于元素`x`，最终集合中小于等于`x`的元素个数。  
最终答案就应该是找到一个最小的`x`满足$count(x)>0$  

因为在本题中，集合中元素不唯一，并且不一定是连续的。  
假设最终集合中最小的元素是`y`，那么在二分过程中，任何大于等于`y`的数返回结果都是$\ge 1$，而比`y`小的数返回结果都是0，因此`y`就是那个满足`count(x)>0`的最小元素

对于最终集合为空的情况，可以用一个非常大的数去检验。因为集合中元素$a[i]\le 10^6$，如果最终集合中小于等于$10^9$的元素为0，就说明集合中不可能存在任何元素。（如果$count(1e9)==0$，就说明最终集合中的最小元素必定是大于$1e9$的，而这与集合中元素$\le 1e6$相矛盾）

```c++
if (count(int(1e9)) == 0) {
    cout << 0 << endl;
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
int n, q;
vector<int>a, k;
int count(int x)
// 计算最终集合里面小于等于x的元素个数
{
	int cnt = 0;
	for (int i = 0; i < n; i++) {	// 初始化
		if (a[i] <= x) cnt++;
	}
	for (int i = 0; i < q; i++) {
		if (k[i] > 0 && k[i] <= x) cnt++;	// 插入正数，并且比x小，必然会排在x前面
		if (k[i] < 0 && abs(k[i]) <= cnt) cnt--;	// 插入负数，如果其绝对值<=cnt，就会删除x左侧的元素
	}
	return cnt;
}
int main()
{
	ios::sync_with_stdio(0), cin.tie(0);
	cin >> n >> q;
	a.resize(n); k.resize(q);
	for (int i = 0; i < n; i++) {
		cin >> a[i];
	}
	for (int i = 0; i < q; i++) {
		cin >> k[i];
	}
	if (count(int(1e9)) == 0) {	// 判断最终集合是否为空
		cout << 0 << endl;
		return 0;
	}
	int left = 1, right = int(1e6) + 1;
	while (left < right) {
		int mid = left + (right - left) / 2;
		if (count(mid) <= 0) left = mid + 1;
		else right = mid;
	}
	cout << right << endl;
    return 0;
}
```

