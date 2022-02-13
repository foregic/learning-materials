
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


```c++
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

```c++
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

## C++核心编程

### 内存分区模型

|        |                                                              |
| :----: | :----------------------------------------------------------: |
| 代码区 |          存放函数体的二进制代码，由操作系统进行管理          |
| 全局区 |               存放全局变量区和静态变量以及常量               |
|  栈区  |      由编译器自动分配释放，存放函数的参数值，局部变量等      |
|  堆区  | 由程序员分配和释放，若程序员不释放，程序结束时由操作系统回收 |

### 代码区

代码区存放 CPU 执行的机器指令
代码区是共享的，共享的目的是对于频繁被执行的程序，只需要在内存中有一份代码
代码区是只读的，只读是为了防止程序意外修改了它的指令

### 全局区

全局区存放全局变量和静态变量
还包括常量区、字符串常量和其他常量
该区域的数据在程序结束后由操作系统释放
全局`const`修饰的变量

`const`修饰的局部变量不在全局区(局部常量)

### 栈区

数据特点：

- 编译器自动分配和释放
- 存放函数的参数值和局部变量

### 堆区

数据特点：

- 由程序员分配释放，若程序员不释放，程序结束时，由操作系统自动回收
- C++主要用 new 在堆区开辟内存
- new创建的数据会返回数据对应的类型的指针

### RAII（Resource Acquisition Is Initialization，资源获取就是初始化）

RAII：资源获取就是初始化，是C++等编程语言常用的管理资源、避免内存泄漏的方法，它保证任何情况下，使用对象时先构造对象，最后析构对象

根据RAII对资源的所有权可分为常性类型和变性类型，代表者范别是boost::shared_ptr<>和std::auto_ptr<>；从所管资源的初始化位置上可分为外部初始化和内部初始化类型

常性类型是指获取资源的地点是构造函数，释放点是析构函数，并且在这两点之间的一段时间里，任何对该RAII类型实例的操纵都不应该从它手里夺走资源的所有权。变变性类型是指可以中途被设置为接管另一个资源，或者干脆被设置为不拥有任何资源。外部初始化类型是指资源在外部被创建，并被传给RAII实例的构造函数，后者进而接管了其所有权。boost::shared_ptr<>和std::auto_ptr<>都是此类型。与之相对的是内部初始化类型。

其中常性且内部初始化的类型是最为纯粹的RAII形式

```c++
std::mutex mutex_;
void function() {
    mutex_.lock();
    ...	//如果中间有异常或者有return语句，互斥锁就不会正确的解锁，会导致线程的死锁，正确的方式是使用std::uniue_lock或者std::lock_guard对互斥锁进行状态管理
    mutex_.unlock();
}
void foo() {
    //创建std::lock_guard对象，会对std::mutex对象进行lock，当std::lock_guard对象在超出作用域时，会自动对std::mutex对象进行解锁，这样就不会因为代码异常造成线程死锁
    std::lock_guard<std::mutex>> lock(mutex_);
    ...
}
```

RAII的核心思想就是将资源或者状态与对象的声明周期进行绑定，通过C++语言机制，实现资源和状态的安全管理

### C++锁

C++提供了`std::lock(lock1, lock2, ...lock3)`来解决死锁问题，获取所有的锁，如果有哪一个锁无法获取到，那么就把已经获取的锁释放掉。因此就不会产生死锁，如：

```c++
//这样会产生死锁
std::mutex m_mutex1;
std::mutex m_mutex2;
void foo1(){
    std::lock_guard<std::mutex> locker1(m_mutex1);
    std::lock_guard<std::mutex> locker2(m_mutex2);
}
void foo2(){
    std::lock_guard<std::mutex> locker1(m_mutex2);
    std::lock_guard<std::mutex> locker2(m_mutex1);
}

//不会产生死锁
std::mutex m_mutex1;
std::mutex m_mutex2;
void foo1(){
    std::lock(m_mutex1, m_mutex2);
    std::lock_guard<std::mutex> locker1(m_mutex1, std::adopt_lock);
    std::lock_guard<std::mutex> locker2(m_mutex2, std::adopt_lock);
}
void foo2(){
    //锁的顺序无所谓，只要有一个锁无法获得，就会释放所有的锁
    std::lock(m_mutex1, m_mutex2);
    std::lock_guard<std::mutex> locker1(m_mutex2, std::adopt_lock);
    std::lock_guard<std::mutex> locker2(m_mutex1, std::adopt_lock);
}
```

