# 位运算

将 num 的二进制的第 K 位设置为 1 :   (x| 1<\<k)

将 num 的二进制的第 K 位设置为 0:     (x& \~（ 1<\<k ）)&#x20;

将 num 的二进制的第 K 位取反   ：  &#x20;

计算 num 的二进制中 1 的数量  ：&#x20;

判断 num 是否是 2 的非负整数次幂：  (x & (x-1)) == 0

删除 num 最低有效位（5 的二进制为 101，删除最低有效位得到 4）

遍历 num 的所有非空子集合（5 的二进制为 101，其非空子集合为 \[001, 100, 101]）

&#x20;二，八，十六进制互相转换

不同位运算（异或，或，和）的运算律（交换律，结合律）

实现changeToOne(num, K) 将num的二进制的第K位设置为1

```cpp
// Some code
int changeToOne(int num, int k)  // k is 0 index
{
    return  num| (1<<k); 
}
```

实现changeToZero(num, K)将num的二进制的第K位设置位0

```cpp
// Some code
int changeToOne(int num, int k)  // k is 0 index
{
    return  num& ~(1<<k); 
}
```

实现reverseK(num, K)将num的二进制的第K位取反

```cpp
// Some code
int reverseK(int num, int k)  // k is 0 index
{
    return  num^ (1<<k); 
}c
```

实现cal(num)，计算num的二进制中1的数量



实现check(num)，判断是否是2的非负整数次幂

实现delete(num)，删除num最低有效位

实现loop(num)，遍历num的所有非空子集合
