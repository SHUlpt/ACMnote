## 数学-数论

### [Two Divisors](https://codeforces.com/contest/1366/problem/D)【GCD】

给定一组整数$a$，判断每一个$a_i$，其因数中是否存在两个因子满足：$d_1>1,d_2>1$，并且$gcd(d_1+d_2,a)=1$



### 题解

对$a_i$进行质因数分解：$a_i=p_1^{s_1}p_2^{s_2}\cdots p_k^{s_k}$

当$k\not=1$时，可以将质因数分为两组：$\{p_1,p_2,\cdots,p_x\},\ \{p_{x+1},\cdots,p_k\}$，并令$d_1=p_1\cdot p_2\cdots p_x,\ d_2=p_{x+1}\cdots p_k$，可以证明$gcd(d_1+d_2,a_i)=1$

证明如下：

任取一个质因数$p_i$，显然有$a_i\equiv 0\mod p_i$。

假设有$d_1\equiv0\mod p_i$，由于$d_1,d_2$互质，必然存在$d_2\not\equiv0\mod p_i$

因此，$d_1+d_2\equiv0+d_2\equiv d_2\not\equiv0\mod p_i$

也就是说，对于$a_i$中任意一个质因数$p_i$，$d_1+d_2$都不能整除，因此$gcd(d_1+d_2,a_i)=1$，得证

#### 结论



### 代码

