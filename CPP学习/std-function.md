### c++常用库函数

### 1 min/max

min函数用于比较得到较小数

max函数用于比较得到较大数

```cpp
int a=1,b=2;
string s1 = "wang";
string s2 = "ming";
cout<<min(a,b)<<endl;
cout<<max(a,b)<<endl;
cout<<min(s1,s2)<<endl;
cout<<max(s1,s2)<<endl;
//输出结果
// 1
// 2
// ming
// kang
```

### 2 sort 排序

Algorithm`中的排序函数是基于快速排序算法实现的，时间复杂度为`O(N\*logN)

基本思想：通过一趟排序将待排序的数据分割成独立的两部分，左边部分的所有数据比右边部分的所有数据都要小，然后再按此方法对这两部分数据分别进行快速排序，整个排序过程可以递归进行，最后达到整个数据变成有序序列。

1 sort构成

```cpp
    sort(数组首指针，数组尾指针，排序规则);
    前两个位置都是填指针
    第三个位置可以不填，不填默认按由小到大排序
```

2 sort默认由小到大进行排序

```cpp
int arr[4]={1,3,2,4};
sort(arr,arr+4);
//排序结果是 1,2,3,4
```

3 自定义排序规则

```cpp
//通过自定义规则进行排序
bool map(int x1,int x2){
    return x1>x2;
}
int arr[5]={3,2,1,4,5};
sort(arr,arr+n,map);
//排序结果是 5,4,3,2,1
//规则的位置不仅可重载大小于号，还可以重载其他符号
```

### 3 二分查找

二分查找算法也称折半查找算法`Binary Search Algorithm`，它是一种效率较高的查找方法，复杂度为*O*(*l**o**g\*\*N*)。二分查找算法要求线性表（序列）必须采用顺序存储结构，而且表中元素按关键字有序排列。

核心思想：将表中间位置记录的关键字与待查找的关键字进行比较，如果两者相等，则查找成功；否则利用中间位置记录将表分成前、后两个子表，如果中间位置记录的关键字大于查找关键字，则进一步查找前一子表，否则进一步查找后一子表。重复以上过程，直到找到满足条件的记录，使查找成功，或直到子表不存在为止，此时查找不成功。

#### 1 binary\_search

```cpp
int arr[4] = {1,2,3,4}; // 升序数组
bool judge1 = binary_search(arr, arr+4, 1); // judge1结果为true
bool judge2 = binary_search(arr, arr+4, 5); // judge2结果为false
//binary_search利用的也是指针
```

#### 2 lower\_bound

在有序序列里查找不小于关键字的元素，并返回元素索引位置最低的地址，最后根据地址来判断是否查找成功

模板函数`lower_bound`的基本用途是查找有序区间中第一个大于或等于某给定值的元素的位置

通常最后利用减去数组首地址得到一个数组下标

例：

```cpp
int arr[5] = {1,2,4,5,6};
int c = lower_bound(arr,arr+5,4)-arr;
// c的值就是2
```

#### 3 upper\_bound

模板函数upper\_bound的基本用途与lower\_bound相对，是查找有序区间中第一个大于某给定值的元素的位置，由此本关卡的任务可以利用upper\_bound获取序列中第一个大于待查找关键字的元素的位置

```cpp
int arr[5] = {1,2,4,5,6};
int c = upper_bound(arr,arr+5,4)-arr;
// c 的值是 3 
```

#### 4 equal\_range

模板函数equal\_range综合了lower\_bound`和`upper\_bound的功能，通过内部调用这两个上下界查找函数，返回两个地址并组成pair：第一个地址是序列中第一个大于等于待查找关键字的元素位置，而第二个地址是第一个大于待查找关键字的元素位置。

```cpp
int arr[5] = {1,2,2,4,5};
auto bounds = equal_range(arr, arr+5, 2);//auto是自动判断定义类型
int a = bounds.first-arr; // a结果为1
int b = bounds.second-arr; // b结果为3
```

