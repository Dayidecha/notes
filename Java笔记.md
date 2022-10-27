+ **JRE**( java runtime environment)：是Java运行时的环境，包含**JVM**和**核心类库**
+ **jdk**(java development kit)：是JAVA程序开发工具包，包含**jre**和开发人员工具
+ 获取src路径下的文件的方式 -- ClassLoader 类加载器



.java ->(编译)->.class->(运行)JVM



**idea快捷键**

| 快捷键              | 功能                             |
| ------------------- | :------------------------------- |
| ctrl+d              | 重复当前行                       |
| ctrl+p              | 查看方法需要哪些参数             |
| alt+enter           | 自动修复                         |
| ctrl+y              | 删除当前行                       |
| ctrl+alt+l          | 格式化代码                       |
| alt+ins             | 自动加入函数                     |
| ctrl+shift+/        | 多行注释                         |
| Ctrl+Alt+V          | 可以引入变量                     |
| Ctrl+H              | 查看继承关系                     |
| 输入/**，然后按回车 | 自动根据参数和返回值生成注释模板 |
| Ctrl+alt+T          | try-catch                        |
| alt+f8              | 计算某一个方法的值               |
| Ctrl+o              | 查看能重写的父类方法             |
| Ctrl+N              | 按名字搜索类                     |
| shift两次           | 约等于上面                       |
| alt+7               | 查看当前class的方法              |
| Ctrl+f12            | 查看类有哪些方法                 |









**debug注意事项**

+ 一般添加在每个方法的第一行

说明

|               | 快捷键   | 说明         |
| ------------- | -------- | ------------ |
| Step Over     | f8       | 逐行执行     |
| Step into     | f7       | 进入方法     |
| Step out      | shift+f8 | 跳出方法     |
| Run to cursor | f9       | 到下一个断点 |

## 杂知识

+ 变量在定义的时候可以没有值，在使用的时候必须有值
+ cmd输入**services.msc**打开服务窗口
+ 静态代码块的执行顺序：**静态代码块----->非静态代码块-------->构造函数**



## Maven

+ 被依赖的jar发生改动之后要先执行`mvn clean install`

## Java程序基本结构

### 类名要求

+ 英文字母开头，后接字母数字下划线
+ 习惯大写字母开头

### var关键字

有些时候，类型的名字太长，写起来比较麻烦。例如：

```
StringBuilder sb = new StringBuilder();
```

这个时候，如果想省略变量类型，可以使用`var`关键字：

```
var sb = new StringBuilder();
```

编译器会根据赋值语句自动推断出变量`sb`的类型是`StringBuilder`。对编译器来说，语句：

```
var sb = new StringBuilder();
```

实际上会自动变成：

```
StringBuilder sb = new StringBuilder();
```

因此，使用`var`定义变量，仅仅是少写了变量类型而已。

### 整数运算

+ 位运算：与或非异或
+ 移位运算： >> ,<< ,>>>



### 数组遍历

Java标准库提供了`Arrays.toString()`，可以快速打印数组内容：

### JAVA中的内存

+ **栈(stack):存放的是方法中的局部变量。方法的运行一定要在栈当中。**
  + 局部变量：方法的参数，或者是方法{}内部的变量
  + 作用域：一旦超出作用域，立刻从栈内存中消失
+ **堆（heap)：凡是new出的东西，都在堆中。**
  + 堆内存里都有一个地址值：16进制
  + 堆内存里的数据，都有默认值。规则：
    + 整数:0
    + 浮点：0.0
    + 字符：\u0000
    + 布尔：false
    + 引用：null
+ **方法区（Method Area)：存储.class相关信息，包含方法的信息。**
+ 本地方法栈（Native Method Stack）：与操作系统相关
+ 寄存器：与CPU相关

## 类

## 方法

可变参数`...`

可变参数用`类型...`定义，可变参数相当于数组类型：

而可变参数可以保证无法传入`null`，因为传入0个参数时，接收到的实际值是一个空数组而不是`null`。



### 继承

所有的`class`最终都继承自`Object`，而`Object`定义了几个重要的方法：

- `toString()`：把instance输出为`String`；
- `equals()`：判断两个instance是否逻辑相等；
- `hashCode()`：计算一个instance的哈希值。



### 多态

#### final

继承可以允许子类覆写父类的方法。如果一个父类不允许子类对它的某个方法进行覆写，可以把该方法标记为`final`。用`final`修饰的方法不能被`Override`。

用`final`修饰`class`可以阻止被继承

用`final`修饰`method`可以阻止被子类覆写

用`final`修饰`field`可以阻止被重新赋值

如果一个类不希望任何其他类继承自它，那么可以把这个类本身标记为`final`。用`final`修饰的类不能被继承



### 包

#### 包作用域

位于同一个包的类，可以访问包作用域的字段和方法。不用`public`、`protected`、`private`修饰的字段和方法就是包作用域。例如，`Person`类定义在`hello`包下面。



Java编译器最终编译出的`.class`文件只使用*完整类名*，因此，在代码中，当编译器遇到一个`class`名称时：

- 如果是完整类名，就直接根据完整类名查找这个`class`；
- 如果是简单类名，按下面的顺序依次查找：
  - 查找当前`package`是否存在这个`class`；
  - 查找`import`的包是否包含这个`class`；
  - 查找`java.lang`包是否包含这个`class`。

### Classpath和jar

`classpath`是JVM用到的一个环境变量，它用来指示JVM如何搜索`class`。

因为Java是编译型语言，源码文件是`.java`，而编译后的`.class`文件才是真正可以被JVM执行的字节码。因此，JVM需要知道，如果要加载一个`abc.xyz.Hello`的类，应该去哪搜索对应的`Hello.class`文件。



现在我们假设`classpath`是`.;C:\work\project1\bin;C:\shared`，当JVM在加载`abc.xyz.Hello`这个类时，会依次查找：

- <当前目录>\abc\xyz\Hello.class
- C:\work\project1\bin\abc\xyz\Hello.class
- C:\shared\abc\xyz\Hello.class

注意到`.`代表当前目录。如果JVM在某个路径下找到了对应的`class`文件，就不再往后继续搜索。如果所有路径下都没有找到，就报错。



`classpath`的设定方法有两种：

在系统环境变量中设置`classpath`环境变量，不推荐；

在启动JVM时设置`classpath`变量，推荐。

我们强烈*不推荐*在系统环境变量中设置`classpath`，那样会污染整个系统环境。在启动JVM时设置`classpath`才是推荐的做法。



## Java核心类

### String

`String`类还提供了多种方法来搜索子串、提取子串。常用的方法有：

```
// 是否包含子串:
"Hello".contains("ll"); // true
```

注意到`contains()`方法的参数是`CharSequence`而不是`String`，因为`CharSequence`是`String`的父类。

搜索子串的更多的例子：

```
"Hello".indexOf("l"); // 2
"Hello".lastIndexOf("l"); // 3
"Hello".startsWith("He"); // true
"Hello".endsWith("lo"); // true
```

提取子串的例子：

```
"Hello".substring(2); // "llo"
"Hello".substring(2, 4); "ll"
```

使用`trim()`方法可以移除字符串首尾空白字符。空白字符包括空格，`\t`，`\r`，`\n`：

```
"  \tHello\r\n ".trim(); // "Hello"
```

另一个`strip()`方法也可以移除字符串首尾空白字符。它和`trim()`不同的是，类似中文的空格字符`\u3000`也会被移除：

```
"\u3000Hello\u3000".strip(); // "Hello"
" Hello ".stripLeading(); // "Hello "
" Hello ".stripTrailing(); // " Hello"
```

`String`还提供了`isEmpty()`和`isBlank()`来判断字符串是否为空和空白字符串：

```
"".isEmpty(); // true，因为字符串长度为0
"  ".isEmpty(); // false，因为字符串长度不为0
"  \n".isBlank(); // true，因为只包含空白字符
" Hello ".isBlank(); // false，因为包含非空白字符
```

替换子串

要在字符串中替换子串，有两种方法。一种是根据字符或字符串替换：

```
String s = "hello";
s.replace('l', 'w'); // "hewwo"，所有字符'l'被替换为'w'
s.replace("ll", "~~"); // "he~~o"，所有子串"ll"被替换为"~~"
```

要把任意基本类型或引用类型转换为字符串，可以使用静态方法`valueOf()`。这是一个重载方法，编译器会根据参数自动选择合适的方法：

```
String.valueOf(123); // "123"
String.valueOf(45.67); // "45.67"
String.valueOf(true); // "true"
String.valueOf(new Object()); // 类似java.lang.Object@636be97c
```

String ->char[]

```java
//遍历字符串1
str.toCharArray() //把字符串转字符数组
    
//遍历字符串2
charAt(索引)
   
```



### 包装类型

我们把能创建“新”对象的静态方法称为静态工厂方法。`Integer.valueOf()`就是静态工厂方法，它尽可能地返回缓存的实例以节省内存。



## ArrayList

泛型：装在集合中的所有元素，全部是统一的什么类型

> 泛型只能是应用类型
>
> 从jdk1.7开始，右侧尖括号可以不写内容
>
> JDK 1.5+ 开始，开始支持自动装箱，自动拆箱

常用方法：

```java
public bool add(E e);

public E get(int index)
    
public E remove(int index)
    
public int size()
```

## 泛型

+ 实现类要想使用泛型，需要和接口的泛型一致
+ 泛型没有继承的概念

Java的泛型是采用擦拭法实现的；

擦拭法决定了泛型`<T>`：

- 不能是基本类型，例如：`int`；
- 不能获取带泛型类型的`Class`，例如：`Pair<String>.class`；
- 不能判断带泛型类型的类型，例如：`x instanceof Pair<String>`；
- 不能实例化`T`类型，例如：`new T()`。

泛型方法要防止重复定义方法，例如：`public boolean equals(T obj)`；

子类可以获取父类的泛型类型`<T>`。

### 泛型通配符

```java
public static void ArraySHow(ArrayList<?> arr){

    }
```



**高级使用**

泛型的上限限定： ？ extends E 代表使用的泛型只能是E类的子类/本身

泛型的上限限定： ？ super E 代表使用的泛型只能是E类的父类/本身

1. 使用类似`<? extends Number>`通配符作为**方法参数**时表示：

   - 方法内部可以调用获取`Number`引用的方法，例如：`Number n = obj.getFirst();`；

   - 方法内部无法调用传入`Number`引用的方法（`null`除外），例如：`obj.setFirst(Number n);`。

​	即一句话总结：使用`extends`通配符表示可以读，不能写。

2. 使用类似`<T extends Number>`定义**泛型类**时表示：
   - 泛型类型限定为`Number`以及`Number`的子类。



### 对比extends和super通配符

我们再回顾一下`extends`通配符。作为方法参数，`<? extends T>`类型和`<? super T>`类型的区别在于：

- `<? extends T>`允许调用读方法`T get()`获取`T`的引用，但不允许调用写方法`set(T)`传入`T`的引用（传入`null`除外）；
- `<? super T>`允许调用写方法`set(T)`传入`T`的引用，但不允许调用读方法`T get()`获取`T`的引用（获取`Object`除外）。

一个是允许读不允许写，另一个是允许写不允许读。



## 集合

Collection：所有集合最顶层的接口，有两个重要的集合：`List`集合,`Set`集合

共性方法：

```java
public bool add(E e)
public void clear()清空集合中所有元素
public bool remove(E e) 删除元素
public bool contains(E e) 判断集合中是否包含该对象
public bool isEmpty() 集合是否为空
public int size() 集合元素个数
public object[] toArray() 把集合元素存到数组中
```





### List(abstract)

+ 有序集合
+ 允许重复
+ 有索引，能用for遍历

常用特有方法：

- 在末尾添加一个元素：`boolean add(E e)`
- 在指定索引添加一个元素：`boolean add(int index, E e)`
- 删除指定索引的元素：E remove(int index)`
- 删除某个元素：`boolean remove(Object e)`
- 获取指定索引的元素：`E get(int index)`
- 获取链表大小（包含元素的个数）：`int size()`



**实现类：**

+ ArrayList

  + 实现不是同步的

+ LinkedList

  + 特有方法：

    ```java
    public void addFirst(E e)
    public void addLast(E e)
    public E getFirst()
    public E getLast()
    public E removeFirst()
    public E remove Last()
    public E pop() //头出
    public void push(E e)//头插
    public void addFirst(E e)
    ```

    

我们来比较一下`ArrayList`和`LinkedList`：

|                     | ArrayList    | LinkedList           |
| :------------------ | :----------- | :------------------- |
| 获取指定元素        | 速度很快     | 需要从头开始查找元素 |
| 添加元素到末尾      | 速度很快     | 速度很快             |
| 在指定位置添加/删除 | 需要移动元素 | 不需要移动元素       |
| 内存占用            | 少           | 较大                 |

通常情况下，我们总是优先使用`ArrayList`。



### Set

+ 无重复
+ 无索引



**HashSet实现类：**

+ 特点
  + 底层是一个哈希表结构（查询速度非常快）
  + 1.8之前：哈希表 = 数组 +链表
  + 1.8之后：哈希表 = 数组+红黑树
  + 哈希表特点：查询速度快



**哈希值**

+ 十进制整数，由系统随机给出（就是系统对象的地址值，是模拟得到的地址，不是数据实际存储的物理地址）

![image-20210316013927130](C:\Users\y\AppData\Roaming\Typora\typora-user-images\image-20210316013927130.png)

**Set集合储存元素不重复的原理**

add方法会调用hashCode和equals方法判断是否重复

> 前提：存储的元素必须重写hashCode和equals方法



**LInkedHashSet实现类**

+ 特点：底层是一个哈希表（数组+链表/红黑树）+链表：多了一条链表（记录元素的存储顺序）



###  迭代器 Iterator

+ Iterator迭代器是一个接口，需要是要Iterator接口的实现对象，获取方法比较特殊
+ Collection接口中有个方法叫iterator()，返回迭代器

方法

```java
bool hasNext() //判断是否还有元素可以迭代，有返回true
    E next() //迭代下一个元素
   void remove() //删除最后一个元素
```



### 增强for

也称for each循环，JDK1.5之后的新特性

底层使用的也是迭代器

用法

```java
for(集合数据类型 变量名：集合名){
    
```





### Map

+ 作为Key的元素，必须重写hashCode和equals

**实现类 HashMap**

+ 底层是哈希表，查询速度特别快
+ hashMap集合是一个无序集合，存储元素和取出元素的顺序有可能不一致

+ 

常用方法：

```java
V get(Object key)
    
V put(K key,V value) //若不重复，返回null

V remove(Object key)

boolean containsKey(Object key)
    
Set<K> keySet() //把Map集合中的所有key元素取出来存到Set集合中
    
Set<Map Entry(E,V)> entrySet()
```



Map.Entry(K,V)：是Map接口中有个一个内部接口Entry

作用：当一个Map集合一创建，就会在Map集合中创建一个Entry对象，用来记录键和值



**实现类 LinkedHashMap**

+ 底层使哈希表+链表，有序

**实现类 Hashtable**

特点：

+ 键值非空
+ 线程同步（单线程
+ Hashtable和Vector集合一样，在jdk1.2版本后，被更先进的集合（HashMap.ArrayList）取代了
+ Hashtable的子类Properties还在用





**JDK9集合优化（of)**

Java 9 添加了几种集合工厂方法，更方便创建少量元素的集合、map实例。新的List接口、Set接口、Map接口的静态工厂方法可以更方便创建集合的不可变实例。

注意：

+ of方法不适用于接口的实现类。
+ of方法的返回值是一个不能改变的集合，不能用add,put。

## 树蕨结构





### 红黑树

+ 趋近于平衡时，查询叶子节点的最大次数和最小次数不能超过2倍

约束：

+ 节点可以是红色或者黑色的
+ 根节点是黑色的
+ 叶子节点（空节点）都是黑色的
+ 每个红色节点的子节点都是黑色的
+ 任何一个节点到其每一个叶子节点的所有路径上的黑色节点数都是相同的



## 可变参数

> JDK1.5之后出现的新特性

使用前提：当方法的参数列表的数据类型已经确定，但参数个数不确定，则可以使用可变参数

使用：

```java
修饰符 返回值类型 方法名（数据类型... 变量名）{}
```

**底层原理：**

可变参数底层就是一个数组，根据传递参数个数的不同，会创建不同长度的数组，可以是0（不传递）

**注意事项：**

+ 一个方法的参数列表，只能有一个可变参数
+ 如果有多个参数，可变参数必须写在参数列表的末尾

**可变参数的特殊（终极）写法：**

```java
publc static void method(Object... obj)
```



## Collections集合工具类



+ static <T> boolean addAll(Collection<T> c,T... elements)
+ void shuffle(List<?> list)
+ sort(List<T> list)
  + 被排序的集合中的元素，必须实现Comparable，重写接口中的CompareTo

+ sort(List<T> list,Comparator<? super T>)



**Comparator和Comparable的区别**

+ Comparable：自己（this）和别人（参数比较）
+ Comparator：相当于找一个第三方的裁判，比较两个



## 异常

+ 异常指的是程序在执行过程中，出现非正常的情况，最终导致JVM非正常停止。

+ Java处理异常的方式是中断处理。
+ 遇到异常时，JVM会创建一个异常对象，包含异常产生的**内容，原因，位置**
+ 出现异常的方法没有异常处理的逻辑（try..catch),JVM就会把异常抛出
+ 父类方法没有抛出异常，子类重写该方法也不能抛出异常，只能捕获处理



异常体系：

**Throwable**

+ Error：不能处理，只能尽量避免
+ Exception（编译期异常）：使用不当导致，可以通过代码方式纠正
  + RuntimeException：运行期异常，java程序运行过程中出现的问题
    + NullPointerException



**Throwable**中定义了3个异常处理的方法

+ String getMessage()
+ String toString()
+ void printStackTrace() JVM打印异常对象，默认此方法，打印的信息最具体

**throw关键字**

使用格式

```java
throw new XXXException("异常产生的原因")
```

注意：

+ throw关键字必须写在方法内部
+ new的对象必须是Exception及其子类
+ 抛出的异常对象，必须处理
  + RuntimeException及其子类可不处理，交给JVM
  + 编译异常，必须处理，要么throws，要么try-catch



**Object类中的静态方法**

判断对象是否为空

```java
public static <T> T requireNoNull(T obj)
    public static <T> T requireNoNull(T obj,String message)
```



**throws关键字**

+ 必须写在方法声明处
+ 如果方法内部抛出多个异常对象，那么throws也必须声明多个异常对象，如果异常有父子类关系，直接声明父类异常即可。
+ 如果调用了一个声明抛出异常的方法，我们就必须处理异常

**try...catch**

+ 如果try中可能抛出多个异常，那么就要使用多个catch来处理这些异常
+ 多个catch时，如果有子父类关系，子必须在父前边

**finally**

无论是否出现异常都会执行

注意事项

+ 一般用于资源释放（IO）
+ finally里最好别有return语句



**自定义异常**

格式:

```java
public class XXXException extends Exception | RuntimeException{
    添加一个空参数的构造方法
    添加一个带异常信息的构造方法
}
```





## **多线程**

线程调度：

+ 分时调度

  所有线程轮流使用CPU

+ 抢占式调度

  优先级高的线程使用CPU,若相同，随机选一个，java为抢占式调度



**创建线程的方式1**

```
1.创建一个Thread类子类
2.在子类中重写run方法
3.创建Thread类的子类对象
4.调用Thread类的void start()方法，开启新的线程，JVM执行run方法
```

**创建线程的方式2**

```java
1.创建Runable接口实现类
2.重写Runable接口的run方法，设置线程任务
3.创建Thread类对象，构造方法中传递Runable接口的实现类对象
4.调用Thread类中的start方法。   

```



**常用方法**

```java
public String getName():获取当前线程的名称
public static void sleep(long millis)
public static Thread currentThread():返回当前正在执行的线程对象的引用
给线程改名1
void setNmae(String name) 
给线程改名2:搞个带参构造方法
myThread(String name){
    super(name);
}
```



**Runable创建多线程程序的好处：**

+ 避免了单继承的局限性
+ 增强了程序的扩展性，降低耦合性
  + 实现Runable接口的方式，把设置线程和开启新线程进行了解耦
+ 适合多个相同的程序代码的线程区共享同一个资源



**匿名内部类**

```java
 new Thread(new Runnable() {
            @Override
            public void run() {
                for (int i = 0; i < 5; i++) {
                    System.out.println(Thread.currentThread().getName()+"哈哈");
                }
            }
        }).start();
```



**线程同步**

+ 同步代码块
+ 同步方法
+ 锁机制



**同步代码块：**`synchronized`

格式：

```java
synchronized(同步锁){
    可能产生线程安全问题的代码块
}
```



**同步方法**：

格式：

```java
public synchronized void method(){
    代码
}
```



**接口Lock**

Lock实现提供了比使用synchronized方法和语句可获得更广泛的锁定操作。

方法：

```java
void lock()//获取锁
    
void unlock()//释放锁
```

使用步骤：

+ 在成员位置创建一个Reentrantlock对象
+ 在可能出问题的代码块前调用lock()
+ 后调用unlock()





**ReentrantLock和Synchronized**

ReentranLock公平锁实现

`new ReentranLock(true)`

**线程状态**

在Java程序中，一个线程对象只能调用一次`start()`方法启动新线程，并在新线程中执行`run()`方法。一旦`run()`方法执行完毕，线程就结束了。因此，Java线程的状态有以下几种：

- New：新创建的线程，尚未执行；
- Runnable：运行中的线程，正在执行`run()`方法的Java代码；
- Blocked：运行中的线程，因为某些操作被阻塞而挂起；
- Waiting：运行中的线程，因为某些操作在等待中；
- Timed Waiting：运行中的线程，因为执行`sleep()`方法正在计时等待；
- Terminated：线程已终止，因为`run()`方法执行完毕。



![image-20210318204510950](C:\Users\y\AppData\Roaming\Typora\typora-user-images\image-20210318204510950.png)



**线程通信**（等待与唤醒）

进入Timed_Wating有两种方式

+ sleep(long m)：计时结束后进入Runable/Blocked状态
+ wait(long m)：计时结束后，害妹被notify唤醒，就会自动醒来进入Runable/Blocked状态



**线程池**

一个容纳多个线程的容器，其中的线程可以反复使用，省去了频繁创建线程对象的操作，无需反复创建线程而消耗过多的资源

JDK1.5之后，JDK内置了线程池

好处

+ 降低资源消耗，减少了创建和销毁线程的次数，每个工作线程都可以被重复使用
+ 提高响应速度。当任务达到时，任务可以不需要等待线程的创建就能立刻执行。
+ 提高线程的可管理性。可以恩据系统的承受能力，调整线程池中的工作线程数目，防止因为消耗过多内存（每个线程大约需要1MB内存）



代码实现：

```java
   /*使用步骤：
    * 1.用线程池的工厂类Executors里边的newFixedThreadPool方法创建线程池
    * 2.搞一个类，实现Runable接口，设置线程任务
    * 3.调用ExecutorService(第一步返回值)的submit(runable)方法开启线程
    */
         ExecutorService executorService = Executors.newFixedThreadPool(5);
        executorService.submit(new myRun("haha1"));
