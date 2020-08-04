## [Kongey Donk](https://codeforces.com/gym/102448/problem/K)

### 【记忆化搜索】

> - 题目
>
>   有`n`颗排成一排且高度为`h`的树，每颗树从上到下依次分布着`h`个香蕉。可以选择从其中任意一个树的顶端开始，每次可以选择向左下、下或者右下移动，问到底地面时最多能拿到多少个香蕉
>
> - 输入
>
>   `n`颗树，树高`h`，$0<n,h\le 2*10^5,0<n*h\le10^6$
>
>   ```
>   3 3
>   1 5 5
>   9 0 0
>   15 2 1
>   ```
>
> - 输出
>
>   ```
>   20
>   ```

### 题解

用$dp[i][j]$表示：当前到达第$i$颗树的高度为$j$时所获得的最大香蕉数
对于第$i$颗树，每一层的结果都可以根据上一层中左上，上，右上的结果进行转移，实质上就是遍历层高，每次对当前层的结果进行更新。

```c++
const int maxn = 2e5 + 10;
int n, h;
vector<int> dp[maxn]; //前i列高度为j时的情况
int main()
{
    cin >> n >> h;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < h; j++) {
            int x;
            cin >> x;
            dp[i].push_back(x);
        }
    }
    for (int i = 1; i < h; i++) { //高度
        for (int j = 0; j < n; j++) { //列数
            int tempa = 0, tempb = 0, tempc;
            if (j >= 1) {
                tempa = dp[j - 1][i - 1];
            }
            if (j + 1 < n) {
                tempb = dp[j + 1][i - 1];
            }
            tempc = dp[j][i - 1];
            dp[j][i] += max(max(tempa, tempb), tempc);
        }
    }
    int ans = 0;
    for (int i = 0; i < n; i++) {
        ans = max(ans, dp[i][h - 1]);
    }
    cout << ans << endl;
}
```

