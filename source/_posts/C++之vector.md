---
title: C++之vector
tags:
 - C++
 - STL
date: 2019-07-14 8:55:00
---

# Vector

<!--more-->

##### 1.vector头文件

`#include"vector"`

##### 2.vector声明及初始化

​	1)一维数组

```c++
vector<int> vec;        //声明一个int型向量
vector<int> vec(5);     //声明一个初始大小为5的int向量
vector<int> vec(10, 1); //声明一个初始大小为10且值都是1的向量
vector<int> vec(tmp);   //声明并用tmp向量初始化vec向量
vector<int> tmp(vec.begin(), vec.begin() + 3);  //用向量vec的第0个到第2个值初始化tmp
int arr[5] = {1, 2, 3, 4, 5};   
vector<int> vec(arr, arr + 5);      //将arr数组的元素用于初始化vec向量
vector<int> vec(&arr[1], &arr[4]); //将arr[1]~arr[4]范围内的元素作为vec的初始值 
```

​	2)二维数组

```c++
vector<vector<int>> vec;  //声明一个二维int向量
vector<vector<int>> vec(n,vector<int>(m,0)); //声明并初始一个n行m列值都是0的向量
```

**如果初始化的为指针，那么所有的全都指向同一个地方,例如vector<Node*> arr(len,new Node(-1))**



##### 3.vector基本操作

(1). 容量

- 向量大小： vec.size();
- 向量最大容量： vec.max_size();
- 更改向量大小： vec.resize();
- 向量真实大小： vec.capacity();
- 向量判空： vec.empty();
- 减少向量大小到满足元素所占存储空间的大小： vec.shrink_to_fit(); //[shrink_to_fit](http://www.cplusplus.com/reference/vector/vector/shrink_to_fit/)

(2). 修改

- 多个元素赋值： vec.assign(); //类似于初始化时用数组进行赋值
- <u>末尾添加元素： vec.push_back();</u>
- <u>末尾删除元素： vec.pop_back();</u>
- 任意位置插入元素： vec.insert();
- 任意位置删除元素： vec.erase();
- 交换两个向量的元素： vec.swap();
- 清空向量元素： vec.clear();
- 去除相邻重复元素 :unique(); 

(3)迭代器

- 开始指针：vec.begin();
- 末尾指针：vec.end(); //指向最后一个元素的下一个位置
- 指向常量的开始指针： vec.cbegin(); //意思就是不能通过这个指针来修改所指的内容，但还是可以通过其他方式修改的，而且指针也是可以移动的。
- 指向常量的末尾指针： vec.cend();

(4)元素的访问

- 下标访问： vec[1]; //并不会检查是否越界
- at方法访问： vec.at(1); //以上两者的区别就是at会检查是否越界，是则抛出out of range异常
- 访问第一个元素： vec.front();
- 访问最后一个元素： vec.back();
- 返回一个指针： int* p = vec.data(); //可行的原因在于vector在内存中就是一个连续存储的数组，所以可以返回一个指针指向这个数组。这是是C++11的特性。

##### 4.算法

- 遍历元素

```c++
vector<int>::iterator it;
for (it = vec.begin(); it != vec.end(); it++)
    cout << *it << endl;
//或者
for (size_t i = 0; i < vec.size(); i++) {
    cout << vec.at(i) << endl;
}
```

- 元素翻转

```c++
#include <algorithm>
reverse(vec.begin(), vec.end());
```

- 元素排序

```c++
#include <algorithm>
sort(vec.begin(), vec.end()); //采用的是从小到大的排序
//如果想从大到小排序，可以采用上面反转函数，也可以采用下面方法:
bool Comp(const int& a, const int& b) {
    return a > b;
}
sort(vec.begin(), vec.end(), Comp);
```