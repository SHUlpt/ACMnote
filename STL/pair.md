## pair使用

### 头文件

```c++
#include<utility>
```

### 定义

```c++
pair<T1,T2> p1;
pair<T1,T2> p1(v1,v2); 
make_pair(v1,v2);
```

>如果定义多个相同的pair类型对象，可以用宏定义简化
>`#define pi pair<string,string>`

### 操作

|            示例             |                 解释                  |
| :-------------------------: | :-----------------------------------: |
|           `p1<p2`           | 根据字典序比较，first相同时比较second |
|    `p1=make_pair(1,1,2)`    |     利用make_pair创建新的pair对象     |
| `cout<<p1.first<<p1.second` |     使用first和second访问两个元素     |

```c++
//当函数以pair作为返回值时，可以通过tie接受
pair<string,int> get()
{
	return make_pair("see",10);
}
int main()
{
	string name;
    int ages;
    tie(name,ages)=get();
    cout << name << ages << endl;
}
```

