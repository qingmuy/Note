# Java



## 基础



### 概念与常识

 **Java 是编译与解释共存的语言**，首先将java代码编译为字节码文件，即`.class`文件，而后JVM将字节码文件载入并逐行进行解释执行，但是该方法执行速度较慢，而后引入了`JIT`(**Just in Time Compilation**)，其属于即时编译，当JIT编译器完成第一次编译之后，其将字节码对应的机器码保存下来，后续可以直接使用。这就是编译与解释共存。



#### AOT模式的优缺点

`AOT`自Java9被引入，与`JIT`不同之处在于该百衲衣模式会在程序被执行前就将代码编译为机器码，属于静态编译，AOT减少了JIT预热各方米娜的开销，提高了Java程序的启动速度。且AOT还能减少内存占用和增强Java程序的安全性，对微服务架构的支持较为友好，适合云原生场景。



但是AOT无法支持Java的一些动态特性，如反射、动态代理、动态加载、JNI等，导致许多库与框架(Spring、CGLIB)无法使用，所以仍然使用JIT即时编译器。

![JIT vs AOT](D:\Note\Note\面经笔记\assets\jit-vs-aot.png)

### 基本语法

#### 标识符与关键字

在编写程序的时候，需要大量地为程序、类、变量、方法等取名字，于是就有了 **标识符** 。简单来说， **标识符就是一个名字** 。

**关键字是被赋予特殊含义的标识符** 。

> Tips：所有的关键字都是小写的，在 IDE 中会以特殊颜色显示。
>
> `default` 这个关键字很特殊，既属于程序控制，也属于类，方法和变量修饰符，还属于访问控制。
>
> - 在程序控制中，当在 `switch` 中匹配不到任何情况时，可以使用 `default` 来编写默认匹配的情况。
> - 在类，方法和变量修饰符中，从 JDK8 开始引入了默认方法，可以使用 `default` 关键字来定义一个方法的默认实现。
> - 在访问控制中，如果一个方法前没有任何修饰符，则默认会有一个修饰符 `default`，但是这个修饰符加上了就会报错。

⚠️ 注意：虽然 `true`, `false`, 和 `null` 看起来像关键字但实际上他们是字面值，同时你也不可以作为标识符来使用。



#### 自增自减运算符

即`++`与`--`，其可置于变量前后，若置于变量后则代表先使用变量再进行加减操作，置于变量后则代表先进行加减操作再使用变量



#### 移位运算符

Java 中有三种移位运算符：

- `<<` :左移运算符，向左移若干位，高位丢弃，低位补零。`x << n`,相当于 x 乘以 2 的 n 次方(不溢出的情况下)。
- `>>` :带符号右移，向右移若干位，高位补符号位，低位丢弃。正数高位补 0,负数高位补 1。`x >> n`,相当于 x 除以 2 的 n 次方。
- `>>>` :无符号右移，忽略符号位，空位都以 0 补齐。

虽然移位运算本质上可以分为左移和右移，但在实际应用中，右移操作需要考虑符号位的处理方式。

由于 `double`，`float` 在二进制中的表现比较特殊，因此不能来进行移位操作。

移位操作符实际上支持的类型只有`int`和`long`，编译器在对`short`、`byte`、`char`类型进行移位前，都会将其转换为`int`类型再操作。



### 基本数据类型

- 6 种数字类型： 
  - 4 种整数型：`byte`、`short`、`int`、`long`
  - 2 种浮点型：`float`、`double`
- 1 种字符类型：`char`
- 1 种布尔型：`boolean`。

这 8 种基本数据类型的默认值以及所占空间的大小如下：

| 基本类型  | 位数 | 字节 | 默认值  | 取值范围                                                     |
| :-------- | :--- | :--- | :------ | ------------------------------------------------------------ |
| `byte`    | 8    | 1    | 0       | -128 ~ 127                                                   |
| `short`   | 16   | 2    | 0       | -32768（-2^15） ~ 32767（2^15 - 1）                          |
| `int`     | 32   | 4    | 0       | -2147483648 ~ 2147483647                                     |
| `long`    | 64   | 8    | 0L      | -9223372036854775808（-2^63） ~ 9223372036854775807（2^63 -1） |
| `char`    | 16   | 2    | 'u0000' | 0 ~ 65535（2^16 - 1）                                        |
| `float`   | 32   | 4    | 0f      | 1.4E-45 ~ 3.4028235E38                                       |
| `double`  | 64   | 8    | 0d      | 4.9E-324 ~ 1.7976931348623157E308                            |
| `boolean` | 1    |      | false   | true、false                                                  |

最大正数大小减1是因为最高位为符号位。

对于 `boolean`，官方文档未明确定义，它依赖于 JVM 厂商的具体实现。逻辑上理解是占用 1 位，但是实际中会考虑计算机高效存储因素。

Java 里使用 `long` 类型的数据一定要在数值后面加上 **L**，否则将作为整型解析。

Java 里使用 `float` 类型的数据一定要在数值后面加上 **f 或 F**，否则将无法通过编译。



#### 基本类型和包装类型

二者区别如下：

**用途**：除了定义一些常量和局部变量之外，我们在其他地方比如方法参数、对象属性中很少会使用基本类型来定义变量。并且，包装类型可用于泛型，而基本类型不可以。

**存储方式**：基本数据类型的局部变量存放在 Java 虚拟机栈中的局部变量表中，基本数据类型的成员变量（未被 `static` 修饰 ）存放在 Java 虚拟机的堆中。包装类型属于对象类型，我们知道几乎所有对象实例都存在于堆中。

**占用空间**：相比于包装类型（对象类型）， 基本数据类型占用的空间往往非常小。

**默认值**：成员变量包装类型不赋值就是 `null` ，而基本类型有默认值且不是 `null`。

**比较方式**：对于基本数据类型来说，`==` 比较的是值。对于包装数据类型来说，`==` 比较的是对象的内存地址。所有整型包装类对象之间值的比较，全部使用 `equals()` 方法。



注意：**基本数据类型存放在栈中是一个常见的误区！** 基本数据类型的存储位置取决于它们的作用域和声明方式。如果它们是局部变量，那么它们会存放在栈中；如果它们是成员变量，那么它们会存放在堆中。成员变量被`static`修饰过后会存储在方法区中。



#### 包装类型的缓存机制

`Byte`,`Short`,`Integer`,`Long` 这 4 种包装类默认创建了数值 **[-128，127]** 的相应类型的缓存数据，`Character` 创建了数值在 **[0,127]** 范围的缓存数据，`Boolean` 直接返回 `True` or `False`。

也就是说，如果使用

```java
Integer i = 40;
Integer j = new Integer(40);
```

二者创建出来的对象使用`==`对比是不相等的，因为第一个在编译后相当于

```java
Integer i = Integer.valueOf(40);
```

若在[-128, 127]的范围内则是直接从缓存中取到的值，超出该范围则使用`new`创建。

两种浮点数类型的包装类 `Float`,`Double` 并没有实现缓存机制。

因此需要注意的是：**所有整型包装类对象之间值的比较，全部使用 equals 方法比较**。

![img](D:\Note\面经笔记\assets\up-1ae0425ce8646adfb768b5374951eeb820d.png)



#### 拆装箱

**如果频繁拆装箱的话，也会严重影响系统的性能。我们应该尽量避免不必要的拆装箱操作。**



#### 浮点数精度丢失

使用`BigDecimal`进行精确运算。



### 变量

#### 成员变量与局部变量的区别？

- **语法形式**：从语法形式上看，成员变量是属于类的，而局部变量是在代码块或方法中定义的变量或是方法的参数；成员变量可以被 `public`,`private`,`static` 等修饰符所修饰，而局部变量不能被访问控制修饰符及 `static` 所修饰；但是，成员变量和局部变量都能被 `final` 所修饰。
- **存储方式**：从变量在内存中的存储方式来看，如果成员变量是使用 `static` 修饰的，那么这个成员变量是属于类的，如果没有使用 `static` 修饰，这个成员变量是属于实例的。而对象存在于堆内存，局部变量则存在于栈内存。
- **生存时间**：从变量在内存中的生存时间上看，成员变量是对象的一部分，它随着对象的创建而存在，而局部变量随着方法的调用而自动生成，随着方法的调用结束而消亡。
- **默认值**：从变量是否有默认值来看，成员变量如果没有被赋初始值，则会自动以类型的默认值而赋值（一种情况例外:被 `final` 修饰的成员变量也必须显式地赋值），而局部变量则不会自动赋值。

