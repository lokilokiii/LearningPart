

# 函数指针

在程序中看到`qsort()`函数，发现了一个叫做函数指针的东西，愣是一下子没搞清楚这个东西，那么就想通过这个`qsort()`函数，在了解使用这个函数的基础上，来了解一下函数指针:v:

#  1.0 qsort()

这是一个用来进行排序的函数，利用快速排序方法进行排序

快速排序：首先，把数组分成两部分，一部分的值都小于另一部分的值，这个过程一直持续到数组完全排序好为止

原型如下：

```c
void qsort(void *base, size_t nmemb, size_t size, int(*compar)(const void *, const void *))
```

1. 第一个参数是指针，指向**待排序数组的首元素**（可以应用任何类型的数组）

   void指针使数组中每个元素大小未知，因此适用于任何类型数组

2. 第二个参数是待排序项的数量 size_t 全称size type，“一种用来记录大小的数据类型”

3. 第三个参数，知名待排序数组中每个元素的大小 ，例如`sizeof(double)`等

4. 第四个参数是一个指向函数的指针，根据排序类型的不同，返回值为1，-1，0



## 1.1 对于compar参数

这里的compar参数就是一种函数指针，这里使用函数名来表示函数地址，与数组名直接存放数组中首元素地址的原理类似

到底什么是函数指针！在后面再解释:alien:

下面为具体的**compar函数**的编写方式：

* 如果返回值<0，那么p1所指向元素会被排在p2所指向元素的**前面**
* 如果返回值=0，那么p1所指向元素与p2所指向元素的顺序不一定
* 如果返回值>0，那么p1所指向元素会被排在p2所指向元素的**后面**

具体编写方式：

   如果要进行**升序排序**(以下以数据为double类型为例)

```c
//升序排序
int compar (const void *p1; const void *p2)
{
    const double *a1 = (const double *)p1;
    const double *a2 = (const double *)p2;
    if(*a1 < *a2) return -1;
    if(*a1 == *a2) return 0;
    if(*a1 > *a2)  return 1;
        
}

//降序排序
int compar (const void *p1; const void *p2)
{
    const double *a1 = (const double *)p1;
    const double *a2 = (const double *)p2;
    if(*a1 < *a2) return -1;
    if(*a1 == *a2) return 0;
    if(*a1 > *a2)  return 1;
        
}
```

或者,直接返回两个值相减

```c
//升序排序
int compar (const void *p1; const void *p2)
{
    return (*(int*)a - *(int*)b);
}

//降序排序
int compar (const void *p1; const void *p2)
{
    return (*(int*)b - *(int*)a);
}
```



## 1.2 排序

利用qsort()函数进行快速排序，下面就是几个例子，对**不同类型的数据**进行排序，以及一些注意事项

### 1.2.1 整数型排序

```c
#include <stdio.h>
#include <stdlib.h>

int values[] = { 40, 10, 100, 90, 20, 25};  //随便定义一组整数型数组

int compar(const void *a, const void *b);  //不要忘记compar函数的声明

int main()
{
    qsort(values, sizeof(values) / sizeof(values[0]), sizeof(int), compar); //使用qsort()
    //排序好后输出
    for(int i=0; i< sizeof(values)/sizeof(values[0]); i++)  
    {
        printf("%d\n", values[i]);
    }
}

int compar(const void *a, const void *b)
{
    return (*(int*)a - *(int*)b);  //此处为升序，若逆序 return (*(int*)b - *(int*)b);
}

```

程序运行结果：![qsort函数整数型排序](D:\科协学习报告\c\2021.11.14c语言字符串和字符串函数\qsort函数整数型排序.png)



### 1.2.2 结构体排序

**注意**：

* 结构体中数据类型多，明确想要排序的数据
* compar返回类型是int，注意类型的转换

```c
#define _CRT_SECUR_NO_WARNINGS
#include <stdio.h>
#include <stdlib.h>

typedef struct        
{
    char name[30];  //存放名字字符
    int Chinese;
    int Math;
    int English;
}st;

int compare( const void* a, const void* b);

int main()
{
    //结构体数组
    st students[7] = {
        {"周",97,68,45},
        {"吴",100,32,88},
        {"郑",78,88,78},
        {"王",87,90,89},
        {"赵",87,77,66},
        {"钱",59,68,98},
        {"孙",62,73,89}
    };
    qsort(students, 7, sizeof(st), compare); //已知7人，使用sizeof来获取st的大小

    for(int i=0; i<7; i++){
            //对总分进行排名
        printf("%s%4d%4d%4d\t", students[i].name, students[i].Chinese, students[i].Math, 	students[i].English);
        printf("总分：%d\n", students[i].Chinese+students[i].Math+students[i].English);
    }
    return 0;

}

//此处说明参与排序的是每个人的总分
int compare( const void* a, const void* b) 
{
    st *pa = (st*)a;  //强制数据转换，将a b转换为指向结构体的指针
    st *pb = (st*)b;
    int num1 = pa->Chinese + pa->English + pa->Math;  //计算总分
    int num2 = pb->Chinese + pb->English + pb->Math;
    return num1 - num2;
}

```