### 4 copy 复制

#### 1 copy

复制整个数组到新的数组中

```cpp
int arr1[4] = {1,3,2,4};
int arr2[4];
copy(arr1, arr1+4, arr2);
```

#### 2 copy\_n

是可选择的复制前`n`个元素到新的数组

```cpp
int arr1[4] = {1,3,2,4};
int arr2[4];
copy_n(arr1, 4, arr2);
```

### 5 swap 交换

```cpp
int a = 1,b = 2;
swap(a,b);
//此时  a = 2, b = 1
```

### 6 replace 取代

`Algorithm`中取代模板函数为`replace`，传入参数`first`为数组首地址，参数`last`为数组尾地址，要被替换的旧元素为参数`old_value`，替换的新的元素为参数`new_value`，函数功能是将数组中所有的`old_value`替换为`new_value`，其函数原型及其应用实例如下：

```cpp
int arr[4] = {1,2,2,3};
replace(arr, arr+4, 2, 0);
// arr结果为{1,0,0,3}
```

### 7 fill 填充

Algorithm`中填充模板函数为`fill`，传入参数`first`为数组首地址，参数`last`为数组尾地址，填充值为参数`val`，函数功能是将数组中的所有元素都重新赋值为`val

```cpp
int arr[4] = {1,2,2,3};
fill(arr, arr+4, 5);
// arr结果为{5,5,5,5}
```

特别的，类似的填充函数还有`memset`，在头文件`string.h`中，但是赋值有限，一般仅限于`-1`，`0`，若设置为其他数值，则实际结果有误。

### 8 reverse 翻转 倒置

`Algorithm`中倒置模板函数为`reverse`，传入参数`first`为数组首地址，参数`last`为数组尾地址，函数功能是将数组中的所有元素对称交换，例如比如`1 2 3`，变为`3 2 1`，其函数原型及其应用实例如下：

```cpp
int arr[4] = {1,2,3,4};
reverse(arr, arr+4);
// arr结果为{4,3,2,1}
```

### 9 rotate 滚动

`Algorithm`中滚动模板函数为`rotate`，传入参数`first`为数组首地址，参数`last`为数组尾地址，而参数`middle`则是数组中要滚动的最后一个元素的后一个地址，滚动完成后该地址将成为首地址，比如数组`arr=[0,1,2,3,4]`，相应的参数为`first=arr`，`last=arr+5`，`middle=arr+3`，则滚动后的结果为`3 4 0 1 2`，其函数原型及其应用实例如下：

```cpp
int arr[5] = {0,1,2,3,4};
rotate(arr, arr+3, arr+5);
\\ arr结果为{3,4,0,1,2}
```

### 10 无序查找

#### 1 无序数组查找指定元素 Find

二分查找`binary_search`不一样，`Algorithm`中的模板函数`find`可以在无序数组中的查找指定元素`x`，若存在则返回第一个`x`所在的地址，否则返回数组尾地址，其函数原型及其应用实例如下，其中传入参数`first`为数组首地址，参数`last`为数组尾地址，待查找指定元素为参数`val`：

```cpp
int arr[4] = {1,3,2,3};
int *p = find(arr, arr+4, 3); // p结果为地址arr+1
int *q = find(arr, arr+4, 0); // q结果为地址arr+4
```

#### 2 无序数组查找指定数组子序列 Find\_End

`Algorithm`中的模板函数`find_end`可以在无序数组`arr1`中的查找指定子数组`arr2`是否存在，若存在则返回待查子数组`arr2`最后出现在原数组`arr1`的地址，否则返回原数组的尾地址，例如`arr1=[0,3,4,3,4]`，`arr2=[3,4]`，返回结果为`arr1+3`，其函数原型及其应用实例如下：

