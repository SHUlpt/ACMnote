## 基础算法-模拟

### Square Dance[^1] （跳舞链）

#### 题目

给定一个$R\times C$的矩阵，每个格子初始都有一个人，<u>能力值</u>为$v_{ij}$。现在进行若干轮比赛，每场比赛的<u>有趣程度</u>为当前比赛在场的所有人的能力值之和。

对于每轮比赛，如果一个人的能力值小于他上、下、左、右四个方向寻找遇到的第一个人能力值的平均值，则被淘汰。当无法淘汰任何一人时，比赛结束。问所有轮比赛的有趣值之和为多少？

#### 样例

```
#input
1		T<=100
3 3		R*C<=1e5
1 1 1
1 2 1
1 1 1

#output
Case #1: 16
```

在第一轮比赛结束后，矩阵的情况为$\left[ \begin{matrix}1 &. &1\\.&2&.\\1&.&1 \end{matrix} \right]$，本轮将不能淘汰任何人，比赛结束。

#### 题解

如果一个人被淘汰了，那么他四周的人的状态就会被改变。用一个队列$(dque)$维护本轮被淘汰的人，用另一个队列$(que)$维护本轮需要被更新的人。

每轮比赛先检查$que$中的人是否会被淘汰，如果淘汰则加入$dque$中。当$que$中所有的人都检查完后，对$deque$中的人进行操作，将他们的上下左右四个邻居加入$que$中（如果存在的话），再下次比赛时进行检查。

因为每次判断一个人是否被淘汰，需要快速找到4个方向第一个遇到的人，所以采用了**链式结构**（跳舞链）存储。如果一个节点被删除，只需要更新其邻居的指针即可，如`x->left->right=x->right`，更新该节点左边节点的右指针指向该节点的右指针指向内容

```c++
#include <bits/stdc++.h>
using namespace std;
#define mem(a,b) memset(a,b,sizeof(a))
#define endl '\n'
typedef long long ll;
const int maxn = 3e5 + 10;
struct Link {
	int l, r, u, d;
} link[maxn];
/*
dque:当前回合被淘汰的人
que:当前回合需要更新的人
inque:是否在当前回合被淘汰的队列中
isdel:某个坐标上的人是否被淘汰
*/
queue<pair<int, int>>dque, que;
bool inque[maxn], isdel[maxn];
int n, m, val[maxn];
int gid(int x, int y) 
{ 	//二维坐标转一维
	return x * (m + 2) + y;	//矩阵周围多围一圈，所以每行有m+2
}
void init()
{	//初始化矩阵
	while (!que.empty()) que.pop();
	while (!dque.empty()) dque.pop();
	for (int i = 0; i <= n + 1; i++) {
		for (int j = 0; j <= m + 1; j++) {
			int idx = gid(i, j);
			link[idx].l = max(0, j - 1);
			link[idx].r = min(m + 1, j + 1);
			link[idx].u = max(0, i - 1);
			link[idx].d = min(n + 1, i + 1);
			inque[idx] = false;
		}
	}

}
inline void in_que(int x, int y)
{	//将邻居节点加入判断队列中
	int idx = gid(x, y);
	if (inque[idx]) return;
	if (isdel[idx]) return;
	que.push(make_pair(x, y));
	inque[idx] = true;
}
void solve()
{
	ll ans = 0, sum = 0;
	for (int i = 1; i <= n; i++) {
		for (int j = 1; j <= m; j++) {
			int idx = gid(i, j);
			cin >> val[idx];
			sum += val[idx];
			inque[idx] = true;
			isdel[idx] = false;
			que.push(make_pair(i, j));
		}
	}
	for (int rt = 0;; rt++) {
		int eli = 0;	//记录本轮淘汰人数，为0时结束
		ans += sum;
		while (!que.empty()) { //本轮需要检查的人
			int x = que.front().first, y = que.front().second;
			int idx = gid(x, y);
			que.pop();
			inque[idx] = false;

			int cnt = 0, skill = 0;
			if (link[idx].u > 0) ++cnt, skill += val[gid(link[idx].u, y)];
			if (link[idx].d <= n) ++cnt, skill += val[gid(link[idx].d, y)];
			if (link[idx].l > 0) ++cnt, skill += val[gid(x, link[idx].l)];
			if (link[idx].r <= m) ++cnt, skill += val[gid(x, link[idx].r)];

			if (cnt * val[idx] < skill){
                dque.push(make_pair(x, y));
                isdel[idx] = true; //淘汰
            } 

		}
		while (!dque.empty()) { //本轮被淘汰的人
			int x = dque.front().first, y = dque.front().second;
			dque.pop();
			int idx = gid(x, y);
			++eli;
			sum -= val[idx];
            //将其邻居节点入队
			if (link[idx].u > 0) in_que(link[idx].u, y);
			if (link[idx].d <= n) in_que(link[idx].d, y);
			if (link[idx].l > 0) in_que(x, link[idx].l);
			if (link[idx].r <= m) in_que(x, link[idx].r);
			//更新邻居节点的指针
			link[gid(link[idx].u, y)].d = link[idx].d;
			link[gid(link[idx].d, y)].u = link[idx].u;
			link[gid(x, link[idx].l)].r = link[idx].r;
			link[gid(x, link[idx].r)].l = link[idx].l;
		}
		if (eli == 0) break;
	}
	cout << ans << endl;
}
int main()
{
	ios::sync_with_stdio(0), cin.tie(0);
	int T; cin >> T;
	for (int i = 1; i <= T; i++) {
		cout << "Case #" << i << ": ";
		cin >> n >> m;
		init();
		solve();
	}
	return 0;
}
```

