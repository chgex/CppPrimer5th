ch02 Variables and Basic Types

变量和基本类型

---

#### 基本内置类型

C++基本数据类型：

1. 算术类型（arithmetic type）：
   1. 整型（integral type），包括字符和布尔值类型；
   2. 浮点型（float）。
2. 空类型（void）



字面值常量（literal）

每个字面值常量都对应了一种数据类型，字面值常量的形式和值决定了它的数据类型。

整型字面值：

整型字面值可以写作十进制数，八进制数，或十六进制数。整型字面值20的三种表示方法：

+ 十进制：20；

+ 以0开头的整数表示八进制： 024；
+ 以0x开头的整数表示十六进制：0x14

浮点型字面值：

表现为一个小数或科学计数法表示的知指数（指数部分用 e 或 E 标识）。默认浮点型字面值是 double 类型。实例：

+ 3.14159
+ 3.14159E0

字符和字符串字面值

由单引号括起来的一个字符称为 char 型字面值，由双引号括起来的零个或多个字符叫做字符串字面值。示例：

+ 字符字面值：'a', 'b'
+ 字符串字面值："a", "hello world", ""

字符串字面值实际上是以一个数组。编译器在每个字符串的末尾会添加一个空字符`'\0'`，所以，字符串字面值的实际长度比内容多1。

转义序列

有两类字符不能直接使用：

+ 不可打印字符（如退格）
+ C++语言中有特殊含义的字符（’，", ?, /）。

如果要使用这些字符，需要以转义序列的方式，其实就是加上`\`，以下是C++中常用的转义序列：

| -      | -    | -          | -    |
| ------ | ---- | ---------- | ---- |
| 换行符 | \n   | 横向制表符 | \t   |
| 回车符 | \r   | 纵向制表符 | \v   |
| 退格符 | \b   |            |      |
| 其它： | `\"` | `\'`       | `\?` |





#### 复合类型

C++中的复合类型指基于其它类型定义的类型。比如：引用和指针。

引用（reference）就是给对象起了另外一个名字。以下是一个引用的示例：

```c++
int val=1024;
int &ref1=val; // 引用 ref1 指向 val

int &ref2; //error, 引用必须被初始化

int &ref3=ref2; // ref2和ref3，都绑定在val

int i=ref1 //定价于 int i = val;
```

在定义引用的时候，必须初始化这个引用。初始化引用就是将引用和它的初始值相绑定（bind），而不是拷贝初始值。

+ 一旦引用的初始化完成，引用就会和它的初始值对象绑定，所以之后将无法重新绑定到另一对象。

+ 引用并不是对象，它只是一个已经存在的对象的别名。

+ 引用本身不是对象，所以不能定义引用的引用。

+ 定义引用之后，对引用的所有操作，都直接作用在与之绑定的对象上。



指针（pointer）

指针是指向另一种类型的复合类型。

+ 指针本身就是一个对象，
+ 允许对指针进行赋值和拷贝，指针在之后还可以指向不同的对象，
+ 指针在定义时，可以不用赋值，此时它拥有一个不确定值。



指针值（就是地址）

指针的值，即地址，属于以下四种状态之一：

+ 指向一个对象
+ 指向对象所占空间的下一个位置；
+ 空指针，即没有指向任何对象；
+ 无效指针，即其它情况。



使用指针，获取对象地址（使用取地址符&）：

```c++
int val = 1024;
int *p = &val;

cout << p; //地址
```



使用指针访问对象（利用解引用符\*)

```c++
int val = 1024;

int *p = &val; //p 存放变量 val 的地址，或者说 p 是指向变量 val 的指针 
//等价于 int *p; p=&val;

cout << *p; // 1024。由* 得到指针p 所指对象
```



符号`&`和`*`的多重含义

+ `int i=42`
+ `int &r = i`，& 是引用申明，r 是一个引用；
+ `int *p` ，p 是一个指针；
+ `p = &i`，& 是取地址符；
+ `*p = i`，* 是解引用符；
+ `int &r2= *p`，& 是引用申明，* 是解引用符。

