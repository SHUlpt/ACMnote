## 常用方法

|       方法        |                      用途                      |
| :---------------: | :--------------------------------------------: |
|    `m.size()`     |               返回当前的元素数量               |
|    `m.empty()`    |                判断大小是否为0                 |
|    `m.clear()`    |                    容器清空                    |
|  `m.count(key)`   |           返回键值等于key的元素个数            |
|   `m.find(key)`   | 返回键值等于key的第一个元素，没找到返回`end()` |
| `m.lower_bound()` |         返回键值>=key的第一个元素位置          |
| `m.upper_bound()` |          返回键值>key的第一个元素位置          |
|   `m.insert()`    |                    插入元素                    |
|  `m.erase(key)`   |           删除所有键值等于key的元素            |
|  `m.erase(pos)`   |            删除迭代器指向位置的元素            |

- set与map使用相同的内部结构，只不过set的value和key指向的是同一元素
- map和multimap会根据**key**对元素进行自动排序，所以应该根据**key**值进行搜索
- 元素的key不能直接修改，需要先移除后再重新插入



## 插入元素

```c++
//pair
m.insert(pair<int,string>(1,"one"));

//make_pair()
m.insert(make_pair(1,"one"));

//用数组方式插入
m[123]="one";

//检查插入是否成功
if(m.insert(make_pair(1,"one")).second){
    cout << "Success" << endl;
}else{
    cout << "Fail" << endl;
}
```

当map中存在某关键字时，不能用insert插入（集合的唯一性），但用数组方式将会覆盖该关键字对应的值



## 删除元素

```c++
//删除所有键值等于key的元素
m.erase(key);	

//仅删除multimap中第一个键值等于key的元素
typedef multimap<string,int> StringIntMap;
StringIntMap a;
StringIntMap::iterator pos;
pos=a.find(key);
if(pos!=a.end()){
    a.erase(pos);
}
```



## 遍历

```c++
map<int,string>::iterator it;
for(it=m.begin(); it!=m.end(); it++){
	cout << it->first << it->second;
}

for(auto& it : m){
	cout << it.first << it.second;
}

//遍历Multimap中所有指定键值的元素
for(pos=a.lower_bound(key); pos!=a.upper_bound(key); pos++){
    cout << pos->second << endl;
}
```



## 排序

将`map`中的元素以`pair`的形式放到`vector`中，实现排序

```c++
map<string,int>m;
vector<pair<string, int>> v;
for(map<string,int>::iterator it=m.begin(); it!=m.end(); it++){
	v.push_back(pair<string,int>(it->first, it->second));
}
sort(vec.begin(),vec.end(),cmp);
```

