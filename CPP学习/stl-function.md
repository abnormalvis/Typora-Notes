STL可谓是C++的利器，深受算法竞赛选手所爱，今天我将常用函数总结了一下，他们是代码中的技巧，使用后代码层次直接提升一个档次。

废话不多说，直接开始

下文介绍常见STL函数，如要了解**STL容器**请访问  
👉[**C++ STL 详解超全总结（快速入门）**](https://blog.csdn.net/qq_50285142/article/details/114026148?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522163057300016780357292358%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fblog.%2522%257D&request_id=163057300016780357292358&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~blog~first_rank_v2~rank_v29-1-114026148.pc_v2_rank_blog_default&utm_term=stl&spm=1018.2226.3001.4450)👈

* * *

> 以下出现的描述说明：（下面函数排序按**字典序排列**）
>
> `beg`为序列的初始地址
>
> `end`为序列的尾地址

## STL函数

### accumulate

```
accumulate(beg, end, init)
```

**复杂度：** O ( N ) O(N) O(N)

> 作用：对一个序列的元素求和

`init`为对序列元素求和的**初始值**

返回值类型：与`init` 相同

```cpp
int a[]={1, 3, 5, 9, 10};

//对[0,2]区间求和，初始值为0，结果为0 + 1 + 3 + 5 = 9
int res1 = accumulate(a, a + 3, 0);

//对[0,3]区间求和，初始值为5，结果为5 + 1 + 3 + 5 + 9 = 23
int res2 = accumulate(a, a + 4, 5);
```

使用lambda表达式

```cpp
typedef long long ll;
struct node {
    ll num;
} st[10];

for(int i = 1; i <= n; i++)
    st[i].num = i + 10000000000;
//返回值类型与init一致，同时注意参数类型（a）也要一样
//初始值为1，累加1+10000000001+10000000002+10000000003=30000000007
ll res = accumulate(st + 1, st + 4, 1ll, [](ll a,node b) {
    return a + b.num;
});
```

### atoi

```
atoi(const char *)
```

> 将字符串转换为`int`类型

注意参数为`char`型数组，如果需要将string类型转换为int类型，可以使用`stoi`函数（参考下文），或者将`string`类型转换为`const char *`类型。

关于输出数字的范围：  
`atoi`**不做**范围检查，如果超出上界，输出上界，超出下界，输出下界。  
`stoi`**会做**范围检查，默认必须在`int`范围内，如果超出范围，会出现RE（Runtime Error）错误。

```cpp
string s = "1234";
int a = atoi(s.c_str());
cout << a << "\n"; // 1234
```

或者

```cpp
char s[] = "1234";
int a = atoi(s);
cout << a << "\n";
```

### fill

```
fill(beg, end, num)
```

**复杂度：** O ( N ) O(N) O(N)

> 对一个序列进行初始化赋值

```cpp
//对a数组的所有元素赋1
int a[5];
fill(a, a + 5, 1);
for(int i = 0; i < 5; i++)
    cout << a[i] << " ";
//1 1 1 1 1
```

注意区分memset：

`memset()`是按**字节**进行赋值，对于初始化赋`0`或`-1`有比较好的效果.

如果赋某个特定的数会**出错**，赋值特定的数建议使用`fill()`

### is\_sorted

```
is_sorted(beg, end)
```

**复杂度：** O ( N ) O(N) O(N)

> 判断序列是否有序（升序），返回`bool`值

```cpp
//如果序列有序，输出YES
if(is_sorted(a, a + n))
    cout << "YES\n";
```

### iota

```
iota(beg, end)
```

> 让序列递增赋值

```cpp
vector<int> a(10);
iota(a.begin(), a.end(), 0);
for(auto i : a)
	cout << i << " ";
// 0 1 2 3 4 5 6 7 8 9
```

### lower\_bound + upper\_bound

**复杂度：** O ( l o g N ) O(logN) O(logN)

> 作用：二分查找

```cpp
//在a数组中查找第一个大于等于x的元素，返回该元素的地址
lower_bound(a, a + n, x);
//在a数组中查找第一个大于x的元素，返回该元素的地址
upper_bound(a, a + n, x);

//如果未找到，返回尾地址的下一个位置的地址
```

### max\_element+min\_element

**复杂度：** O ( N ) O(N) O(N)

> 找最大最小值

```cpp
//函数都是返回地址，需要加*取值
int mx = *max_element(a, a + n);
int mn = *min_element(a, a + n);
```

### max+min

**复杂度：** O ( 1 ) O(1) O(1)

> 找多个元素的最大值和最小值

```cpp
//找a，b的最大值和最小值
mx = max(a, b);
mn = min(a, b);
```

```cpp
//找到a,b,c,d的最大值和最小值
mx = max({a, b, c, d});
mn = min({a, b, c, d});
```

### minmax

```
minmax(a, b)
```

**复杂度：** O ( 1 ) O(1) O(1)

> 返回一个`pair`类型，第一个元素是`min(a, b)`， 第二个元素是`max(a, b)`

```cpp
pair<int, int> t = minmax(4, 2);
// t.first = 2, t.second = 4
```

### minmax\_element

```
minmax_element(beg, end)
```

**复杂度：** O ( N ) O(N) O(N)

> 返回序列中的最小和最大值组成pair的对应的地址，返回类型为`pair<vector<int>::iterator, vector<int>::iterator>`

```cpp
int n = 10;
vector<int> a(n);
iota(a.begin(), a.end(), 1);
auto t = minmax_element(a.begin(), a.end()); // 返回的是最小值和最大值对应的地址
// *t.first = 1, *t.second = 10 输出对应最小最大值时需要使用指针
```

### nth\_element

```
nth_element(beg, nth, end)
```

**复杂度：** 平均 O ( N ) O(N) O(N)

> 寻找第序列第n小的值

`nth`为一个迭代器，指向序列中的一个元素。第n小的值恰好在`nth`位置上。

执行`nth_element()`之后，序列中的元素会围绕nth进行划分：**nth之前的元素都小于等于它，而之后的元素都大于等于它**

**实例：求序列中的第3小的元素**

```cpp
nth_element(a, a + 2, a + n);
cout << a[2] << '\n';
```

### next\_permutation

```
next_permutation(beg, end)
```

**复杂度：** O ( N ) O(N) O(N)

> 求序列的下一个排列，下一个排列是字典序大一号的排列

返回`true`或`false`

```cpp
//对a序列进行重排
next_permutation(a, a + n);
```

**应用：求所有的排列**

输出`a`的所有排列

```cpp
// 数组a不一定是最小字典序序列，一定注意将它排序
sort(a, a + n);
do {
 	for(int i = 0; i < n; i++)
        printf("%d ", a[i]);
} while(next_permutation(a, a + n));
```

> 求出前一个排列，如果序列为最小的排列，将其重排为最大的排列，返回false

### partial\_sort

```
partial_sort(beg, mid, end)
```

**复杂度：** 大概 O ( N l o g M ) O(N logM) O(NlogM) `M`为距离

> 部分排序,排序mid-beg个元素，mid为要排序区间元素的尾后的一个位置
>
> 从beg到mid**前**的元素都排好序

对a数组前5个元素排序按从小到大排序

```cpp
int a[] = {1,2,5,4,7,9,8,10,6,3};
partial_sort(a, a + 5, a + 10);
for(int i = 0; i < 10; i++) 
    cout << a[i] << ' ';
//1 2 3 4 5 9 8 10 7 6
//前五个元素都有序
```

也可以添加自定义排序规则：

`partial_sort(beg,mid,end,cmp)`

对a的前五个元素都是降序排列

```cpp
int a[] = {1,2,5,4,7,9,8,10,6,3};
partial_sort(a, a + 5, a + 10, greater<int>());
for(int i = 0; i < 10; i++) 
    cout << a[i] << ' ';
//10 9 8 7 6 1 2 4 5 3
//前五个元素降序有序
```

### partial\_sum

> 求序列的前缀和，来源容器为x，目标容器为y，则 y \[ i \] = x \[ 0 \] + ⋯ + x \[ i \] y\[i\] = x\[0\] + \\cdots + x\[i\] y\[i\]\=x\[0\]+⋯+x\[i\]

```cpp
vector<int> a{0, 2, 3, 4, 1, 5};
vector<int> s; // 不用声明大小
partial_sum(a.begin(), a.end(), back_inserter(s)); // 直接将a的前缀和插入到s的后面
for (auto c: s) {
    cout << c << " ";
}
// 0 2 5 9 10 15 
```

### random\_shuffle

**复杂度：** O ( N ) O(N) O(N)

> 1.  随机打乱序列的顺序
> 2.  `random_shuffle` 在 `C++14` 中被弃用，在 `C++17` 中被废除，C++11之后应尽量使用`shuffle`来代替。

```cpp
vector<int> b(n);
iota(b.begin(), b.end(), 1);// 序列b递增赋值 1, 2, 3, 4,...
// 对a数组随机重排
random_shuffle(a, a + n);
// C++11之后尽量使用shuffle
shuffle(b.begin(), b.end());
```

### reverse

```
reverse(beg,end)
```

**复杂度：** O ( N ) O(N) O(N)

> 对序列进行翻转

```cpp
string s = "abcde";
reverse(s.begin(), s.end());//对s进行翻转
cout << s << '\n';//edcba

//对a数组进行翻转
int a[] = {1, 2, 3, 4};
reverse(a, a + 4);
cout << a[0] << a[1] << a[2] << a[3];//4321
```

### set\_union, set\_intersection,set\_difference

复杂度： O ( N + M ) O(N+M) O(N+M)

> 求两个集合的并集，交集，差集。手动实现双指针就可以搞定，嫌麻烦可以使用该函数。

注意：两个集合必须为有序集合，所以下面演示代码使用了排序。vector容器可以替换成set容器，因为set自动会对元素进行排序。

函数的参数有五个，前两个为第一个容器的首尾迭代器，第三四个为第二个容器的首尾迭代器，最后一个为插入位置，即将结果插入到哪个地址之后。

```cpp
vector<int> a = {4, 5, 2, 1, 8}, b = {5, 3, 8, 9, 2};
sort(a.begin(), a.end()); // 1 2 4 5 8
sort(b.begin(), b.end()); // 2 3 5 8 9
vector<int> c, d, e;
// a并b：1 2 3 4 5 8 9
set_union(a.begin(), a.end(), b.begin(), b.end(), inserter(c, c.begin()));
// a交b：2 5 8
set_intersection(a.begin(), a.end(), b.begin(), b.end(), inserter(d, d.begin()));
// a差b： 1 4
set_difference(a.begin(), a.end(), b.begin(), b.end(), inserter(e, e.begin()));
```

### sort

**复杂度：** O ( N l o g N ) O(N logN) O(NlogN)

> 作用：对一个序列进行排序

```cpp
//原型：
sort(beg, end);
sort(beg, end, cmp);
```

几种排序的常见操作：

```cpp
int a[N]; // 普通数组定义
// 对 a 数组的[1, n]位置进行从小到大排序
sort(a + 1, a + 1 + n);

vector<int> b(n + 1); // vector数组定义
sort(b.begin() + 1, b.end());
```

```cpp
//对a数组的[0, n-1]位置从大到小排序
sort(a, a + n, greater<int>());
//对a数组的[0, n-1]位置从小到大排序
sort(a, a + n, less<int>());

vector<int> b(n + 1);
sort(b.begin() + 1, b.end()); // 升序
sort(b.begin() + 1, b.end(), greater<int>()); // 降序
```

```cpp
vector<int> a(n);
sort(a.rbegin(), a.rend()); // 使用反向迭代器进行降序排序
```

```cpp
// 1. 使用函数自定义排序，定义比较函数
bool cmp(node a, node b) {
    //按结构体里面的x值降序排列
    return a.x > b.x;
}
sort(node, node + n, cmp); // 只能接受 以函数为形式的自定义排序规则，无法接受以结构体为形式的自定义排序规则

// 2. 或者使用匿名函数自定义排序规则
sort(node, node + n, [](node a, node b) {
    return a.x > b.x;
});
```

> **注意**：针对自定义排序的方法需要注意，自定义排序规则中的比较函数参数最好写成 **引用形式** 。  
> 非引用形式会进行参数拷贝，导致额外的时间性能损耗。参数为引用形式会避免这一点。
>
> ```cpp
> // vector里面存储的是大小为3的vector
> vector<vector<int>> a(n + 1);
> sort(a.begin(), a.end(), [](vector<vector<int>>& a, vector<vector<int>>& b) {
> 	return a[0] < b[0];
> });
> ```

### stable\_sort

**复杂度：** O ( N l o g N ) O(N logN) O(NlogN)

> 功能和 `sort()` 基本一样
>
> 区别在于`stable_sort()`能够保证相等元素的相对位置，排序时不会改变相等元素的相对位置

使用用法和`sort()`一样，见上

### stoi

```
stoi(const string*)
```

> 将对应string类型字符串转换为数字，记忆：`s -> t 分别对应两个数据类型的某个字母`

注意参数为`string`字符串类型。

关于输出数字的范围：

```cpp
string s = "1234";
int a = stoi(s);
cout << a << "\n"; // 1234
```

### transform

**复杂度：** O ( N ) O(N) O(N)

> 作用：使用给定操作，将结果写到dest中

```cpp
//原型：
transform(beg, end, dest, unaryOp);
```

一般不怎么使用，徒增记忆负担，不如手动实现。

```cpp
//将序列开始地址beg到结束地址end大小写转换，把结果存到起始地址为dest的序列中
transform(beg, end, dest, ::tolower);
transform(beg, end, dest, ::toupper);
```

### to\_string

> 将数字转化为字符串，支持小数（double）

```cpp
int a = 12345678;
cout << to_string(a) << '\n';
```

### unique

```
unique(beg, end)
```

**复杂度：** O ( N ) O(N) O(N)

> 消除重复元素，返回消除完重复元素的下一个位置的地址
>
> 如：`a[] = {1, 3, 2, 3, 6}`;
>
> `unique` 之后 `a` 数组为`{1, 2, 3, 6, 3}`前面为无重复元素的数组，后面则是重复元素移到后面，返回`a[4]`位置的地址（不重复元素的尾后地址）

消除重复元素一般需要原序列是**有序序列**

**应用：离散化**

```cpp
for(int i = 0; i < n; i++) {
    cin >> a[i];
    b[i] = a[i];//将a数组复制到b数组
}
// 排序后 b：{1, 2, 3, 3, 6}
sort(b, b + n);//对b数组排序
// 消除重复元素b：{1, 2, 3, 6, 3} 返回的地址为最后一个元素3的地址 
int len = unique(b, b + n) - b;//消除 b 的重复元素，并获取长度
for(int i = 0; i < n; i++) {
    //因为b有序，查找到的下标就是对应的 相对大小（离散化后的值）
    int pos = lower_bound(b, b + len, a[i]) - b;//在b数组中二分查找第一个大于等于a[i]的下标
    a[i] = pos; // 离散化赋值
}
```

```cpp
vector<int> a(n);
for (int i = 0; i < n; ++i) {
    cin >> a[i];
}
vector<int> b = a;
sort(b.begin(), b.end());
b.erase(unique(b.begin(), b.end()), b.end());
for (int i = 0; i < n; ++i) {
	a[i] = lower_bound(b.begin(), b.end(), a[i]) - b.begin() + 1; // 离散后的数据从1开始   
}
```

### \_\_gcd

C++14中支持 `std::__gcd()` ，对于C++17，可以使用 `<numeric>` 头中的 `std::gcd` 与 `std::lcm` 来求最大公约数和最小公倍数。

```
__gcd(a, b)
```

> 求a和b的最大公约数

`__gcd(12,15) = 3`

`__gcd(21,0) = 21`

### \_\_lg

```
__lg(a)
```

> 1.  求一个数二进制下最高位位于第几位（从**第0位**开始）（或二进制数下有几位）
> 2.  `__lg(x)`相当于返回 ⌊ l o g 2 x ⌋ \\lfloor log\_2 x \\rfloor ⌊log2x⌋
> 3.  复杂度 O ( 1 ) O(1) O(1)

`__lg(8) = 3`

`__lg(15) = 3`

### \_*builtin* 内置位运算函数

> 需要注意：内置函数有相应的`unsigned lnt`和`unsigned long long`版本，`unsigned long long`只需要在函数名后面加上`ll`就可以了，比如`__builtin_clzll(x)` ，默认是32位`unsigned int`
>
> 很多题目和 `long long` 数据类型有关，如有需要注意添加 `ll`

#### \_\_builtin\_ffs

```
__builtin_ffs(x)
```

> 二进制中对应最后一位`1`的位数，比如`4`会返回`3`（100）

#### \_\_builtin\_popcount

```
__builtin_popcount(x)
```

> `x`中`1`的个数

#### \_\_builtin\_ctz

```
__builtin_ctz(x)
```

> `x`末尾`0`的个数（`count tail zero`）

#### \_\_builtin\_clz

```
__builtin_clz(x)
```

> `x`前导`0`的个数（`count leading zero`）

```cpp
cout << __builtin_clz(32); // 26
//因为共有6位,默认数据范围为32位，32 - 6 = 26
```

#### \_\_builtin\_parity

```
__builtin_parity(x)
```

> `x`中1的个数的奇偶性， 奇数输出`1`，偶数输出`0`

## C++20 ranges

ranges主要用来简化迭代器操作，可以少写很多迭代器操作相关的代码。  
ranges集成了很多STL函数

```cpp
vector a{1, 2, 3, 4, 7, 6, 5};
ranges::sort(a); // sort(a.begin(), a.end());
ranges::sort(a, greater<int>()); // sort(a.begin(), a.end(), greater<int>());
int mx = *ranges::max_element(a);
int mn = *ranges::min_element(a);

```

> 可参考链接：
>
> 1.  [C++语法糖](https://www.luogu.com.cn/blog/AccRobin/grammar-candies) https://www.luogu.com.cn/blog/AccRobin/grammar-candies

* * *

**如果你有任何想要补充的函数，欢迎评论留言补充呀！**

> 如要获取所有内容的PDF文件，请在公众号【行码棋】回复【STL】获取，非常抱歉了。  
> Update：2023-12-11更新PDF文件