一个示例：

```c++
#include<iostream>
using namespace std;

int main(){
    int val=42;
    
    int *p;
    p = &val;
    //以上两句等价于一句：int *p = &val;
    cout<< p << endl; //地址
    cout<< *p << endl; //42
    
    int *q;
    *q = val;
    cout<< q << endl; //地址
    cout<< *q << endl; //42
    return 0;
}
```



空指针（null pointer）

空指针不指向任何对象，以下是3种定义空指针的方式：

+ `int *p = nullptr;`
+ `int *p = 0; //必须是0`
+ `int *p = NULL; // 等价于int *p = 0`.



#### const 限定符

const限定符修饰的变量定义，该变量的值不能被改变。示例：

​	`const int buffSize=512; `

于是 buffSize 就变成了常量，任何对该值的修改，都会出错。



const 对象必须在定义时就初始化：

+ `const int i = get_size();`，正确：运行时初始化；
+ `const int j =42;`，正确：编译时初始化；
+ `const int k;` ，错误：k 必须有初始化。 



const 常量只要不是修改值，其它操作：去初始化另一个非const 对象，或 被另一个非const 对象初始化都可以：

```c++
int i = 42;

const int c=i; // ok, i的值被拷贝给了c

int j =c; // ok, c的值被拷贝给了j
```



多个文件之间共享 const 变量

const 对象只在该文件内有效。如果程序包含多个文件，一个const对象需要在多个文件共享时，可以使用 extern 关键字，只需要在一个文件中定义并初始化，在其它文件中只需要定义，就可以做到多个文件共享该const常量了。示例：

```c++
//file_1.cc，定义并初始化一个const常量，该常量可以被其它文件访问到
extern const int bufSize=fcn();

//file_1.h
extern const int bufSize; //与file_1.cc 中 的 bufSize 是同一个
```



const的引用

可以将引用绑定到 const 对象上，叫做对常量的引用（reference to const）。但是对常量的引用，无法用它来修改绑定的对象。示例：

```c++
const int val = 1024;
const int &ref = val; //ok, ref 也是常量

ref = 512; // error, ref 是对常量的引用，所以是不能修改的
int &ref2= val; // error, ref2是非常量引用，所以不能指向一个常量对象
```



const 和指针

和引用一样，指针也可以指向常量或非常量。就像常量的引用一样，指向常量的指针，也不能改变所指对象的值了。

要想存放常量对象的地址，只能用指向常量的指针。示例：

```c++
const double pi=3.14; //pi 是常量，值不能改变

double *ptr = &pi; // error,
const double *ptr = &pi; // ok,

*ptr = 42; //error,不能改变值
```



const 指针

可以将指针定为常量，即常量指针（const pointer）。常量指针必须初始化，而且初始化之后，它的值（就是那个存放在指针中的地址）不能再修改了。这里需要区分的是，常量指针定义后，本身的值是不变的，但是这个指针指向的值是可以发生变化的。示例：

```c++
const double pi=3.14;
const double *const p=&pi; //p 是指向常量对象的常量指针
*p= 3.10 // error,不能修改常量的值

int errNum=0;
int *const curErr=&errNum; // 常量指针 curErr将一直指向errNum
*curErr=0; //ok, errNum 为非常量整数，可以使用常量指针来修改它的值
```



顶层const

两个名词：

+ 顶层const（top-level const），表示指针本身是一个常量；
+ 底层const（low-level const），表示指针所指对象是一个常量。

换句话说就是，一个指针本身是常量，则定义这个指针的cons是顶层const，如果一个指针所指的对象是常量，则这个指针的const是顶层const。

示例：

```c++
int val =0; 
int *const p1 = &val; // 顶层const,不能改变p1的值（但可以改变val的值）。指针本身就是一个常量


const int c= 42;
const int *p2 = &c;  //底层const，可以改变p2的值


const int *const p3= p2; //靠右的是顶层const， 靠左的是底层const。因为指针本身是常量，指针所指也是常量。
```