**为什么成员变量有默认值？**

1. 先不考虑变量类型，如果没有默认值会怎样？变量存储的是内存地址对应的任意随机值，程序读取该值运行会出现意外。
2. 默认值有两种设置方式：手动和自动，根据第一点，没有手动赋值一定要自动赋值。成员变量在运行时可借助反射等方法手动赋值，而局部变量不行。
3. 对于编译器（javac）来说，局部变量没赋值很好判断，可以直接报错。而成员变量可能是运行时赋值，无法判断，误报“没默认值”又会影响用户体验，所以采用自动赋默认值。



#### 静态变量

静态变量也就是被 `static` 关键字修饰的变量。它可以被类的所有实例共享，无论一个类创建了多少个对象，它们都共享同一份静态变量。也就是说，静态变量只会被分配一次内存，即使创建多个对象，这样可以节省内存。



### 方法

#### 静态变量的作用

静态变量也就是被 `static` 关键字修饰的变量。它可以被类的所有实例共享，无论一个类创建了多少个对象，它们都共享同一份静态变量。也就是说，静态变量只会被分配一次内存，即使创建多个对象，这样可以节省内存。



#### 静态方法不能调用非静态成员的原因

这个需要结合 JVM 的相关知识，主要原因如下：

1. 静态方法是属于类的，在类加载的时候就会分配内存，可以通过类名直接访问。而非静态成员属于实例对象，只有在对象实例化之后才存在，需要通过类的实例对象去访问。
2. 在类的非静态成员不存在的时候静态方法就已经存在了，此时调用在内存中还不存在的非静态成员，属于非法操作。

**注意**：可以导入静态方法，如`import static 相对位置/方法名`



#### 静态方法与实例方法

**调用方式**

在外部调用静态方法时，可以使用 `类名.方法名` 的方式，也可以使用 `对象.方法名` 的方式，而实例方法只有后面这种方式。也就是说，**调用静态方法可以无需创建对象** 。

不过，需要注意的是一般不建议使用 `对象.方法名` 的方式来调用静态方法。这种方式非常容易造成混淆，静态方法不属于类的某个对象而是属于这个类。

因此，一般建议使用 `类名.方法名` 的方式来调用静态方法。

**访问类成员是否存在限制**

静态方法在访问本类的成员时，只允许访问静态成员（即静态成员变量和静态方法），不允许访问实例成员（即实例成员变量和实例方法），而实例方法不存在这个限制。



### 重载与重写

> 重载就是同样的一个方法能够根据输入数据的不同，做出不同的处理
>
> 重写就是当子类继承自父类的相同方法，输入数据一样，但要做出有别于父类的响应时，你就要覆盖父类方法



#### 重载

发生在同一个类中（或者父类和子类之间），方法名必须相同，参数类型不同、个数不同、顺序不同，方法返回值和访问修饰符可以不同。重载就是同一个类中多个同名方法根据不同的传参来执行不同的逻辑处理。



#### 重写

重写发生在运行期，是子类对父类的允许访问的方法的实现过程进行重新编写。

1. 方法名、参数列表必须相同，子类方法返回值类型应比父类方法返回值类型更小或相等，抛出的异常范围小于等于父类，访问修饰符范围大于等于父类。
2. 如果父类方法访问修饰符为 `private/final/static` 则子类就不能重写该方法，但是被 `static` 修饰的方法能够被再次声明。
3. 构造方法无法被重写



#### 总结

综上：**重写就是子类对父类方法的重新改造，外部样子不能改变，内部逻辑可以改变。**

| 区别点     | 重载方法 | 重写方法                                                     |
| :--------- | :------- | :----------------------------------------------------------- |
| 发生范围   | 同一个类 | 子类                                                         |
| 参数列表   | 必须修改 | 一定不能修改                                                 |
| 返回类型   | 可修改   | 子类方法返回值类型应比父类方法返回值类型更小或相等           |
| 异常       | 可修改   | 子类方法声明抛出的异常类应比父类方法声明抛出的异常类更小或相等； |
| 访问修饰符 | 可修改   | 一定不能做更严格的限制（可以降低限制）                       |
| 发生阶段   | 编译期   | 运行期                                                       |

**方法的重写要遵循“两同两小一大”**（以下内容摘录自《疯狂 Java 讲义》）：

- “两同”即方法名相同、形参列表相同；
- “两小”指的是子类方法返回值类型应比父类方法返回值类型更小或相等，子类方法声明抛出的异常类应比父类方法声明抛出的异常类更小或相等；
- “一大”指的是子类方法的访问权限应比父类方法的访问权限更大或相等。

**注意**：如果方法的返回类型是 void 和基本数据类型，则返回值重写时不可修改。但是如果方法的返回值是引用类型，重写时是可以返回该引用类型的子类的。



### 可变长参数

可变长参数编译后就是一个数组。

可变参数只能作为函数的最后一个参数，但其前面可以有也可以没有任何其他参数。

若遇到方法重载，其优先匹配固定参数，因为固定参数的方法匹配度更高。



### 面向对象基础

#### 面向对象和面向过程的区别

面向对象和面向过程的主要区别在于解决问题的方式不同：

- **面向过程编程（POP）**：面向过程把解决问题的过程拆成一个个方法，通过一个个方法的执行解决问题。
- **面向对象编程（OOP）**：面向对象会先抽象出对象，然后用对象执行方法的方式解决问题。

面向对象编程有如下优点：

- **易维护**：由于良好的结构和封装性，OOP 程序通常更容易维护。
- **易复用**：通过继承和多态，OOP 设计使得代码更具复用性，方便扩展功能。
- **易扩展**：模块化设计使得系统扩展变得更加容易和灵活。

面向过程的编程方式更为简单和直接，适合处理一些较简单的任务。



#### 对象实体与对象引用有何不同

对象实例（对象实例在堆内存中），对象引用指向对象实例（对象引用存放在栈内存中）。



#### 对象的相等和引用相等的区别

- 对象的相等一般比较的是内存中存放的内容是否相等。
- 引用相等一般比较的是他们指向的内存地址是否相等。



#### 构造方法的特点

- **名称与类名相同**：构造方法的名称必须与类名完全一致。
- **没有返回值**：构造方法没有返回类型，且不能使用 `void` 声明。
- **自动执行**：在生成类的对象时，构造方法会自动执行，无需显式调用。

构造方法**不能被重写（override）**，但**可以被重载（overload）**。因此，一个类中可以有多个构造方法，这些构造方法可以具有不同的参数列表，以提供不同的对象初始化方式。



#### 面向对象三大特征

**封装**、**继承**、**多态**

##### 封装

封装是指把一个对象的状态信息（也就是属性）隐藏在对象内部，不允许外部对象直接访问对象的内部信息。但是可以提供一些可以被外界访问的方法来操作属性。



##### 继承

继承是使用已存在的类的定义作为基础建立新类的技术，新类的定义可以增加新的数据或新的功能，也可以用父类的功能，但不能选择性地继承父类。通过使用继承，可以快速地创建新的类，可以提高代码的重用，程序的可维护性，节省大量创建新类的时间 ，提高开发效率。

使用继承需要注意的地方：

子类拥有父类对象所有的属性和方法（包括私有属性和私有方法），但是父类中的私有属性和方法子类是无法访问，**只是拥有**。

子类可以拥有自己属性和方法，即子类可以对父类进行扩展。

子类可以用自己的方式实现父类的方法。（以后介绍）



##### 多态

表示一个对象具有多种的状态，具体表现为父类的引用指向子类的实例。

特点如下：

对象类型和引用类型之间具有继承（类）/实现（接口）的关系；

引用类型变量发出的方法调用的到底是哪个类中的方法，必须在程序运行期间才能确定；

多态不能调用“只在子类存在但在父类不存在”的方法；

如果子类重写了父类的方法，真正执行的是子类重写的方法，如果子类没有重写父类的方法，执行的是父类的方法。



#### 接口和抽象类的共同点与区别

##### 共同点

- **实例化**：接口和抽象类都不能直接实例化，只能被实现（接口）或继承（抽象类）后才能创建具体的对象。
- **抽象方法**：接口和抽象类都可以包含抽象方法。抽象方法没有方法体，必须在子类或实现类中实现。



##### 区别