```







## Lambda

使用前提

+ 使用Lambda必须具有接口，且接口中有且只有一个抽象方法。

  如JDK内置的`Runable`，`Comparator`接口还是自定义接口

+ 使用Lambda必须具有**上下文推断**

  也就是方法的参数或局部变量类型必须为Lambda对应的接口类型，才能使用Lambda作为该接口的实例

  > 备注：有且仅有一个抽象方法的接口，称为"函数式接口"。(FunctionalInterface)

  ```java
  @FunctionalInterface
  public interface Callable<V> {
      V call() throws Exception;
  }
  ```



## File类

+ File类与操作系统无关，任务操作系统都可以使用类的方法

重点：

+ file
+ directory
+ path

**File的四个成员变量：**

static String pathSeparator //路径分隔符 windows为 `;`,linux为`:`

static char pathSeparatorChar

separator //文件名称分隔符 windows为 `\`,linux为正斜杠`/`

separatorChar

**File类的构造方法：**

+ `File(String path)`
+ `File(String parent,String child)`
+ `File(File parent,String)`
  + 好处：父路径是File类，可以使用File类的一些方法对路径操作，再创建对象

**获取方法**

`String getAbsolutePath()` 返回此File的绝对路径

`String getPath()`返回构造函数的路径

`String getName()`返回结尾的文件或文件夹名字

`long length()` 返回File文件的大小,以字节为单位

**判断方法**

`boolean exists()`此File表示的文件/目录是否存在

`boolean isDirectory()`

`boolean isFile() ` 

**创建和删除方法**

`boolean createNewFile()`具有该名称的文件夹不存在，创建

`boolean delete()`删除File表示的文件/目录

+ 直接删除硬盘里的内容，不走回收站
+ 删除文件夹时，若文件夹内有内容，返回false

`boolean mkdir()` 创建目录

`boolean mkdirs()`父级目录不存在一并创建

**文件夹遍历**

`String[] list()`

`File[] listFiles()`

> 如果构造方法中的目录路径不存在，抛出空指针异常
>
> 隐藏文件夹也能遍历到

**文件过滤**

`File[] listFiles(FileFilter filter)`

+ FileFilter接口，用于抽象路径名（File对象）的过滤器

+ 抽象方法：用来过滤文件的方法

  + `boolean accept(File pathname)`

    File pathname：使用listFiles方法遍历目录，得到的每一个文件对象

`File[] listFiles(FilenameFilter filter)`

+ FilenameFilter接口，用于抽象路径名（File对象）的过滤器
+ 抽象方法：用来过滤文件的方法
  + `boolean accept(File dir,String name)`
  + dir:目录，name:目录中的文件名称

+ 用于过滤文件名称

> 注意：两个过滤器接口是没有实现类的1，需要我们自己写实现类

**递归**

注意：递归要有条件限定/次数限定，防止栈内存溢出

构造方法，禁止递归



## IO

|            |     输入流      |      输出流      |
| :--------: | :-------------: | :--------------: |
| **字节流** | **InputStream** | **OutputStream** |
| **字符流** |   **Reader**    |    **Write**     |



### 字节输出流OutputStream

作用：把内存中的数据写入到硬盘文件中

接口方法

| `void`          | `close()`  关闭此输出流并释放与此流相关联的任何系统资源。    |
| --------------- | ------------------------------------------------------------ |
| `void`          | `flush()`  刷新此输出流并强制任何缓冲的输出字节被写出。      |
| `void`          | `write(byte[] b)`  将 `b.length`字节从指定的字节数组写入此输出流。 |
| `void`          | `write(byte[] b,  int off, int len)`  从指定的字节数组写入 `len`个字节，从偏移 `off`开始输出到此输出流。 |
| `abstract void` | `write(int b)`  将指定的字节写入此输出流。                   |

**FileOutputStream**

构造方法

| Constructor and Description                                  |
| ------------------------------------------------------------ |
| `FileOutputStream(File file)`  创建文件输出流以写入由指定的 `File`对象表示的文件。 |
| `FileOutputStream(File file,  boolean append)`  创建文件输出流以写入由指定的 `File`对象表示的文件。 |
| `FileOutputStream(FileDescriptor fdObj)`  创建文件输出流以写入指定的文件描述符，表示与文件系统中实际文件的现有连接。 |
| `FileOutputStream(String name)`  创建文件输出流以指定的名称写入文件。 |
| `FileOutputStream(String name,  boolean append)`  创建文件输出流以指定的名称写入文件。 |

> 自带文件创建的方法

**写入原理：**

java程序 -->JVM--->OS（操作系统）-->OS调用写数据的方法-->把数据写入到文件中

**使用步骤：**

+ 创建对象
+ 调用方法write
+ 释放资源（流使用会占用一定的内存，使用完毕要把内存清空，提供程序的效率）

**方法**

`void write(byte[] b,int off,int len)`

+ 从off开始，把b[]的前len个字节写入到文件中

**追加写**

使用构造方法

`FileOutputStream(File file,  boolean append)`  创建文件输出流以写入由指定的 `File`对象表示的文件。 `append`为`true`表示追加写

**换行**

windows:\r\n

linux:/n



### 字节输入流InputStream

| Modifier and Type | Method and Description                                       |
| ----------------- | ------------------------------------------------------------ |
| `int`             | `available()`  返回从该输入流中可以读取（或跳过）的字节数的估计值，而不会被下一次调用此输入流的方法阻塞。 |
| `void`            | `close()`  关闭此输入流并释放与流相关联的任何系统资源。      |
| `void`            | `mark(int readlimit)`  标记此输入流中的当前位置。            |
| `boolean`         | `markSupported()`  测试这个输入流是否支持 `mark`和 `reset`方法。 |
| `abstract int`    | `read()`  从输入流读取数据的下一个字节。                     |
| `int`             | `read(byte[] b)`  从输入流读取一些字节数，并将它们存储到缓冲区 `b` 。 |
| `int`             | `read(byte[] b,  int off, int len)`  从输入流读取最多 `len`字节的数据到一个字节数组。 |
| `void`            | `reset()`  将此流重新定位到上次在此输入流上调用 `mark`方法时的位置。 |
| `long`            | `skip(long n)`  跳过并丢弃来自此输入流的 `n`字节数据。       |



**FileInputStraem**

`int read()`

`int read(byte[] b)`

+ 从输入流中读取一定数量的字节，并将其存储在缓冲区数组b中
+ byte数组起到缓冲的作用，可以存储每次读取的多个字节，数组长度一般定义为1024的整数倍



**使用字符流读取中文文件的问题**

1个中文：GBK占用两个字节

UTF-8占用三个字节







### 字符输入流Reader

抽象类

| Modifier and Type | Method and Description                                       |
| ----------------- | ------------------------------------------------------------ |
| `abstract void`   | `close()`  关闭流并释放与之相关联的任何系统资源。            |
| `void`            | `mark(int readAheadLimit)`  标记流中的当前位置。             |
| `boolean`         | `markSupported()`  告诉这个流是否支持mark（）操作。          |
| `int`             | `read()`  读一个字符                                         |
| `int`             | `read(char[] cbuf)`  将字符读入数组。                        |
| `abstract int`    | `read(char[] cbuf,  int off, int len)`  将字符读入数组的一部分。 |
| `int`             | `read(CharBuffer target)`  尝试将字符读入指定的字符缓冲区。  |
| `boolean`         | `ready()`  告诉这个流是否准备好被读取。                      |
| `void`            | `reset()`  重置流。                                          |
| `long`            | `skip(long n)`  跳过字符                                     |



**FileReader**

构造方法

`FileReader(String filename)`

`FileReader(File file)`



### **字符输出流Writer**

同OutputStream

**FileWriter**

字符输出流的使用步骤：

1. 创建FileWriter对象，构造方法中绑定要写入数据的目的地
2. **使用FileWriter中的方法write，把数据写入到内存缓冲区中（字符转字节的过程）**
3. **使用FileWriter中的方法flush,把内存缓冲区中的数据，刷新到文件中**
4. 释放资源（会先把内存缓冲区中的数据刷新到文件中）

和字节输出流不同



**异常在流中的使用**

**JDK7新特性**，try后边可以添加一个（），在括号中可以定义流对象

那么这个流对象作用域在try中有效

try中代码执行完毕，会自动把流对象释放，不用写finally

格式

```java
        try(FileReader fr = new FileReader("D:\\JavaDemo\\day08-code\\test.txt");) {
            int len = 0;
            char[] cs = new char[10];
            while ((len = fr.read(cs))!=-1){
                //   System.out.println(len);
                System.out.println(String.valueOf(cs,0,5));
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
```



**JDK9新特性**

在try前可以定义流对象

在try后边的（）中可以直接引用流对象的对象名

```java
A a = new A();
B b = new B();
try(a,b){
    代码
}catch(e){
    代码
}
```



### Properties

extends Hashtable<Object,Object>  implements Map(k,v)

`Properties`表示一个持久的属性集。可保存在流中或在流中加载。属性列表中每个

键及其值都是一个**字符串**。

+ Properties是一个双列集合，key和value默认都是字符串

常用方法

`void store(OutputStream out,String comments) `方法可以把集合中的临时数据，持久化写到硬盘中存储

`void store(Writer writer,String comments) ` 字符流，**能传输中文**，comments：注释，用来解释说明保存操作是干什么的，默认是Unicode编码，不能使用中文，一般使用`""`

**`void load(InputStream inStream/Reader reader)`把硬盘的文件（键值对）,读取到集合中使用**

+ **InputStream**流不能读取含有中文的键值对
+ 存储的键值对中，默认的键和值的连接为`-`,或者空格
+ 可以使用`#`添加注释

`void setProperty(String key,String key)`

`String getProperty(String key)`相当于map中的get

`Set<String>`  `stringPropertyNames()`  返回此属性列表中的一组键，其中键及其对应的值为字符串，包括默认属性列表中的不同键，如果尚未从主属性列表中找到相同名称的键。



### 缓冲流

缓冲流，也叫高效流，是对基本流的增强

+ **字节缓冲流**：`BufferedInputStream`,`BufferedOutputStream`
+ **字符缓冲流**：`BufferedReader`,`BufferedWriter`

原理：在创建流对象时，会创建一个内置的默认大小的缓冲区数组，通过缓冲区读写，减少IO次数，从而提高读写效率。



**BufferedOutputStream**

构造方法

`BufferedOutputStream(OutputStream out,[int size])`size指定缓冲流内部缓冲区大小

其他略



**BufferedInPutStream**

构造方法

同上

略



**BufferedWriter**

特有成员方法

`void newLine()` 写入一个行分隔符

**BufferedReader**

特有成员方法

`STRING readLine()` 读取一个文本行



**转换流**

编码：字符 --> 字节

+ **字符编码**：一套自然语言的字符与二进制数之间的对应规则。

+ **字符集**：也叫编码表，是一个系统支持的所有字符的集合。

**Unicode字符集**

+ 最多使用4字节的数字来表示每个字母、符号、或文字、有三种编码方案：
  + utf-8
  + utf-16
  + utf-32

**UTF-8编码**

使用1~4字节为每个字符编码

中文3个



**编码问题**

IDEA中使用`FileReader`读取文本文件时，由于Windows默认编码时GBK编码，会有乱码



### 转换流

**InputStreamReader**

该类是字节通向字符的桥梁，它用指定的**chartset**读取字节并**解码**为字符

使用时的注意事项：

构造方法中的编码表名称要和文件的编码相同，否则会乱码

**OutputStreamWriter**

将字符编码成字节

构造方法

`OutputStreamWriter(OutputStream out,String charsetName)`创建指定编码的转换流

+ charsetName不区分大小写，不指定默认utf-8

使用：

1. 创建**OutputStreamWriter**对象
2. 使用对象中的`write`方法，把字符转化为字节存储在缓冲区中
3. 使用`flush`方法,把内存缓冲区中的字节刷新到文件中
4. 释放资源



### **序列化流**

实现序列化的类需要实现`Serializable`接口,即给类添加一个标记

**ObjectOutputStream**

把对象存到文件中，叫对象的序列化

构造方法

`ObjectOutputStream(OutputStream out)`

特有的成员方法

`void writeObject(Object obj)`

**ObjectInputStream**

把文件中的对象，以流的方式读取出来，叫反序列化

特有的成员方法

`Object obj = readObject`



static关键字：

+ 静态优先于非静态加载到内存中（静态优先于对象进入到内存中）
+ 被static修饰的成员变量不能被序列化，序列化的都是对象

**transient关键字**

+ 被修饰的成员变量不能被序列化



**反序列化操作**

**Serializable**接口提供了一个序列版本号。`seriaVersionUID`该版本号目的在于验证序列化的对象和对应的类释放版本匹配。

+ `seriaVersionUID`是编译器（javac.exe）添加的

可自定义`serialVersionUID`

`static final long serialVersionUID = 15241;`



### 打印流PrintStream

+ 只负责数据的输出，不负责数据的读取
+ 永不会抛出IOException
+ 有特有的方法
  + print
  + println

注意：

+ 如果使用继承自父类的write方法写数据，那么查看数据的时候会查询编码表
+ 如果使用自己特有的方法print/println方法，写数据原样输出



使用System.setOut方法改变输入语句的目的地改为参数传递的打印流目的地

`static void setOut(PrintStream out)`



## **网络编程入门**

**软件结构**

+ C/S结构
+ B/S结构

**协议分类**

UDP

+ 数据被限制在64kb以内



### **TCP通信程序**

**java提供两类用于tcp通信的程序:**

客户端：`java.net.Socket`

服务的：`java.net.ServerSocket`

+ 连接包含一个IO对象
+ IO对象是字节流对象
+ 服务器使用客户端的IO流和客户端交互



**客户端Socket**

构造方法

`Socket(String host,int port)`

+ host：服务器主机名称/ip地址
+ port服务器端口号

使用方法

`OutputStream`  `getOutputStream()`  返回此套接字的输出流。 

`InputStream`  `getInputStream()`  返回此套接字的输入流。 



**服务端ServerSocket**

构造方法

`ServerSocket(int port)`  创建绑定到指定端口的服务器套接字。 

使用方法

`Socket accept()`



**HTTP编程**

![image-20210327171909330](C:\Users\y\AppData\Roaming\Typora\typora-user-images\image-20210327171909330.png)







## 函数式接口

函数式接口在java中是指：有且仅有一个抽象方法的接口。

函数式接口，即适用于函数式编程场景的接口，java中的体现是Lambda。

格式：

```java
修饰符 interface 接口名称{
    public abstract 返回值类型 方法名称（参数）;
    //其他非抽象方法内容（默认，静态，私有）
}
```

其中的`public abstract`可省略

注解`@FunctionalInterface`

作用：可以检测接口是否是一个函数式接口

> lambda表达式比匿名内部类高些，因为编译之后不会生成Demo$1.class文件



**lambda表达式的延迟执行**

~



## 常用的函数式接口

在`java.util.function`中

**Supplier接口**

`Supplier<T>`包含一个无参方法：`T get()`。用来获取一个泛型参数指定类型的对象数据。

+ supplier称为生产型接口

**Consumer接口**

`Consumer<T>`包含一个抽象方法`void accept(T t)`,意为消费一个数据



+ **默认方法：andThen**

  如果一个方法的参数和返回值都是`Consumer`类型的，那么就可以实现效果：消费数据的时候，首先做一个操作，然后做一个操作，实现组合。而这个方法就是`Consumer`接口中的default方法`andThen`。



**Predicate接口**

`Predicate<T>`包含一个抽象方法：`boolean test(T t)`。other用于条件判断的场景。

+ **默认方法 and**

  `dafult Predicate<T> and (Pridicate<? super T) other{}`

+ **默认方法 or**

  略

+ **默认方法:negate**

  略



**Function接口**

`Function<T,R>`包含一个抽象方法：`R apply(T,t)`用来根据一个类型的数据得到另一个类型的数据

**默认方法 andThen**

和consumer的andThen类似



## Stream流

Stream流是jdk1.8出现的，关注的是做什么，而不是怎么做

> 得益于Lambda的延迟执行特性。Stream流其实是一个集合元素的函数模型，它并不是集合，也不是数据结构，其本分并不存储任何元素/地址。

Stream（流）是一个来自数据源的元素队列

+ 元素是特定类型的对象（有泛型） 
+ **数据源**流的来源。可以说集合，数组等。

和以前的Collection操作不同，Stream操作还有两个基础特征：

+ Pipelining：中间操作都会返回流对象本身。这样操作可以串联成一个管道，如同流式风格。

+ 内部迭代：以前对集合遍历时通过Iterator或者增强for的方式，显式在集合外部进行迭代，这叫做外部迭代。Stream提供了内部迭代的方式，流可以直接调用遍历方法。



### 获取流

`java.util.stream.Stream<T>`是 Java 8 新加入的最常用的流接口。（不是函数式接口）。

获取流的方式：

+ 所有的`Collection`集合都可以通过`Stream()`默认方法获取流
+ `Stream`接口的静态方法`of`可以获取数组对应的流

### 常用方法

+ **延迟方法**：返回值是`Stream`接口自身类型的方法，因此支持链式调用

+ **终结方法**：返回值不是`Stream`接口的方法，如`count`和`forEach`



**逐一处理：forEach**

`void forEach(Consumer<? super T> action);`

**过滤：filter**

`Stream<T> filter(Predicate<? super T) predicate`



> Stream流属于管道流，只能被消费（使用）一次
>
> 第一个Stream流调用完毕方法，数据就会流转到下一个Stream上
>
> 第一个Stream流就不能再调用方法了 

**映射：Map**

`<R> Stream<R> map(Function<? super T, ? extends R> mapper);`



**统计个数：count**

终结方法

正如`collection`中的`size`方法一样，流提供`count`方法来计算流中的元素个数：

`long count()`;



**截取：limit**

`Stream<T> limit(long maxSize);`



**跳过前n个：skip**

如果当前流长度大于n，怎跳过前n个，否则得到长度为0的空流。

`Stream<T> skip(long n);`



**组合：concat**

如果有两个流，希望合并成为一个流，那么可以使用`Stream`接口的静态方法`concat`：

`static <T> Stream<T> concat(Stream<? extends T> a,Stream<? extends T> b)`



## **方法引用**

主要是对lambda表达式的优化

```java
//printString(Printable p) printtable为函数式接口
//直接调用现有对象中的方法，不用lambda表达式
printString(System.out::print)
```

`::`称为引用运算符，而它所在的表达式称为**方法引用**，如果lambda要表达的函数方案已经存在于某个方法的实现中，那么可以通过双冒号来引用该方法作为Lambda的替代者



**通过对象名应用成员方法**

```java
 //obj为已经存在的对象
// printprintUpperCaseString为已经存在的方法
  printString(obj::printUpperCaseString)  
```



**通过类名引用静态成员方法**



**通过super引用成员方法**

```java
//其中method的参数是个函数式接口
method(super::sayHello);
```

**使用this引用本类的成员方法**



**类的构造器引用**

```java
printName("haha",Person::new);
```

**数组构造器引用**

```java
createArrays(10,int[]::new);
```



> 就是参数为函数式接口的地方可以用现有的方法来替代lambda表达式（要求该方法参数和接口定义的方法的参数一致）





## Junit单元测试

测试分类：

+ 黑盒测试
+ 白盒测试
  + Junit是白盒测试的一种

**Junit的使用：白盒测试**

**步骤**：

1.定义一个测试类（测试用例）

+ 建议：
  + 测试类名：被测试的类名+Test
  + 包名：xxx.xxx.xx.test

2.定义测试方法：可以独立运行

+ 建议：
  + 方法名：test+测试的方法名 `testAdd()`
  + 返回值：`void`
  + 参数列表：空参

3.给方法加@Test

4.导入JUnit依赖



**结果判定：**

​	**断言**

`Assert.assertEquals(期望值,真实值)`



@Before

+ 用于申请资源，所有的测试方法执行之前都会先执行该方法

@After

+ 用于释放资源

## 反射

框架设计的灵魂



![image-20210329172133513](C:\Users\y\AppData\Roaming\Typora\typora-user-images\image-20210329172133513.png)

反射：将类的各个组成部分封装为其他对象。

**好处**

+ 可以在程序运行过程中，操作这些对象。
+ 可以解耦

**获取Class对象的方式**

+ Class.forName("全类名")：将字节码文件加载进内存，返回Class对象
  + 多用于配置文件中，将类名定义在配置文件中
+ 类名.class：通过类名的属性class来获取
  + 多用于参数的传递
+ 对象名.getClass()：在object类中定义
  + 多用于对象获取字节码的方式

> 同一个字节码文件（*.class）在一次程序运行过程中，只会被加载一次。

**Class对象的功能**

* 获取功能：

  1. 获取成员变量

     `Field[] getFields()`  获取所有public修饰的成员变量

     `Field getField(String name)`

     `Field[] getDeclaredFields()`  获取所有成员变量

     `Field getDeclaredField(String name)`

  2. 获取构造方法

     `Constructor<?>[] getConsstructors()`

     `Constructor<?> getConsstructor(类<?>... oarameterTyes)`

     创建方法

     + `constructor.newInstance(...)`
     + 可以简化:Class对象的newInstance()

     其他同上

  3. 获取成员方法

     - `Method getMethod(name, Class...)`：获取某个`public`的`Method`（包括父类）
     - `Method getDeclaredMethod(name, Class...)`：获取当前类的某个`Method`（不包括父类）
     - `Method[] getMethods()`：获取所有`public`的`Method`（包括父类）
     - `Method[] getDeclaredMethods()`：获取当前类的所有`Method`（不包括父类）

     **执行方法**

     + `Object invoke(Object obj,object ... args)`
     + `invoke`的第一个参数是对象实例，即在哪个实例上调用该方法，后面的可变参数要与方法参数一致，否则将报错。
     + 如果获取到的Method表示一个静态方法，调用静态方法时，由于无需指定实例对象，所以`invoke`方法传入的第一个参数永远为`null`。

     ```java
     public class Main {
         public static void main(String[] args) throws Exception {
             // 获取Integer.parseInt(String)方法，参数为String:
             Method m = Integer.class.getMethod("parseInt", String.class);
             // 调用该静态方法并获取结果:
             Integer n = (Integer) m.invoke(null, "12345");
             // 打印调用结果:
             System.out.println(n);
         }
     }
     ```

     

  4. 获取类名（包括包名）

     + `String getName()`

* 暴力反射`Field d.setAccessible(true)`：忽略修饰符的安全检查



## 动态代理

作用：只用写个接口，运行时动态创建该接口的实例（class）

使用步骤：

1. 定义一个`InvocationHandler`实例，它负责实现接口的方法调用；
2. 通过`Proxy.newProxyInstance()`创建`interface`实例，它需要3个参数：
   1. 使用的`ClassLoader`，通常就是接口类的`ClassLoader`；
   2. 需要实现的接口数组，至少需要传入一个接口进去；
   3. 用来处理接口方法调用的`InvocationHandler`实例。
3. 将返回的`Object`强制转型为接口。



## 补充：

`class.getSimpleName()`：返回类名





## 

## 注解

注解（Annotation）也叫元数据。

作用：

+ 编写文档：【生产doc文档】
+ 代码分析：【使用反射】
+ 编译检查：【override】

文档生成：在控制台输入`javadoc xxx.java` 

**JDK中预定义的一些注解**

+ @Override：检查被标注的方法是否继承自父类
+ @Deprecated：将该注解标准的内容已弃用
+ @SuppressWarnings("all")：压制警告

**自定义注解**

格式：

+ 元注解

  `public @interface 注解名称`

本质：本质上是一个接口，该接口默认继承Annotation接口

属性：接口中可以定义的抽象方法

+ 要求：

  1. 属性的返回值类型

     + 基本数据类型
     + String
     + 枚举
     + 注解
     + 以上类型的数组

  2. 定义了属性，在使用时需要给属性赋值,如果使用default关键字给默认属性赋值，则使用注解时，可不进行属性的赋值

     + 如果只有一个属性需要赋值，属性名可省略

     `int age() default 5`

+ 元注解：用于描述注解的注解
  + @Target：描述注解能作用的位置
  + @Retention：描述注解被保留的阶段
    + @Retention(RetentionPolicy.RUNTIME)：当前被描述的注解，会保留到class字节码文件中，被JVM读取到
  + @Documented：描述注解是否被抽取到api文档中
  + @Inherited：描述注解是否被子类继承

**反编译**

`javap xxx.class`

**在程序使用（解析）注解：获取注解中定义的属性值**

1. 获取注解定义位置的对象（Class,Method,Field)

2. 获取指定的注解：`*.getAnnotation(注解名.class)`

   > 此步骤会返回一个注解接口的实现类

3.调用注解中的抽象方法获取配置的属性值



### **小结**

+ 以后大多数时候，我们会使用注解，而不是自定义注解
+ 注解给谁用
  + 编译器
  + 解析程序用
+ 注解不是程序的一部分
  + 可以理解为标签





## 数据库DataBase

**数据库的基本概念**

特点：

1. 持久化存储数据
2. 方便存储和管理数据
3. 使用统一的方式操作数据库 --SQL

**MySQL数据库软件**

1. 安装
2. 卸载
3. 配置
   + MySQL服务（服务：没有界面的应用程序）
   + 关闭服务：net stop mysql80
   + 开启：net start mysql80
4. 登录
   + mysql -uroot -p密码
5. 退出
   + exit
   + quit
6. 远程登录
   + mysql -h127.0.0.1 -uroot -p
   + mysql --host =127.0.0.1 --user=root --password=
7. MYSQL目录结构
   + MYSQL安装目录
   + MYSQL数据目录

### SQL

+ Structured Query Language：结构化查询语言
+ 定义了关系型数据库的操作规则

**SQL通用语法**

+ ，语句不区分大小写，关键字建议大写
+ 注释
  + 单行注释： `-- 注释内容 `或`# 注释内容 
  + 多行注释 `/* */`

```sql
show databases; //查询有几个数据库
```



**SQL分类**

1. DDL（Data Definition Language）数据定义
   + 关键字`create`、`drop`、`alter`等
2. DML（Data Manipulaton Language）数据操作
   + `insert`、`delete`、`update`等
3. DQL（Data Query Language）数据查询
   + `select`、`where`
4. DCL（Data Control Language）数据控制



### 1.DDL

**数据库操作**



1. C创建

   + `create database if not exists db2 character set gbk;`

2. R查询

+ `show database;`

+ `show create database;`查询数据库创建语句
3. U修改

+ `alter database 名称 character set`修改数据库字符集

4. D删除

+  `drop database if exits 数据库名称`判断是否存在再删除

5. 使用数据库
   + `select database(); `查询当前正在使用的数据库名称
   + `use db1;`进入db1数据库

**表操作**

1. 创建

   + `create table 表名(列名1 数据类型1，列名2 数据类型2，...列名n 数据类型n)`创建表

     数据类型：

     1. int
     2. double(5,2) 数一共有5位，保留2位 例：999.99
     3. date：日期，只包含年月日，yyyy-MM-dd
     4. datetime：包含年月日时分秒 yyyy-MM-dd HH:mm:ss
     5. timestamp：时间戳，如果不给这个字段赋值，默认赋值当前的时间
     6. varchar(n)：字符串，最大n

2. 查询

   + `show tables;`查看所有表名称
   + `desc 表名` 查询表名

   

3. 修改

   + `alter table tablename rename to new name`修改表名
   + `alter table name character set utf8;` 修改字符集
   + `alter table 表名 add 列名 数据类型`     添加一列
   + `alter table 表名 change gender sex varchar(20)` 修改列名称
     + `alter table 表名 modify sex varchar(10)`只改数据类型
   + 删除列

4. 删除

   + `drop table if exists 表名`

5. 操作表

   + `create table stu like student;`复制表



**Demo**

```sq
创建表
create table student(id int,name varchar(32), age int,score double(4,1),birth date, insert_time timestamp);
```

UPDATE TABLE "2021客运国庆数据_v2" 
set timestamp1 = extract(epoch from cast (substr(train_date, 1,4)||'-'||substr(train_date, 5,2)||'-'||substr(train_date, 8)||' '||substr(start_time, 1 ,2)||':'||substr(start_time, 3 ,2)||':'||substr(start_time, 5 ,2) as TIMESTAMP))



update table test1 set timestamp1 = t1.stp
from (
select extract(epoch from cast (substr(train_date, 1,4)||'-'||substr(train_date, 5,2)||'-'||substr(train_date, 8)||' '||substr(start_time, 1 ,2)||':'||substr(start_time, 3 ,2)||':'||substr(start_time, 5 ,2) as TIMESTAMP)as stp)
from test1) t1



select  
	case substr(train_date, 1,4)||'-'||substr(train_date, 5,2)||'-'||substr(train_date, 7,2)||' '||substr(start_time, 1 ,2)||':'||substr(start_time, 3 ,2) >= '2022-09-05 08:11'
from "2021客运国庆数据_v2"

### 2.DML

1. 添加数据
+ `insert into 表名(列名1，列名2，...，列名n) values (值1，值2，...，值n)；`
2. 删除数据

   + `delete from 表名 [where 条件] `

     > 不加条件则全部删除

   + `truncate table 表名` 删除表，并创建一个一模一样的表

3. 修改数据

   + `update 表名 set 列名1 = 值1，列名2 = 值2 where 条件`

### **3.DQL**

+ `select * from 表名`

1. **语法：**

```sql
select
	字段列表
from
	表名列表
where
	条件列表
group by
	分组字段
having
	分组之后的条件
order by
	排序
limit
	分页限定

```



2. **基础查询**

   + 多字段查询

     ```sql
     select
     	字段1 -- 注释
     	字段2 --注释
     from
     	表名
     ```

     

   + 去除重复:`distinct`

     ```sql
     select distinct 字段名 from 表名
     ```

     

   + 计算列

     一般只进行数值型计算

     ifnull(表达式1，表达式2)

     ```sql
     select name,math,enghlish math+ifnull(english,0) from tablename
     ```

     

   + 起别名 `as`

     ```sql
     select name,math,enghlish math+ifnull(english,0) as total from tablename
     select name,math,enghlish math+ifnull(english,0)  total from tablename
     ```

3. **条件查询**

   + 运算符： <>不等与

   + BETWEEN...AND... 在范围内，如between 100 and 200 ,包头包尾

   + IN(集合)：集合表示多个值，用逗号分隔

     ```sql
     SELECT * FROM student WHERE age IN (22,18,25)
     ```

   + LIKE ‘张%’：模糊查询

     + `_` ：单个任意字符
     + `%` ：多个任意字符，包括0

   + IS NULL `IS NOT NULL`

   + and

   + or

   + not或 ！

Demo

```sql
CREATE TABLE student3 id int,--编号
name varchar (20) , --姓名age int, --年龄
sex varchar (5) , --性别
address varchar (100) , --地址math int, --数学
english int --英语);
INSERT INTO student(id,NAME, age, sex,address , math, english) VALUES 
(1,'马云' ,55,'男','杭州' ,66,78),
(2,'马化腾',45,'女','深圳',98,87),
(3,'马景涛',55,'男','香港',56,77),
(4,'柳岩',20,'女','湖南',76,65),
(5,'柳青',20,'男','湖南',86,NULL),
(6,'刘德华',57,'男','香港',99,99),
(7,'马德',22,'女','香港',99,99),
(8,'德玛西亚',18,'男','南京',56,65);
```



4. **排序查询**

   ```sql
   order by 排序字段1 排序方式1 , 排序字段2 排序方式2, ...
   ```

   + 排序方式
     + asc 升序默认
     + desc 默认

5. **聚合函数**：将一列数据作为一个整体，进行纵向计算,排除null

   + count：一般计算非空列（主键）
   + max
   + min
   + sum
   + avg

   ```sql
   SELECT COUNT(NAME),SUM(english) FROM student
   ```

   

6. **分组查询**

   + 注意：分组之后查询字段：分组字段，聚合函数字段

   ```sql
   SELECT gender,sum(math) FROM student group by gender
   ```

   

7. **分页查询**

   + 语法

     ```sql
     limit 开始索引,每页查询条数;
     ```

     开始索引 = (当前页码-1）*每页条数

     分页操作是一个“**方言**”

8. **where和having的区别**

   + where在分组之前进行限定，如果不满足条件，则不参与分组。Having在分组之后进行限定，如果不满足条件不会被查询出来
   + where后不可以跟聚合函数，having可以进行聚合函数的判断

### 约束

概念：对表中的数据进行限定，从而保证数据的正确性，完整性和有效性。

分类：

+ 主键约束：primary key

  + 含义：非空且唯一
  + 一张表只能有一个字段为主键
  + 删除

  ```sql
  alter table stu drop primary key
  ```

  + 自动增长
    + 如果某一类是数值类型的，使用auto_increment

+ 非空：not null

+ 唯一：unique（null除外）

  + 删除唯一约束

    ```sql
    alter table stu drop index 列名 
    ```

+ 外键：foreign key

数据冗余时，表单拆分

**语法：**

```sql
create table 表名(
...
    外键列,
    constraint 外键名称 foreign key (外键列名称) references 主表名称(主表主键名称)

)
```

**删除外键**

```sql
alter table employee drop foreign key 外键名称
```

**添加外键**

```sql
alter table employee add constraint 外键名称 foreign key(外键列名称) references 主表名称（主表主键名称）
```

**级联操作**

+ 更新/删除

```sql
alter table employee add constraint 外键名称 foreign key(外键列名称) references 主表名称（主表主键名称） on update cascade on delete cascade;
```



DEMO

```sql
CREATE TABLE emp ( -- 创建emp表
id INT PRIMARY KEY AUTO_INCREMENT,NAME VARCHAR (30),
age INT,
dep_name VARCHAR(30) ,-- 部门名称
dep_location VARCHAR(30)-- 部门地址
    );

