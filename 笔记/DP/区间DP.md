## 区间DP

### 石子合并

```c++
const int maxn = 250;

int n, sum[maxn];
int solve()
{
    int dp[maxn][maxn]; //从第i堆到第j堆的最小花费
    mem(dp, 0);
    for (int len = 1; len <= n - 1; len++) { //i和j之间的距离(最大为n-1)
        for (int i = 1; i <= n - len; i++) { //从第i堆开始
            int j = i + len; //到第j堆结束
            dp[i][j] = INF;
            for (int k = i; k < j; k++) {
                dp[i][j] = min(dp[i][j], dp[i][k] + dp[k + 1][j] + sum[j] - sum[i - 1]);
            }
        }
    }
    return dp[1][n];
}
int main()
{
    while (cin >> n) {
        sum[0] = 0;
        for (int i = 1, temp; i <= n; i++) {
            cin >> temp;
            sum[i] = sum[i - 1] + temp; //sum[i,j]的值等于sum[j]-sum[i-1]
        }
        cout << solve() << endl;
    }
}
```

#### 平行四边形优化

```c++
const int maxn = 250;

int n, sum[maxn];
int solve()
{
    int dp[maxn][maxn], s[maxn][maxn]; //s[i][j]表示区间[i,j]中的最优分割点
    mem(dp, 0);
    for (int i = 1; i <= n; i++) { //初始值
        s[i][i] = i;
    }
    for (int len = 1; len <= n - 1; len++) {
        for (int i = 1; i <= n - len; i++) {
            int j = i + len;
            dp[i][j] = INF;
            for (int k = s[i][j - 1]; k <= s[i + 1][j]; k++) { //缩小范围
                if (dp[i][k] + dp[k + 1][j] + sum[j] - sum[i - 1] < dp[i][j]) {
                    dp[i][j] = dp[i][k] + dp[k + 1][j] + sum[j] - sum[i - 1];
                    s[i][j] = k; //记录[i,j]的最优分割点
                }
            }
        }
    }
    return dp[1][n];
}
int main()
{
    ios::sync_with_stdio(0);
    cin.tie(0);
    while (cin >> n) {
        mem(sum, 0);
        for (int i = 1, temp; i <= n; i++) {
            cin >> temp;
            sum[i] = sum[i - 1] + temp; //sum[i,j]的值等于sum[j]-sum[i-1]
        }
        cout << solve() << endl;
    }
}
```

#### 环形石子处理

```c++
const int maxn = 100 + 10;
int n, sum[maxn];
int a[maxn * 2]; //stone
pair<int, int> solve()
{
    int dp1[2 * maxn][2 * maxn], dp2[2 * maxn][2 * maxn];
    mem(dp1, 0);
    mem(dp2, 0);
    for (int len = 1; len < n; len++) {
        for (int i = 1; i <= 2 * n - 1 - len; i++) {
            int j = i + len;
            dp1[i][j] = INF;
            dp2[i][j] = 0;
            for (int k = i; k < j; k++) {
                dp1[i][j] = min(dp1[i][j], dp1[i][k] + dp1[k + 1][j] + sum[j] - sum[i - 1]);
                dp2[i][j] = max(dp2[i][j], dp2[i][k] + dp2[k + 1][j] + sum[j] - sum[i - 1]);
            }
        }
    }
    int ans1 = INF, ans2 = 0;
    for (int i = 1; i <= n; i++) {
        ans1 = min(ans1, dp1[i][i + n - 1]);
        ans2 = max(ans2, dp2[i][i + n - 1]);
    }
    return make_pair(ans1, ans2);
}
int main()
{
    cin >> n;
    for (int i = 1; i <= n; i++) {
        cin >> a[i];
    }
    int j = 1;
    for (int i = n + 1; i <= 2 * n - 1; i++) {
        a[i] = a[j++];
    }
    for (int i = 1; i <= 2 * n - 1; i++) {
        sum[i] = sum[i - 1] + a[i];
    }
    cout << solve().first << endl
         << solve().second;
}
```

### [poj3280]()：变成回文串的最小花费
给定一个字符串，可以在任意位置增删字母，每个字母花费不同，求将字符串变为回文串的最小花费
```c++
const int maxn = 30;
const int maxm = 2010;
int n, m, w[maxn];
int dp[maxm][maxm]; //字符串s的子区间s[i,j]变成回文的最小花费
string s;
int solve()
{
    mem(dp, 0);
    for (int i = m - 1; i >= 0; i--) {
        for (int j = i + 1; j < m; j++) {
            if (s[i] == s[j]) {
                dp[i][j] = dp[i + 1][j - 1];
            } else {
                dp[i][j] = min(dp[i + 1][j] + w[s[i] - 'a'], dp[i][j - 1] + w[s[j] - 'a']);
            }
        }
    }
    return dp[0][m - 1];
}
int main()
{
    while (cin >> n >> m) {
        cin >> s;
        char c;
        int x, y;
        for (int i = 0; i < n; i++) {
            cin >> c >> x >> y;
            w[c - 'a'] = min(x, y);
        }
        cout << solve() << endl;
    }
}
```