- **设计目的**：接口主要用于对类的行为进行约束，你实现了某个接口就具有了对应的行为。抽象类主要用于代码复用，强调的是所属关系。

- **继承和实现**：一个类只能继承一个类（包括抽象类），因为 Java 不支持多继承。但一个类可以实现多个接口，一个接口也可以继承多个其他接口。

- **成员变量**：接口中的成员变量只能是 `public static final` 类型的，不能被修改且必须有初始值。抽象类的成员变量可以有任何修饰符（`private`, `protected`, `public`），可以在子类中被重新定义或赋值。

- **方法**： 

  - Java 8 之前，接口中的方法默认是 `public abstract` ，也就是只能有方法声明。自 Java 8 起，可以在接口中定义 `default`（默认） 方法和 `static` （静态）方法。 自 Java 9 起，接口可以包含 `private` 方法。

  - 抽象类可以包含抽象方法和非抽象方法。抽象方法没有方法体，必须在子类中实现。非抽象方法有具体实现，可以直接在抽象类中使用或在子类中重写。

自从Java8之后，接口引入了新的方法类型：`default` 方法、`static` 方法和 `private` 方法。这些方法让接口的使用更加灵活。

Java 8 引入的`static` 方法无法在实现类中被覆盖，只能通过接口名直接调用（ `MyInterface.staticMethod()`），类似于类中的静态方法。`static` 方法通常用于定义一些通用的、与接口相关的工具方法，一般很少用。

Java 9 允许在接口中使用 `private` 方法。`private`方法可以用于在接口内部共享代码，不对外暴露。



#### 深拷贝与浅拷贝的区别

**浅拷贝**：浅拷贝会在堆上创建一个新的对象（区别于引用拷贝的一点），不过，如果原对象内部的属性是引用类型的话，浅拷贝会直接复制内部对象的引用地址，也就是说拷贝对象和原对象共用同一个内部对象。

**深拷贝**：深拷贝会完全复制整个对象，包括这个对象所包含的内部对象，即引用也会被复制。



#### Object

Object类是所有类的父类，其提供了11个基本的方法。

```java
/**
 * native 方法，用于返回当前运行时对象的 Class 对象，使用了 final 关键字修饰，故不允许子类重写。
 */
public final native Class<?> getClass()
/**
 * native 方法，用于返回对象的哈希码，主要使用在哈希表中，比如 JDK 中的HashMap。
 */
public native int hashCode()
/**
 * 用于比较 2 个对象的内存地址是否相等，String 类对该方法进行了重写以用于比较字符串的值是否相等。
 */
public boolean equals(Object obj)
/**
 * native 方法，用于创建并返回当前对象的一份拷贝。
 */
protected native Object clone() throws CloneNotSupportedException
/**
 * 返回类的名字实例的哈希码的 16 进制的字符串。建议 Object 所有的子类都重写这个方法。
 */
public String toString()
/**
 * native 方法，并且不能重写。唤醒一个在此对象监视器上等待的线程(监视器相当于就是锁的概念)。如果有多个线程在等待只会任意唤醒一个。
 */
public final native void notify()
/**
 * native 方法，并且不能重写。跟 notify 一样，唯一的区别就是会唤醒在此对象监视器上等待的所有线程，而不是一个线程。
 */
public final native void notifyAll()
/**
 * native方法，并且不能重写。暂停线程的执行。注意：sleep 方法没有释放锁，而 wait 方法释放了锁 ，timeout 是等待时间。
 */
public final native void wait(long timeout) throws InterruptedException
/**
 * 多了 nanos 参数，这个参数表示额外时间（以纳秒为单位，范围是 0-999999）。 所以超时的时间还需要加上 nanos 纳秒。。
 */
public final void wait(long timeout, int nanos) throws InterruptedException
/**
 * 跟之前的2个wait方法一样，只不过该方法一直等待，没有超时时间这个概念
 */
public final void wait() throws InterruptedException
/**
 * 实例被垃圾回收器回收的时候触发的操作
 */
protected void finalize() throws Throwable { }
```



##### `==`和`equals()`的区别

对于基本数据类型来说，`==` 比较的是值。

对于引用数据类型来说，`==` 比较的是对象的内存地址。

若类没有重写equals方法，则使用的默认是`Object`类的`equals()`方法；而一般情况下都会重写`equals()`方法比较两个对象中的属性是否相等，若属性相等即返回true。



##### hashCode()的作用

用于计算哈希码，计算出的值是一个`int`整数，计算出的哈希码即为该对象再哈希表中的索引位置。



##### hashCode存在的意义

以HashSet举例，在一个对象加入HashSet前，会先计算该对象的`HashCode`，同时也会与已经加入的对象的`HashCode`及逆行比较，若出现相同的HashCode则会使用`equals()`方法检查二者是否真正的相等。若不相等则将重新散列到其他的位置。

即通过`HashCode`来简化对对象的比较。



##### 重写`equals()`必须重写`hashCode()`的原因

避免哈希碰撞导致的问题。如果在使用`HashMap`时只重写了equals方法而没有重写hashCode方法，会导致相同的键的二次散列问题。



#### String



##### String、StringBuffer、StringBuilder 的区别

String本身是不可变的。

`StringBuilder` 与 `StringBuffer` 都继承自 `AbstractStringBuilder` 类，在 `AbstractStringBuilder` 中也是使用字符数组保存字符串，不过没有使用 `final` 和 `private` 关键字修饰，最关键的是这个 `AbstractStringBuilder` 类还提供了很多修改字符串的方法比如 `append` 方法。



**线程安全性**：`StringBuffer` 对方法加了同步锁或者对调用的方法加了同步锁，所以是线程安全的。`StringBuilder` 并没有对方法进行加同步锁，所以是非线程安全的。

**性能**：每次对 `String` 类型进行改变的时候，都会生成一个新的 `String` 对象，然后将指针指向新的 `String` 对象。`StringBuffer` 每次都会对 `StringBuffer` 对象本身进行操作，而不是生成新的对象并改变对象引用。相同情况下使用 `StringBuilder` 相比使用 `StringBuffer` 仅能获得 10%~15% 左右的性能提升，但却要冒多线程不安全的风险。



**总结**

- 操作少量的数据: 适用 `String`
- 单线程操作字符串缓冲区下操作大量数据: 适用 `StringBuilder`
- 多线程操作字符串缓冲区下操作大量数据: 适用 `StringBuffer`



##### String不可变的原因

保存字符串的数组被 `final` 修饰且为私有的，并且`String` 类没有提供/暴露修改这个字符串的方法。

`String` 类被 `final` 修饰导致其不能被继承，进而避免了子类破坏 `String` 不可变。



##### String的底层实现

Java 9 将 `String` 的底层实现由 `char[]` 改成了 `byte[]`

原因是`byte` 相较 `char` 节省一半的内存空间。



##### 字符串的拼接

Java 语言本身并不支持运算符重载，“+”和“+=”是专门为 String 类重载过的运算符，也是 Java 中仅有的两个重载过的运算符。

```java
String str1 = "he";
String str2 = "llo";
String str3 = "world";
String str4 = str1 + str2 + str3;
```

上述代码对应字节码为：

![img](D:\Note\Note\面经笔记\assets\image-20220422161637929.png)

可以看出：字符串通过"+"方法进行字符串拼接的方式实际上是通过`StringBuilder` 调用 `append()` 方法实现的，拼接完成之后调用 `toString()` 得到一个 `String` 对象 



**注意**：若是在循环中使用`StringBuilder`则会创建多个`StringBuilder`对象，如果直接使用其做字符串拼接则不会出现该问题。

在 JDK 9 中，字符串相加“+”改为用动态方法 `makeConcatWithConstants()` 来实现，通过提前分配空间从而减少了部分临时对象的创建。然而这种优化主要针对简单的字符串拼接，如： `a+b+c` 。对于循环中的大量拼接操作，仍然会逐个动态分配内存（类似于两个两个 append 的概念），并不如手动使用 StringBuilder 来进行拼接效率高。



##### String的equals()方法和Object的equals()方法的区别

String的equals方法被重写过，其比较的是String字符串的值；而Object的equals方法则是比较对象的内存地址。



##### 字符串常量池

**字符串常量池** 是 JVM 为了提升性能和减少内存消耗针对字符串（String 类）专门开辟的一块区域，主要目的是为了避免字符串的重复创建。