INSERT INTO emp (NAME,age,dep_name,dep_location) VALUES ('张三',20,'研发部','广州');
INSERT INTO emp (NAME,age,dep_name,dep_location) VALUES ('李四',21,'研发部','广州');
INSERT INTO emp (NAME,age,dep_name,dep_location) VALUES ('王五',20,'研发部','广州');
INSERT INTO emp (NAME,age,dep_name,dep_location) VALUES ('老王',20,'销售部','深圳');
INSERT INTO emp (NAME,age,dep_name,dep_location) VALUES ('大王',22,'销售部','深圳');
INSERT INTO emp (NAME,age,dep_name,dep_location) VALUES ('小王',18,'销售部','深圳');



```



拆分成两张表

```sql
-- 一方，主表
CREATE TABLE department(
id INT PRIMARY KEY AUTO_INCREMENT,
dep_name VARCHAR(20),
dep_location VARCHAR(20)
);
-- 多方，从表
CREATE TABLE employee(
id INT PRIMARY KEY AUTO_INCREMENT,
NAME VARCHAR(20),
age INT,
dep_id INT,
CONSTRAINT emp_dept_fk FOREIGN KEY(dep_id) REFERENCES department(id)); -- 外键对应主表的主键


INSERT INTO department VALUES(NULL,'研发部','广州'),(NULL,'销售部','深圳');

