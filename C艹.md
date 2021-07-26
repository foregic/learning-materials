
# C++

[TOC]



## 函数

### 占位符参数

```c++
void f(int i, int = 0, float = 1.1); //version 1
void f(int i, int, float flt);		 // version 2
```



## 指针

### int型



#### 数组指针


数组指针本质是指针，指向的内容是数组

```C++
int *p;//int型指针
int array[]={1,2,3,4,5,6};//int型数组，长度为6；
p=array;//合法，array此时是array数组的首地址，类型为int*可以赋值给int*
```
#### 指针数组

指针数组本质是数组，存储的元素是指针

```C++
int main(){
	int *array[3];//指针数组，数组里每个元素都是一个指针
	int a = 1, b = 2, c = 3;
	array[0] = &a;
	array[1] = &b;
	int **p;//二级指针
	p = array;
	//int *p,p=array，会报错，说不能将int**型的值分配到int*类型的实体
	
	std::cout << &p << '\n';		//0x61fdf0,输出二级指针p的地址
	std::cout << *p << '\n';		//0x61fdfc,输出p指向地址的内容，二级指针，所以指向地址的内容也是一个地址
	std::cout << p << '\n';			//0x61fe00,输出p指向的地址
	std::cout << &array << '\n';	//0x61fe00，指针数组的首地址
	std::cout << *array << '\n';	//0x61fdfc，指针数组首位指向的地址，这里的array是一个int**型的指针，所以取其指向地址的内容是int*型指针所指向的地址，也就是array[0]
	std::cout << array << '\n';		//0x61fe00，array指向数组首元素的地址
	std::cout << &array[0] << '\n'; //0x61fe00，指针数组首位元素的地址
	std::cout << *array[0] << '\n'; //1，指针数组首位元素指向地址的内容
	std::cout << array[0] << '\n';	//0x61fdfc，指针数组首位元素指向的地址
	std::cout << &a << '\n';		//0x61fdfc，a的地址
	
	for (int i = 0; i < 2; i++)
	{
		std::cout << *p[i] << '\n';//输出1，2
	}
}
```
#### 指针函数

是一个返回值为指针的函数

> 声明格式：
>
> ```c++
> int*func(int x,int y)//返回值是一个int型指针，是一个地址
> ```

#### 函数指针

本质是一个指针变量，该指针指向一个函数的地址

> 声明格式：
> ~~~c++
> int (*func)(int x,int y)
> func=&Function;
> func= Function;
> ~~~
> 调用格式：
> ~~~C++
> x=(*func)();
> x=   func();
> ~~~





## 字符串

```C++
#include <iostream>
#include <cstring>

int main()
{

	char array[] = "abcde";
	std::cout << sizeof(array) << '\n'; //6,末尾有个'\000'
	std::cout << strlen(array) << '\n'; //5,字符串长度
	std::cout << array << '\n';			//abcde

	std::string s = "123";
	strcpy(array, &s[0]);

	std::cout << sizeof(array) << '\n'; //6,数组空间没变
	std::cout << strlen(array) << '\n'; //3,拷贝了'123\000',所以d的位置被'\000'替代
	std::cout << array << '\n';			//123,识别到'\000'，结束输出

	for (int i = 0; i < 5; i++)
	{
		std::cout << array[i] << ','; //1,2,3, ,e,
	}

	return 0;
}

```


## 关键字
### const

> 1. const修饰指针正指向的对象 const int* a;
> 2. const修饰在指针中的地址 int* const a;

#### const和指针结合

* 常量指针`const int*p` `int const*p`

>  指针可以指向其他地址，但是内容不可以改变
> ```C++
>int a = 10, b = 20;
>const int *p = &a;
>p = &b; // 指针可以指向其他地址，但是内容不可以改变
>```

* 指针常量` int *const p`

> 指针指向的地址是一定的，但是内容可以更改
>
> ```c++
> int a = 10, b = 20;
> int *const p = &a;
> *p = 30; // p指向的地址是一定的，但其内容可以修改
> ```



#### 使用

> ```c++
> int main()
> {
> 	int m = 10;
> 	const int n = 20; // 必须在定义的同时初始化
> 
> 	const int *ptr1 = &m; // 指针指向的内容不可改变
> 	int *const ptr2 = &m; // 指针不可以指向其他的地方
> 
> 	ptr1 = &n; // 正确
> 	ptr2 = &n; // 错误，ptr2不能指向其他地方
> 
> 	*ptr1 = 3; // 错误，ptr1不能改变指针内容
> 	*ptr2 = 4; // 正确
> 
> 	int *ptr3 = &n;		  // 错误，常量地址不能初始化普通指针吗，常量地址只能赋值给常量指针
> 	const int *ptr4 = &n; // 正确，常量地址初始化常量指针
> 
> 	int *const ptr5; // 错误，指针常量定义时必须初始化
> 	ptr5 = &m;		 // 错误，指针常量不能在定义后赋值
> 
> 	const int *const ptr6 = &m; // 指向“常量”的指针常量，具有常量指针和指针常量的特点，指针内容不能改变，也不能指向其他地方，定义同时要进行初始化
> 	*ptr6 = 5;					// 错误，不能改变指针内容
> 	ptr6 = &n;					// 错误，不能指向其他地方
> 
> 	const int *ptr7; // 正确
> 	ptr7 = &m;		 // 正确
> 
> 	int *const ptr8 = &n;
> 	*ptr8 = 8;
> 
> 	return 0;
> }
> ```