即若创建完一个字符串对象，则该字符串对象的引用会保存在字符串常量池中，若再创建相同的对象则会直接返回字符串常量池中的相同字符串的对象引用。

```java
// 在堆中创建字符串对象”ab“
// 将字符串对象”ab“的引用保存在字符串常量池中
String aa = "ab";
// 直接返回字符串常量池中字符串对象”ab“的引用
String bb = "ab";
System.out.println(aa==bb);// true
```



##### new String("*")创建了几个字符串对象

创建了1或2个，参考字符串常量池，若该对象存在于字符串常量池则只创建1个对象；若不存在则创建2个。



##### String的intern方法的作用

`String.intern()` 是一个 native（本地）方法，其作用是将指定的字符串对象的引用保存在字符串常量池中，可以简单分为两种情况：

- 如果字符串常量池中保存了对应的字符串对象的引用，就直接返回该引用。
- 如果字符串常量池中没有保存了对应的字符串对象的引用，那就在常量池中创建一个指向该字符串对象的引用并返回。



##### String类型的变量和常量做"+"运算

由于被`final`关键字修饰过的常量属于类，其会在编译期直接存入字符串常量池，并且，字符串常量拼接得到的字符串常量在编译阶段就已经被存放字符串常量池。



在编译过程中，编译器会进行一个叫做`常量折叠`的代码优化。

常量折叠会把常量表达式的值求出来作为常量嵌在最终生成的代码中，这是 Javac 编译器会对源代码做的极少量优化措施之一(代码优化几乎都在即时编译器中进行)。

对于 `String str3 = "str" + "ing";` 编译器会给你优化成 `String str3 = "string";` 。

并不是所有的常量都会进行折叠，只有编译器在程序编译期就可以确定值的常量才可以：

- 基本数据类型( `byte`、`boolean`、`short`、`char`、`int`、`float`、`long`、`double`)以及字符串常量。
- `final` 修饰的基本数据类型和字符串变量
- 字符串通过 “+”拼接得到的字符串、基本数据类型之间算数运算（加减乘除）、基本数据类型的位运算（<<、>>、>>> ）

而**引用的值在程序编译期是无法确定的，编译器无法对其进行优化。**

举例如下：

```java
String str1 = "str";
String str2 = "ing";
String str3 = "str" + "ing";
String str4 = str1 + str2;
String str5 = "string";
System.out.println(str3 == str4);//false
System.out.println(str3 == str5);//true
System.out.println(str4 == str5);//false
```



### 异常

> ![Java 异常类层次结构图](D:\Note\Note\面经笔记\assets\types-of-exceptions-in-java.png)



#### Exception 和 Error 的区别

所有的异常都有一个共同的祖先： `java.lang` 包中的 `Throwable` 类。`Throwable` 类有两个重要的子类：

> - **`Exception`** :程序本身可以处理的异常，可以通过 `catch` 来进行捕获。`Exception` 又可以分为 Checked Exception (受检查异常，必须处理) 和 Unchecked Exception (不受检查异常，可以不处理)。
>
> - **`Error`**：`Error` 属于程序无法处理的错误 ，我们没办法通过 `catch` 来进行捕获不建议通过`catch`捕获 。例如 Java 虚拟机运行错误（`Virtual MachineError`）、虚拟机内存不够错误(`OutOfMemoryError`)、类定义错误（`NoClassDefFoundError`）等 。这些异常发生时，Java 虚拟机（JVM）一般会选择线程终止。



#### Checked Exception 和 Unchecked Exception 的区别

> Checked Exception 即 受检查异常 ，Java 代码在编译过程中，如果受检查异常没有被 `catch`或者`throws` 关键字处理的话，就没办法通过编译。

可以理解为显式错误：即编译器强制开发者处理，该类型异常主要是由于特殊（外界）原因发生的，为了提高程序的健壮性所以必须要进行处理：例如网络连接错误的IOException。

> 除了`RuntimeException`及其子类以外，其他的`Exception`类及其子类都属于受检查异常 。常见的受检查异常有：IO 相关的异常、`ClassNotFoundException`、`SQLException`...。



> **Unchecked Exception** 即 **不受检查异常** ，Java 代码在编译过程中 ，我们即使不处理不受检查异常也可以正常通过编译。

可以理解为只有在代码运行时才会产生的错误。



checked Exception：代表程序不能直接控制的无效外界情况（如用户输入，数据库访问，网络异常，文件访问和丢失等），除了Error和RuntimeException及其子类之外的异常， 需要try catch处理或throws声明抛出异常。
Unchecked Exception：指的是程序的瑕疵或逻辑错误，并且在运行时无法恢复，包括系统异常，语法上不需要声明抛出异常。



#### Throwable 的常用方法

> `String getMessage()`: 返回异常发生时的简要描述
>
> `String toString()`: 返回异常发生时的详细信息
>
> `String getLocalizedMessage()`: 返回异常对象的本地化信息。使用 `Throwable` 的子类覆盖这个方法，可以生成本地化信息。如果子类没有覆盖该方法，则该方法返回的信息与 `getMessage()`返回的结果相同
>
> `void printStackTrace()`: 在控制台上打印 `Throwable` 对象封装的异常信息



#### try-catch-finally语句

当在 `try` 块或 `catch` 块中遇到 `return` 语句时，`finally` 语句块将在方法返回之前被执行。

> **注意：不要在 finally 语句块中使用 return!** 当 try 语句和 finally 语句中都有 return 语句时，try 语句块中的 return 语句会被忽略。这是因为 try 语句中的 return 返回值会先被暂存在一个本地变量中，当执行到 finally 语句中的 return 之后，这个本地变量的值就变为了 finally 语句中的 return 返回值。



#### finally中的代码不一定会执行

如在finally语句执行前虚拟机被终止运行、程序所在线程死亡、CPU被关闭，则finally语句内代码不会被执行。



#### try-with-resources

语句结束可以自动关闭声明的资源。

> **适用范围（资源的定义）：** 任何实现 `java.lang.AutoCloseable`或者 `java.io.Closeable` 的对象
>
> **关闭资源和 finally 块的执行顺序：** 在 `try-with-resources` 语句中，任何 catch 或 finally 块在声明的资源关闭后运行

使用`try-catch-resources`语句：

```java
try (Scanner scanner = new Scanner(new File("test.txt"))) {
    while (scanner.hasNext()) {
        System.out.println(scanner.nextLine());
    }
} catch (FileNotFoundException fnfe) {
    fnfe.printStackTrace();
}
```

也可以关闭多个资源：

```java
try (BufferedInputStream bin = new BufferedInputStream(new FileInputStream(new File("test.txt")));
     BufferedOutputStream bout = new BufferedOutputStream(new FileOutputStream(new File("out.txt")))) {
    int b;
    while ((b = bin.read()) != -1) {
        bout.write(b);
    }
}
catch (IOException e) {
    e.printStackTrace();
}
```



#### 使用异常需要注意的地方

> - 不要把异常定义为静态变量，因为这样会导致异常栈信息错乱。每次手动抛出异常，我们都需要手动 new 一个异常对象抛出。
>
> - 抛出的异常信息一定要有意义。
>
> - 建议抛出更加具体的异常比如字符串转换为数字格式错误的时候应该抛出`NumberFormatException`而不是其父类`IllegalArgumentException`。
>
> - 避免重复记录日志：如果在捕获异常的地方已经记录了足够的信息（包括异常类型、错误信息和堆栈跟踪等），那么在业务代码中再次抛出这个异常时，就不应该再次记录相同的错误信息。重复记录日志会使得日志文件膨胀，并且可能会掩盖问题的实际原因，使得问题更难以追踪和解决。



### 泛型

**Java 泛型（Generics）** 是 JDK 5 中引入的一个新特性。使用泛型参数，可以增强代码的可读性以及稳定性。

> 编译器可以对泛型参数进行检测，并且通过泛型参数可以指定传入的对象类型。比如 `ArrayList<Person> persons = new ArrayList<Person>()` 这行代码就指明了该 `ArrayList` 对象只能传入 `Person` 对象，如果传入其他类型的对象就会报错。

并且，原生 `List` 返回类型是 `Object` ，需要手动转换类型才能使用，使用泛型后编译器自动转换。

**注意：泛型属于语法糖！**



#### 泛型的使用方式

泛型一般有三种使用方式:**泛型类**、**泛型接口**、**泛型方法**。

**泛型类**：

