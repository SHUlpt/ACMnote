## 常用方法

|         方法          |                     用途                     |
| :-------------------: | :------------------------------------------: |
|   `s.insert(elem)`    |       插入一个elem元素，返回新元素位置       |
|    `s.erase(elem)`    | 删除所有与elem相等的元素，返回被移除元素个数 |
|    `s.erase(pos)`     |     删除迭代器pos所指位置元素，无返回值      |
|      `s.clear()`      |                   清空set                    |
|      `s.empty()`      |                 判断是否为空                 |
|      `s.size()`       |                 返回元素个数                 |
|    `s.count(elem)`    |            返回元素值为elem的个数            |
|    `s.find(elem)`     |  返回一个迭代器，指向第一个键值为elem的元素  |
| `s.lower_bound(elem)` |   返回一个迭代器，指向键值>=k的第一个元素    |
| `s.upper_bound(elem)` |    返回一个迭代器，指向键值>k的第一个元素    |



## set示例

```c++
typedef set<int,greater<int>> IntSet;
IntSet a;
IntSet::iterator pos;

//遍历set
for(it=s.begin(); it!=s.end(); it++)
{
	cout << *it << endl;
}

for (auto& it : a) {
	cout << it << endl;
}

//插入重复元素
pair<IntSet::iterator,bool>status=a.insert(4);
if(status.second){
    cout << "4 inserted as element"
         << distance(a.begin(),status.first)+1
         << endl;
}
else{
    cout << "4 already exists" << endl;
}

//copy set后进行输出
copy(a.begin(), a.end(), ostream_iterator<int>(cout, " "));
```



## multiset示例

```c++
typedef multiset<int,greater<int>> IntSet;
IntSet a;

//删除所有排在指定值之前的元素
a.erase(a.begin(),a.find(2));
//1 1 2 2 3 -> 2 2 3
//3 2 2 1 1 -> 2 2 1 1

//删除所有特定值的元素，并返回删除元素个数
int num;
num = a.erase(5);	//删除所有值为5的元素
cout << num << endl;	
```