INSERT INTO employee (NAME,age,dep_id) VALUES ('张三',20,1);
INSERT INTO employee (NAME,age,dep_id) VALUES ('李四',21,1);
INSERT INTO employee (NAME,age,dep_id) VALUES ('王五',22,1);
INSERT INTO employee (NAME,age,dep_id) VALUES ('大王',20,1);
INSERT INTO employee (NAME,age,dep_id) VALUES ('小王',222,1);
INSERT INTO employee (NAME,age,dep_id) VALUES ('王佩',18,1);
```





### 4.DCL

管理用户，授权

SET PASSWORD FOR 'lin'@'localhost' = PASSWORD('0o0occc');

	1. 管理用户
		1. 添加用户：
			* 语法：CREATE USER '用户名'@'主机名' IDENTIFIED BY '密码';
		2. 删除用户：
			* 语法：DROP USER '用户名'@'主机名';
		3. 修改用户密码：
			
			UPDATE USER SET PASSWORD = PASSWORD('新密码') WHERE USER = '用户名';
			UPDATE USER SET PASSWORD = PASSWORD('abc') WHERE USER = 'lisi';
			
			SET PASSWORD FOR '用户名'@'主机名' = PASSWORD('新密码');
			SET PASSWORD FOR 'root'@'localhost' = PASSWORD('123');
	
			* mysql中忘记了root用户的密码？
				1. cmd -- > net stop mysql 停止mysql服务
					* 需要管理员运行该cmd
	
				2. 使用无验证方式启动mysql服务： mysqld --skip-grant-tables
				3. 打开新的cmd窗口,直接输入mysql命令，敲回车。就可以登录成功
				4. use mysql;
				5. update user set password = password('你的新密码') where user = 'root';
				6. 关闭两个窗口
				7. 打开任务管理器，手动结束mysqld.exe 的进程
				8. 启动mysql服务
				9. 使用新密码登录。
		4. 查询用户：
			-- 1. 切换到mysql数据库
			USE myql;
			-- 2. 查询user表
			SELECT * FROM USER;
			
			* 通配符： % 表示可以在任意主机使用用户登录数据库
	
	2. 权限管理：
		1. 查询权限：
			-- 查询权限
			SHOW GRANTS FOR '用户名'@'主机名';
			SHOW GRANTS FOR 'lisi'@'%';
	
		2. 授予权限：
			-- 授予权限
			grant 权限列表 on 数据库名.表名 to '用户名'@'主机名';
			-- 给张三用户授予所有权限，在任意数据库任意表上
			
			GRANT ALL ON *.* TO 'zhangsan'@'localhost';
		3. 撤销权限：
			-- 撤销权限：
			revoke 权限列表 on 数据库名.表名 from '用户名'@'主机名';
			REVOKE UPDATE ON db3.`account` FROM 'lisi'@'%';


## 数据库设计 

#### 多表之间的关系

1. 1：1
   + 实现，可在任意地添加外键指向另一方，外键唯一
2. 1：n
   + 实现：在多的一方建立外键，指向一的一方的主键
3. m:n
   + 实现：需要借助中间表，最少2个字段，分别指向两张表的主键，这两个字段作为外键外别指向两张表的主键

demo

```sql
CREATE TABLE tab_category(   -- 旅游分类表
 cid INT PRIMARY KEY AUTO_INCREMENT,
 cname VARCHAR(100) NOT NULL UNIQUE);
 
 
 CREATE TABLE tab_route( -- 线路表
 
 rid INT PRIMARY KEY AUTO_INCREMENT,
 rname VARCHAR(100) NOT NULL UNIQUE,
 price DOUBLE,
 rdate DATE,
 cid INT,
 FOREIGN KEY (cid) REFERENCES tab_category(cid));
 
 CREATE TABLE tab_user( -- 用户表
 uid INT PRIMARY KEY AUTO_INCREMENT,
 username VARCHAR(100) UNICODE NOT NULL,
 PASSWORD VARCHAR(15) NOT NULL,
 NAME VARCHAR(100),
 birthday DATE,
 sex CHAR(1) DEFAULT '男',
 telephone VARCHAR(11),
 email VARBINARY(100)
 );
 
 CREATE TABLE tab_favorite(
  rid INT,
  DATE DATETIME,
  uid INT,
  -- 创建复合主键
  PRIMARY KEY(rid,uid),
  FOREIGN KEY(rid) REFERENCES tab_route(rid),
  FOREIGN KEY(uid) REFERENCES tab_user(uid)
  );
