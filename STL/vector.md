# vector

## 常用方法

|         操作         |       效果        |
| :------------------: | :---------------: |
|      `a.size()`      |     读取大小      |
| `a.resize(num,elem)` |     改变大小      |
|     `a.empty()`      |     是否为空      |
| `a.push_back(elem)`  |  向尾部添加元素   |
|  `a.pop_back(elem)`  | 删除最后一个元素  |
|     `a.front()`      |  返回头元素引用   |
|      `a.back()`      |  返回尾元素引用   |
|    `a.erase(pos)`    | 删除pos位置的元素 |
|     `a.clear()`      |     容器清空      |



## 二维vector

### 声明

```c++
vector<vector<int>> v;
vector<vector<int>> v(n, vector<int>(n+1, INT_MIN)); //初始值为-∞，INT_MAX代表+∞
```

### 添加行

```c++
for(int i=0;i<n;i++){ //n行，每行元素个数不定
	v.push_back(vector<int>());
}
```

### 遍历二维vector中的元素

```c++
int n=v.size(); //行数
int m; //每行元素个数
for(int i=0;i<n;i++){
	for(int j=0;j<m;j++){ 
		v[i].push_back();
    }
}
```

### 取最大\最小元素

```c++
cout << *max_element(v[n].begin(), v[n].end()) << endl;
cout << *min_element(v[n].begin(), v[n].end()) << endl;
```



## 反向迭代器

```c++
vector<int>::reverse_iterator r_it;
for(r_it=vec.rbegin(); r_it=!vec.rend();r_it++){
    cout << *r_it;
}
//反向排序
sort(vec.rbegin(),vec.rend());
```



## erase-连续删除所有目标值

### 方法1
```c++
vector<int>::iterator it=v.begin();
while(it!=v.end())
{
	if(*it==1){	//要被删除的值
		it=v.erase(it);
	}else{
		it++;
	}
}
```
### 方法2
`remove(iterator fist, iterator last, val`
remove函数会将范围内所有等于val的值移动到最后
```c++
vector<int> v;
v.erase(remove(v.begin(), v.end(), val),v.end());
```



## 去重

`unique()`函数将相邻且重复的元素放到`vector`的尾部，然后返回指向第一个重复元素的迭代器。再用`erase()`函数删去从这个元素到末尾的所有元素

```c++
vector<int> v;
sort(v.begin(), v.end());
v.erase(unique(v.begin(), v.end()), v.end());
```

