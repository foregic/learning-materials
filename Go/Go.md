```go
// defer 延时机制 
// 程序执行到defer后，把defer后面的内容值拷贝到栈中
// 当此函数执行完毕后，从栈中取出defer后面的代码执行

// 函数闭包


func demo(args...int) {
	fmt.Println(args[0])
	for k,v := range args {
		fmt.Println(k," ",v)
	}
}
demo(1,2,3,4)
/*0 1
 *1 2
 *2 3
 *3 4
 */

// 匿名函数
var Func = func (num1, num2 int) int {
	return num1 - num2
}

//每个go文件都又严格init方法
//执行顺序；全局变量 > init() > main()

// 变参，必须是切片且未同类型
func main() {
	// 第一种
	// test1("h", 1, 2, 3)

	// 切片
	//str := "h"
	//s := []int{1, 2, 3}
	//test1(str, s...)

	// 数组
	str := "h"
	arr := [2]int{1, 2}
	test1(str, arr[:]...) // 数组转变成切片，然后展开
}
func test1(str string, s ...int) {
	fmt.Println(str, s)
}

sync.Mutex	// 互斥锁
sync

// 标准库中有四个包对字符串处理尤为重要：bytes、strings、strconv和unicode包。strings包提供了许多如字符串的查询、替换、⽐较、截断、拆分和合并等功能。
// bytes包也提供了很多类似功能的函数，但是针对和字符串有着相同结构的[]byte类型。因为字符串是只读的，因此逐步构建字符串会导致很多分配和复制。在这种情况下，使⽤bytes.Buffer类型将会更有效，
// strconv包提供了布尔型、整型数、浮点数和对应字符串的相互转换，还提供了双引号转义相关的转换。
// unicode包提供了IsDigit、IsLetter、IsUpper和IsLower等类似功能，它们⽤于给字符分类。每个函数有⼀个单⼀的rune类型的参数，然后返回⼀个布尔值。⽽像ToUpper和ToLower之类的转换函数将⽤于rune字符的⼤⼩写转换。所有的这些函数都是遵循Unicode标准定义的字⺟、数字等分类规范。strings包也有类似的函数，它们是ToUpper和ToLower，将原始字符串的每个字符都做相应的转换，然后返回新的字符串。


r := [...]int{99: -1}
// 在这种形式的数组字⾯值形式中，初始化索引的顺序是⽆关紧要的，⽽且没⽤到的索引可以省略，和前⾯提到的规则⼀样，未指定初始值的元素将⽤零值初始化
// strs := [4]string{"123", "123"}
// strs = []string{"123", "123"} 	// 报错[4]string和[]string不是同类型
strs := []string{"123", "1", "2"}
strs = []string{"123", "123"} // 无报错

// 当调⽤⼀个函数的时候，函数的每个调⽤参数将会被赋值给函数内部的参数变量，所以函数参数变量接收的是⼀个复制的副本，并不是原始调⽤的变量。因为函数参数传递的机制导致传递⼤的数组类型将是低效的，并且对数组参数的任何的修改都是发⽣在复制的数组上，并不能直接修改调⽤时原始的数组变量。

// ⻓度对应slice中元素的数⽬；⻓度不能超过容量，容量⼀般是从slice的开始位置到底层数据的结尾位置。
// len是长度，cap是slice开始位置到底层数据结构结束位置的元素个数
a := make([]int, 8)
b := a[3:7]
fmt.Println(len(b), cap(b))
// len =7-3=4 cap =8-3=5
// r果切⽚操作超出cap(s)的上限将导致⼀个panic异常
// 但是超出len(s)则是意味着扩展了slice，因为新slice的⻓度会变⼤：

// 为了提⾼内存使⽤效率，新分配的数组⼀般略⼤于保存x和y所需要的最低⼤⼩。通过在每次扩展数组时直接将⻓度翻倍从⽽避免了多次内存分配，也确保了添加单个元素操的平均时间是⼀个常数时间。


// map是一个哈希表的引用，key对应的必须是支持==比较运算符的数据类型。虽然浮点数类型也是支持相等运算符比较的，但是可能会出现NaN和任何浮点数都不相等。

ages := make(map[string]int)
delete(ages, "alice") // 删除元素，删除之后没有对应的key-value

// map中的元素并不是一个变量，因此不能对map的元素进行取址操作
// 禁止取址的原因是map可能随着元素数量的增长而分配更大的内存空间导致之前的地址无效
// 遍历map中全部的key/value对，可以使用range for
// map的遍历顺序是不确定的，并且不同的哈希函数实现可能导致不同的的遍历顺序，实践中遍历的顺序是随机的，每一次遍历的顺序都不相同。



```

