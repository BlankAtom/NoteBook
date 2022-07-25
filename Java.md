#  Java 基础知识

![image-20220202131209518](https://hong-not-pic-1258424340.cos.ap-nanjing.myqcloud.com/notepic/202202021315760.png)



编译型、解释型



## Java的数据结构



### ArrayList

[Arraylist动态扩容详解 - 适AT - 博客园](https://www.cnblogs.com/kuoAT/p/6771653.html)

ArrayList是基于索引的数据接口，它的底层是**数组**。

它可以以O(1)时间复杂度对元素进行随机访问。

ArrayList**不是线程安全**的，只能用在单线程环境下。

实现了Serializable接口，因此它支持序列化，能够通过序列化传输；

实现了RandomAccess接口，支持快速随机访问，实际上就是通过下标序号进行快速访问；

实现了Cloneable接口，能被克隆。

使用 `Collections.synchronizedList(list);`可以将list转为线程安全的。

> 底层方法使用synchronized代码块锁，碎叶也是锁住了所有的代码，但是所在方法里面，相比于所在外边，可以说性能有所提高，毕竟方法本身是要分配资源的。



### LinkedList

LinkedList是以元素列表的形式存储它的数据，每一个元素都和它的前一个和后一个元素链接在一起，在这种情况下，查找某个元素的时间复杂度是O(n)。

LinkedList的插入，添加，删除操作速度更快，因为当元素被添加到集合任意位置的时候，不需要像数组那样重新计算大小或者是更新索引。



### HashMap

HashMap 根据键的 `hashCode` 值存储数据，大多数情况下可以直接定位到它的值，因而具有很快的访问速度，但遍历顺序却是不确定的。 

HashMap最多只允许一条记录的键为null，允许多条记录的值为 null。

HashMap 非线程安全，即任一时刻可以有多个线程同时写 HashMap，可能会导致数据的不一致。

如果需要满足线程安全，**可以用 Collections 的 `synchronizedMap` 方法使 HashMap 具有线程安全的能力**，或者**使用 `ConcurrentHashMap`。**



1. capacity：当前数组容量，始终保持 2^n，可以扩容，扩容后数组大小为当前的 2 倍。 
2. loadFactor：负载因子，默认为 0.75。 
3. threshold：扩容的阈值，等于 capacity * loadFactor 



JAVA7 实现 

大方向上，HashMap 里面是一个数组，然后数组中每个元素是一个单向链表。

实体是嵌套类 Entry 的实例，Entry 包含四个属性：key， value， hash 值和用于单向链表的 next。 



JAVA8实现 

Java8 对 HashMap 进行了一些修改，最大的不同就是利用了红黑树，所以其由 数组+链表+红黑树 组成。 

在 Java8 中，当链表中的元素超过了 8 个（可以修改）以后，会将链表转换为红黑树，在这些位置进行查找的时候可以降低时间复杂度为 O(logN)。 



### HashTable

HashTable线程同步

HashTable不允许<键，值>有空值

HashTable中hash数组的默认大小是11，增加方式的old*2+1

Java1.8的hash()中，将hash值高位（前16位）参与到取模的运算中，使得计算结果的不确定性增强，降低发生哈希碰撞的概率



### Hashset



### Collection/Collection.sort()



### BloackingQuene



## 高并发中的集合

**第一代线程安全集合类 **

Vector、Hashtable 

使用synchronized修饰方法保证线程安全

缺点：效率低下 

**第二代线程非安全集合类**

ArrayList、HashMap

线程不安全，但是性能好，用来替代Vector、Hashtable             

使用ArrayList、HashMap，需要线程安全怎么办呢？ 

使用 `Collections.synchronizedList(list);` `Collections.synchronizedMap(m);` 

底层使用synchronized代码块锁，虽然也是锁住了所有的代码，但是锁在方法里边，并所在方法外边性能可以理解为稍有提高吧。毕竟进方法本身就要分配资源的  

**第三代线程安全集合类**

在大量并发情况下如何提高集合的效率和安全呢？ 

`java.util.concurrent.*` 、`ConcurrentHashMap：`、`CopyOnWriteArrayList ：`、`CopyOnWriteArraySet：`   注意 不是`CopyOnWriteHashSet`

底层大都采用Lock锁（1.8的`ConcurrentHashMap`不使用Lock锁），保证安全的同时，性能也很高。       



## Lock锁





#### 单链表



#### 红黑树



### 算法



#### 单链表反转



#### 排序



# 注解

> Annotation是从JDK5.0开始引入的新技术。

Annotation的作用:

- 不是程序本身，可以对程序作出解释.(这一点和注释(comment)没什么区别)
- 可以被其他程序(比如:编译器等)读取.

Annotation的格式:

- 注解是以"@注释名"在代码中存在的，还可以添加一些参数值，例如：`@SuppressWarnings(value="unchecked")`。

Annotation在哪里使用？

- 可以附加在package , class , method , field等上面﹐相当于给他们添加了额外的辅助信息,我们可以通过反射机制编程实现对这些元数据的访问



**内置注解**：

1. Ovrride
2. Deprecated：不推荐程序员使用，但是可以使用，可能存在更好的方式
3. SuppressWarnings：定义在lang包下，用来抑制警告信息，需要使用参数

**元注解**：

> 元注解的作用是注解其他注解，Java定义了4个标准的meta-annotation，他们被用来提供对其他annotation类型作说明。

1. Target：用于描述注解的使用范围（可以被用在什么地方）
2. Retention：表示需要在什么级别保存该注解信息，用于描述注解的生命周期（Source<Class<Runtime）
3. Document：说明注解将被包含在javadoc中
4. lnherited：说明子类可以继承父类中的该注解

**自定义注解：**

- `@interface`用来声明一个注解，格式：public @interface注解名{定义内容}；
- 其中的每一个方法实际上是声明了一个配置参数；
- 方法的名称就是参数的名称；
- 返回值类型就是参数的类型(返回值只能是基本类型，Class , String , enum )；
- 可以通过default来声明参数的默认值；
- 如果只有一个参数成员，一般参数名为value；
- 注解元素必须要有值﹐我们定义注解元素时,经常使用空字符串,0作为默认值；

```java
@Target({ElementType.TYPE, ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@interface MyAnnotation {
    String name() default "";
}
```



# 反射

>  Java.Reflection

**动态语言：**

是一类在运行时可以改变其结构的语言:例如新的函数、对象、甚至代码可以被引进，已有的函数可以被删除或是其他结构上的变化。通俗点说就是在运行时代码可以根据某些条件改变自身结构。

主要动态语言:Object-C、C#、JavaScript、PHP、Python等。

**静态语言:**

与动态语言相对应的，运行时结构不可变的语言就是静态语言。如Java、C、C++。

Java不是动态语言，但Java可以称之为“准动态语言”。即Java有一定的动态性，我们可以利用反射机制获得类似动态语言的特性。Java的动态性让编程的时候更加灵活！

> 弊端就是会带来不安全性



Reflection(反射）是Java被视为动态语言的关键，反射机制允许程序在执行期借助于Reflection API取得任何类的内部信息，并能直接操作任意对象的内部属性及方法。

加载完类之后，在堆内存的方法区中就产生了一个Class类型的对象（一个类只有一个Class对象)，这个对象就包含了完整的类的结构信息。我们可以通过这个对象看到类的结构。这个对象就像一面镜子，透过这个镜子看到类的结构，所以，我们形象的称之为:反射

![image-20220401174828606](https://hong-not-pic-1258424340.cos.ap-nanjing.myqcloud.com/notepic/202204011748368.png)

```java

public class Main {
    public static void main(String[] args) {
        Class c1 = Class.forName("demo01.User");
    }
}

class User {
    String name;
}
```



**Class类：**

对象照镜子后可以得到的信息:某个类的属性、方法和构造器、某个类到底实现了哪些接口。对于每个类而言，JRE 都为其保留一个不变的Class类型的对象。一个Class对象包含了特定某个结构(class/interface/enum/annotation/primitive type/void/)的有关信息。

Class本身也是一个类

Class对象只能由系统建立对象

一个加载的类在JVM中只会有一个Class实例

一个Class对象对应的是一个加载到JVM中的一个.class文件

每个类的实例都会记得自己是由哪个Class 实例所生成

通过Class可以完整地得到一个类中的所有被加载的结构

Class类是Reflection的根源，针对任何你想动态加载、运行的类，唯有先获得相应的Class对象



**获取Class类的实例**

1、若已知具体的类，通过类的class属性获取，该方法最安全可靠，程序性能最高

`Class cla = Preson.class;`

2、已知某个类的实例，调用该实例的getClass()方法获取Class对象

`Class clazz = person.getClass();`

3、已知一个类的全类名，且该类在类路径之下，可以通过Class类的静态方法forName()获取，抛出一个ClassNotFoundException

`Class clazz = Class.forName("demo01.Student");`

4、内置基本数据类型可以直接用类名.Type

5、还可以使用ClassLoader



> 无论什么样的方式获取到的Class，都指向同一个Class，这一点通过hash值可以判断

**类的加载：**

- 加载：将class文件字节码内容加载到内存中，并将这些静态数据转换成方法区的运行时数据结构,然后生成一个代表这个类的java.lang.Class对象.

- 链接：将Java类的二进制代码合并到JVM的运行状态之中的过程。
  - 验证：确保加载的类信息符合JVM规范，没有安全方面的问题
  - 准备：正式为类变量(static)分配内存并设置类变量默认初始值的阶段，这些内存都将在方法区中进行分配。
  - 解析：虚拟机常量池内的符号引用（常量名）替换为直接引用(地址)的过程。
- 初始化：
  - 执行类构造器`<clinit>()`方法的过程。类构造器`<clinit>()`方法是由编译期自动收集类中所有类变量的赋值动作和静态代码块中的语句合并产生的。(类构造器是构造类信息的，不是构造该类对象的构造器）。
  - 当初始化一个类的时候，如果发现其父类还没有进行初始化，则需要先触发其父类的初始化。
  - 虚拟机会保证一个类的`<clinit>()`方法在多线程环境中被正确加锁和同步。

**什么时候发生类的初始化：**

类的主动引用(一定会发生类的初始化)

- 当虚拟机启动，先初始化main方法所在的类
- new一个类的对象
- 调用类的静态成员（除了final常量)和静态方法
- 使用java.lang.reflect包的方法对类进行反射调用
- 当初始化一个类，如果其父类没有被初始化，则先会初始化它的父类

类的被动引用(不会发生类的初始化)

- 当访问一个静态域时，只有真正声明这个域的类才会被初始化。如:当通过子类引用父类的静态变量，不会导致子类初始化
- 通过数组定义类引用，不会触发此类的初始化
- 引用常量不会触发此类的初始化（常量在链接阶段就存入调用类的常量池中了)



**有了Class对象，我们可以做什么？**

1、创建类的对象：调用Class.newInstance()方法

> 这样调用要求类必须有一个无参构造器，且类的构造器的访问权限需要足够

2、有参构造器创建对象：调用Class.getDeclaredConstrutor(Class ... parameterTypes)取得本类的指定形式构造器；向构造器形参中传递一个对象数组进去，里面包含了构造器中所需的各个参数；通过Constructor实例化对象。

```java
c = class.getDeclaredConstructor(String.class, int.class);
c.newInstance("123", 1);
```



**关掉Java安全权限检测**

```java
Field name = ...
name.setAccessible(true)
```

参数为true，使之关闭。

参数为false，使之开启

> 如果代码中必须使用反射的话，且该句代码需要频繁调用，请设置为true从而提升性能。



**反射操作泛型**

```java
Method test01 = GenericParameter.class.getMethod("test01");

System.out.println("#" +  test01.getGenericReturnType());
// System.out.println("#" +  test01.getGenericParameterTypes);
if(test01.getGenericReturnType() instanceof ParameterizedType) {
    Type[] types = ((ParameterizedType) test01.getGenericReturnType()).getActualTypeArguments();
    for (Type type : types) {
        System.out.println(type);
    }
}
```



**ORM**

# JVM

- 请你谈谈你对jvm的理解？java8虚拟机和之前的变化更新
- 什么是OOM，什么是栈溢出StackOverFlowError？怎么分析？
- JVM的常用调优参数有哪些？
- 内存快照如何抓取，怎么分析Dump文件？
- 谈谈JVM中，类加载器你的认识？



## JVM的位置

硬件之上是系统，系统之上是jvm。

![image-20220330145310691](https://hong-not-pic-1258424340.cos.ap-nanjing.myqcloud.com/notepic/202203301453324.png)





## JVM的体系结构

![image-20220330150015320](https://hong-not-pic-1258424340.cos.ap-nanjing.myqcloud.com/notepic/202203301500697.png)

栈，不会有垃圾回收！

方法区属于特殊的堆

大部分时间调优调的是堆



## 类加载器

作用：加载Class文件

1. 类加载器收到类加载的请求

2. 将这个请求向上委托给父类加载器去完成，一直向上委托，直到启动类加载器

3. 启动加载器检查是否能够加载当前这个类，能加载就结束，使用当前的加载器，否则，抛出异常，通知子加载器进行加教

4. 重复步骤3



引导类加载器，c++编写

1. 虚拟机自带加载器（引导类加载器，C++编写的，无法直接获取到）
2. 启动类（根）加载器
3. 扩展类加载器
4. 应用程序加载器



## 双亲委派机制



![](https://hong-not-pic-1258424340.cos.ap-nanjing.myqcloud.com/notepic/202203292245413.png)

双亲委派模式是在Java 1.2后引入的，其工作原理的是，如果一个类加载器收到了类加载请求，它并不会自己先去加载，而是把这个**请求委托给父类的加载器去执行**，如果父类加载器还存在其父类加载器，则**进一步向上委托，依次递归**，**请求最终将到达顶层的启动类加载器**，如果父类加载器可以完成类加载任务，就成功返回，倘若父类加载器无法完成此加载任务，子加载器才会尝试自己去加载，这就是双亲委派模式。

**好处**：

   - 每一个类都只会被加载一次，避免了重复加载
   - 每一个类都会被尽可能的加载（从引导类加载器往下，每个加载器都可能会根据优先次序尝试加载它）
   - 有效避免了某些恶意类的加载（比如自定义了Java.lang.Object类，一般而言在双亲委派模型下会加载系统的Object类而不是自定义的Object类）

**如何破坏双亲委派机制？**

1. 双亲委派模型的第一次“被破坏”是重写自定义加载器的`loadClass()`，jdk不推荐这么做。一般都只是重写`findClass()`，这样可以保持双亲委派机制。而loadClass方法加载规则由自己定义，就可以随心所欲的加载类。
2. 双亲委派模型的第二次“被破坏”是`ServiceLoader`和`Thread.setContextClassLoader()`。即线程上下文类加载器（contextClassLoader）。双亲委派模型很好地解决了各个类加载器的基础类统一问题（越基础的类由越上层的加载器进行加载），基础类之所以被称为“基础”，是因为它们总是作为被调用代码调用的API。但是，如果基础类又要调用用户的代码，那该怎么办呢？线程上下文类加载器就出现了。
   1. SPI。这个类加载器可以通过`java.lang.Thread`类的`setContextClassLoader()`方法进行设置，如果创建线程时还未设置，它将会从父线程中继承一个；如果在应用程序的全局范围内都没有设置过，那么这个类加载器默认就是应用程序类加载器。有了线程上下文类加载器，**JNDI**服务使用这个线程上下文类加载器去加载所需要的SPI代码，也就是父类加载器请求子类加载器去完成类加载动作，这种行为实际上就是打通了双亲委派模型的层次结构来逆向使用类加载器，已经违背了双亲委派模型，但这也是无可奈何的事情。Java中所有涉及SPI的加载动作基本上都采用这种方式，例如JNDI，JDBC，JCE，JAXB和JBI等。
   2. 线程上下文类加载器默认情况下就是`AppClassLoader`，那为什么不直接通过`getSystemClassLoader()`获取类加载器来加载classpath路径下的类的呢？其实是可行的，但这种直接使用`getSystemClassLoader()`方法获取`AppClassLoader`加载类有一个缺点，那就是代码部署到不同服务时会出现问题，如把代码部署到Java Web应用服务或者EJB之类的服务将会出问题，因为这些服务使用的线程上下文类加载器并非AppClassLoader，而是Java Web应用服自家的类加载器，类加载器不同。所以我们应用该少用getSystemClassLoader()。总之不同的服务使用的可能默认ClassLoader是不同的，但使用线程上下文类加载器总能获取到与当前程序执行相同的ClassLoader，从而避免不必要的问题。
3. 双亲委派模型的第三次“被破坏”是由于用户对程序动态性的追求导致的，这里所说的“动态性”指的是当前一些非常“热门”的名词：代码**热替换**、模块**热部署**等，简答的说就是机器不用重启，只要部署上就能用。

## 沙箱安全机制

Java安全模型的核心就是**Java沙箱（sandbox）**，什么是沙箱？沙箱是一个限制程序运行的环境。沙箱机制就是将Java代码限定在虚拟机（JVM）特定的运行范围中，并且严格限制代码对本地系统资源访问，通过这样的措施来保证对代码的有效隔离，防止对本地系统造成破坏。沙箱主要限制系统资源访问，那系统资源包括什么? CPU、内存、文件系统、网络。不同级别的沙箱对这些资源访问的限制也可以不一样。

所有的Java程序运行都可以指定沙箱，可以定制安全策略。

在Java中将执行程序分成本地代码和远程代码两种，本地代码默认视为可信任的，而远程代码则被看作是不受信的。对于授信的本地代码，可以访问一切本地资源。而对于非授信的远程代码在早期的ava实现中，安全依赖于沙箱(Sandbox)机制。如下图所示JDK1.0安全模型

![image-20220330213656059](https://hong-not-pic-1258424340.cos.ap-nanjing.myqcloud.com/notepic/202203302136362.png)

但如此严格的安全机制也给程序的功能扩展带来障碍，比如当用户希望远程代码访问本地系统的文件时候，就无法实现。因此在后续的Java1.1版本中，针对安全机制做了改进，增加了安全策略，允许用户指定代码对本地资源的访问权限。如下图所示JDK1.1安全模型

![image-20220330213707195](https://hong-not-pic-1258424340.cos.ap-nanjing.myqcloud.com/notepic/202203302137617.png)

在Java1.2版本中，再次改进了安全机制，增加了代码签名。不论本地代码或是远程代码，都会按照用户的安全策略设定，由类加载器加载到虚拟机中权限不同的运行空间，来实现差异化的代码执行权限控制。如下图所示JDK1.2安全模型。

![image-20220330213741868](https://hong-not-pic-1258424340.cos.ap-nanjing.myqcloud.com/notepic/202203302137241.png)

当前最新的安全机制实现，则引入了域(Domain)的概念。虚拟机会把所有代码加载到不同的系统域和应用域，系统域部分专门负责与关键资源进行交互，而各个应用域部分则通过系统域的部分代理来对各种需要的资源进行访问。虚拟机中不同的受保护域(Protected Domain)，对应不一样的权限(Permission)。存在于不同域中的类文件就具有了当前域的全部权限，如下图所示最新的安全模型(jdk 1.6)



![image-20220330213834562](https://hong-not-pic-1258424340.cos.ap-nanjing.myqcloud.com/notepic/202203302138682.png)

组成沙箱的基本组件:

- 字节码校验器(bytecode verifier)︰确保lava类文件遵循]ava语言规范。这样可以帮助Java程序实现内存保护。但并不是所有的类文件都会经过字节码校验，比如核心类。

- 类装载器(class loader) :其中类装载器在3个方面对Java沙箱起作用
  - 它防止恶意代码去干涉善意的代码;
  - 它守护了被信任的类库边界;
  - 它将代码归入保护域，确定了代码可以进行哪些操作。

虚拟机为不同的类加载器载入的类提供不同的命名空间，命名空间由一系列唯一的名称组成，每一个被装载的类将有一个名字，这个命名空间是由Java虚拟机为每一个类装载器维护的，它们互相之间甚至不可见。

类装载器采用的机制是双亲委派模式。

1. 从最内层VM自带类加载器开始加载，外层恶意同名类得不到加载从而无法使用;

2. 由于严格通过包来区分了访问域，外层恶意的类通过内置代码也无法获得权限访问到内层类，破坏代码就自然无法生效。

- 存取控制器 （access controller)︰存取控制器可以控制核心API对操作系统的存取权限，而这个控制的策略设定，可以由用户指定。

- 安全管理器(security manager)︰是核心API和操作系统之间的主要接口。实现权限控制，比存取控制器优先级高。

- 安全软件包(security package) : java.security下的类和扩展包下的类，允许用户为自己的应用增加新的安全特性，包括:
  - 安全提供者
  - 消息摘要
  - 数字签名
  - 加密
  - 鉴别

## Native

凡是带上native关键字的，说明java的作用范围达不到了，它会调用底层C语言的库，进入本地方法栈，调用本地方法接口JNI，扩展java的使用，融合不同的编程语言为java所用！最初想融合C和C++。

它在本地开辟了一个区域（本地方法栈）来标记native方法。在最终执行的时候，加载本地方法库中的方法。

## PC寄存器

程序计数器：Program Counter Register

每个线程都有一个程序计数器，是线程私有的，就是一个指针，指向方法区中的方法字节码（用来存储指向像一条指令的地址，也即将要执行的指令代码)，在执行引擎读取下一条指令，是一个非常小的内存空间，几乎可以忽略不计。

## 方法区

Method Area方法区

方法区是被所有线程共享，所有字段和方法字节码，以及一些特殊方法，如构造函数，接口代码也在此定义，简单说，所有定义的方法的信息都保存在该区域，此区域属于共享区间;

静态变量、常量、类信息(构造方法、接口定义)、运行时的常量池存在方法区中，但是实例变量存在堆内存中，和方法区无关

static、final 、Class模板



## 栈

栈：栈内存，主管程序的运行，生命周期和线程同步;

线程结束，栈内存也就是释放，对于栈来说，不存在垃圾回收问题一旦线程结束，栈就Over!

栈：8大基本类型＋对象引用＋实例的方法

每一次函数的调用，都会在`调用栈`（call stack）上维护一个独立的`栈帧`（stack frame）。每个独立的栈帧一般包括：

- 函数的返回地址和参数
- 临时变量：包括函数的非静态局部变量以及编译器自动生成的其他临时变量
- 函数调用的上下文

栈是从高地址向低地址延伸，一个函数的栈帧用EBP和ESP这两个寄存器来划定范围。`EBP`指向当前栈帧的底部，`ESP`**始终指向**栈帧的顶部。



## 三种JVM

- sun公司 HotSpot
- BEA JRockit
- IBM J9 VM



## 堆

一个JVM只有一个堆内存，堆内存的大小是可以调节的。

类加载器读取了文件后，一般会把什么放在堆中？类、方法、常量、变量，保存所有的引用类型的真实对象。

堆内存中还要细分3个区域

- 新生区（伊甸园区、幸存区）
- 养老区
- 永久区（1.8取消，改为元空间）

> 当一个对象经历了15次GC仍然存活，就会被复制到老年代。
>
> 可以通过-XX:MaxTenuringThreshold参数设置进入老年代的GC次数

**新生区**

- 类：诞生、成长、死亡的地方；
- 伊甸园区：所有的对象都是在伊甸园区new出来的！
- 幸存者区（0、1）：伊甸园中存活下来的对象会交给0区或者1区（二者可能会交替位置）（谁空谁是To）

> 事实上，绝大多数的对象都是临时对象，是没办法活到养老去的



**永久区**

这个区域常驻内存，用来存放JDK自身携带的Class对象，interface元素据，存储的是java运行的一些环境类信息。

这个区域不存在垃圾回收，关闭虚拟机会释放这个区域的内存。

一个启动类，加载大量的第三方jar包、tomcat部署太多应用，大量动态生成的反射类，是可能导致永久区OOM的。

- 1.6以前：永久代，常量池在方法区中；
- 1.7：永久代，去永久化，常量存放在堆中；
- 1.8以后：无永久代，常量池在元空间中；



![](https://hong-not-pic-1258424340.cos.ap-nanjing.myqcloud.com/notepic/202203311516342.png)

![image-20220331151834350](https://hong-not-pic-1258424340.cos.ap-nanjing.myqcloud.com/notepic/202203311518388.png)

元空间中有方法区，方法区中有常量池

> 元空间在逻辑上存在，物理上不存在，通过参数看到的堆细节中的元空间数据是算出来的结果

> 默认情况下：分配的总内存是电脑内存的1/4，初始化内存是1/64

-Xms1024m -Xmx1024m -XX :+PrintGcDetails



**OOM**

1. 尝试扩大内存看结果
2. 分析内存，找问题（专业工具）



## MAT、JPofiler

一Xms1m一Xm:8m 一XX : +HeapDumpOnOutOfllemoryError





## GC：垃圾回收机制

GC的作用区域只有元空间和堆

JVM在GC时，并不是堆新生代、幸存区、老年区统一回收，大部分时候，回收的都是新生代。

GC的两种类别：轻GC（普通GC）、重GC（全局GC）



GC算法：标记清除、标记压缩、复制、引用计数法



**引用计数法**

每个对象分配一个计数器，记录被引用的次数。

引用次数为0的被作为垃圾清楚。



**复制算法**

一般是将from的内容复制到to区域、新生代复制到幸存区。

**优点**：没有内存碎片

**缺点**：浪费内存空间，多了一半空间是空的。假设对象100%存活（极端）

最佳使用场景：对象存活度较低的时候：新生区 



**标记清除压缩算法**

扫描对象，对活着的对象进行标记。

对没有标记的对象进行清除。

**优点**：不需要额外的空间

**缺点**：两次扫描严重浪费时间，会产生内存碎片



再次优化：压缩（防止内存碎片）

再次扫描，向一段移动存活的对象，但是多了移动成本。



再次优化：多次清除再压缩





## 总结

内存效率：复制 > 标记清除 > 标记压缩

内存整齐：复制 = 标记压缩 > 标记清除

内存利用率：标记压缩 = 标记清除 > 复制





## 判断对象已死的算法

##### 1、引用计数器算法

> 当存在相互引用（循环引用）的情况时，会套死无法正确判断。



##### 2、可达性分析算法

算法的思想是通过一系列的“GC Roots”，这些节点向下搜索，走过的路径成为引用链，当一个对象到“GC Roots”没有任何的引用链时，证明此对象是不可用的。



哪些对象是GC Roots？

在Java中，可以作为GC Roots的对象包括下面几种：

1. 虚拟机栈中的引用的对象
2. 方法区中类静态属性引用的对象
3. 方法区中常量引用的对象
4. 本地方法栈中JNI（一般说的Native方法）的引用对象。

2018级外出实习申请表     请仔细查阅，先填写完红色框单元格、家长知情书，转存pdf在学生签字处“手写签字”，提交至此链接：     【腾讯文档】https://docs.qq.com/form/page/DUWhSTVdubWhVRVRa

##### 引用

无论是引用计数器还是可达性分析，判断对象是否存在可用都和引用有关。

在JDK1.2以前，引用被定义为：当一个Reference类型的数据代表的是另一块内存的起始地址，该类型的数据被称之为引用。这样的定义十分狭隘。



强引用：

类似于`Object obj = new Object();`

只要引用一直存在就不会被回收



软引用：

用来描述一些还有用但非必须的对象，对于软引用关联着的对象，当内存溢出异常发生之前，通过垃圾回收进行二次回收。如果垃圾回收二次回收之后，系统内存依旧不够的话，才会抛出内存溢出异常。在JDK1.2以后用`SoftReference`来实现软引用。



弱引用：

也是描述非必须对象的，特点是只能生存到下一次垃圾回收之前。无论内存是否足够，都会被回收。用`WeakReference`实现



虚引用：

最弱的引用关系，一个对象是否有虚引用的存在，完全不会对其生存的时间产生影响。也无法通过一个虚引用来获取一个实例对象。为一个对象设置虚引用的唯一目的就是让该对象在垃圾回收的时候收到一个系统通知。用`PhantomReference`实现虚引用。





#### 垃圾收集算法

##### 标记清除算法

- 会产生内存碎片



##### 复制算法

把存货的对象复制到预留区域，清理原区域。

- 复制花销太大



##### 标记整理算法

将存活对象依次复制到开始位置。

复制完成后，将剩余的空间全部清理。



##### 分代垃圾回收

将空间分为年轻代、老年代

将年轻代分为伊甸区Eden、幸存区Survivor（from、to）

Eden : survivor-from : survivor-to = 8 : 1 : 1

每次只保留10%的空间为预留区域，每次90%的空间为新生对象。

当Survivor中的对象生存次数达到15次，则复制到老年代。

当Survivor的内存不够时，将对象复制到老年代。（内存担保机制）。



###### Minor GC（新生代垃圾回收）

> 程度一般

当新生代每次将对象复制到老年代，都称为一次Minor GC



###### Full GC （老年代垃圾回收）

当新生代分配不出空间，老年代也没有更多空间时触发，对整个空间进行垃圾回收。



Minor Gc 和Full Gc 都会引起STW

但是Minor Gc耗时短。

（Stop The World：当我工作的时候全部线程停止。简称STW）



#### 垃圾收集器

##### Serial收集器

最基本、发展最悠久的收集器。在JDK 1.3.1之前时虚拟机新生代垃圾回收的唯一选择。这个收集器时单一线程的（STW）。

虚拟机团队一直为了减少STW的时间而努力着。

虽然在服务端有其弊端，但是在客户端有其好用的点，因为它会集中硬件资源进行GC，可以极大的减少GC时间。

依旧是最重要的垃圾收集器。





##### ParNew收集器

是Serial收集器的多线程版本。开发目的是为了缩短STW的时间。

除了使用多线程进行垃圾回收之外，其余可控参数、收集算法、停止工作线程、对象分配原则、回收策略和Serial没有区别。

但是是Server模式新生代的首选收集器。其最重要的一个原因是可以和CMS（并发交互清除）配合使用。而1.4诞生的新生代收集器和CMS不兼容。

单CPU性能表现比Serial差。

可以使用`-XX:ParallelGCThreads`来限制垃圾回收的线程数。



##### Parallel Scavenge 收集器

是一个新生代收集器，采用复制算法，是并行的多线程垃圾收集器。其关注点在于达到一个可控制的吞吐量（CPU运行用户线程的时间和CPU运行总时间的比值），这和CMS等收集器的关注点是在于缩短用户线程停止的时间这点是不通的。

即：	

$$
吞吐量=\frac{用户线程工作时间}{用户线程工作时间 + GC 时间}
$$

有两个控制参数控制吞吐量，分别为：

1、最大垃圾收集时间：`-XX:MaxGCPauseMills`

2、直接设置吞吐量大小：`-XX:GCTimeRatio`（用户 比  垃圾）



`-XX:+UseAdaptiveSizePolicy`自适应策略

自动的分配新生代大小。





##### Serial Old 收集器

是Serial的老年代的版本，标记整理算法。主要用于Client，在Server下有两个作用：配合Parallel Scavenge、作为CMS的备选方案，在Concurrent Mode Failure 时被迫使用的。





##### Parallel Old 收集器

是Parallel Scavenge的老年代版本。

多线程、标记整理算法。

此前的Parallel Scavenge处于一个极为尴尬的阶段，因为使用Parallel的话，老年代除了Serial Old 别无选择。由于老年代Serial的拖累，使得Parallel Scavenge也就无法达到吞吐量最大化的效果。

“吞吐量优先收集器”组合成立。

在注重吞吐量优先和CPU资源敏感的环境下，可以采用这个组合。



##### CMS收集器

Concurrent Mark Sweep

并发标记清除处理器。

它的过程分为四个步骤：

1. 初始标记：（STW）仅仅关联GC Roots能直接关联到的对象，速度很快。
2. 并发标记：进行GC Roots Tracing 的过程
3. 重新标记：（STW） 为了修正标记期间，因用户程序运行而导致标记产生变动的的那一部分的对象的标记记录。
4. 并发清除



真正意义上的边污染边处理的垃圾收集器。



存在的缺点：

1. CMS收集器对于CPU资源非常敏感
2. 无法处理浮动垃圾
3. 因为基于标记清除算法，所以有大量垃圾碎片产生。`-XX:UseCMSCompactAtFullCollection`







##### G1收集器

G1的设计原则就是简单可行的性能调优。

JDK9以后的默认收集器



-XX:+UseG1GC  -Xmx32g  -XX:MaxGcPauseMills=200

开启G1垃圾收集器

设置堆内存的最大内存是32g

GC的最大暂停时间

如果需要调优，在内存一定的情况下只需要设置暂停时间。



G1收集器对整个内存中的对象进行随机标记，标记为Eden、Survivor、Old、Humongous（大型对象区）



Remember Set：

在老年代中专门分一块区域用于记录有哪些对象引用了新生代的对象。那么这些对象就是根对象。

然后使用可达性分析算法确定哪些对象死掉了。



Mix垃圾回收：（混合垃圾回收）

整体和G1的标记顺序一致，主要要注意的是并发标记。

并发标记使用三色标记法，防止标漏。

但是为了防止标记过程中因用户线程导致的调用链变更，而产生的误删，故有一个机制防止这个问题，那就是在初始标记的时候先快照。



### JVM常用参数



![image-20210507172304835](https://hong-not-pic-1258424340.cos.ap-nanjing.myqcloud.com/notepic/20210507172306.png)



-XX:PermGen 是永久代的（方法区）不是老年代（堆）？？



# 多线程

##  线程简介

### 线程、进程

进程是执行程序的一次执行过程，是一个动态的概念，是系统资源分配的单位。

通常一个进程中可以包含多个线程，一个进程至少有一个线程。线程是CPU调度和执行的单位。

>  注：很多的多线程是模拟出来的，真正的多线程是指有多个CPU，即多核，如服务器。如果是模拟出来的多线程，即在一个CPU的情况下，在同一时间点，只能执行一个代码，因为切换得很快，所以就有同时执行的错觉。





## 线程的创建

> Thread class, Runnable, Callable

### **Thread:**

继承`Thread`，重写`run()`。

线程不一定执行，由CPU调度。

*不建议使用，避免OOP单继承的局限性*

### **Runnable:**

实现`Runnable`接口，实现`run()`

*推荐使用，避免单继承局限性，灵活方便，方便同一个对象多个线程使用*



**并发问题：**

多个线程对一个对象进行处理出现了问题。



实例试验：

龟兔赛跑





### **Callable：**

优点：

可以定义返回值

可以抛出异常



实现：

1. 实现Callable接口，需要返回值类型

2. 重写call方法，需要抛出异常3．创建目标对象

4. 创建执行服务:`ExecutorService ser = Executors.newFixedThreadPool(1);`
5. 提交执行:`Future<Boolean> result1 = ser.submit(t1);`

6. 获取结果:`boolean r1 = result1.get()`

7. 关闭服务: `ser.shutdownNow();`



### 静态代理

真实对象和代理对象都实现同一个接口。

代理对象要代理真实角色。



优点：

代理对象可以做一些真实对象做不到的事情。

真实对象只需要关注自身的事情



## 线程状态

![image-20210508170011443](https://hong-not-pic-1258424340.cos.ap-nanjing.myqcloud.com/notepic/20210508170013.png)



当创建时进入 **新生状态**

当调用start()方法时进入 **就绪状态**，但不意味着立刻调度执行

当调用sleep、wait或同步锁定时，线程进入**阻塞状态**，该状态下代码不往下执行，阻塞事件接触后，重新进入就绪状态，等待cpu调度执行。

进入 **运行状态** 线程才是真正执行线程体的代码块

线程终端或者结束，一旦进入 **死亡状态**，就不能再次启动。





### 线程停止

- 不推荐使用jdk提供的`stop()`, `destroy()`
- 推荐让线程自己停止下来
- 建议使用一个标志位进行终止变量，例如`flag=false`，则停止线程线程运行



### 线程休眠

- sleep(时间) 指定当前线程阻塞的毫秒数
- sleep 时间达到后线程进入就绪状态
- sleep可以模拟网络延时、倒计时
- 每一个对象都有一个锁，sleep不会释放锁。



### 线程礼让

yield

礼让线程，让当前执行线程暂停，但是不阻塞。

将线程从运行状态转为就绪状态。

让cpu重新调度，礼让不一定成功！



### Join

Join合并线程，待此线程完成后，再执行其他线程，其他线程阻塞。





### 线程的优先级

Java提供一个线程调度器来监控程序中启动后进入就绪状态的所有线程，线程调度器按照优先级决定应该调度哪个线程来执行。

线程的优先级用数字表示，范围为1~10

> Thread.MIN_PRIORITY = 1
>
> Thread.MAX_PRIORITY = 10
>
> Thread.NORM_PRIORITY = 5

使用以下方法改变或获取优先级

`getPriority()`, `setPriority(int x)`



注：优先级只能改变被调度的概率，而不能直接决定被调度的顺序。



### 守护线程

> daemon

线程分为用户线程和守护线程

虚拟机必须确保用户线程的执行完成

**虚拟机不用等待守护线程**

可以用于后台记录日志、监控内存、垃圾回收等。



![image-20210508224055901](https://hong-not-pic-1258424340.cos.ap-nanjing.myqcloud.com/notepic/20210508224057.png)



## 线程同步

> 多个线程操作同一个资源

并发

处理多线程问题时，线程同步其实是一种等待机制，多个需要同时访问此对象的线程进入这个对象的等待池，形成队列，等待前面的线程使用完毕，下一个线程再使用。

线程同步的形成条件：锁+队列

为了保证数据在方法中被访问时的正确性，在访问时加入锁机制`synchronized`，当一个线程获得对象的排他锁，独占资源，其他线程必须等待。使用后释放锁即可。

存在以下问题：

- 一个线程持有锁会导致其他所有需要此锁的线程挂起。
- 多线程竞争下，会导致比较多的上下文切换、调度时延，引起性能问题。
- 如果一个优先级高的等待一个优先级低的线程释放锁，会导致优先级倒置问题，引起性能问题。



### 三大不安全案例



## 同步、同步块

同步：在方法的前面增加synchronized，该方法即为同步方法。

调用时会对对象上锁。

默认锁住的的时this对象。



同步块：

```java
synchronized( obj ) {
    
}
```

同步块在括号中锁住对象，执行代码块。

![image-20210509132622110](https://hong-not-pic-1258424340.cos.ap-nanjing.myqcloud.com/notepic/image-20210509132622110.png)





## 死锁

![image-20210509133248653](https://hong-not-pic-1258424340.cos.ap-nanjing.myqcloud.com/notepic/image-20210509133248653.png)



![image-20210509154844101](https://hong-not-pic-1258424340.cos.ap-nanjing.myqcloud.com/notepic/image-20210509154844101.png)



## Lock锁

![image-20210509155138209](https://hong-not-pic-1258424340.cos.ap-nanjing.myqcloud.com/notepic/image-20210509155138209.png)

![image-20210512141045605](https://i.loli.net/2021/05/12/FI9eYdO21h6VmK4.png)





## 线程协作

生产者消费者模式

![image-20210512142259895](https://i.loli.net/2021/05/12/POAetCipRMYTvaW.png)



管程法、缓冲区法：





信号灯法：

缓冲区法长度为1 的管程法



## 线程池

![image-20210512174843113](https://i.loli.net/2021/05/12/F8Pq4JmbLzeWKXw.png)





## 并发编程



#### syncronized





#### ConcurrentHashmap 结构和实现原理



#### ThreadPoolExcuter 原理



#### volitate 关键字



# Spring



#### IOC、AOP



#### 动态代理



#### Spring 拦截机制



# 数据库相关



### 索引原理





### 最左匹配原则





### 读写分离



### 分库方法ThreadLocal



### 事务



#### 事务的四个隔离级别、mysql的默认隔离级别



#### 数据锁



#### 死锁、死锁的条件





#### 事务注解的用法





#### OCID理解

原子性、有序性、可先行、幂等性



#### Mysql调优





# 开源框架





### 数据存储

#### memcache

#### redis 利弊

#### redis 锁

#### redis aof、rdb 落盘方式

#### redis集群部署

#### 一致性哈希算法

#### Mongo数据库





### RocketMQ 



#### 消息丢失



#### 监听



#### 高可用部署



### ElasticJob



#### 架构、流程图



#### 执行过程



#### 选举算法



#### 其他作业调度框架



### mybatis用法



# 网络编程

## TCP编程示例

## UDP编程实例

**UDP多线程在线咨询**

TalkSend.java:

```java
public class TalkSend implements Runnable {
    private int fromPort;
    private String toIP;
    private int toPort;
    
    DatagramSocket socket = null;
    BufferedReader reader = null;
    
    public TalkSend(int fromPort, String toIP, int toPort){
        this.fromPort = fromPort;
        this.toIP = toIP;
        this.toPort = toPort;
        
        try {
            socket = new DatagramSocket(fromPort);
            reader = new BufferReader(new InputStreamReader(System.in));
        } catch (Exception e){
            e.printStackTrace();
        }
    }
    
    @Override
    public void run() {
        while(true){ 
        	try {
                String data = reader.readLine();
                byte[] datas = data.getBytes();
                DatagramPacket packet = new DatagramPacket(datas, 0, datas.lenggth, new InetSocketAddress(this.toIP, this.toPort));
                
                socket.send(packet);
                if(data.equals("bye")) {
                    break;
                }
            }catch(Exception e){
                e.printStackTrace();
            }
        }
    }
}
```

TalkRcvd.java

```java
public class TalkRcvd implements Runnable {
    DatagramSocket socket = null;
    private int port;
    private String msgFrom;
    
    
    public TalkRcvd(int port, String msgFrom){
        this.port = port;
        this.msgFrom = msgFrom;
        try {
            socket = new DatagramSocket(port);
        }catch (Exception e) {
            e.printStackTrace();
        }
    }
    
    @Override
    public void run() {
        while(true){
            try{
                byte[] container = new byte[1024];
                DatagramPacket packet = new DatagramPacket(container, 0, container.length);
                socket.receive(packet);
                
                // Disconnect
                byte[] data = packet = getData();
                String receiveData = new String(data, 0, data.length);
                
                System.out.println(msgFrom + ": " + receiveData);
                
                if(receiveData.equals("bye")) {
                    break;
                }
            } catch (Exception e) {
                e.printStackTracer();
            }
        }
        socket.close();
    }
}
```





### 网络超时



#### 避免重试



#### cookie机制



#### http、https

#### cdn原理























