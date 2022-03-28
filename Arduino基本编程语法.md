# Arduino基本编程语法

> API :应用程序编程接口（Application Programming Interfece) 用来驱动硬件设备

## Arduino程序结构

```c
void setup() {
    //输入的代码只运行一次，一般用来作为初始值的设定
}

void loop() {
    //这里的代码则会不断地循环重复运行
}
```

Arduino的程序结构基本上分为这两部分

控制器通电或者复位之后，就会开始执行setup()函数中的程序，只会执行一次。在该函数中，常常用于端口串口**初始化设置**以及**I/O状态**。

当该函数执行完之后，接着执行loop()函数中的程序，这是一个死循环，会**不断重复**的执行。一般用于完成程序的主要功能，驱动各种模块，采集数据。

## C语言在其中的应用

### 数据类型 

#### 变量

```c
//类型 变量名；
int x = 10; //与c中大差不差
```

#### 常量

1. 直接在程序运行中定义

```c
//const 类型 常量名 = 常量值；
const int m = 10;
```

2. 使用宏定义

```c
//#define 宏名 值
#define password "1124531"
```

#### 整型

int / unsigned int / long / usigned long / short

#### 浮点型

float / double (<u>计算较慢并且存在误差，通常会转换成整型之后再进行相关的运算</u>)

#### 字符型

char

#### 字符串

1. 用数组进行存放

   ```c
   //char 字符串名称[字符个数]
   ```

2. 利用字符型String直接来定义字符串

   ```c
   String A;
   A = "Arduino";
   // String A = "Arduino";
   ```

   

#### 布尔型

boolean 只有真假两个值，分别对应1和0

### 语句结构

循环结构/选择结构/顺序结构

（while循环, do_while循环, for循环）（switch_case语句， if语句）

注意：此处的elseif 是 else if ，中间有空格