### 参数

引⽤类型（包括slice、指针、map、chan和函数）
其他都是值类型传递参数
struct的method要修改数据要传指针，传值不能修改原本struct的数据成员

> 2.传指针比较轻量级 (8bytes),只是传内存地址，我们可以用指针传递体积大的结构体。如果用参数值传递的话, 在每次copy上面就会花费相对较多的系统开销（内存和时间）。所以当你要传递大的结构体的时候，用指针是一个明智的选择。
>
> 3.Go语言中channel，slice，map这三种类型的实现机制类似指针，所以可以直接传递，而不用取地址后传递指针。（注：若函数需改变slice的长度，则仍需要取地址传递指针） 546
>
> 

#### Context

#### net/http

几个概念：

1. Request：用户请求的信息，用来解析用户的请求，包括post、get、cookie、url等信息
2. Response：服务器需要反馈给客户端的信息
3. Conn：用户的每次请求链接
4. Handler：处理请求和生成返回信息的处理逻辑

客户端每次请求都会创建一个Conn，这个Conn里面保存了该次请求的信息，然后在传递到对应的handler，该handler中便可以读取到相应的header信息，这样保证类每个请求的独立性

go http包的两个核心功能：Conn、ServeMux，前者独立请求实现高并发，后者内部有一个锁

```go
type ServeMux struct {
    mu sync.RWMutex // 锁，并发处理需要一个锁
    m map[string]muxEntry // 路由规则，一个string对应一个mux实体，这里的实体体ring即使注册的路由表达式
}
type muxEntry struct {
    explicit bool // 是否精确匹配
    h Handler // 这个路由表达式对应哪个handler
}
type Handler interface {
    ServerHttp(ResponseWriter, *Request) // 路由实现器
}

type HandlerFunc func(ResponseWriter, *Request)

// ServeHTTP calls f(w, r)
func (f HandlerFunc)ServeHTTP(w ResponseWriter, r *Request) {
    f(w, r)
}
// 调用HandlerFunc强制类型转换f称为HandlerFunc类型，这样f就拥有了ServeHTTP方法

// 路由器接收到请求之后调用mux.handler(r).ServeHTTP(w, r)
// 也旧式低啊用对应路由的handler的ServerHTTP接口
```

go代码HTTP的执行流程：

1. 首先调用http.HandlerFunc

   按顺序

   1. 调用了DefaultServerMux的HandlerFunc
   2. 调用了DefaultServerMux的Handler
   3. 往DefaultServerMux的map[string]muxEntry中增加对应的handler和路由规则

2. 其次调用http.ListenAndServer(":8080", nil)

   1. 实例化Server
   2. 调用Server的ListenAndServer()
   3. 低啊用net.Listen("tcp", addr)监听端口
   4. 启动一个for循环，在循环体中Accept请求
   5. 对每个请求实例化一个Conn，并且开启一个goroutine为这个请求进行服务go c.serve()
   6. 读取每个请求的内容为w, err := c.readRequest()
   7. 判断handler是否为空，如果没有设置handler， handler就设置为DefaultServerMux
   8. 调用handler的ServeHttp
   9. 根据request选择handler，并且进入到这个handler的ServeHTTp
   10. 选择handler
       1. 判断是否有路由能否满足这个request
       2. 如果有路由满足，调用这个路由的handler的ServeHttp
       3. 如果没有路由满足，调用NotFoundHamdler的ServeHttp



go的接口是一种抽象的类型，不会暴露所代表的的对象的内部值的结构和这个对象支持的基础操作的集合；只会表现出它们自己的方法。也就是说当蓝岛一个有接口类型的值时，不知道是什么，唯一知道的就是可以额通过方法来做什么。
一个类型如果拥有一个接口需要的所有办法，那么这个类型就实现了这个接口。
经常会把一个具体的类型描述成一个特定的接口类型。

```go
interface
_type *_type // d类型元数据
data unsafe.Pointer // 动态值

var e interface{}	// _type = nil, data = nil
```