```java
//此处T可以随便写为任意标识，常见的如T、E、K、V等形式的参数常用于表示泛型
//在实例化泛型类时，必须指定T的具体类型
public class Generic<T>{
    private T key;
}
```

实例化泛型类：

```java
Generic<Integer> genericInteger = new Generic<Integer>(123456);
```



**泛型接口**：

```java
public interface Generator<T> {
    public T method();
}
```

实现泛型接口，不指定类型：

```java
class GeneratorImpl<T> implements Generator<T>{
    @Override
    public T method() {
        return null;
    }
}
```

实现泛型接口，指定类型：

```java
class GeneratorImpl implements Generator<String> {
    @Override
    public String method() {
        return "hello";
    }
}
```



**泛型方法**：

```java
   public static < E > void printArray( E[] inputArray )
   {
         for ( E element : inputArray ){
            System.out.printf( "%s ", element );
         }
         System.out.println();
    }
```

使用：

```java
// 创建不同类型数组：Integer, Double 和 Character
Integer[] intArray = { 1, 2, 3 };
String[] stringArray = { "Hello", "World" };
printArray( intArray  );
printArray( stringArray  );
```



注意：由于泛型本身是一个语法糖，其本身并没有任何意义，只是助于开发者理解，所以只有当传递真实的类型参数之后其才有意义；若要传递泛型类型的参数则必须先完成类的实例化，而静态方法与类绑定，其加载优先程度是高于任何一个类的实例化的，所以其只能使用自身声明的泛型`<E>`



#### 举例项目中使用到泛型的地方

> - 自定义接口通用返回结果 `CommonResult<T>` 通过参数 `T` 可根据具体的返回类型动态指定结果的数据类型
>
> - 定义 `Excel` 处理类 `ExcelUtil<T>` 用于动态指定 `Excel` 导出的数据类型
>
> - 构建集合工具类（参考 `Collections` 中的 `sort`, `binarySearch` 方法）。



### 反射

通过反射可以获取任意一个类的所有属性和方法，还可以调用这些方法和属性。



#### 反射优缺点

优点：

> 反射可以使代码更加灵活、为各种框架提供开箱即用的功能提供了便利。

缺点：

> 基本无视了访问权限：如public、pricate等。
>
> 增加了安全问题，比如可以无视泛型参数的安全检查（泛型参数的安全检查发生在编译时）。
>
> 反射的性能也要稍差点，不过，对于框架来说实际是影响不大的。



#### 使用场景

> 像 Spring/Spring Boot、MyBatis 等等框架中都大量使用了反射机制。
>
> **这些框架中也大量使用了动态代理，而动态代理的实现也依赖反射。**
>
> **注解** 的实现也用到了反射。



### 注解

> `Annotation` （注解）自Java5 开始被引入，可以看作是一种特殊的注释，主要用于修饰类、方法或者变量，提供某些信息供程序在编译或者运行时使用。



#### 注解的解析方法

> **编译期直接扫描**：编译器在编译 Java 代码的时候扫描对应的注解并处理，比如某个方法使用`@Override` 注解，编译器在编译的时候就会检测当前的方法是否重写了父类对应的方法。
>
> **运行期通过反射处理**：像框架中自带的注解(比如 Spring 框架的 `@Value`、`@Component`)都是通过反射来进行处理的。



### SPI

> SPI 即 Service Provider Interface ，字面意思就是：“服务提供者的接口”，我的理解是：专门提供给服务提供者或者扩展框架功能的开发者去使用的一个接口。
>
> SPI 将服务接口和具体的服务实现分离开来，将服务调用方和服务实现者解耦，能够提升程序的扩展性、可维护性。修改或者替换服务实现并不需要修改调用方。



#### SPI和API的区别

> ![SPI VS API](D:\Note\Note\面经笔记\assets\spi-vs-api.png)



#### SPI优缺点

优点：通过 SPI 机制能够大大地提高接口设计的灵活性。

缺点：

- 需要遍历加载所有的实现类，不能做到按需加载，这样效率还是相对较低的。
- 当多个 `ServiceLoader` 同时 `load` 时，会有并发问题。



### 序列化和反序列化

通过序列化将Java对象保存在文件中，亦或在网络中传输Java对象。

> - **序列化**：将数据结构或对象转换成二进制字节流的过程
> - **反序列化**：将在序列化过程中所生成的二进制字节流转换成数据结构或者对象的过程

一些使用序列化的常用场景：

> - 对象在进行网络传输（比如远程方法调用 RPC 的时候）之前需要先被序列化，接收到序列化的对象之后需要再进行反序列化；
>
> - 将对象存储到文件之前需要进行序列化，将对象从文件中读取出来需要进行反序列化；
>
> - 将对象存储到数据库（如 Redis）之前需要用到序列化，将对象从缓存数据库中读取出来需要反序列化；
>
> - 将对象存储到内存之前需要进行序列化，从内存中读取出来之后需要进行反序列化。

总结为：

> **序列化的主要目的是通过网络传输对象或者说是将对象存储到文件系统、数据库、内存中。**



#### 序列化协议与TCP/IP 4层模型对应位置

> ![TCP/IP 四层模型](D:\Note\Note\面经笔记\assets\tcp-ip-4-model.png)

由图可知，在OSI 七层协议模型中，表示层做的工作即将用户数据转换为二进制流，而表示层属于TCP/IP层模型中的应用层， 所以对应应用层一部分。



#### 禁止序列化

使用 `transient` 关键字修饰不想进行序列化的变量。

> `transient` 关键字的作用是：阻止实例中那些用此关键字修饰的的变量序列化；当对象被反序列化时，被 `transient` 修饰的变量值不会被持久化和恢复。

需要注意的是：

> - `transient` 只能修饰变量，不能修饰类和方法。
>
> - `transient` 修饰的变量，**在反序列化后变量值将会被置成类型的默认值**。例如，如果是修饰 `int` 类型，那么反序列后结果就是 `0`。
>
> - `static` 变量因为不属于任何对象(Object)，所以无论有没有 `transient` 关键字修饰，均不会被序列化。



#### 常见的序列化协议

> JDK 自带的序列化方式一般不会用 ，因为序列化效率低并且存在安全问题。比较常用的序列化协议有 Hessian、Kryo、Protobuf、ProtoStuff，这些都是基于二进制的序列化协议。
>
> 像 JSON 和 XML 这种属于文本类序列化方式。虽然可读性比较好，但是性能较差，一般不会选择。



#### JDK自带序列化的缺点

> **不支持跨语言调用** : 如果调用的是其他语言开发的服务的时候就不支持了。
>
> **性能差**：相比于其他序列化框架性能更低，主要原因是序列化之后的字节数组体积较大，导致传输成本加大。
>
> **存在安全问题**：序列化和反序列化本身并不存在问题。但当输入的反序列化的数据可被用户控制，那么攻击者即可通过构造恶意输入，让反序列化产生非预期的对象，在此过程中执行构造的任意代码。



### I/O

即输入输出流，因其类似水流，所以称之为IO流，根据数据的处理方式分为字节流和字符流。

> Java IO 流的 40 多个类都是从如下 4 个抽象类基类中派生出来的。
>
> - `InputStream`/`Reader`: 所有的输入流的基类，前者是字节输入流，后者是字符输入流。
> - `OutputStream`/`Writer`: 所有输出流的基类，前者是字节输出流，后者是字符输出流。



#### 字节流和字符流的区别

问题的本质在于：**不管是文件读写还是网络发送接收，信息的最小存储单元都是字节，那为什么 I/O 流操作要分为字节流操作和字符流操作呢？**

原因如下：

> - 字符流是由 Java 虚拟机将字节转换得到的，这个过程还算是比较耗时；
> - 如果不知道编码类型的话，使用字节流的过程中很容易出现乱码问题。



### 语法糖

> **语法糖（Syntactic sugar）** 代指的是编程语言为了方便程序员开发程序而设计的一种特殊语法，这种语法对编程语言的功能并没有影响。实现相同的功能，基于语法糖写出来的代码往往更简单简洁且更易阅读。

实际上JVM并不能识别语法糖，若要正确执行语法糖，需要先通过编译器解糖：即在编译阶段将语法糖转换为基本语法。

解糖的方法在：`com.sun.tools.javac.main.JavaCompiler`源码中`compile()`中的`desugar()`方法。

**Java中真正支持语法糖的是Java编译器而不是JVM。**



#### 常见语法糖

