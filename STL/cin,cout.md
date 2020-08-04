## cin, cout

### cin

#### cin >>

1. `cin>>`等价于`cin.operator>>()`
2. 当`cin>>`从缓冲区读取数据时，若缓冲区中第一个字符是空格、tab或者换行符等分隔符时，将忽略并清除，继续读取下一个字符。但是如果读取成功，字符后面的分隔符是残留在缓冲区的，`cin>>`不做处理

#### cin.get

- 读取当单个字符：`cin.get(a)`或者`a=cin.get()`

1. `cin.get()`从输入缓冲区读取单个字符时不忽略分隔符，直接将其读取。
2. `cin.get()`的返回值是int类型，遇到文件结束符时返回`EOF`

读取整行：`cin.get(array, 20)`，`cin.get(array, 20, ' ')`

`cin.get()`读取整行时，遇到换行符时结束读取，但是不对换行符进行处理，换行符仍然残留在输入缓冲区

#### cin.getline

- 读取整行：`cin.getline(array, 20)`，`cin.getline(array, 20, '\n')`

`cin.getline()`不会将结束符或者换行符残留在输入缓冲区

#### getline

- 读取整行：`getline(cin,string)`

### cout

```c++
//分别以十六进制，八进制输出n
cout << hex << n << " " << oct << n;

//保留5位有效数字
cout << setprecision(5) << x;

//保留小数点后面5位
cout << fixed << setprecision(5) << x;
```

