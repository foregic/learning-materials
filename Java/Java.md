# Java

基础数据类型：int、byte、char、float、double、bool，其他类型为引用类型。引用类型为本质是一个指针，指向内存中的位置。

`final`：常量修饰符

`var`关键字：自动分配变量类型

`extends`：继承关键字

`System.out.println()`：是`print line`的缩写，表示输出后自动换行不换行，如果不想换行可用`System.out.print()`，`Symtem.out.printf`进行格式化输出，通过控制站位符调整数字格式

| 占位符 | 说明                             |
| ------ | :------------------------------- |
| %d     | 格式化输出整数                   |
| %x     | 格式化输出十六进制数             |
| %f     | 格式化输出浮点数                 |
| %e     | 格式化输出科学技术法表示的浮点数 |
| %s     | 格式化字符串                     |



```java
int[] array = new int[5];//定义数组
System.out.println(array.length);//获取数组长度
int[] array_1 = new int[]{1, 1, 1, 1, 1, 1, 1, 1};//编译器自动推算数组大小
System.out.printf("n=%d,hex=%08x", 12345, 12345);//格式化输出十六进制数，不足位置补0补到8位

//Java13支持
Stirng s="""
    123456
    123465
    """

Scanner scanner = new Scanner(System.in); // 创建Scanner对象
System.out.print("Input your name: "); // 打印提示
String name = scanner.nextLine(); // 读取一行输入并获取字符串
System.out.print("Input your age: "); // 打印提示
int age = scanner.nextInt(); // 读取一行输入并获取整数
System.out.printf("Hi, %s, you are %d\n", name, age); // 格式化输出

String s1,s2;
s1 != null && s1.equals(s2); //判断s1和s2指向的字符串是否相同
"hello".equals(s1); //判断s1指向的字符串是否为hello

//Java12支持，新语法没有穿透效应
//还可以有返回值
String s = "fruit";
switch(s){
        case "apple"->System.out.println("Selected apple");
        case "pear"->System.out.println("Selected pear");
        case "mango"->{
            System.out.println("Selected mango");
            System.out.println("Good choice");
        }
        default->System.out.println("No fruits selected");
}
```



## 类

`super`：类中表示父类，编译器会自动定位到父类的field字段，在java中任何class的构造方法会自动调用父类的构造方法，如果没有明确调用父类的构造方法，编译器会自动添加一句`super()`

只要没有`final`修饰符，任何类都能从该class继承

`sealed`：修饰class，通过`permits`明确写出能够从该class继承的子类名称

```java
public sealed class Shape permits Rect, Circle, Triangle {
    ...
}
```

`instanceof`判断某个变量所指向的实例是否是指定类型，或者这个类型的子类，如果一个变量为null，那么任何instanceof的判断都为false。Java14开始，判断`instanceof`后可以直接转变为指定变量，避免再次强制转型。

```java
if (obj instanceof String s) {
            // 可以直接使用变量s:
            System.out.println(s.toUpperCase());
        }
```

继承是is关系，组合是has关系。

```java
class Person {
            protected int age;
            protected String name;
            public void run() {}
        }
        class Student extends Person {
            //override，覆写父类方法，方法名相同，参数相同，返回值相同
            //overload，重载，方法名相同，参数相同，返回值不同也是不同方法
            @Override//让编辑器帮助检查是否进行了正确的覆写
            public void run() {
                System.out.println("Student.run");
            }
        }
```

### 继承



继承可以允许子类覆写父类方法。如果一个父类方法不允许子类对它的某个方法进行覆写，可以把该方法标记为`final`。用`final`修饰的方法不能被`Override`。如果一个类不希望任何类继承自它，那么可以把这个类本身标记为`final`。用`final`修对`final`字段重新赋值会报错：饰的类不能被继承。对于一个类的实例字段，同样可以用`final`修饰。用`final`修饰的字段在初始化后不能被修改。可以在构造方法中初始化`final`字段。

```java
class Person {
    public final String name = "Unamed";
}

Person p = new Person();
p.name = "New Name"; // compile error!

class Person {
    public final String name;
    public Person(String name) {//在构造方法中初始化final字段
        this.name = name;
    }
}

```

###### 小结

- 子类可以覆写父类的方法（Override），覆写在子类中改变了父类方法的行为；
- Java的方法调用总是作用于运行期对象的实际类型，这种行为称为多态；
- `final`修饰符有多种作用：
  - `final`修饰的方法可以阻止被覆写；
  - `final`修饰的class可以阻止被继承；
  - `final`修饰的field必须在创建对象时初始化，随后不可修改。

##### 抽象类

`abstract`：如果父类的方法本身不需要实现任何功能，仅仅是为了定义方法签名，目的是让子类去覆写它，那么，可以把父类的方法声明为抽象方法：

```java
abstract class Person { 
    //类名要要声明为abstract，因为抽象方法是无法执行的，Person类也无法被实例化，编译器会告诉我们无法编译Person类 
    public abstract void run();
}
```

使用`abstract`修饰的就是抽象类，无法实例化一个抽象类。抽象类本身设计成只能用于被继承

在抽象类中，抽象方法本质上是定义接口规范：即规定高层类的接口，从而保证所有子类都有相同的接口实现。

##### 接口

如果一个抽象类没有字段，所有方法全部都是抽象方法：