> Java 中最常用的语法糖主要有泛型、自动拆装箱、变长参数、枚举、内部类、增强 for 循环、try-with-resources 语法、lambda 表达式等。



# 数据库



## 基础



### 数据库基础



#### 数据库, 数据库管理系统, 数据库系统, 数据库管理员?

**数据库** : 数据库(DataBase 简称 DB)就是信息的集合或者说数据库是由数据库管理系统管理的数据的集合。

**数据库管理系统** : 数据库管理系统(Database Management System 简称 DBMS)是一种操纵和管理数据库的大型软件，通常用于建立、使用和维护数据库。

**数据库系统** : 数据库系统(Data Base System，简称 DBS)通常由软件、数据库和数据管理员(DBA)组成。

**数据库管理员** : 数据库管理员(Database Administrator, 简称 DBA)负责全面管理和控制数据库系统。



#### 元组, 码, 候选码, 主码, 外码, 主属性, 非主属性

**元组**：元组（tuple）是关系数据库中的基本概念，关系是一张表，表中的每行（即数据库中的每条记录）就是一个元组，每列就是一个属性。 在二维表里，元组也称为行。

**码**：码就是能唯一标识实体的属性，对应表中的列。

**候选码**：若关系中的某一属性或属性组的值能唯一的标识一个元组，而其任何、子集都不能再标识，则称该属性组为候选码。例如：在学生实体中，“学号”是能唯一的区分学生实体的，同时又假设“姓名”、“班级”的属性组合足以区分学生实体，那么{学号}和{姓名，班级}都是候选码。

**主码** : 主码也叫主键。主码是从候选码中选出来的。 一个实体集中只能有一个主码，但可以有多个候选码。

**外码** : 外码也叫外键。如果一个关系中的一个属性是另外一个关系中的主码则这个属性为外码。

**主属性**：候选码中出现过的属性称为主属性。比如关系 工人（工号，身份证号，姓名，性别，部门）. 显然工号和身份证号都能够唯一标示这个关系，所以都是候选码。工号、身份证号这两个属性就是主属性。如果主码是一个属性组，那么属性组中的属性都是主属性。

**非主属性：** 不包含在任何一个候选码中的属性称为非主属性。比如在关系——学生（学号，姓名，年龄，性别，班级）中，主码是“学号”，那么其他的“姓名”、“年龄”、“性别”、“班级”就都可以称为非主属性。



#### ER图

使用ER图捋清数据库设计结构，**面试经常在介绍自己项目时被问到**。

ER图即实体联系图，提供了表示实体类型、属性和联系的方法。

ER 图由下面 3 个要素组成：

**实体**：通常是现实世界的业务对象，当然使用一些逻辑对象也可以。比如对于一个校园管理系统，会涉及学生、教师、课程、班级等等实体。在 ER 图中，实体使用矩形框表示。

**属性**：即某个实体拥有的属性，属性用来描述组成实体的要素，对于产品设计来说可以理解为字段。在 ER 图中，属性使用椭圆形表示。

**联系**：即实体与实体之间的关系，在 ER 图中用菱形表示，这个关系不仅有业务关联关系，还能通过数字表示实体之间的数量对照关系。例如，一个班级会有多个学生就是一种实体间的联系。

如下图为一个学生选课的ER图：

> ![学生与课程之间联系的E-R图](D:\Note\Note\面经笔记\assets\c745c87f6eda9a439e0eea52012c7f4a.png)



#### 数据库范式

> 数据库范式有 3 种：
>
> - 1NF(第一范式)：属性不可再分。
> - 2NF(第二范式)：1NF 的基础之上，消除了非主属性对于码的部分函数依赖。
> - 3NF(第三范式)：3NF 在 2NF 的基础之上，消除了非主属性对于码的传递函数依赖 。



##### 1NF(第一范式)

表中的字段不能再被分割。**1NF 是所有关系型数据库的最基本要求** ，也就是说关系型数据库中创建的表一定满足第一范式。

##### 2NF(第二范式)

2NF 在 1NF 的基础之上，消除了非主属性对于码的部分函数依赖。如下图所示，展示了第一范式到第二范式的过渡。第二范式在第一范式的基础上增加了一个列，这个列称为主键，非主属性都依赖于主键。**基本自己设计的数据库都满足第二范式**。

> ![第二范式](D:\Note\Note\面经笔记\assets\bd1d31be3779342427fc9e462bf7f05c.png)

##### 3NF(第三范式)

3NF 在 2NF 的基础之上，消除了非主属性对于码的传递函数依赖 。符合 3NF 要求的数据库设计，**基本**上解决了数据冗余过大，插入异常，修改异常，删除异常的问题。

**概念补充**：

**函数依赖（functional dependency）**：若在一张表中，在属性（或属性组）X 的值确定的情况下，必定能确定属性 Y 的值，那么就可以说 Y 函数依赖于 X，写作 X → Y。

**部分函数依赖（partial functional dependency）**：如果 X→Y，并且存在 X 的一个真子集 X0，使得 X0→Y，则称 Y 对 X 部分函数依赖。比如学生基本信息表 R 中（学号，身份证号，姓名）当然学号属性取值是唯一的，在 R 关系中，（学号，身份证号）->（姓名），（学号）->（姓名），（身份证号）->（姓名）；所以姓名部分函数依赖于（学号，身份证号）；

**完全函数依赖(Full functional dependency)**：在一个关系中，若某个非主属性数据项依赖于全部关键字称之为完全函数依赖。比如学生基本信息表 R（学号，班级，姓名）假设不同的班级学号有相同的，班级内学号不能相同，在 R 关系中，（学号，班级）->（姓名），但是（学号）->(姓名)不成立，（班级）->(姓名)不成立，所以姓名完全函数依赖与（学号，班级）；

**传递函数依赖**：在关系模式 R(U)中，设 X，Y，Z 是 U 的不同的属性子集，如果 X 确定 Y、Y 确定 Z，且有 X 不包含 Y，Y 不确定 X，（X∪Y）∩Z=空集合，则称 Z 传递函数依赖(transitive functional dependency) 于 X。传递函数依赖会导致数据冗余和异常。传递函数依赖的 Y 和 Z 子集往往同属于某一个事物，因此可将其合并放到一个表中。比如在关系 R(学号 , 姓名, 系名，系主任)中，学号 → 系名，系名 → 系主任，所以存在非主属性系主任对于学号的传递函数依赖。



#### 主键和外键的区别

**主键(主码)**：主键用于唯一标识一个元组，不能有重复，不允许为空。一个表只能有一个主键。

**外键(外码)**：外键用来和其他表建立联系用，外键是另一表的主键，外键是可以有重复的，可以是空值。一个表可以有多个外键。



#### 不推荐使用外键和级联

阿里巴巴开发手册中说明了外键和级联：

> 【强制】不得使用外键与级联，一切外键概念必须在应用层解决。
>
> 说明: 以学生和成绩的关系为例，学生表中的 student_id 是主键，那么成绩表中的 student_id 则为外键。如果更新学生表中的 student_id，同时触发成绩表中的 student_id 更新，即为级联更新。外键与级联更新适用于单机低并发，不适合分布式、高并发集群；级联更新是强阻塞，存在数据库更新风暴的风险；外键影响数据库的插入速度

不用外键的原因：

1. **增加了复杂性：** a. 每次做 DELETE 或者 UPDATE 都必须考虑外键约束，会导致开发的时候很痛苦, 测试数据极为不方便; b. 外键的主从关系是定的，假如那天需求有变化，数据库中的这个字段根本不需要和其他表有关联的话就会增加很多麻烦。
2. **增加了额外工作**：数据库需要增加维护外键的工作，比如当我们做一些涉及外键字段的增，删，更新操作之后，需要触发相关操作去检查，保证数据的的一致性和正确性，这样会不得不消耗数据库资源。如果在应用层面去维护的话，可以减小数据库压力；
3. **对分库分表不友好**：因为分库分表下外键是无法生效的。
4. ……

外键的好处：

1. 保证了数据库数据的一致性和完整性；
2. 级联操作方便，减轻了程序代码量；
3. ……

如果系统不涉及分库分表，并发量不是很高的情况还是可以考虑使用外键的。



#### 存储过程

