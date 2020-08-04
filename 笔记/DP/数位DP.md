## 数位DP

### 题目：不要62（hdu2089）

#### 递推实现

> 参考：https://www.cnblogs.com/dplearning/p/4719375.html

```c++
const int maxl = 8;
int dp[maxl][3];
void init()
{
    mem(dp, 0);
    //0:存储0到i位数字中不含4和62的数字个数，即幸运数
    //1:存储0到i位数字中以2开头的幸运数。
    //2:来存储0到i位数字中的非幸运数，即包含4或者62的数字。
    dp[0][0] = 1;
    dp[0][1] = 0;
    dp[0][2] = 0;
    for (int i = 1; i <= maxl; i++) {
        //0到i位数字中的幸运数个数=(i-1)位幸运数字前面加上0~9中除去4以外的9个数字 - 以2开头的(i-1)位幸运数字前面加上了6
        dp[i][0] = dp[i - 1][0] * 9 - dp[i - 1][1];
        //0到i位数字中以2开头的幸运数=0到i位数字中所有的幸运数字前面加上2
        dp[i][1] = dp[i - 1][0];
        //0到i位的非吉利数=0到i-1位的非吉利数前面加上0-9的任何数字 + i-1位的吉利数字前面加上了4 + i-1位以2开头的吉利数字前面加上了6.
        dp[i][2] = dp[i - 1][2] * 10 + dp[i - 1][0] + dp[i - 1][1];
    }
}
int solve(int x)
{
    int digit[maxl]; //记录数字
    int cnt = 0, temp = x;
    while (temp) {
        digit[++cnt] = temp % 10;
        temp /= 10;
    }
    digit[cnt + 1] = 0;
    int flag = 0; //记录是否出现了非吉利数字
    int ans = 0;
    for (int i = cnt; i > 0; i--) {
        //先加上0~i-1位的所有不吉利数字
        ans += digit[i] * dp[i - 1][2];
        if (flag) {
            //如果已经出现了吉利数字,那么0~i-1位的所有吉利数字都会成为不吉利数字
            ans += digit[i] * dp[i - 1][0];
        } else {
            if (digit[i] > 4) {
                //如果大于4,那么就要考虑0~i-1位的所有吉利数字中前面添加4的情况
                ans += dp[i - 1][0];
            }
            if (digit[i] > 6) {
                //如果大于6,那么就要考虑0~i-1位的所有以2开头的吉利数字中前面添加6的情况
                ans += dp[i - 1][1];
            }
            if (digit[i + 1] == 6 && digit[i] > 2) {
                //如果前一位是6,并且本位大于2,就需要考虑0~i位的所有以2开头的吉利数字前面添加6的情况
                ans += dp[i][1];
            }
        }
        if (digit[i] == 4 || digit[i + 1] == 6 && digit[i] == 2) {
            ///如果出现了非吉利数字,进行标记
            flag = 1;
        }
    }
    return x - ans;
}
int main()
{
    init();
    int n, m;
    while (cin >> n >> m) {
        if (n == 0 && m == 0)
            break;
        cout << solve(m + 1) - solve(n) << endl; 
        //每次在判断数字末尾时，都比该位数字少1，所以选择 m+1, 而 n-1 就变成了 n
    }
    return 0;
}
```



