## 数学-线性基

[Grand Prix of Nanjing : A. Everyone Loves  Playing Games](https://official.contest.yandex.ru/opencupXX/contest/18242/problems/A/)【线性基博弈】

### 题目

两个人玩游戏，第一个人有$n$个二元组，第二个人有$m$个二元组。第一个人先从自己的每个二元组里选一个数，然后第二个人再从自己的每个二元组里选一个数，最后结果就是这些数的异或和

两个人都知道对方的所有手牌并且都会采取最优策略。第一个人希望最后的结果尽可能大，第二个人希望最后的结果尽可能小，求最终结果

### 题解

可以假设两人都选择二元组中的第一个数，答案为$s$，这样问题就可以转化为每次选与不选$x_i\oplus y_i$的问题，可以用线性基来解决。

对于$s$，从高位向低位考虑，对于每一位：

- 如果当前位是0，判断第一个人的线性基能否控制这一位
- 如果当前位是1，判断第二个人的线性基能否控制这一位
- 如果两个人同时可以控制这一位，则把两个人的线性基的这个向量异或起来插到第一个人的线性基里

对于第3种情况，因为第一个人是先手，所以选择权是在第一个人手上的。  
如果第一个人选择操作，那么第二个人为了使结果变小，一定也会选择操作。因此对于这一位来说，虽然结果一定会是0，但是第一个人实际上是有两种选择情况的。  
这里先假设第一个人选择操作，将$a\oplus b$插入第一个人的线性基里，这样在之后的情况里，第一个人可以通过选择$a\oplus b$来实现“反悔”操作，从而保证结果最大

> 对于$a\oplus b$插入操作的理解：虽然就结果而言，本位一定会变成0，但选与不选带来的后果是不同的。如果选的话，可能不仅仅会改变当前位，之后的位也可能被改变，因此就把$a\oplus b$插入第一个人的线性基。如果在之后的选择过程中，发现某一位是0（假设是因为之前选择操作带来的0），如果此时先前插入的$a\oplus b$能够控制该位，就说明如果之前选择不操作，实际上答案可以更大（正是因为之前操作了而导致是1的位变成了0，但是之前不论操作还是不操作，那一位都是0，所以当时应该选择不操作，也就相当于在这一位选择$a\oplus b$来改变之前的选择），$a\oplus b$正是给了第一个人“反悔”的机会。

### 代码

```c++
#include <bits/stdc++.h>
using namespace std;
#define mem(a,b) memset(a,b,sizeof(a))
#define endl '\n'
typedef long long ll;
const int maxn = 1e4 + 10;
const int Bit = 63;
int n, m;
ll a[maxn], b[maxn];			//原集合
ll Pa[Bit + 10], Pb[Bit + 10];	//新集合
void init()
{
	fill_n(Pa, Bit + 10, 0);
	fill_n(Pb, Bit + 10, 0);
}
void Insert(ll x, ll *P) {
	for (int j = Bit; j >= 0; j--) {
		if ((x >> j) & 1) {
			if (!P[j]) {
				P[j] = x;
				break;
			}
			else x ^= P[j];
		}
	}
}
int main()
{
	ios::sync_with_stdio(0), cin.tie(0);
	int t; cin >> t;
	while (t--) {
		init();
		cin >> n >> m;
		ll ans = 0;
		ll x, y;
		for (int i = 1; i <= n; i++) {
			cin >> x >> y;
			a[i] = x ^ y;
			ans ^= x;
			Insert(a[i], Pa);
		}
		for (int i = 1; i <= m; i++) {
			cin >> x >> y;
			b[i] = x ^ y;
			ans ^= x;
			Insert(b[i], Pb);
		}
		for (int i = Bit; i >= 0; i--) {
			if (Pa[i] && Pb[i]) {
				Insert(Pa[i]^Pb[i], Pa);
				if ((ans >> i) & 1) ans ^= Pa[i];
			}
			else if ((ans >> i) & 1) {
				if (Pb[i]) ans ^= Pb[i];
			}
			else {
				if (Pa[i]) ans ^= Pa[i];
			}
		}
		cout << ans << endl;
	}
}
```