> 可以把存储过程看成是一些 SQL 语句的集合，中间加了点逻辑控制语句。存储过程在业务比较复杂的时候是非常实用的，比如很多时候我们完成一个操作可能需要写一大串 SQL 语句，这时候我们就可以写有一个存储过程，这样也方便了我们下一次的调用。存储过程一旦调试完成通过后就能稳定运行，另外，使用存储过程比单纯 SQL 语句执行要快，因为存储过程是预编译过的。
>
> 存储过程在互联网公司应用不多，因为存储过程难以调试和扩展，而且没有移植性，还会消耗数据库资源。

阿里巴巴 Java 开发手册里要求禁止使用存储过程。

![阿里巴巴Java开发手册: 禁止存储过程](D:\Note\Note\面经笔记\assets\0fa082bc4d4f919065767476a41b2156.png)

**总结：可以理解为函数**



#### drop、delete与truncate的区别

##### 使用方法不同

> `drop`(丢弃数据): `drop table 表名` ，直接将表都删除掉，在删除表的时候使用。
>
> `truncate` (清空数据) : `truncate table 表名` ，只删除表中的数据，再插入数据的时候自增长 id 又从 1 开始，在清空表中数据的时候使用。
>
> `delete`（删除数据） : `delete from 表名 where 列名=值`，删除某一行的数据，如果不加 `where` 子句和`truncate table 表名`作用类似。

`truncate` 和不带 `where`子句的 `delete`、以及 `drop` 都会删除表内的数据，但是 **`truncate` 和 `delete` 只删除数据不删除表的结构(定义)，执行 `drop` 语句，此表的结构也会删除，也就是执行`drop` 之后对应的表不复存在**。



##### 语言类别不同

> `truncate` 和 `drop` 属于 DDL(数据定义语言)语句，操作立即生效，原数据不放到 rollback segment 中，不能回滚，操作不触发 trigger。而 `delete` 语句是 DML (数据库操作语言)语句，这个操作会放到 rollback segment 中，事务提交之后才生效。



##### 效率不同

一般来说：`drop` > `truncate` > `delete`

- `delete`命令执行的时候会产生数据库的`binlog`日志，而日志记录是需要消耗时间的，但是也有个好处方便数据回滚恢复。
- `truncate`命令执行的时候不会产生数据库日志，因此比`delete`要快。除此之外，还会把表的自增值重置和索引恢复到初始大小等。
- `drop`命令会把表占用的空间全部释放掉。

> Tips：你应该更多地关注在使用场景上，而不是执行效率。



#### 数据库的设计流程

1. **需求分析** : 分析用户的需求，包括数据、功能和性能需求。
2. **概念结构设计** : 主要采用 E-R 模型进行设计，包括画 E-R 图。
3. **逻辑结构设计** : 通过将 E-R 图转换成表，实现从 E-R 模型到关系模型的转换。
4. **物理结构设计** : 主要是为所设计的数据库选择合适的存储结构和存取路径。
5. **数据库实施** : 包括编程、测试和试运行
6. **数据库的运行和维护** : 系统的运行与数据库的日常维护。

 

### NoSQL基础

> NoSQL（Not Only SQL 的缩写）泛指非关系型的数据库，主要针对的是**键值、文档以及图形类型数据存储**。并且，NoSQL 数据库天生支持分布式，数据冗余和数据分片等特性，旨在提供可扩展的高可用高性能数据存储解决方案。
>
> 一个常见的误解是 NoSQL 数据库或非关系型数据库不能很好地存储关系型数据。NoSQL 数据库可以存储关系型数据—它们与关系型数据库的存储方式不同。
>
> NoSQL 数据库代表：HBase、Cassandra、MongoDB、Redis。



#### SQL和NoSQL的区别

|              | SQL 数据库                                                   | NoSQL 数据库                                                 |
| ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 数据存储模型 | 结构化存储，具有固定行和列的表格                             | 非结构化存储。文档：JSON 文档，键值：键值对，宽列：包含行和动态列的表，图：节点和边 |
| 发展历程     | 开发于 1970 年代，重点是减少数据重复                         | 开发于 2000 年代后期，重点是提升可扩展性，减少大规模数据的存储成本 |
| 例子         | Oracle、MySQL、Microsoft SQL Server、PostgreSQL              | 文档：MongoDB、CouchDB，键值：Redis、DynamoDB，宽列：Cassandra、 HBase，图表：Neo4j、 Amazon Neptune、Giraph |
| ACID 属性    | 提供原子性、一致性、隔离性和持久性 (ACID) 属性               | 通常不支持 ACID 事务，为了可扩展、高性能进行了权衡，少部分支持比如 MongoDB 。不过，MongoDB 对 ACID 事务 的支持和 MySQL 还是有所区别的。 |
| 性能         | 性能通常取决于磁盘子系统。要获得最佳性能，通常需要优化查询、索引和表结构。 | 性能通常由底层硬件集群大小、网络延迟以及调用应用程序来决定。 |
| 扩展         | 垂直（使用性能更强大的服务器进行扩展）、读写分离、分库分表   | 横向（增加服务器的方式横向扩展，通常是基于分片机制）         |
| 用途         | 普通企业级的项目的数据存储                                   | 用途广泛比如图数据库支持分析和遍历连接数据之间的关系、键值数据库可以处理大量数据扩展和极高的状态变化 |
| 查询语法     | 结构化查询语言 (SQL)                                         | 数据访问语法可能因数据库而异                                 |



#### NoSQL数据库的优势

NoSQL 数据库非常适合许多现代应用程序，例如移动、Web 和游戏等应用程序，它们需要灵活、可扩展、高性能和功能强大的数据库以提供卓越的用户体验。

- **灵活性：** NoSQL 数据库通常提供灵活的架构，以实现更快速、更多的迭代开发。灵活的数据模型使 NoSQL 数据库成为半结构化和非结构化数据的理想之选。
- **可扩展性：** NoSQL 数据库通常被设计为通过使用分布式硬件集群来横向扩展，而不是通过添加昂贵和强大的服务器来纵向扩展。
- **高性能：** NoSQL 数据库针对特定的数据模型和访问模式进行了优化，这与尝试使用关系数据库完成类似功能相比可实现更高的性能。
- **强大的功能：** NoSQL 数据库提供功能强大的 API 和数据类型，专门针对其各自的数据模型而构建。



#### NoSQL数据库的类型

NoSQL 数据库主要可以分为下面四种类型：

- **键值**：键值数据库是一种较简单的数据库，其中每个项目都包含键和值。这是极为灵活的 NoSQL 数据库类型，因为应用可以完全控制 value 字段中存储的内容，没有任何限制。Redis 和 DynanoDB 是两款非常流行的键值数据库。
- **文档**：文档数据库中的数据被存储在类似于 JSON（JavaScript 对象表示法）对象的文档中，非常清晰直观。每个文档包含成对的字段和值。这些值通常可以是各种类型，包括字符串、数字、布尔值、数组或对象等，并且它们的结构通常与开发者在代码中使用的对象保持一致。MongoDB 就是一款非常流行的文档数据库。
- **图形**：图形数据库旨在轻松构建和运行与高度连接的数据集一起使用的应用程序。图形数据库的典型使用案例包括社交网络、推荐引擎、欺诈检测和知识图形。Neo4j 和 Giraph 是两款非常流行的图形数据库。
- **宽列**：宽列存储数据库非常适合需要存储大量的数据。Cassandra 和 HBase 是两款非常流行的宽列存储数据库。

> ![NoSQL 数据模型](D:\Note\Note\面经笔记\assets\types-of-nosql-datastores.png)



### 字符集

MySQL 字符编码集中有两套 UTF-8 编码实现：**`utf8`** 和 **`utf8mb4`**。如果使用 **`utf8`** 的话，存储 emoji 符号和一些比较复杂的汉字、繁体字就会出错。

> 字符是各种文字和符号的统称，包括各个国家文字、标点符号、表情、数字等等。 **字符集** 就是一系列字符的集合。字符集的种类较多，每个字符集可以表示的字符范围通常不同，就比如说有些字符集是无法表示汉字的。
>
> 计算机只能存储二进制的数据，计算机将**英文、汉字、表情等字符**和二进制的数据一一对应起来，比如说字符“a”对应“01100001”，反之，“01100001”对应 “a”。我们将字符对应二进制数据的过程称为"**字符编码**"，反之，二进制数据解析成字符的过程称为“**字符解码**”。



#### 字符编码

即将字符与二进制对应的不同的规则