如果只需要一个锁就不会产生死锁，如果需要两个以上的锁，就可能会产生死锁，这时就要注意上锁的顺序，或者使用`std::lock()`来上锁，该函数接收不定长参数的锁，给所有锁上锁，如果有哪一个锁获取不到就会将锁全部释放掉

1. `std::lock_guard<std::mutex>`，互斥锁包装器，为作用域块期间占有互斥提供便利RAII风格机制，创建lock_guard对象时，试图接收给定互斥的所有权，控制离开创建lock_guard对象的作用域时，销毁lock_guard并释放互斥。离开作用域自动释放锁

2. mutex.lock()，mutex.unlock()上锁与解锁

3. `std::unique_lock<std::mutex>`，通用互斥包装器，允许延迟锁定、锁定的有时限尝试、递归锁定、所有权转移和与条件变量一同使用。弹性锁定，会消耗更多的计算机性能

   unique_lock可移动，但不可赋值，满足可移动构造和可移动赋值，但不满足可复制构造或可复制赋值。

   构造的时候不会自动上锁，可以手动上锁

|             |                                  |
| ----------- | -------------------------------- |
| defer_lock  | 不获得互斥的所有权               |
| adopt_lock  | 假设调用方线程已拥有互斥的所有权 |
| try_to_lcok |                                  |

```c++
std::mutex m_mutex1;
std::mutex m_mutex2;
void foo(){
    //锁定两个互斥而不死锁
	std::lock(m_mutex1, m_mutex2);
    //保证第二个已锁定互斥在作用域结尾解锁
	std::lock_guard<std::mutex> locker1(m_mutex1, std::adopt_lock);
    std::lock_guard<std::mutex> locker2(m_mutex2, std::adopt_lock);
// 等价方法：
//    std::unique_lock<std::mutex> locker1(m_mutex1, std::defer_lock);
//    std::unique_lock<std::mutex> locker2(m_mutex1, std::defer_lock);
//    std::lock(locker1, locker2);
}
```

如果有多个线程操作同一个文件打开，为了保证线程安全和文件只打开一次使用`std::call_once(std::once_flag, callable对象)`准确执行一次可调用对象，即使同时被多个线程调用，`std::call_once(m_flag, [&](){ f.open("log.txt"); });`保证了f只打开文件对象一次。

```c++
void print(int func(int)) {
    std::cout << func;
}

void print(short num) {
    std::cout << num;
}

int main() {
    print(::std::putchar);
//    print(0);//无法判断调用哪个函数引起歧义
}

template<typename T>
void foo(T t) {
    std::cout << 0;
}

template<>
void foo<int *>(int *t) {
    std::cout << 10;
}

//重载的函数或函数模板取出来，根据规则去判断调用哪一个
//如果确定之后是一个模板，再去看有没有对模板进行特化，如果有用户定义的特化，就调用用户定义的特化，如果没有，就会把模板实例化，实例化的结果是一个template initialization
//特化在重载的时候，只有在决定了调用哪一个函数或者函数模板之后才会去看有没有特化，在重载的判定阶段是没有特化的
template<typename T>
void foo(T *p) {
    std::cout << 20;
}

int main() {
    int i = 42;
    foo(&i);	//输出20
}

struct Print {
    template<typename T>
    Print(T t) { std::cout << (t += 10); }
};

int main() {
    //定义了返回值为Print类型，参数为string的一个函数
    Print p(::std::string());//可以编译运行，但是什么都不会输出
}
```

## C++11

1. 左值、右值

2. 智能指针：unique_ptr，shared_ptr，引用计数线程安全，对象使用线程不安全

3. 初始化列表，initializer_list

4. auto，自动类型推断

5. 线程库，thread，mutex，condition_variable，unique_lock，lock_guard，call_once，

6. future，promise，async，volatile

   async(std::launch::async | std::launch::deferred, func, args ...)

   std::launch::async：表示任务执行在另一线程

   std::launch::deferred：表示延迟执行任务，调用get或者wait时才会执行，不会创建线程，惰性执行在当前线程。

   如果不指明创建策略，则使用一个基于任务的程序设计，内部有一个调度器(线程池)，会根据实际情况采用哪种策略

7. 原子操作库，atomic，store()，is_lock_free()，load()，exchange()，wait()，notify_one()，notify_all()

