## KMP

### 模板

```c++
#include <bits/stdc++.h>
using namespace std;
vector<int> Next;
vector<int> res; //模式串在主串中所有出现位置
void GetNext(const string &p) {
	int n = p.length();
	Next.resize(n+1);
	Next[0] = -1;
	int i = 0, j = -1;
	while (i < n) {
		if (j == -1 || p[i] == p[j]) {
			i++; j++;
            //未优化时，直接Next[i]=j;
			if (p[i] != p[j]) {
				Next[i] = j;
			}
			else {//如果相等，Next值应该再次回溯
				Next[i] = Next[j];
			}
		}
		else {
			j = Next[j];
		}
	}
}

void KMP(const string &s, const string &p) {//匹配所有位置
	int n = s.length(), m = p.length();
	GetNext(p);
	int i = 0, j = 0;
	while (i < n) {
		while (j != -1 && s[i] != p[j]) j = Next[j];
		i++; j++;
		if (j >= m) {
			cout << i - m + 1 << endl;
			res.push_back(i - m);
			j = Next[j];
		}
	}
}
```