> 字符编码是一种将字符集中的字符与计算机中的二进制数据相互转换的方法，可以看作是一种映射规则。也就是说，字符编码的目的是为了让计算机能够存储和传输各种文字信息。
>
> 每种字符集都有自己的字符编码规则，常用的字符集编码规则有 ASCII 编码、 GB2312 编码、GBK 编码、GB18030 编码、Big5 编码、UTF-8 编码、UTF-16 编码等。



#### 常见的字符集

> 常见的字符集有：ASCII、GB2312、GB18030、GBK、Unicode……。
>
> 不同的字符集的主要区别在于：
>
> - 可以表示的字符范围
> - 编码方式



#### Unicode & UTF-8

> Unicode 字符集中包含了世界上几乎所有已知的字符。不过，Unicode 字符集并没有规定如何存储这些字符（也就是如何使用二进制数据表示这些字符）。
>
> **UTF-8** 是目前使用最广的一种字符编码。

注意UTF-8和UTF-16、32的区别在于：存储一个字符所使用几个字符编码，如UTF-8使用1-4个字符编码； UTF-16 使用 2 或 4 个字节为每个字符编码；UTF-32 固定位 4 个字节为每个字符编码。

> UTF-8 可以根据不同的符号自动选择编码的长短，像英文字符只需要 1 个字节就够了，这一点 ASCII 字符集一样 。因此，对于英语字符，UTF-8 编码和 ASCII 码是相同的。



#### MySQL字符集

> MySQL 支持很多种字符集的方式，比如 GB2312、GBK、BIG5、多种 Unicode 字符集（UTF-8 编码、UTF-16 编码、UCS-2 编码、UTF-32 编码等等）。

使用`SHOW CHARSET`命令查看MySQL支持的字符集。



#### 默认字符集

在 MySQL5.7 中，默认字符集是 `latin1` ；在 MySQL8.0 中，默认字符集是 `utf8mb4`



#### 默认字符集

> MySQL 中的字符集有以下的层次级别：
>
> - `server`（MySQL 实例级别）
> - `database`（库级别）
> - `table`（表级别）
> - `column`（字段级别）
>
> 它们的优先级可以简单的认为是从上往下依次增大，也即 `column` 的优先级会大于 `table` 等其余层次的。如指定 MySQL 实例级别字符集是`utf8mb4`，指定某个表字符集是`latin1`，那么这个表的所有字段如果不指定的话，编码就是`latin1`。



#### JDBC对链接字符集的影响

> 不知道你们有没有碰到过存储 emoji 表情正常，但是使用类似 Navicat 之类的软件的进行查询的时候，发现 emoji 表情变成了问号的情况。这个问题很有可能就是 JDBC 驱动引起的。
>
> 根据前面的内容，我们知道连接字符集也是会影响我们存储的数据的，而 JDBC 驱动会影响连接字符集。
>
> `mysql-connector-java` （JDBC 驱动）主要通过这几个属性影响连接字符集：
>
> - `characterEncoding`
> - `characterSetResults`
>
> 以 `DataGrip 2023.1.2` 来说，在它配置数据源的高级对话框中，可以看到 `characterSetResults` 的默认值是 `utf8` ，在使用 `mysql-connector-java 8.0.25` 时，连接字符集最后会被设置成 `utf8mb3` 。那么这种情况下 emoji 表情就会被显示为问号，并且当前版本驱动还不支持把 `characterSetResults` 设置为 `utf8mb4` ，不过换成 `mysql-connector-java driver 8.0.29` 却是允许的。



### SQL

#### 概念

数据库术语

`数据库（database）` - 保存有组织的数据的容器（通常是一个文件或一组文件）。

`数据表（table）` - 某种特定类型数据的结构化清单。

`模式（schema）` - 关于数据库和表的布局及特性的信息。模式定义了数据在表中如何存储，包含存储什么样的数据，数据如何分解，各部分信息如何命名等信息。数据库和表都有模式。

`列（column）` - 表中的一个字段。所有表都是由一个或多个列组成的。

`行（row）` - 表中的一个记录。

`主键（primary key）` - 一列（或一组列），其值能够唯一标识表中每一行。



SQL语法

SQL 语法结构包括：

- **`子句`** - 是语句和查询的组成成分。（在某些情况下，这些都是可选的。）
- **`表达式`** - 可以产生任何标量值，或由列和行的数据库表
- **`谓词`** - 给需要评估的 SQL 三值逻辑（3VL）（true/false/unknown）或布尔真值指定条件，并限制语句和查询的效果，或改变程序流程。
- **`查询`** - 基于特定条件检索数据。这是 SQL 的一个重要组成部分。
- **`语句`** - 可以持久地影响纲要和数据，也可以控制数据库事务、程序流程、连接、会话或诊断。

SQL语法要点：

> **SQL 语句不区分大小写**，但是数据库表名、列名和值是否区分，依赖于具体的 DBMS 以及配置。例如：`SELECT` 与 `select`、`Select` 是相同的。
>
> **多条 SQL 语句必须以分号（`;`）分隔**。
>
> 处理 SQL 语句时，**所有空格都被忽略**。

SQL 支持三种注释：

```sql
## 注释1
-- 注释2
/* 注释3 */
```



SQL语句分类

**数据定义语言（DDL）**

> 数据定义语言（Data Definition Language，DDL）是 SQL 语言集中负责数据结构定义与数据库对象定义的语言。
>
> DDL 的主要功能是**定义数据库对象**。
>
> DDL 的核心指令是 `CREATE`、`ALTER`、`DROP`。



**数据操纵语言（DML）**

> 数据操纵语言（Data Manipulation Language, DML）是用于数据库操作，对数据库其中的对象和数据运行访问工作的编程语句。
>
> DML 的主要功能是 **访问数据**，因此其语法都是以**读写数据库**为主。
>
> DML 的核心指令是 `INSERT`、`UPDATE`、`DELETE`、`SELECT`。这四个指令合称 CRUD(Create, Read, Update, Delete)，即增删改查。



**事务控制语言（TCL）**

> 事务控制语言 (Transaction Control Language, TCL) 用于**管理数据库中的事务**。这些用于管理由 DML 语句所做的更改。它还允许将语句分组为逻辑事务。
>
> TCL 的核心指令是 `COMMIT`、`ROLLBACK`。



**数据控制语言（DCL）**

> 数据控制语言 (Data Control Language, DCL) 是一种可对数据访问权进行控制的指令，它可以控制特定用户账户对数据表、查看表、预存程序、用户自定义函数等数据库对象的控制权。
>
> DCL 的核心指令是 `GRANT`、`REVOKE`。
>
> DCL 以**控制用户的访问权限**为主，因此其指令作法并不复杂，可利用 DCL 控制的权限有：`CONNECT`、`SELECT`、`INSERT`、`UPDATE`、`DELETE`、`EXECUTE`、`USAGE`、`REFERENCES`。
>
> 根据不同的 DBMS 以及不同的安全性实体，其支持的权限控制也有所不同。



#### 增删查改

即CRUD。数据库基本操作中的基本操作。



##### 查询数据

`SELECT` 语句用于从数据库中查询数据。

`DISTINCT` 用于返回唯一不同的值。它作用于所有列，也就是说所有列的值都相同才算相同。

`LIMIT` 限制返回的行数。可以有两个参数，第一个参数为起始行，从 0 开始；第二个参数为返回的总行数。

- `ASC`：升序（默认）
- `DESC`：降序

限制查询结果：

```sql
-- 返回前 5 行
SELECT * FROM mytable LIMIT 5;
SELECT * FROM mytable LIMIT 0, 5;
-- 返回第 3 ~ 5 行
SELECT * FROM mytable LIMIT 2, 3;
```



##### 排序

> `order by` 用于对结果集按照一个列或者多个列进行排序。默认按照升序对记录进行排序，如果需要按照降序对记录进行排序，可以使用 `desc` 关键字。
>
> `order by` 对多列排序的时候，先排序的列放前面，后排序的列放后面。并且，不同的列可以有不同的排序规则。

```SQL
SELECT * FROM products
ORDER BY prod_price DESC, prod_name ASC;
```



##### 分组

> **`group by`**：
>
> - `group by` 子句将记录分组到汇总行中。
> - `group by` 为每个组返回一个记录。
> - `group by` 通常还涉及聚合`count`，`max`，`sum`，`avg` 等。
> - `group by` 可以按一列或多列进行分组。
> - `group by` 按分组字段进行排序后，`order by` 可以以汇总字段来进行排序。