```



#### 数据库设计范式

+ 设计数据库时需要遵循的一些规范。
+ 目前关系型数据库有六种范式：第一范式，第二范式、第三范式、巴斯-科德范式、第四范式、第五范式

1. 第二范式:非码必须完全依赖于候选码（码）（消除部分依赖）

   + 几个概念

     + 函数依赖：a-->b如果通过a属性（属性组）能唯一确定b的值，则称b依赖与a

       > 例如：学号-->姓名

     + 完全函数依赖：a是一个是属性组~

       > （学号，课程名称）-->分数

     + 部分函数依赖：a是一个属性组的部分属性唯一确定b的值

     + 传递函数依赖

     + 码：如果一个属性或属性组，被其他属性所完全依赖，则称这个属性为该表的码。

       +  主属性：码属性
       + 非主属性：其他

2. 第三范式：任何非主属性不依赖于其他非主属性（消除传递依赖）

#### **备份和还原**

1. 命令行：

   备份：`mysqldump -u用户名 -p密码 数据库名 > 保存的路径`

   还原：

   1. 登录数据库
   2. 创建数据库
   3. 使用数据库
   4. 执行文件。 source 文件路径

   

## 多表查询

语法

```sql
select
	列名列表
from
	表名列表
where
 	...
```

### 

DEMO

```sql
select * from emp,dep;
-- 笛卡尔积 a,b集合的组合
```



### 多半查询分类

+ 内连接查询

  + 隐式内连接：使用where条件消除无用数据

    ```sql
    SELECT * FROM emp,dept WHERE emp.dept_id = dept.id
    ```

  + 显示内连接

    ```sql
    select 字段列表 from 表名1 【inner】 join 表名2 on 条件
    ```

    

+ 外链接查询

  + 左外连接：查询的是左表所有数据以及和右表的交集

  ```sql
  select 字段列表 from 表名1 left [outer] join 表名2 on 条件
  ```

  

  + 右外连接

    同理

+ 子查询

  概念：查询种套查询

  ```sql
  SELECT emp.*,dept.`name` FROM emp INNER JOIN dept ON dept.`id` = emp.`dept_id` WHERE salary = (SELECT MIN(salary) FROM emp);
  ```

  不同的情况：

  + 子查询是单行单列的
  + 子查询是多行单列的
  + 子查询是多行多列的
    + 子查询可以作为一张虚拟表



练习：

```sql
-- 查询所有员工编号，姓名，工资，植物名称，职务描述
SELECT 
	emp.`id`,emp.`ename`,emp.`salary`,job.`jname`,job.`description`