```cpp
int arr[5] = {0,3,4,3,4};
int arr1[2] = {3,4};
int arr2[2] = {3,5};
int *p = find_end(arr, arr+5, arr1, arr1+2); // p结果为地址arr+3
int *q = find_end(arr, arr+5, arr2, arr2+2); // q结果为地址arr+5
```

特别的，如想要第一次出现的地址，模板函数`search`可以实现这一功能，使用方式同上

### 11 count 统计

`Algorithm`中的模板函数`count`可以在数组中统计指定元素`x`出现的次数，传入参数`first`为数组首地址，参数`last`为数组尾地址，参数`x`为待统计的指定元素，其函数原型及其应用实例如下：

```cpp
int arr[5] = {0,3,4,3,4};
int cnt = count(arr, arr+5,3); // cnt结果为2
```

### 12 比较 Equal

两个数组相等意思是数组个数相同，对应位置上的元素值也相同，`Algorithm`中的模板函数`equal`就可以比较两个数组是否相等，返回比较真值，

```cpp
int arr1[2] = {3,4};
int arr2[2] = {3,4};
int arr3[2] = {3,5};
bool judge1 = equal(arr1, arr1+2, arr2); // judge1结果为地址true
bool judge2 = equal(arr1, arr1+2, arr3); // judge2结果为地址false
```

### 13 merge 合并

合并函数的核心思想是设置两个头指针，分别指向两个升序数组首地址，通过比较两个头指针的大小，每次都将小的数值放入新的数组，然后小数值指针后移，最后新的数组也是有序的，从而完成合并过程，复杂度为*O*(*N*+*M*)。

```cpp
int arr1[3] = {1,2,3};
int arr2[4] = {2,3,4,5};
int arr3[7];
merge(arr1, arr1+n1, arr2, arr2+n2, arr3);
// arr3结果为{1,2,2,3,3,4,5}
```

### 14 includes 包含

无序数组的包含问题就像是字符串应用中的最长公共子序列，解法是动态规划，而有序数组的包含则是简单的判断问题，解法类似有序数组的合并。

同样的，其核心思想也是设置两个头指针分别指向两个升序数组，若指针指向的元素相等，则两个指针都往后移动，否则指向数组*a**r**r*1的指针往后移动，直到指针移向数组尾地址。

```cpp
int arr1[4] = {1,2,3,4};
int arr2[2] = {2,3};
bool judge(arr1, arr1+4, arr2, arr2+2);
// judge结果为true
```

### 15 集合相关函数

集合是由一个或多个确定的元素所构成的整体。集合中的元素有三个特征：

+   确定性（集合中的元素是确定的）；
+   互异性（集合中的元素是互不相同），例如：集合*A*\=(1,*a*)，则*a*不能等于1；
+   无序性：集合中的元素没有先后之分，如集合(3,4,5)和(3,5,4)算作同一个集合，

#### 1 set\_union 集合的并

集合*A*和集合*B*的并是由所有属于集合*A*或属于集合*B*的元素所组成的集合，记作*A*⋃*B* 或者*B*⋃*A*，并集的一个重要属性就是越并越多。假定集合*A*\=(1,2,3,4,5,6,7)，集合*B*\=(6,7,8,9)，那么集合*A*和集合*B*的并集为*A*⋃*B*\=(1,2,3,4,5,6,7,8,9)。

`Algorithm`算法模板中集成了集合的并操作，函数名称为`set_union`，其作用是将两个集合合并成一个集合，但是要求输入的两个集合必须是有序的，这看似违背了集合的定义，但是有序的目的是为了让求并的过程实现起来变得简单。

```cpp
int arr1[3]={1,2,3};
int arr2[3]={2,3,4};
int arr3[4];
int n = set_union(arr1, arr1+n1, arr2, arr2+n2, arr3)-arr3;
// arr3结果为{1,2,3,4}
// n结果为4
```

#### 2 集合的交 set\_intersection

