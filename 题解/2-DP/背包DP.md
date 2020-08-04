## 背包问题
### [hdu2955](http://acm.hdu.edu.cn/showproblem.php?pid=2955)：01背包与概率结合
题目：有n个银行，给出每个银行的价值和抢这个银行被抓的概率。求在小于总被抓概率时，最多能获得的价值
> 因为钱数有限，可以将被抓概率转化为逃跑概率，$dp[i]$表示获得钱数为$i$时，逃跑的概率。
**注意：逃跑概率随着抢银行的数量增加而减少，并且是相乘的关系**
```c++
const int maxn = 100 + 10;
struct node {
    int m;
    double p;
}a[maxn];
double p, dp[10005]; //钱数为i时，不被抓的概率
int n;
int main()
{
    int T;
    scanf("%d", &T);
    while (T--)
    {
        int sum = 0; //总钱数
        scanf("%lf %d", &p, &n);
        for (int i = 1; i <= n; i++) {
            scanf("%d %lf", &a[i].m, &a[i].p);
            sum += a[i].m;
        }
        mem(dp, 0);
        dp[0] = 1;
        for (int i = 1; i <= n; i++) {
            for (int j = sum; j >= a[i].m; j--) {
                dp[j] = max(dp[j], dp[j - a[i].m] * (1 - a[i].p));
            }
        }
        for (int i = sum; i >= 0; i--) {
            if (dp[i] > (1 - p)) { 
                printf("%d\n", i);
                break;
            }
        }
    }
    return 0;
}
```


### :question: :star2: [hdu4283](http://acm.hdu.edu.cn/showproblem.php?pid=4283)：区间DP
题目：给定一个队列，每个人有一个愤怒之d，如果他是第k个上场，不开心指数就是$(k-1)\times d$，但是有一个小黑屋（堆），先放进去的人最后才能出场，求不开心指数之和的最小值
> 用$dp[i][j]$表示从第$i$个人到第$j$个人这段区间的最小花费（只考虑这$j-i+1$个人）
> 从第$i$个人到第$j$个人，枚举第$i$个人是第几个上场，$k\in [1,j-i+1]$，其实相当于是调整$i$这个人在队列中的顺序，所以$i$之后的$k-1$个人会率先上场

转移过程：
```c++
  for (int l = 1; l <= n - 1; l++) { //枚举当前的区间长度
        for (int i = 1; i <= n - l; i++) { //区间左端点
            int j = i + l; //区间右端点
            for (int k = i; k <= j; k++) { //第k个出场(k是区间[i,j]的相对位置)
                dp[i][j] = min(dp[i][j], d[i] * (k - i) + dp[i + 1][k] + dp[k + 1][j] + (k - i + 1) * (sum[j] - sum[k]));
            }
        }
    }
```
`d[i]*(k-i)`表示第$i$个人的不开心指数，`dp[i+1][k]`代表从第$i+1$到$k$的人都提前上去了，`dp[k+1][j]`代表第$k+1$到$j$的人在第$i$个人之后上去，这些人在之前已经计算过了，现在整体被移动到了k个人的后面，所以每个人都需要加上不开心指数
#### 代码
```c++
const int maxn = 100 + 10;
int n, d[maxn], sum[maxn];
int dp[maxn][maxn]; //从第i个人到第j个人这段区间的最小花费
void init(int t)
{

	cin >> n;
	for (int i = 1; i <= n; i++) {
		cin >> d[i];
		sum[i] = sum[i - 1] + d[i];
	}
	mem(dp, 0);
	for (int i = 1; i <= n; i++) {
		for (int j = i + 1; j <= n; j++) {
			dp[i][j] = INF;
		}
	}
}
void solve()
{
	for (int l = 1; l <= n - 1; l++) { //枚举当前的区间长度
		for (int i = 1; i <= n - l; i++) { //区间左端点
			int j = i + l;	//区间右端点
			for (int k = i; k <= j; k++) { //第k个出场
				dp[i][j] = min(dp[i][j], d[i] * (k - i) + (k - i + 1) * (sum[j] - sum[k]) + dp[i + 1][k] + dp[k + 1][j]);
			}
		}
	}
}
int main()
{
	int T;
	cin >> T;
	for (int i = 1; i <= T; i++) {
		init(i);
		solve();
		cout << "Case #" << i << ": " << dp[1][n] << endl;
	}
}
```