FROM
	emp, job
WHERE
	job_id = job.`id`
	
```

```sql
-- 查询员工姓名，工资，工资等级
SELECT 
	emp.`ename`,emp.`salary`,salarygrade.`grade`
FROM
	emp,salarygrade
WHERE
	emp.`salary`>= losalary AND emp.`salary` <= hisalary
-- between ... and ...
```

```sql
-- 查询员工编号，员工姓名，工资，职务名称，职务描述，部门名称，部门位置
SELECT 
	emp.`id`,emp.`ename`,emp.`salary`,job.`jname`,job.`description`,dept.`dname`,dept.`loc`
FROM
	emp, job,dept
WHERE
	job_id = job.`id` AND emp.`dept_id` = dept.`id`
	

```

```sql
-- 5.查询部门编号，部门名称，部门位置，部门人数
SELECT
	dept.`id`,dept.`dname`,dept.`loc`,COUNT(emp.`dept_id`) total
FROM
	dept -- 左外查询
LEFT JOIN	
	emp
ON
	dept.`id` = emp.`dept_id`
GROUP BY 
	dept.`id`
	

```



```sql
-- 6.查询所有员工的姓名及其直接上级的姓名，没有领导的员工也需要查询

SELECT 
	t1.ename,
	t1.mgr,
	t2.id,
	t2.ename

