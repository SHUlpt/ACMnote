## 常见用法

```c++
int cmp(int a, int b)
{
    return a > b;
}
int main()
{
    int num[6] = { 1, 2, 4, 7, 15, 34 };
    
    sort(num, num + 6); //按从小到大排序
    //返回数组中第一个大于或等于被查数的位置
    int pos1 = lower_bound(num, num + 6, 7) - num; 
    //返回数组中第一个大于被查数的位置
    int pos2 = upper_bound(num, num + 6, 7) - num;
    cout << pos1 << " " << num[pos1] << endl;
    cout << pos2 << " " << num[pos2] << endl;
    
    sort(num, num + 6, cmp); //按从大到小排序
    //返回数组中第一个小于或等于被查数的位置
    int pos3 = lower_bound(num, num + 6, 7, greater<int>()) - num; 
    //返回数组中第一个小于被查数的位置
    int pos4 = upper_bound(num, num + 6, 7, greater<int>()) - num; 
    cout << pos3 << " " << num[pos3] << endl;
    cout << pos4 << " " << num[pos4] << endl;
    return 0;
}
```

- 不管数组下标是从0开始还是从1开始，在计算位置的时候都应该减去首地址`num`

