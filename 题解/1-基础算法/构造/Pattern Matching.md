## 基础算法-构造

### Pattern Matching[^1] （字符串构造）

> 题目：
> 给定$n$个字符串，每个字符串仅由大写字母和若干个$*$构成，其中$*$可以表示成任意一个字符串（或者空串）。比如$*FUL$，可以表示成$AWFUL,FUL$
> 问能不能找到一个字符串$ans$，使得这$n$个字符串将所有的$*$号替换后都能够表示成$ans $
>
> 输入：
> 第一行一个整数$T(1\le T\le 100)$，代表$T$组测试数据
> 第二行一个整数$n(1\le n \le 50)$，代表有$n$个字符串需要同时满足
> 接下来$n$行，每行一个字符串$P_i(2\le P_i\le 100)$
>
> 输出：
> 如果存在，输出$ans$；否则输出$*$
>
> 样例：
>
> ```
> #input
> 2
> 5
> *CONUTS
> *COCONUTS
> *OCONUTS
> *CONUTS
> *S
> 2
> *XZ
> *XYZ
> 
> #output
> Case #1: COCONUTS
> Case #2: *
> ```

> **思路**

本题有3个难度，从最简单的开始考虑。

最简单的情况是$*$号在字符串的最左侧，也就是要求所有的字符串都**具有相同的后缀**。
可以找到这$n$个字符串里最长的一个，将其去掉$*$号后，看作答案。然后将其他字符串依次与最长的从后到前进行比较。

难度二中，依然只有一个$*$，只是位置是不固定的。如果$*$出现在字符串最右侧，则该字符串应该是答案的**前缀**；如果在最左侧，应该是答案的**后缀**；如果出现在中间，则$*$以左是**前缀**，以右是**后缀**。按照$*$位置对字符串进行分类，找到最长的前缀与最长的后缀，拼接后就是答案。

难度三中，$*$不止有一个。不过通过前两个难度，不难发现：对于一个字符串，它的第一个$*$号左边的部分应该是答案串的前缀，最后一个$*$号右边的部分应该是答案串的后缀。而对于剩下的中间部分，只需要将其中的所有字母加入到答案串中，一定能够满足。
因为在去掉字符串第一个$*$号左边的部分与最后一个$*$号右边的部分后，剩下的部分可以看作是被两个$*$号包住的（或者没有了）。比如$A*B*C$，可以得到$*B*$。因为左右都有$*$，所以可以匹配任意内容，只有$*$里面的字母出现了，就一定能够满足

> **代码**

```c++
#include <bits/stdc++.h>
using namespace std;
#define mem(a,b) memset(a,b,sizeof(a))
#define endl '\n'
typedef long long ll;
const int maxn = 50 + 10;
int n;
vector<string>pre, in, suf; //对于每个字符串，将其按照*划分
void init()
{
	pre.clear();
	in.clear();
	suf.clear();
}
void div(string& s) //对每个字符串进行划分
{
	int n = s.length();
	//取出第一个*左边的部分
	int l = 0;
	for (int i = 0; i < n; i++) {
		if (s[i] == '*') break;
		l++;
	}
	if (l > 0) {
		pre.push_back(s.substr(0, l));
	}
	//取出最后一个*右边的部分
	int r = 0;
	for (int i = n - 1; i >= 0; i--) {
		if (s[i] == '*') break;
		r++;
	}
	if (r > 0) {
		suf.push_back(s.substr(n - r));
	}
	//取出中间部分
	if (n - r - 1 > l-1) {
		in.push_back(s.substr(l, n - r - l));
	}
}
bool check(string& a, string& b)
{
	int n = a.length();
	for (int i = 0; i < n; i++) {
		if (a[i] != b[i]) return false;
	}
	return true;
}
bool cmp(string& a, string& b)
{
	return a.length() < b.length();
}
void solve()
{
	string ans;
	//检查前缀
	sort(pre.begin(), pre.end(),cmp);
	string temp_pre, temp_suf;
	if (!pre.empty()) {
		temp_pre = pre.back(); //找到最长的
		for (int i = 0; i < (int)pre.size() - 1; i++) {
			if (!check(pre[i], temp_pre)) {
				cout << "*" << endl;
				return;
			}
		}
		ans += temp_pre;
	}
	//检查后缀
	sort(suf.begin(), suf.end(),cmp);
	if (!suf.empty()) {
		temp_suf = suf.back();	//找到最长的
		reverse(temp_suf.begin(), temp_suf.end());	//翻转以方便比较
		for (int i = 0; i < (int)suf.size() - 1; i++) {
			string a = suf[i];
			reverse(a.begin(), a.end());
			if (!check(a, temp_suf)) {
				cout << "*" << endl;
				return;
			}
		}
	}
	
	//补充中间部分
	for (int i = 0; i < (int)in.size(); i++) {
		for (int j = 0; j < (int)in[i].size(); j++) {
			if (in[i][j] != '*') ans += in[i][j]; //取出中间部分所有字母
		}
	}
	reverse(temp_suf.begin(), temp_suf.end());
	ans += temp_suf;
	cout << ans << endl;
}
int main()
{
	ios::sync_with_stdio(0), cin.tie(0);
	int T;
	cin >> T;
	for (int t = 1; t <= T; t++) {
		cout << "Case #" << t << ": ";
		init();
		cin >> n;
		for (int i = 1; i <= n; i++) {
			string temp;
			cin >> temp;
			div(temp);
		}
		solve();
	}
}
```

[^1]:Code Jam 2020 Round1A A