FROM
	emp t1 , emp t2
WHERE	t1.mgr = t2.id;
```

```sql
SELECT 
	t1.ename,
	t1.mgr,
	t2.id,
	t2.ename

FROM
	emp t1 LEFT JOIN emp t2
ON	t1.mgr = t2.id;
```

## 事务

### 事务的基本介绍

1. 概念：

   如果一个包含多个步骤的业务操作，被事务管理，那么这些操作要么同时成功，要么同时失败。

2. 操作

   + 开启事务：start transaction
   + 回滚：rollback
   + 提交：commit
   
   ```sql
   
   START TRANSACTION;
   UPDATE account SET balance =  balance - 500 WHERE NAME = "zhangsan";
   
   
   UPDATE account SET balance =  balance + 500 WHERE NAME = "lisi";
   
   COMMIT;
   
   ROLLBACK;
   ```

3. Mysql数据库中事务默认自动提交（oracle是手动提交事务

   + 一条增删改会默认提交一次事务

   + 修改事务的默认提交方式

     ```sql
     select @@autocommit; -- 1默认提交 0手动提交
     ```



### 事务的四大特征

1. 原子性：是不可分割的最小操作单位。
2. 持久性：当事务提交或回滚后，数据库会持久化的保存数据。
3. 隔离性：多个事务之间，相互独立。
4. 一致性：事务操作前后事务总量不变。

### 事务的隔离级别（了解）

概念：多个事务之间是隔离的，相互独立的，如果多个事务操作同一批数据，则会引发一些问题，设置不同的隔离级别就可以解决这些问题。

存在的问题：

1. 脏读：一个事务，读取到另一个事务中没提交的数据
2. 不可重复读（虚读）：在同一个事务中，两次读取到的数据不一样。
3. 幻读：一个事务操作（DML）数据表中所有记录，而另一个事务添加了一条数据，则第一个事务查询不到自己的修改。



隔离级别：

1. read uncommiteed：读未提交
   + 产生的问问题： 脏读，不可重复读，幻读

2. read committed：读已提交（oracle默认级别）
   + 产生的问问题：不可重复读，幻读
3.  repeatable read：可重复读（MySQL默认级别）
   + 产生的问题：幻读
4. serializable：串行化
   + 能解决所有问题

> 隔离级别越高安全性越高，效率越低

* 数据库查询隔离级别：
	* select @@tx_isolation;
* 数据库设置隔离级别：
	* set global transaction isolation level  级别字符串;





## JDBC

+ Java Database Connectivity 



**快速入门**

1. 导入驱动jar包
   + 复制jar包到项目Libs目录下
   + 右键 --> add as library
2. 注册驱动
3. 获取数据库连接对象 `Connection`
4. 定义sql
5. 获取执行sql语句的对象 `Statement`
6. 执行sql,结束返回结果
7. 处理结果
8. 释放资源

```java
        //驱动注册 //可不写 mysql 5后默认注册
        Class.forName("com.mysql.cj.jdbc.Driver");
        //获取数据库连接对象
        Connection conn = 						DriverManager.getConnection("jdbc:mysql://127.0.0.1:3306/db3", "root", "0o0occc");	
        //定义sql语句
        String s ="update account set balance = 500 where id = 1";
        //获取执行sql对象 Statement
        Statement stmt = conn.createStatement();
        //执行
        int count = stmt.executeUpdate(s);
        //处理结果
        System.out.println(count);
        //释放资源
        stmt.close();
        conn.close();