集合*A*和*B*的交是由所有属于集合*A*以及属于集合*B*的元素所组成的集合，记作*A*∩*B*或者*B*∩*A*，交集的一个重要属性就是越交越少。假定集合*A*\=(1,2,3,4,5,6,7)，集合*B*\=(6,7,8,9)，那么集合*A*和集合*B*的交集为*A*∩*B*\=(6,7)。

`Algorithm`算法模板中集成了集合的交操作，函数名称为`set_intersection`，其作用是将两个集合交成一个集合，同样的要求输入的两个集合必须是有序的。因此，首先需要将两个集合排序，然后才调用`set_intersection`函数计算出交集。其函数原型及应用实例如下，输入参数是两个集合的首尾地址以及一个保存交集结果的数组的首地址，最后返回数组尾地址：

```cpp
int arr1[3]={1,2,3};
int arr2[3]={2,3,4};
int arr3[2];
int n = set_intersection(arr1, arr1+n1, arr2, arr2+n2, arr3)-arr3;
// arr3结果为{2,3}
// n结果为2
```

#### 3 集合相对差集 `set_difference`

集合差集也叫集合补集，是一个相对的定义：由属于*A*而不属于*B*的元素组成的集合，称为*B*关于*A*的相对补集，记作*A*−*B*。例如集合*A*\=(1,2,3,4)，集合*B*\=(3,4,5,6)，那么集合*A*−*B*\=(1,2)。

`Algorithm`算法模板中集成了集合的差操作，函数名称为`set_difference`，其作用是计算出两个集合的差集，

```cpp
int arr1[4]={1,2,3,4};
int arr2[4]={3,4,5,6};
int arr3[4];
int n = set_difference(arr1, arr1+n1, arr2, arr2+n2, arr3)-arr3;
// arr3结果为{1,2}
// n结果为2
```

#### 4 集合对称差集

集合*A*与集合B的对称差集定义为属于集合*A*与集合*B*，但不属于它们交集*A*∩*B*的元素集合，记为*A*△*B*。也就是说*A*△*B*\=*x*∣*x*∈*A*∪*B*且*x*∈/*A*∩*B*，即*A*△*B*\=(*A*∪*B*)—(*A*∩*B*)。同样也可以用相对差集的并来表示*A*△*B*\=(*A*—*B*)∪(*B*—*A*)。例如上述的两个集合，他们的对称差集为*A*△*B*\=(1,2,5,6)。

`Algorithm`算法模板中集成了集合对称差集的操作，函数名称为`set_symmetric_difference`，其作用是计算出两个集合的对称差集，同样的，要求输入的两个集合必须是有序的。

```cpp
int arr1[4]={1,2,3,4};
int arr2[4]={3,4,5,6};
int arr3[8];
int n = set_symmetric_difference(arr1, arr1+n1, arr2, arr2+n2, arr3)-arr3;
// arr3结果为{1,2,5,6}
// n结果为4
```

### 14 字典序函数

#### 1 序列排列

一般地，从*n*个不同元素中取出*m*个元素，按照一定的顺序排成一列，叫做从*n*个元素中取出*m*个元素的一个排列`permutation`。特别地，当*m*\=*n*时，这个排列被称作全排列`all permutation`，本关卡就是关于*n*的全排列问题。

#### 2 顺序 next\_permutation

给定一个排列序列，`Algorithm`中的模板函数`next_permutation`可以产生该排列的下一个序列，输入参数为序列的首地址和尾地址，

```cpp
do{
       for(int i=0;i<n;i++)cout<<arr[i]<<' ';
       next_permutation(arr,arr+n);
       i++;
   }while(i<m);
```

3 逆序 prev\_permutation

给定一个排列序列，`Algorithm`中的模板函数`prev_permutation`可以产生该排列的上一个序列，输入参数为序列的首地址和尾地址，

```cpp
do{
       for(int i=0;i<n;i++)cout<<arr[i]<<' ';
       prev_permutation(arr,arr+n);
       i++;
   }while(i<m);
```