## fill和fill_n
### fill函数
对给定区间进行赋值操作
```c++
//1.一般数组
int a[10],b[10][10];
fill(a,a+10,0);
//数组整体赋值
fill(a,sizeof(a)/sizeof(int),0);
//二维数组整体赋值
fill(b,sizeof(b)/sizeof(int),0);

//2.STL容器
vector<int>v;
fill(v.begin(),v.end(),1);
```

### fill_n函数
将数据的n个元素进行赋值
```c++
vector<int>v={1,2,3,4,5};
fill_n(v.begin(),5,1);

// 数组初始化
// 1维数组
int a[maxn];
fill_n(a,maxn,0);
// 2维数组
int a[maxn][maxn];
fill_n(a[0],maxn*maxn,0);
```