输出结果如下：

![qsort()结构体排序](D:\科协学习报告\c\2021.11.14c语言字符串和字符串函数\qsort()结构体排序.png)



### 1.2.3 字符串指针数组排序

这里是指针数组的排序比较，而非字符串数组

**注意**：

* 因为是指针数组，故在compar函数中，要将参数强制转换成指针的指针`char **` 
* 在qsort参数中，注意数组中每个元素大小就是数组中存放的指针的大小

```c
#include<stdio.h>
#include<string.h>
#include<stdlib.h>

int compar(const void* arg1, const void* agr2);

int main()
{
	int i;
	char* arr[5] = { "i", "love", "c", "programming", "language" }; //字符串指针数组，数组元素中存放的是地址
	
	qsort(arr, sizeof(arr) / sizeof(arr[1]), sizeof(char *), compar); //sizeof(char *)数据类型对应

    //排序后输出
	for (int i = 0; i < sizeof(arr) / sizeof(arr[1]); i++) {
		printf("%s\n", arr[i]);
	}
	return 0;
}

int compar(const void* arg1, const void* arg2) {
	char* a =  *(char**) arg1;    //指向指针的指针，利用两个星号来定义
	char* b = *(char**)arg2;
	int result = strcmp(a, b);
	if (result > 0) {
		return 1;
	}
	else if (result < 0) {
		return -1;
	}
	else return 0;
}
```

输出结果如下：

![qsort()字符串指针数组排序输出结果](D:\科协学习报告\c\2021.11.14c语言字符串和字符串函数\qsort()字符串指针数组排序输出结果.png)



****



# 2.0 函数指针

以上文中的`qsort()`函数为例，这个函数可以处理任意类型的数组，但是要明确利用哪一种函数来比较元素（升序还是降序）

为此，`qsort()`函数中有一个参数，接受**函数指针`compar()`** 

**什么是函数指针？**:alien:

1. **函数具有地址**：函数的机器语言实现由载入内存的代码组成

2. 指向函数的指针中存放着**函数代码的起始处地址**

3. 在声明一个函数指针时，声明指针指向的**函数类型**，函数类型由函数本身的返回类型和参类型数共同描述

   > 例如：
   >
   > 对于函数 `void sum_total(int *) ` 而言
   >
   > `sum_total()`函数的类型是 带`int*`类型参数，返回类型为void的函数



明确以上几点之后，再定义一个函数指针，就非常直观了:running_man:

```c
void sum_total(int *);//声明一个函数

void (*pf)(int *);  //pf是一个指向函数的指针

pf = sum_total;     //pf指向sum_total函数
```

以上为定义一个函数指针，并且进行**赋值**之后，在定义一个   `int array[] = {1, 2, 3};`的基础上

> `(*pf)(array)`
>
> 将`sum_total`作用在array上

==可以直接利用函数名作为该函数的地址，原理同数组==



## 2.1 函数指针数组

顾名思义，函数指针数组就是**存放函数指针的数组**，本质是**指针数组**，其中的指针指向函数

通过这样的数组，可以更清晰简单地编写，例如计算器等，利用大量返回值和参数相同的函数的问题了。



例如，通过函数指针数组编写**计算器**

```c
#include<stdio.h>
#include<stdlib.h>
 //加法
int Add(int a, int b) 
{
    return a + b;
}
//减法
int Sub(int a, int b)
{
    return a - b;
}
//除法和乘法类似，这里就不举例了了
int main()
{
    int (* alth[4])(int, int) = {Add, Sub, Div, Mul}; //这里特别注意定义数组的时候的括号，不要加错啦
    int n, m;
    scanf("%d %d", &n, &m);
    for (int i = 0; i < 2; i++)
    {
        printf("%d\n", (* alth[i])(m, n));//注意参数放置的位置
    }
    return 0;
}
```

这里要注意的点主要有以下几个：

* 函数指针数组，主要功能是为了方便使用**返回值和参数相同的函数**
* 在定义函数指针数组的时候注意其**定义方式** `DataType (* Name[num])(DateType_1, DataType_2..)` 



:star:在这样的函数指针背景下，同时又有一个新的概念：**回调函数**

即， 把一个函数的地址进行传参，最后通过地址来调用函数，在文章最开始，其实就是通过`qsort()` 这个回调函数来引入今天的主题的