```java
abstract class Person {
    public abstract void run();
    public abstract String getName();
}
```

就可以把该抽象类改写为接口：`interface`。

在Java中，使用`interface`可以声明一个接口：

```java
interface Person {
    void run();
    String getName();
}
```

所谓`interface`，就是比抽象类还要抽象的纯抽象接口，因为它连字段都不能有。因为接口定义的所有方法默认都是`public abstract`的，所以这两个修饰符不需要写出来（写不写效果都一样）。


>当一个具体的`class`去实现一个`interface`时，需要使用`implements`关键字。举个例子：
>```java
>class Student implements Person {
>private String name;
>
>public Student(String name) {
>   this.name = name;
>}
>
>@Override
>public void run() {
>   System.out.println(this.name + " run");
>}
>
>@Override
>public String getName() {
>   return this.name;
>}
>}
>
>我们知道，在Java中，一个类只能继承自另一个类，不能从多个类继承。但是，一个类可以实现多个interface，例如：
>class Student implements Person, Hello { // 实现了两个interface
>    ...
>}
>```

###### 术语

|            |    abstract class    |          interfere          |
| :--------: | :------------------: | :-------------------------: |
|    继承    | 只能extends一个class | 可以implements多个interfere |
|    字段    |   可以定义实例字段   |        不能实例字段         |
|  抽象方法  |   可以定义抽象方法   |      可以定义抽象方法       |
| 非抽象方法 |       可以定义       |     可以定义default方法     |

接口继承，一个`interface`可以继承另一个`interface`，`interface`继承自`interface`使用`extends`，相当于扩展了接口的方法。如
```java
interface Hello {
    void hello();
}

interface Person extends Hello {
    void run();
    String getName();
}
```

合理设计`interface`和`abstract class`的继承关系，可以充分复用代码。一般来说，公共逻辑适合放在`abstract class`中，具体逻辑放到各个子类，而接口层次代表抽象程度。

`default`方法的目的是需要给接口新增一个方法时，会涉及到修改全部的子类，如果新增的是`default`方法那么子类就不必全部修改，只需要在需要覆写的地方去覆写新增方法。
		default方法和抽象类的普通方法有所不同，因为interface没有字段，default方法无法访问字段，而抽象类的普不同方法可以访问字段。

`static`修饰的字段称为静态字段`static field`，实例字段在每个实例中都有一个独立的空间，但是静态字段只有一个共享“空间”，所有实例都会共享该空间。通过类名访问静态字段，实例访问静态字段是因为编译器根据实例类型转为`类名.静态字段`来访问静态字段
		静态方法就是用`static`修饰的方法，通过类名调用。因为静态方法属于类不属于实例，因此静态方法内部无法访问`this`变量也无法访问实例字段，只能访问静态字段。`interface`可以拥有静态字段，并且静态字段必须声明为`final`类型。

```java
public interface Person {
    public static final int MALE = 1;
    public static final int FEMALE = 2;
}
public interface Person {
    // 编译器会自动加上public statc final:
    int MALE = 1;
    int FEMALE = 2;
}
```

##### 包

java定义了一种名字空间，称之为`package`，一个类总属于某个包，真正完整的类名是`包名.类名`，jvm只看完整类名，包名不同，类名相同也是不同的类。包可以是多层结构。`要特别注意：包没有父子关系。java.util和java.util.zip是不同的包，两者没有任何继承关系。`同一个包的类可以访问包作用域的字段和方法，不用`public`、`protected`、`private`修饰的字段和方法就是包作用域。

嵌套类：如果一个类内部还定义了一个类，那么嵌套类拥有访问`private`的权限

final修饰class可以防止类被继承。用final修饰method的可以防止子类覆写。final修饰field可以阻止被重新赋值，final修饰局部变量可以组织被重新赋值。

#### 最佳实践

如果不确定是否需要`public`，就不声明为`public`，即尽可能少地暴露对外的字段和方法。

把方法定义为`package`权限有助于测试，因为测试类和被测试类只要位于同一个`package`，测试代码就可以访问被测试类的`package`权限方法。

一个`.java`文件只能包含一个`public`类，但可以包含多个非`public`类。如果有`public`类，文件名必须和`public`类的名字相同。

#### 小结

Java内建的访问权限包括`public`、`protected`、`private`和`package`权限；

Java在方法内部定义的变量是局部变量，局部变量的作用域从变量声明开始，到一个块结束；

`final`修饰符不是访问权限，它可以修饰`class`、`field`和`method`；

一个`.java`文件只能包含一个`public`类，但可以包含多个非`public`类。



### 内部类

Inner class：定义在一个类内部的类。不能单独存在。要实例化一个Inner必须先实例化一个Outer实例，然后调用Outer实例的new来创建Inner实例。这是因为Inner Class除了有一个this指向自己，还隐含的持有一个Outer Class实例，可以用Outer.this访问这个实例，所以实例化一个Inner Class不能脱离Outer实例。Inner Class除了能引用Outer实例外还可以修改Outer Class的private字段，因为Inner Class的作用域在Outer Class内部，所以能访问Outer Class的`private`字段和方法。

匿名

static nested Class：静态内部类static修饰，与Inner Class不同，不再依附于Outer而是一个独立的类，但它可以访问Outer的private静态字段和静态方法

 classpath和jar

自带依赖关系的class容器就是模块