```



详解各个对象：

1. DriverManager：驱动管理对象

   + 功能：注册驱动，获取数据库连接

   + `jdbc:mysql://ip:port/数据库名称`

     > 本机服务器且端口为3306：jdbc:mysql:///数据库名称

2. Connection：数据库连接对象

   + 功能：
     1. 获取执行sql的对象
        + statement createStatement()
        + preparedStatement  prepareStatement(String sql)
     2. 管理事务
        + 开启事务 `void setAutoCommit(false)`
        + 提交事务`void commit()`
        + 回滚`rollback()`

   

3. Statement：执行SQL对象

   + 功能：执行静态sql对象
   + 方法
     + `boolean execute(String sql)` 不常用
     + `int executeUpdate(String sql) `执行 DML（insert、update、delete)，DDL（create，alter,drop），**返回影响表的行数**
     + `ResultSet executeQuery(String sql)`，执行DQL

4. ResultSet：结果集对象，封装查询结果

   方法：

   + `boolean next()` 游标移动到下一行
   + `getXXX(参数)`：获取数据，xxx是数据类型
     + 参数：
       1. int 列的参数,从1开始
       2. String 列的名称 getDouble("balance")

5. **PreparedStatement：执行SQL对象**

+ SQL注入：在拼接sql时，有一些sql的特殊关键字参与字符串的拼接，会造成安全性问题
+ 解决：使用PreparedStatement对象
+ 预编译的SQL：参数使用`?`作为占位符

```sql
conn.prepareStatement(String sql)

setXXX(参数1，参数2) -- 参数1：?的位置，参数2：传递的参数
```



**注意：后期都会使用preparedStatement完成增删改查所有操作**

1. 防止SQL注入
2. 效率更高

### 抽取JDBC工具类

+ 目的：简化书写

+ 分析：

  1. 注册驱动抽取

  2. 抽取一个方法获取连接对象

     + 需求：不想传递参数，还得保证工具类的通用性

     + 解决：配置文件

       jdbc,properties

       ​	url=

       ​	user=

       ​	password=

       

  3. 抽取一个方法来释放资源



### **JDBC控制事务**

使用Connection对象来管理事务

+ 开启事务 `void setAutoCommit(false)`
+ 提交事务`void commit()`
+ 回滚`rollback()`



## 数据连接池

1. 概念：其实就是一个容器（集合），存放数据库连接的容器。

   ​		当系统初始化后，容器被创建，容器中会申请一些对象，当用户访问数据库时，从容器中获		取连接对象，访问完成之后，会释放连接对象归还给容器

2. 好处：

+ 节约资源
+ 用户访问高效

3. 实现：
      1. 标准接口：`DataSource`
         + 获取连接：getConnection( )
         + 归还连接：如果连接对象Connection是从连接池中获取的，则调用`close()`方法归还

   

   2. 一般我们不区实现它，由数据库厂商来实现
      1. C3P0：数据库连接池技术
      2. Druid：数据库连接层实现技术，由阿里巴巴提供

4. C3P0：数据库连接层技术

   步骤：

   1. 导入jar包（不要忘了导入数据库驱动jar包）
   2. 定义配置文件
      + 路径直接放在src目录下即可
   3. 创建核心对象：数据库连接池对象 `ComboPooledDataSource`
   4. 获取连接：`getConnection`

4. Druid：数据库连接池实现技术

   **步骤：**

   1. 导入jar包
   2. 定义配置文件
      + 是properties形式的，可放在任意目录下
   3. 加载配置文件。Properties
   4. 获取数据库连接池对象：通过工厂来获取`DruidDataSourceFactory`
   5. 获取连接：`getConnection`

   **定义工具类**

   ​		提供方法：

   1. 获取连接方法，通过数据库连接池获取连接

         			2. 释放资源

   

   获取连接池的方法

   ```java
       private static DataSource ds;
   
       static {
           //加载配置文件
           Properties pro = new Properties();
           InputStream is =JDBCUtils.class.getClassLoader().getResourceAsStream("druid.properties");
           try {
               pro.load(is);
               ds = DruidDataSourceFactory.createDataSource(pro);
   
           } catch (IOException e) {
               e.printStackTrace();
           } catch (Exception e) {
               e.printStackTrace();
           }
       }
   ```

   





## Spring  JDBC

* Spring框架对JDBC的简单封装，提供了一个JDBCTemplate对象简化JDBC开发

* 会自动释放资源

* 步骤：

  1. 导入jar包

  2. 创建JDBCTemplate对象，依赖于数据源DataSource

     `JdbcTemplate template = new JdbcTemplate(ds);`

  3. 调用`JdbcTemplate`的方法来完成CRUD操作

     + **update()**：执行DML语句。增删改语句

     + queryForMap()：查询结果集封装为map集合

       + 只能查询1行，列名封装为key，值为value

     + queryForList()：查询结果封装为List集合

       + 将查询封装成map再装进List里

     + **query()**：查询结果封装为JavaBean对象

       `new BeanPropertyRowMapper<>(Emp.class)` 可以完成数据到JavaBean的自动封装

     + queryForObject：查询结果封装为对象

       + 一般执行一些聚合函数的

         ```java
            String sql = "select count(id) from emp";
                 Long total = template.queryForObject(sql,Long.class);
         ```







# **JDK监控**

+ jsp 查看Java进程概况

+ jconsole 图形化查看内存线程等信息（集成了jstat,jstack)

+ jstat 检测某个进程的内存状况

  ```shell
  jstat -gcutil 46040 1000 (1000ms检测一次) 显示的内存占量是百分比
  jstat -gc 46040 查看当前线程的状况 显示具体使用了多少内存
  
  ```

+ jstack 检测栈=

========重要==========

+ jmap

  ```shell
  //把内存信息dump下来
  jmap -dump:file=a(filename) 46040(pid)
  
  //打印当前堆内存信息
  jmap -heap pid
  ```

+ visualVM

+ 在VM options中可以设置内存溢出时自动dump下内存信息

  `-XX:+HeapDumpOnOutOfMemoryError`