8. final&override， default， delete，explicit，constexpr，enum class

9. static-assert

10. chrono，ratio<分子，分母>

11. thread_local，thread_local修饰的变量具有thread周期，每一个线程都拥有并只拥有一个该变量的独立实例，一般用于需要保证线程安全的函数中。

12. 随机数库，正则表达式

13. 容器库新增forward_list

14. all_of()、any_of()、none_of()、find_if_not()、copy_if、itoa

## C++14

1. 函数返回值类型推导，auto func();，不能返回初始化列表，多个return语句必须返回相同的类型，虚函数不能返回值类型推导

2. lambda表达式参数类型可以是auto

3. 变量模板`template<typename T> constexpr T pi = T(3.1415926535897932385L)`

4. 别名模板

5. ### [[`deprecated`]]标记，编译产生警告，提示修饰的内容将来可能会被抛弃

6. 二进制字面常量与 ` 分隔符

7. std::make_uniqe

8. std::shared_timed_mutex(可以携带超时时间)与std::shared_lock

9. integer_sequence

10. exchange，对左边赋值，没有对右边赋值

11. std::quoted，用于给字符串添加双引号

## C++17

1. 构造函数模板推导

   ```c++
   pair<int, double> p(1, 2.2); // c++17之前
   pair p(1, 2.2); // c++17 自动推导
   vector v = {1, 2, 3}; // c++17
   ```

2. 结构化绑定，对于tuple、map

   ```c++
   std::tuple<int, doubl> func() {
   	return std::tuple(1, 1.2);
   }
   void f() {
       auto [i, d] = func();
       map<int, string> m = {
           {0, "a"},
           {1, "b"},
       };
       for(const auto &[k, v] : m) {
           std::cout << k << " " << v << std::endl;
       }
   }
   ```
   
   结构化绑定不能用于constexpr， c++17不行，C++20可以
   
   还尅绑定数组和结构体，必须是public
   
   自定义类的结构化绑定需要实现相关的tuple_size和tuple_element和get\<N>方法


3. if-switch语句初始化

   ```
   if(a < 101) {
   	std::cout << a << std::endl;
   }	// c++17之前
   
   if(int a = getValue(); a < 101) {
   	std::cout << a << std::endl;
   } 
   ```

   

4. 内联变量

5. 折叠表达式

6. constexpr lambda表达式

   注意：constexpr函数有如下限制：

   函数体不能包含汇编语句、goto语句、label、try块、静态变量、线程局部存储、没有初始化的普通变量，不能动态分配内存，不能有new delete等，不能虚函数。

7. namespace嵌套

8. __has_include预处理表达式

9. 在lambda表达式用*this捕获对象副本

   this捕获对象的而引用，多线程情况下对象可能被析构

   捕获*this，不持有this指针，而是持有对象的拷贝，这样生命周期就与对象的声明周期不相关

10. 新增Attribute

    [[fallthrough]]，用在switch中提示可以直接落下去，不需要break，让编译期忽略警告

    [[nodiscard]] ：表示修饰的内容不能被忽略，可用于修饰函数，标明返回值一定要被处理

    [[maybe_unused]] ：提示编译器修饰的内容可能暂时没有使用，避免产生警告

11. 字符串转换

    新增from_chars函数和to_chars函数

    from_chars，分析字符序列[first, last) 若无字符匹配模式或若按照分析匹配字符获得的值不能以value的类型表示，则不修改value，否则将匹配模式的字符转译成算术值的文本表示，并将值存储于value。返回类型为from_chars_result{const char *ptt;  std::errc ec;}

    

12. std::variant

    类似union的功能

    variant的第一个类型一半要有对应的构造函数，否则编译失败

    可以使用std::monostate打桩，模拟空状态

    `std::variant<std::monostate, A> var;`

13. std::optional

    返回对象时，如果出现异常，希望返回空指针，这样返回的类型就是就只能是指针，涉及到了内存管理，也可以使用空指针，此时可以使用std::optional，

14. std::any

    可以存储任何类型的单个值

15. std::apply

    使用std::apply可以将tuple展开作为函数的参数传入

16. std::make_from_tuple

    使用make_from_tuple可以将tuple展开作为构造函数参数

17. as_const

    可以将左值转换成const类型

18. std::string_view

    传递一个string会触发对象的拷贝操作，大字符串的拷贝赋值会触发堆内存分配，影响运行效率。

    ```
    ```

    

19. file_system

    提供文件的大多数功能

20. std::shared_mutex

    共享锁，可以实现读写锁

## C++20

1. 比较运算符\<=>

   如果a>b 则运算结果>0，如果a < b则运算结果＜0，如果a==b则运算结果=0，运算符的记过结果类型会根据a和b的类型来决定

2. 指定初始化，必须按顺序进行初始化

3. for循环里初始化

   `for(auto n = v.size(); auto i : v)`

4. char8_t

5. **[[no_unique_address]]**

6. **[[likely]]和[[unlikely]]**：在分支预测时，用于告诉编译器哪个分支更容易被执行，哪个不容易执行，方便编译器做优化。
   ```c++
   constexpr long long fact(long long n) noexcept {
       if (n > 1) [[likely]]
           return n * fact(n - 1);
       else [[unlikely]]
           return 1;
   }
   ```
   
7. lambda表达式值捕获
   ```c++
   struct S2 { void f(int i); };
    void S2::f(int i)
    {
    [=]{};          // OK: by-copy capture default
    [=, &i]{};      // OK: by-copy capture, except i is captured by reference
    [=, *this]{};   // until C++17: Error: invalid syntax
                    // since c++17: OK: captures the enclosing S2 by copy
    [=, this] {};   // until C++20: Error: this when = is the default
                    // since C++20: OK, same as [=]
    }
    
    // lambda表达式可以使用模板
    // generic lambda, operator() is a template with two parameters
    auto glambda = []<class T>(T a, auto&& b) { return a < b; };
    
    // generic lambda, operator() is a template with one parameter pack
    auto f = []<typename ...Ts>(Ts&& ...ts) {
    return foo(std::forward<Ts>(ts)...);
    };
   ```
   
8. consteval
   consteval修饰的函数只会在编译期间执行，如果不能编译期间执行，则编译失败。
   
9.  constinit
    断言一个变量有静态初始化，即零初始化和常量初始化，否则程序是有问题的。
    
10. 协程，顺序代码的逻辑执行异步的任务

    co_await 操作符暂停执行，直到恢复

    co_yield 暂停执行，返回一个值

    co_return 完成执行， 返回一个值

11. module 

    ```c++
    // helloworld.cpp
    export module helloworld;  // module declaration
    import <iostream>;         // import declaration
     
    export void hello() {      // export declaration
        std::cout << "Hello world!\n";
    }
    
    // main.cpp
    import helloworld;  // import declaration
     
    int main() {
        hello();
    }
    ```

    module使用方式和include差不多，但module使用比include头文件速度快

12. using 可以引用enum

13. constaints 和 concepts 约束和概念

14. std::format 

15. 增加日历和时区的支持

16. 增加std::atomic 让智能指针线程安全

17. source_location：可作为\_\_LINE\_、\_\_func\_\_ 宏的替代

18. span

19. endian获取当前平台是大端序还是小端序

20. make_shared支持构造数组

21. std::remove_cvref：去除cv，去除引用

22. std::to_address：获得由p表示的地址，而不对p所指向的对象的引用

23. 线程同步

    - barrier：屏障
    - lath：CountDownLatch
    - counting_semaphore

24. std::jthread：std::thread在析构时如果没有join或者detach会crash，而jthread在析构时会自动join。jthread也可以取消线程，request_stop()

25. 中断线程执行的相关类

    - stop_token：查询线程是否中断
    - stop_source：请求线程停止运行
    - stop_callback：stop_token执行时，可以触发的回调函数

26. basic_osyncstream：对std::basic_syncbuf的再包装，直接使用std::cout多线程下可能出现数据交叉，osyncstream不会出现

27. string的系列操作

    - string::starts_with
    - string::ends_with
    - string_view::starts_with
    - string::view::ends_with

28. std::assume_aligned

    指定多少字节对齐，进一步生成有效的代码

29. bind_front：和使用std::bind绑定第一个参数效果类似，可以绑定前x个参数

30. std::ssize：signed size

31. midpoint 计算中位数

    lerp 计算线性差值

32. Ranges库：ranges库提供了用于处理元素范围的组件，包括各种视图适配器。表示连续元素或者连续元素的片段

33. std::is_bounded_array：检查数组是不是有界

    std::is_unbounded_array

34. numbers头文件定义了一些数字常量

35. 







  
