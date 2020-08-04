## 基础算法-二分
### Widespread[^1]（二分答案）
>- 题目：
>
>  n 个怪物，每个怪物都有一个血量 `h[i]`，攻击一个怪物可以对其造成 A 点伤害，对其他怪物造成 B 点伤害，问最少攻击多少次能打死所有怪物？
>
>- 输入：
>
>  ```
>   4 5 3
>   8 7 4 2
>  ```
>
>- 输出：
>
>  ```
>  2
>  ```
>

假设攻击了$x$次，共攻击$k$个怪物，则有$x=t_1+t_2+\cdots +t_k$，那么对第$i$个怪物造成的伤害为$(x-t_i)\times B+t_i\times A=x\times B+(A-B)\times t_i$
因此可以二分枚举攻击次数，最多攻击$right+=a[i]/B+1$次，最少1次。每次判断$\sum t_i$是否小于等于$x$即可
```c++
ll n, a, b;
ll l = 1, r;
vector<ll> h;
bool judge(ll x)
{
    ll sum = 0; //最少攻击次数
    ll temp = x * b;
    for (int i = 1; i <= n; i++) {
        if (h[i] > temp) {
            sum += (h[i] - temp) / (a - b) + ((h[i] - temp) % (a - b) == 0 ? 0 : 1);
        }
    }
    if (sum <= x) {
        return true;
    }
    return false;
}
int main()
{
    cin >> n >> a >> b;
    h.resize(n + 1);
    for (int i = 1; i <= n; i++) {
        cin >> h[i];
        r += h[i] / b + 1; //最多攻击次数
    }
    sort(h.begin() + 1, h.end());
    while (l < r) {
        ll mid = (l + r) >> 1;
        if (judge(mid)) {
            r = mid;
        } else {
            l = mid + 1;
        }
    }
    cout << l << endl;
    return 0;
}
```

[^1]:AtCoder-2580