```c++
// 类
class A
{
private:
	const int a; // 常对象成员，可以使用初始化列表或者类内初始化
public:
	// 构造函数
	A() : a(0) { };
	A(int x) : a(x) { }; // 初始化列表
    


	// const可用于对重载函数的区分
	int getValue(); // 普通成员函数static
	int getValue() const; // 常成员函数，不得修改类中的任何数据成员的值
};
void function()
{
	// 对象
	A b; // 普通对象，可以调用全部成员函数
	const A a; // 常对象，只能调用常成员函数
	const A *p = &a; // 指针变量，指向常对象
	const A &q = a; // 指向常对象的引用
	// 指针
	char greeting[] = "Hello";
	char* p1 = greeting; // 指针变量，指向字符数组变量
	const char* p2 = greeting; // 指针变量，指向字符数组常量（const 后面是 char，说明指向的字符（char）不可改变）
	char* const p3 = greeting; // 自身是常量的指针，指向字符数组变量（const 后面是 p3，说明 p3 指针自身不可改变）
	const char* const p4 = greeting; // 自身是常量的指针，指向字符数组常量
}
// 函数
void function1(const int Var); // 传递过来的参数在函数内不可变
void function2(const char* Var); // 参数指针所指内容为常量
void function3(char* const Var); // 参数指针为常量
void function4(const int& Var); // 引用参数在函数内为常量
// 函数返回值
const int function5(); // 返回一个常数
const int* function6(); // 返回一个指向常量的指针变量，使用：const int*p = function6();
int* const function7(); // 返回一个指向变量的常指针，使用：int* constp = function7();
```
### static

static变量优点在于在函数范围外它是不可用的

当static用于函数名和函数外部变量时，意味着在文件外部不可以使用该名字，即拥有文件作用域。
### enum

枚举变量。

C++中不允许枚举变量自加操作如`a++`(a为`color`型枚举)，因为在这个操作中`a++`做了两次类型转换。首先是color型枚举转换为`int`，然后自加1之后再将该值转换为`color`第二次转换是非法的。

### extern

### #define



### new和malloc

写法

~~~c++
int *p1 = new int;
	int *p2 = new int(1024);
	int *p3 = (int *)malloc(sizeof(int));
~~~

|                          new                          |                  malloc                  |
| :---------------------------------------------------: | :--------------------------------------: |
|               new/delete是C++**关键字**               | malloc/freee是**库函数**，需要包含csdlib |
|                 new无需指定内存块大小                 |       malloc需要指定需要的内存大小       |
|        new从**自由存储区**上为对象动态分配内存        |      malloc 从**堆**上动态分配内存       |
|              C++允许重载new/delete操作符              |             malloc不允许重载             |
| new不止分配内存还会调用构造函数，delete会调用析构函数 |   malloc仅分配内存不会对对象进行初始化   |
|          new分配内存失败会返回bad_alloc异常           |           malloc失败会返回NULL           |

函数原型

```c++
void *__CRTDECL operator new(size_t _Size);
void *__cdecl malloc(_In_ _CRT_GUARDOVERFLOW size_t _Size);
```





### struct

####  struct和class区别





## 类

protect

对类内为protect，不可以直接访问。

可以被子类继承，同时子类继承来的也是protect。

在保密上，private最私密，protect其次，public最开放。private里的内容不能被继承和直接访问。

### 数据成员
### 构造函数
构造函数的本质是对象的初始化

派生类的对象初始化会先调用基类的构造函数再调用派生类的构造函数。

无论子类是否有自己的构造函数，创建子类对象时都会调用父类的构造函数。子类是不能继承父类的构造函数，只能调用父类的构造函数。

### 析构函数
### 拷贝构造函数
### 成员函数



### 常成员函数
形式——返回值类型 方法名（参数表）const

1. 常成员函数不能改变对象的值，也不能在函数体里面调非常成员函数。

2. 常对象可以调常成员函数和静态成员函数

3. 常成员函数实现的时候，函数头是应该和声明的函数头一致的，const也是函数类型的一个组成部分，而对于友元函数和静态成员函数是不用friend和static的

4. 非常对象也可以调用常成员函数，但是当常成员函数与非常成员函数同名的时候（可以视为函数重载），对于非常对象是会优先调用非常成员函数的。

如：
​```c++
class Student
{
public:
	int age;
	string name;
	bool read() const;//后面加了const，表示这个函数只读不写
	bool set(int i);//成员函数声明，又叫方法
};

bool Student::read() const {}//实现时也要加const
```
实际也可以不使用const，普通方法也可以读取数据，加const只是为了安全起见防止意外修改数据

### 友元

使用`friend`关键字可以访问类内的私有和保护数据成员和成员函数。

### 常数据成员
形式——const 数据类型 变量名；

1. 常数据成员的初始化只能通过构造函数的列表来完成
2. 但是静态的常数据成员必须在类外进行初始化，不能在构造函数中进行，并且const不能省
3. 如果类有多个重载构造函数，就应该在每一个重载构造函数的初始化列表中去初始化常数据成员，当然刚才说的2.除外。


### static关键字
#### 静态数据成员

定义：描述全局，又与某个对象无关的，叫做静态数据成员

#### 静态成员函数
定义：读取静态数据成员的叫静态成员函数

### 纯虚函数

​```C++
class Student//抽象类，至少声明一个函数为纯虚函数
{
public:
    virtual void read()=0;//纯虚函数，只有声明没有定义，依靠子类来多态性来实现，具体函数内容需要子类来实现。
};
```



### 多态

重载时编译时决定，虚函数表是运行时决定。早绑定与晚绑定。

#### 虚函数表





## 重载

不同函数可以具有相同的函数名，通过不同输入参数来识别。

重载两个函数的参数格式必须是不一样的。

重载不依赖于对象，依赖于编译器。

