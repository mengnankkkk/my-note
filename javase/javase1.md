---
title: javase知识总结
categories:
  - javase
  - java
  
abbrlink: 2701
date: 2025.5.19
tags: 
   - java

---

# 基础语法

## 数据类型

java中有8种基本类型：

byte, short, int, long, float, double, boolean, char

还有引用类型

类、数组、接口等。

比如**String**就是引用类型。引用类型的默认是 `null`

| 类型    | 位数 | 默认值   | 包装类    |
| ------- | ---- | -------- | --------- |
| byte    | 8    | 0        | Byte      |
| short   | 16   | 0        | Short     |
| int     | 32   | 0        | Integer   |
| long    | 64   | 0L       | Long      |
| float   | 32   | 0.0f     | Float     |
| double  | 64   | 0.0d     | Double    |
| char    | 16   | '\u0000' | Character |
| boolean | 1    | false    | Boolean   |

浮点数的默认类型是double 赋给 `float` 需加 `f` 后缀。精度变小

**byte是循环的，满了128就从-128开始**

double和long都是默认占64位的存储空间

long用于存储整数值，范围为-2^63^到 2^63^-1，double用于存储浮点数，遵循IEEE 754标准。

基本数据类型和引用类型的主要区别在于:

1. 基本类型变量存储的是实际的数据值

2. 引用类型变量存储的是对象的引用(内存地址)

3. 基本类型在栈中分配内存,引用类型在堆中分配内存

   

   只有 **Byte、Short、Integer、Long** 实现了缓存机制，且默认缓存 -128 到 127 范围内的值。缓存机制可以提升性能，减少不必要的对象创建。

   

### String

| 类名          | 可变性 | 线程安全 | 性能     |
| ------------- | ------ | -------- | -------- |
| String        | 不可变 | 安全     | 最低     |
| StringBuilder | 可变   | 不安全   | **最快** |
| StringBuffer  | 可变   | 安全     | 中等     |

String类型只要是字符串一样，**==与equals都一样**，因为都在**字符串常量池**的一个位置里。会调用原先有的 String是需要初始化的。但是如果是new了一个的话就不一样了

单纯的字符串是一样的。他们是线程共享的。然后String是final，是不可以修改的。

如果连接字符串的时候，**遇见数字，直接连接**，没有遇到就按数学计算

StringBuilder是非线程安全的,不需要维护线程同步,所以运行速度最快。他**是单线程操作的**，适用于单线程环境中需要大量修改字符串的场景，如高频拼接操作。

StringBuffer运行速度确实比String快。**因为String的不可变性**,每次操作都会产生新对象,而**StringBuffer是可变的,在原对象上直接修改。**但是他们都是final修饰的

StringBuffer的所有公共方法都是**synchronized**修饰的,是线程安全的,适合在多线程环境下使用。

可以直接修改字符串

在 Java 8 及以后，编译器会对字符串的常量拼接做优化，**将字符串拼接转换为 StringBuilder** 操作，这种优化提高了代码性能。

\- String适用于**少量的字符串操作的情况**
\- StringBuilder适用**于单线程**下在字符缓冲区进行大量操作的情况
\- StringBuffer适用于**多线程下**在字符缓冲区进行大量操作的情况

这三者性能从高到低排序为:StringBuilder > StringBuffer > String

但是**intern**方法会在首先在常量池中寻找，有的话就直接用



### 运算法则

boolean b = true ? false : true==true ? false : true

首先要明确**三元运算符**的结合性是从**右到左**的，但此处有一个条件表达式true在最前面

他直接是true了，所以直接返回false

**复合赋值运算符/=的优先级低于++运算符**

double类型除以int类型，结果会自动转换为double类型

在进行除法运算的话，**如果都是整形，那么会向下取整，舍去小数部分**

>> 是算术右移运算符，它使所有的位向右移动，但保持符号位不变。对于负数，左边会自动补1，正数则补0。

\>>> 是逻辑右移运算符（也称无符号右移），它使所有的位向右移动，并且左边总是补0，不管原来的数是正数还是负数。

他俩都是相当于/2，但是正负数的时候不同。由二进制可看出来

其中有<<=

没有<<<=

==是比较在内存中的位置，equals是比较value的大小

在java中小于int精度的计算都会自动转为int进行计算



## 类

### final

final**不能修饰接口和抽象类**。final表示"最终的"含义,而接口和抽象类本身就是需要被实现或继承的,与final的语义相矛盾。final只能修饰**具体的类、方法和变量。**

final修饰的方法不能被重写(Override),但是**可以被重载**(Overload)。重写是子类对父类方法的覆盖,而重载是同一个类中方法名相同但参数不同。

final修饰的变量是常量,一旦被赋值就不能再次修改。对于**基本类型,是值不能改变;对于引用类型,是引用不能改变(但对象的内容可以改变)。**

### static

`static` 表示“静态”的含义，用于修饰类的成员（变量、方法、代码块、内部类）。被 `static` 修饰的成员**属于类本身**，而不是某个对象。

因此，**无需创建对象即可访问静态成员**，推荐通过“类名.成员”的方式调用。

**static 不能修饰构造方法和局部变量**，因为构造方法是用来创建对象的，局部变量只在方法执行时存在，二者与类无关。

### this

`this` 表示**当前对象的引用**，常用于区分成员变量与局部变量、在构造方法中调用其他构造方法、将当前对象作为参数传递等。

`this()` 调用构造器必须放在构造方法的第一行。

this和super不能同时使用在一个构造函数里

### super

`super` 表示**父类对象的引用**，可用于访问父类的成员变量、方法、构造方法。

因为在子类构造方法中调用父类构造方法**super()**必须位于**第一行**,这是Java语言规范的要求。这样设计的原因是为了确保在初始化子类之前,父类已经完成初始化。

**不能在static环境**(包括static方法和static代码块)中使用。因为static成员属于类,而不是实例,而this和super都是和实例相关的概念。

super不仅可以在**子类构造方法**中使用,还可以在**子类的实例方法**中使用,**用于调用父类被覆盖的方法或访问父类的属性。**

### volatile

`volatile` 是 **轻量级的同步机制**，用于修饰**成员变量**，确保多线程间变量的可见性。

**不保证原子性**，如 `count++` 仍需配合 `synchronized` 或 `AtomicInteger`。

不适用于复合操作（读-改-写）。

**保证变量的可见性**：一个线程修改了值，其他线程能立即看到。

**禁止指令重排序**：用于保证一定的执行顺序，适合标志类变量。

volatile的实现确实遵循**happens-before**原则。happens-before原则是Java内存模型中的重要概念，它保证了volatile写操作一定happens-before于后续对这个volatile变量的读操作。

**volatile关键字是Java中用于保证变量可见性和有序性的重要机制。**

volatile变量在每次被线程访问时，**都强制从主内存中重新读取最新值，而不是使用线程工作内存中的值。这确保了变量的可见性。**

volatile关键字通过**内存屏障(Memory Barrier)来阻止指令重排序**。它能确保volatile变量读写操作的顺序性，防止编译器和处理器对这些操作进行重排序优化。

volatile不能保证线程安全。它只能保证变量的可见性和禁止指令重排序，但不能保证原子性。

volatile只能保证**可见性和有序性**,**无法保证互斥性和原子性**。例如count++这样的操作,volatile无法保证其原子性,因为这个操作实际包含读取、递增、写入三个步骤。所以不能包装线程安全

volatile只能用于**修饰变量**,不能修饰方法和类。作用是告诉编译器和虚拟机，**该变量可能会被多个线程同时访问**，因此不应该进行编译器优化或缓存

### 接口&抽象类

#### 抽象类

抽象类是一种特殊的类,其最**显著的特征就是不能被实例化。抽象类主要用于被其他类继承**,为子类提供通用的属性和方法实现,同时也可以**声明抽象方法要求子类必须实现。**只有**当子类是非抽象类时才必须实现所有抽象方法**，如果子类也是抽象类则可以不实现父类的抽象方法。

抽象类中可以包含普通方法,也可以包含抽象方法,并不要求所有方法都是抽象方法。抽象类中的普通方法可以有具体的实现代码。

一个类可以被声明为抽象类，**即使它不包含任何抽象方法。这种设计可以用来阻止类的实例化**

抽象类可以有构造方法。尽管抽象类不能被实例化,但其构造方法可以被子类通过super()调用,用于初始化从抽象类继承的属性。

抽象类的主要作用是作为**基类**使用,通过**继承和多态机制**实现代码的复用和扩展。它既可以包含抽象方法强制子类实现,又可以提供通用方法的具体实现,是面向对象编程中重要的设计工具。

#### 接口

接口是一种特殊的抽象类型,其中定义的方法默认都是public abstract的。在Java接口中,方法的修饰符具有严格的限制。

默认访问权限是default（包访问权限）

接口内的变量都具有默认的修饰符，**等价于public static final。**

❌ 不能定义构造方法，就是方法他也不能实现，不能使用大括号 可以使用static default private方法(jdk9)

✅ 只能是 `public static final` 常量

接口中的方法默认就是public和abstract的。这是Java接口的特性，即使不显式声明这些修饰符，编译器也会自动添加。这样可以确保接口方法的可访问性和抽象性。

| 特性                     | 接口（interface）                                            | 抽象类（abstract class）                                     |
| ------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **是否可以实例化**       | 否                                                           | 否                                                           |
| **是否支持构造方法**     | ❌ 不能定义构造方法                                           | ✅ 可以有构造方法（用于子类构造时调用）                       |
| **是否可以包含成员变量** | ✅ 只能是 `public static final` 常量                          | ✅ 可以有普通字段、静态字段、常量等                           |
| **是否可以包含方法实现** | ✅ 从 Java 8 开始：— `default` 默认方法— `static` 静态方法— Java 9 起允许 `private` 方法 | ✅ 可以包含抽象方法、非抽象方法（即有方法体的）               |
| **继承机制**             | 只能继承接口（多继承）隐式继承 `Object` 的方法签名（如 `toString()`），但不继承其实现 | 只能继承一个抽象类（单继承）显式或隐式继承 `Object`          |
| **访问修饰符限制**       | 方法默认是 `public`，不能是 `protected`、`private`（Java 9 起可以定义 `private` 方法） | 方法、字段可以用 `private`、`protected`、`public`            |
| **关键字约束**           | 默认方法用 `default` 修饰；所有变量自动是 `public static final` | 抽象方法必须使用 `abstract` 修饰；类必须用 `abstract` 修饰   |
| **实现/继承方式**        | 使用 `implements` 实现一个或多个接口                         | 使用 `extends` 继承抽象类（只能单继承）                      |
| **适合场景**             | 关注“规范”，用于定义行为接口，强调能力的实现（like 能说话、能飞） | 关注“共性”，用于抽象出共享代码或状态（like 动物有名字、吃饭等） |

### 匿名对象类

匿名内部类可以**继承一个类或实现一个接口**，并且可以重写父类的方法
方法重写时，**子类方法返回值类型、方法名和参数必须与父类相同**
通过对象调用方法时，优先**调用对象实际类型中的方法**（动态绑定）

## 对象

对象的四种创建方式

这四种方式各有特点和适用场景:
\- new操作符适用于普通对象创建
\- 反射方式适用于动态加载场景
\- clone方式适用于对象复制场景
\- 反序列化方式适用于数据传输场景

实例化对象的顺序：

父类Ｂ**静态**代码块->子类Ａ静态代码块->父类Ｂ非静态代码块->父类Ｂ构造函数->子类Ａ非静态代码块->子类Ａ构造函数

- **静态代码块**只执行一次，随类加载而执行；
- **实例初始化块**每次 new 时执行，优先于构造方法；
- 构造方法最后执行，用于初始化对象本身。



函数调用的两种主要参数传递方式:**传值调用**(call by value)和**引用调用**(call by reference)的特点。

传值调用保护了**实际参数不被修改**,而引用调用则允许通过引用修改实际参数的内容,**但不能改变引用本身指向的地址**。

### 反射

**反射机制允许在运行时加载类、访问属性、调用方法、构造对象**。

- 运行时动态操作对象
- 解耦合，提高灵活性

使用场景：

- **判断对象所属类**：`obj.getClass()` 获取对象的 Class 对象。
- **加载类**：`Class.forName("全类名")` 动态加载类。
- **获取构造器并实例化对象**：`clazz.getConstructor(...)` + `constructor.newInstance(...)`
- **访问类的成员变量和方法**：`clazz.getDeclaredFields()`, `clazz.getDeclaredMethods()`
- **调用对象方法**：`method.invoke(obj, args...)`
- **访问私有成员**：通过 `setAccessible(true)` 取消访问检查。+

反射机制**实际上会降低程序的性能**,因为它需要在运行时进行类型检查和解析。同时,反射也可能破坏封装性,带来安全风险,因为它可以访问私有成员。

反射机制不仅可以访问public成员,通过s**etAccessible(true)方法**,它还可以访问private、protected等其他访问控制级别的成员。这也是反射强大但需要谨慎使用的原因之一。违反了封装性

JAVA反射机制主要提供以下功能：

在运行时判断一个对象所属的类

在运行时构造一个类的对象

在运行时判断一个类所具有的成员变量和方法

在运行时调用一个对象的方法

| 优点                                             | 缺点                                             |
| ------------------------------------------------ | ------------------------------------------------ |
| **提高程序灵活性和扩展性**，支持动态加载类和调用 | **性能较慢**，需要绕过 JVM 优化，存在额外开销    |
| 支持框架、工具实现通用逻辑和动态操作             | **破坏封装性**，可访问私有成员，可能导致安全风险 |
| 支持动态代理、注解解析等高级功能                 | **编译时缺乏类型检查**，错误只能运行时发现       |

反射性能优化建议：

- **避免频繁调用**反射操作，尤其是在热点代码中。
- **缓存反射的 Class、Method、Field 对象**，减少重复解析。
- 使用 Java 8+ 的 `MethodHandle` 或 `LambdaMetafactory` 替代传统反射调用，提升性能。
- 只在初始化或配置阶段使用反射，业务逻辑中尽量避免。

### 序列化

序列化是将**Java对象的状态转换成字节流**的过程，方便对象的存储（如写入文件）或网络传输。反序列化是从字节流重构Java对象的过程。

对象所属的类必须实现`java.io.Serializable`接口。

- **Serializable是一个标记接口**，不包含任何方法，只表示该类的对象可以被序列化。

**静态变量和transient修饰的成员变量不会被序列化。**

与 JSON/BSON 序列化不同，Java 序列化格式是 **二进制不可读**

transient关键字：

用于声明不需要被序列化的字段。

序列化时，这些字段的值不会被保存。

反序列化后，transient字段被赋予该类型的默认值：

- 基本类型：0, false等
- 引用类型：null

正确的序列化操作步骤：

1. 类实现`Serializable`接口。
2. 创建`ObjectOutputStream`，并包装一个字节输出流（如`FileOutputStream`）。
3. 调用`writeObject()`方法，将对象写出。
4. 反序列化时使用`ObjectInputStream`调用`readObject()`。

```java
// 序列化
FileOutputStream fos = new FileOutputStream("object.dat");
ObjectOutputStream oos = new ObjectOutputStream(fos);
oos.writeObject(obj);
oos.close();

// 反序列化
FileInputStream fis = new FileInputStream("object.dat");
ObjectInputStream ois = new ObjectInputStream(fis);
Object obj = ois.readObject();
ois.close();

```

| 选项 | 说明                                                | 正确与否 |
| ---- | --------------------------------------------------- | -------- |
| A    | FileOutputStream只能写字节数据，不能直接序列化对象  | 错误     |
| B    | PrintWriter写字符文本，不能进行对象序列化           | 错误     |
| C    | transient修饰的变量不会被序列化，反序列化后为默认值 | 正确     |
| D    | 类必须实现Serializable接口才能被序列化              | 正确     |

FileOutputStream是字节流,它只能处理原始字节数据的写入,不能直接序列化对象。要序列化对象,需要使用**ObjectOutputStream**包装FileOutputStream。

PrintWriter是处理字符数据的输出流,主要用于写入文本数据,不能直接进行对象序列化。要序列化对象必须使用**ObjectOutputStream**。

### 深拷贝&浅拷贝

| 对比项   | 浅拷贝（Shallow Copy）           | 深拷贝（Deep Copy）             |
| -------- | -------------------------------- | ------------------------------- |
| 拷贝级别 | 拷贝对象本身+**引用地址**        | 拷贝对象本身+**引用对象的内容** |
| 引用对象 | 原对象与副本**共享引用对象**     | 副本拥有独立的引用对象          |
| 默认实现 | Object.clone() 默认是浅拷贝      | 需手动实现递归复制              |
| 影响     | 改变副本的引用成员，会影响原对象 | 改变副本不影响原对象            |
| 应用场景 | 轻量对象复制，性能优先           | 要求对象完全独立、互不干扰      |

```
class Person implements Cloneable {
    String name;
    Address address;

    public Person clone() throws CloneNotSupportedException {
        // 浅拷贝
        return (Person) super.clone();
    }

    public Person deepClone() throws CloneNotSupportedException {
        Person p = (Person) super.clone();
        p.address = new Address(this.address.city); // 手动复制引用类型
        return p;
    }
}

```

## 泛型

**泛型是一种参数化类型机制**，允许类、接口、方法在定义时使用类型参数。

目的是让代码更加**类型安全**、**可重用**，避免频繁的类型转换。

好处：

**编译期类型检查**，提升代码安全性，防止运行时类型转换异常（ClassCastException）。编译时会自动装箱拆箱来配合基本类型的包装类。

**避免强制类型转换**，代码更简洁清晰。

**提高代码复用性**，同一份代码可以处理不同类型的数据。

不能用于：**基本数据类型（如int、double等）**，只能使用对应的包装类（Integer、Double等）。

泛型可以用于数组的引用类型声明，但**不能直接创建泛型数组实例**（如`new T[]`不合法）。

类型擦除机制：

Java 泛型采用**类型擦除**实现，泛型信息在编译后被擦除，泛型变量变为原始类型（通常是Object或边界类型）。

类型擦除意味着泛型的类型信息**在运行时不可用**，这限制了泛型在某些场景的应用（如无法创建泛型数组、无法进行类型判断等）。

不能直接创建泛型数组的原因：

泛型类型在运行时被擦除为原始类型，导致**创建参数化类型的数组会产生类型安全隐患**。

因此只能创建原始类型数组，或者创建Object数组再强制转换（不安全）。

但是，可以声明泛型数组的引用变量，如`List<String>[] array`是允许的。

| 特性       | 说明                                                 |
| ---------- | ---------------------------------------------------- |
| 类型参数   | 泛型只能是引用类型，不支持基本数据类型               |
| 类型擦除   | 编译后泛型类型被擦除为原始类型，运行时泛型信息不可用 |
| 适用范围   | 类、接口、方法均支持泛型                             |
| 泛型数组   | 不能创建泛型数组实例，但可以声明泛型数组类型的变量   |
| 装箱与拆箱 | 基本类型通过包装类实现与泛型兼容                     |
| 类型限制   | 支持上限（extends）、下限（super）                   |
| 编译期检查 | 泛型提供静态类型安全保障，避免运行时类型错误         |

## 异常

Java中所有异常和错误的基类是java.lang.Throwable。其中：

\- Error和Exception都继承自Throwable
\- RuntimeException是Exception的子类

java.lang.Throwable
 ├── Error （系统错误，通常不捕获）
 └── Exception
      ├── RuntimeException（非受检异常，不强制捕获）
      │    ├─ NullPointerException
      │    ├─ ClassCastException
      │    ├─ ArrayIndexOutOfBoundsException
      │    └─ ArithmeticException
      └── 受检异常（必须捕获或声明）
           ├─ IOException
           │    └─ FileNotFoundException
           └─ NoSuchMethodException

error:

代表JVM级别的错误或资源耗尽，如：

- `StackOverflowError`（栈溢出）
- `OutOfMemoryError`（内存溢出）

通常是严重错误，程序不应捕获，无法恢复。

 Exception:

受检异常（Checked Exception）:

继承自`Exception`，但**不继承**`RuntimeException`。

必须被显式处理：用`try-catch`捕获，或在方法签名中用`throws`声明。

通常表示**程序外部问题**，如文件I/O、网络错误等。

典型例子：

- `IOException`
- `FileNotFoundException`（`IOException`子类）
- `NoSuchMethodException`

编译器会检查这些异常的处理，确保程序健壮。

非受检异常（Unchecked Exception）:

- 继承自`RuntimeException`。

- **不强制处理**，可选择捕获。

- 代表程序错误，如逻辑漏洞、编程错误。

- 常见异常：

  - `NullPointerException`：空指针访问
  - `ArrayIndexOutOfBoundsException`：数组越界
  - `ClassCastException`：类型转换异常
  - `ArithmeticException`：算术异常（如除0）

  

| 异常类型   | 是否必须处理   | 典型示例                                             | 处理方式                             |
| ---------- | -------------- | ---------------------------------------------------- | ------------------------------------ |
| Error      | 不需要且不推荐 | StackOverflowError, OutOfMemoryError                 | 一般不捕获                           |
| 受检异常   | 必须处理       | IOException, FileNotFoundException                   | try-catch或throws声明                |
| 运行时异常 | 不强制必须处理 | NullPointerException, ArrayIndexOutOfBoundsException | 可捕获也可忽略，应该通过代码逻辑避免 |

**异常处理顺序原则:**

"先具体后笼统"

多重catch语句应遵循**"先具体异常后父类异常"**原则。

例如，先捕获`FileNotFoundException`，再捕获`IOException`，最后捕获`Exception`。

否则，子类异常永远不会被捕获，造成编译错误。

| 误区描述                            | 正确说明                                                     |
| ----------------------------------- | ------------------------------------------------------------ |
| `Error`可以被捕获且建议处理         | 虽然可以捕获，但通常不建议捕获，错误多为不可恢复的严重问题。 |
| 运行时异常必须用try-catch捕获       | 不必须，运行时异常不强制捕获，建议通过代码逻辑避免。         |
| 所有异常都继承自Exception           | 不对，Error和Exception都继承自Throwable。                    |
| `FileNotFoundException`不是受检异常 | 错误，它是IOException子类，是受检异常。                      |
| 多个catch的顺序无关紧要             | 错误，必须先捕获子类异常，再捕获父类异常。                   |

# 集合

集合体系分为三大部分：

- **Collection 接口族**：List、Set、Queue
- **Map 接口族**：HashMap、TreeMap、ConcurrentHashMap
- **工具类**：Collections、Arrays、Objects

## Collection

### list

有序、可重复，支持按索引访问。





**ArrayList**

- **底层结构**：动态数组（Object[]）
- **是否线程安全**：否（使用 `Collections.synchronizedList` 或 `CopyOnWriteArrayList`）
- **访问快，插入删除慢**
- **默认初始容量**：10，扩容为原容量的 **1.5 倍**

**为什么 ArrayList 默认扩容机制是扩容为原数组的 1.5 倍？**

- ArrayList 的扩容机制是：

  ```java
  newCapacity = oldCapacity + (oldCapacity >> 1); // 即1.5倍
  ```

- **兼顾性能与内存浪费的平衡**：

  - 小扩容频繁迁移，效率低
  - 大扩容浪费内存
  - **1.5 倍是经验权衡结果**，比 Hashtable 的 2 倍更节省空间

- **适用场景**：读多写少、频繁随机访问

**写时复制机制（Copy-On-Write）**：

- 每次写操作（如 add、remove）会：
  - 复制当前数组
  - 在新数组上修改
  - 替换原数组引用

优点：

- 读操作无需加锁，**读写分离，读性能高**

缺点：

- 写操作开销大，不适合写多读少场景







**LinkedList**

基于 **双向链表** 实现。在中间插入或删除元素只需要改变相邻节点的引用,**操作开销是固定的。**

插入和删除操作效率高，**适用于频繁插入/删除的场景**。要访问任意位置的元素**必须从头节点或尾节点遍历**,不能像数组那样直接通过索引访问,因此不支持高效的随机访问。

**支持 null 和重复元素**。

插入顺序即遍历顺序。

**访问元素性能不如 ArrayList**（需要从头/尾遍历）。

实现了 `Deque` 接口，可作为队列或栈使用。

**不是线程安全的**。







**Vector**

基于 **数组实现的动态数组**。

**线程安全**，所有方法都用 `synchronized` 修饰。

线程安全导致性能较低，**不推荐在新项目中使用**，推荐使用 `ArrayList + 显式同步`。

**允许 null 和重复元素**。

**是stack的父类**

### Set







**hashset**

元素唯一，不允许重复。

ashSet是基于HashMap实现的无序集合,不保证元素的顺序

**不允许重复元素**（根据 `equals()` 和 `hashCode()` 判断）。

**不保证元素顺序**。

**允许 null 元素**，最多一个。

**线程不安全**。

HashSet作为Java集合框架中的一个重要实现类,通过**hashCode()和equals()**这两个方法的组合来确保元素的唯一性。这是因为HashSet**内部实际使用HashMap来存储**数据,其中**元素的hashCode值用于确定存储位置,而equals方法则用于处理hash冲突时的比较。**

具体工作流程是:
\1. 当添加元素时,先调用hashCode()方法计算元素的哈希值
\2. 根据哈希值确定元素在HashSet中的存储位置
\3. 如果发生hash冲突,则调用equals()方法判断元素是否真正相等





**linkedHashSet**

LinkedHashSet在**HashSet的基础上增加了一个双向链表**来维护元素的插入顺序,因此是有序的。

**有序集合**，迭代顺序为插入顺序。

插入、删除、查找操作时间复杂度仍为 **O(1)**。



**Treeset**

TreeMap基于**红黑树**实现,可以保证键的自然顺序或指定顺序

保证**键的有序性**：

- 默认按键的 **自然顺序（Comparable）** 排序。
- 或使用构造函数传入的 **Comparator** 自定义排序。

**键必须实现 Comparable 接口或提供 Comparator**。

查询、插入、删除操作时间复杂度为 **O(log n)**。

**不允许 null 键**（会抛 `NullPointerException`），但允许 null 值。

**线程不安全**。



## Map 

键值对存储，key 唯一，value 可重复。



**Hashmap**

**底层结构**：数组 + 链表（JDK 1.7）或数组 + 链表/红黑树（JDK 1.8+）

**默认初始容量**：16，负载因子 0.75，扩容为原容量的 2 倍

**线程不安全**

**允许一个 null 键，多个 null 值**

**hash 冲突处理**：链地址法，链表超过 8 转红黑树

HashMap的主要特点：
\1. **允许null键和null值**
\2. **不保证元素的顺序**
\3. 非线程安全
\4. 查找效率高,时间复杂度接近O(1)
\5. **键必须是唯一的,而值可以重复**

HashMap中解决哈希冲突采用的是**链地址法(拉链法)**,而不是开放地址法

在HashMap的实现中,当**多个key的哈希值映射到数组的同一个位置时**,HashMap会在**该位置构建一个链表**(JDK1.8之后在链表长度超过8时会转换为红黑树)**来存储所有映射到该位置的Entry**。这种方式就是链地址法。

HashMap的底层确实使用**Entry数组(在JDK1.8中改名为Node但本质相同)**存储键值对。每个Entry包含key、value、next指针等信息。

**HashMap 的添加元素流程**

1. 计算 key 的 hash 值，定位数组索引
2. 若该索引为空，直接插入节点
3. 若存在冲突（哈希碰撞）：
   - 使用链表或红黑树进行存储
4. 添加元素后，判断是否超过阈值（`容量 × 负载因子`）：
   - 若超过，**触发扩容**

 **HashMap 扩容加载因子为什么是 0.75？**

**0.75 是经验值**，在**时间效率（查找速度）和空间利用率之间取得平衡**。

太低会浪费内存，太高会增加哈希碰撞。

 **HashMap 扩容为什么扩容为数组长度的 2 倍？**

旧长度为 `n`，新长度为 `2n`：

- 原 hash 值与新容量 `&` 计算时，**元素位置要么保持不变，要么移动到 `index + n`**。

这样可以**避免重新计算 hash，提高扩容效率**

hashmap并不是线程安全的，

多线程环境下使用：

- `ConcurrentHashMap`
- `Collections.synchronizedMap`
- 自行加锁

 **ConcurrentHashMap 的实现原理**

DK 1.7：

- **Segment 分段锁机制**（ReentrantLock）

JDK 1.8：

- **CAS + synchronized 实现并发控制**
- 使用 **链表 + 红黑树** 解决冲突
- 核心结构：
  - Node[] 数组 + 每个桶内链表或红黑树
  - 高并发下比 Hashtable 更优

当在遍历**HashMap**的同时对其进行结构性修改(如删除元素)时,会抛出**ConcurrentModificationException**异常。代码会运行错误

比如：



```java
public` `static` `void` `main(String[] args) {
  ``Map<Integer, String> map = ``new` `HashMap<>();
  ``map.put(``1``, ``"A"``);
  ``map.put(``2``, ``"B"``);
  ``map.put(``3``, ``"C"``);
  ``map.forEach((key, value) -> {
    ``if` `(key == ``2``) {
      ``map.remove(key);
    ``}
  ``});
  ``System.out.println(map.size());
}
```

会抛出异常，运行错误

正确做法是：

正确做法之一是使用 `Iterator` 遍历并使用其 `remove()` 方法：

```java
Iterator<Map.Entry<Integer, String>> iterator = map.entrySet().iterator();
while (iterator.hasNext()) {
    Map.Entry<Integer, String> entry = iterator.next();
    if (entry.getKey() == 2) {
        iterator.remove(); // 安全删除
    }
}

```

java.util.HashMap类是Java集合框架中实现键值对存储的主要类,它实现了Map接口,允许我们使用键值对(key-value pair)的形式来存储数据。HashMap使用**哈希表的数据结构**,每个元素都包含一个键和与之对应的值。







**LinkedHashMap**

**HashMap + 双向链表**

**维护插入顺序或访问顺序**

**常用于实现 LRU 缓存**





**TreeMap**

**底层结构**：红黑树

**键排序（自然或自定义）**

**不允许 null 键，允许 null 值**

**线程不安全**



**Hashtable（过时）**

老版本的 Map 实现，**线程安全**，所有方法都被 `synchronized` 修饰。

不允许 **null 键或 null 值**。

**不保证顺序**。

已被 `ConcurrentHashMap` 替代，在现代项目中已很少使用。



**ConcurrentHashMap**

**线程安全，替代 Hashtable**

JDK 1.7：Segment 分段锁（ReentrantLock）

JDK 1.8：**CAS + synchronized + 分段数组**

**支持高并发读写操作**

**适合多线程环境**

## Queue 接口

常用于并发

两者内部都使用 **`ReentrantLock`** 和 **`Condition`** 控制线程安全和阻塞操作。

它们都属于 **阻塞队列（Blocking Queue）的一种实现**，适用于多线程生产者-消费者模型。

两者构造方法中可以设置**容量上限**（有界）。

- `new LinkedBlockingQueue<>(1000)` 限定最大容量为 1000。

如果使用无参构造，默认容量是：`Integer.MAX_VALUE`，**理论上无界**。

所以 **从默认行为看是无界的**，但实际上 **可以设置为有界队列**。





**LinkedBlockingQueue**

- **阻塞队列，线程安全**
- **基于链表结构**
- **支持 FIFO（先进先出）操作**
- 插入满了会阻塞，移除空了也会阻塞
- **常用于生产者-消费者模型**
- 支持一个方向的插入和移除（头出尾进）。

使用 **`ReentrantLock`** 实现线程安全

使用两个锁：**takeLock、putLock**，避免入队和出队相互阻塞





**LinkedBlockingDeque**

**线程安全**

**基于链表结构**

**双端阻塞队列**（支持两端操作）

既可以作为 **队列（FIFO）**，也可以作为 **栈（LIFO）** 使用

能实现 **队列模型**（tail add，head remove）

也能实现 **栈模型**（head add，head remove）

| 集合类型                      | 线程安全方式       |
| ----------------------------- | ------------------ |
| `Vector`                      | synchronized       |
| `Hashtable`                   | synchronized       |
| `Collections.synchronizedXXX` | 显式同步包装       |
| `CopyOnWriteArrayList`        | 写时复制机制       |
| `ConcurrentHashMap`           | CAS + synchronized |

# Io

- Input指从外部读入数据到内存，例如，把文件从磁盘读取到内存，从网络读取数据到内存等等。
- Output指把数据从内存输出到外部，例如，把数据从内存写入到文件，把数据从内存输出到网络等等。

IO流以`byte`（字节）为最小单位，因此也称为***字节流***。例如，我们要从磁盘读入一个文件，包含6个字节，就相当于读入了6个字节的数据：

如果我们需要读写的是字符，并且字符不全是单字节表示的ASCII字符，那么，按照`char`来读写显然更方便，这种流称为***字符流***。

Java提供了`Reader`和`Writer`表示字符流，字符流传输的最小数据单位是`char`。

同步IO是指，**读写IO时代码必须等待数据返回后才继续执行后续代码**，它的优点是代码编写简单，缺点是CPU执行效率低。

而异步IO是指，**读写IO时仅发出请求，然后立刻执行后续代码**，它的优点是CPU执行效率高，缺点是代码编写复杂。

## File

要构造一个`File`对象，需要传入文件路径：

```java
import java.io.*;

public class Main {
    public static void main(String[] args) {
        File f = new File("C:\\Windows\\notepad.exe");
        System.out.println(f);
    }
}
```



构造File对象时，既可以传入绝对路径，也可以传入相对路径。

有盘符的是绝对路径，没盘符的是相对路径

File对象有3种形式表示的路径，一种是`getPath()`，返回构造方法传入的路径，一种是`getAbsolutePath()`，返回绝对路径，一种是`getCanonicalPath`，它和绝对路径类似，但是返回的是规范路径。

绝对路径可以表示成`C:\Windows\System32\..\notepad.exe`，而规范路径就是把`.`和`..`转换成标准的绝对路径后的路径：`C:\Windows\notepad.exe`。

当File对象表示一个文件时，可以通过`createNewFile()`创建一个新文件，用`delete()`删除该文件：

有些时候，程序需要读写一些临时文件，File对象提供了`createTempFile()`来创建一个临时文件，以及`deleteOnExit()`在JVM退出时自动删除该文件。

当File对象表示一个目录时，可以使用`list()`和`listFiles()`列出目录下的文件和子目录名。`listFiles()`提供了一系列重载方法，可以过滤不想要的文件和目录：

## InputStream

```
InputStream`并不是一个接口，而是一个抽象类，它是所有输入流的超类。这个抽象类定义的一个最重要的方法就是`int read()
```

这个方法会读取输入流的下一个字节，并返回字节表示的`int`值（0~255）。如果已读到末尾，返回`-1`表示不能继续读取了。

`InputStream`和`OutputStream`都是通过`close()`方法来关闭流。关闭流就会释放对应的底层资源。

注意在方法完成后一定要关闭流

## OutputStream

和`InputStream`类似，`OutputStream`也是抽象类，它是所有输出流的超类。这个抽象类定义的一个最重要的方法就是`void write(int b)`

`OutputStream`还提供了一个`flush()`方法，它的目的是将缓冲区的内容真正输出到目的地。能强制把缓冲区内容输出。

## 读取配置

```java
try (InputStream input = getClass().getResourceAsStream("/default.properties")) {
    if (input != null) {
        // TODO:
    }
}
```

```java
Properties props = new Properties();
props.load(inputStreamFromClassPath("/default.properties"));
props.load(inputStreamFromFile("./conf.properties"));
```

读取配置文件，但是这个之后就被Spring取代了吧哈哈

## Reader

`Reader`是Java的IO库提供的另一个输入流接口。和`InputStream`的区别是，`InputStream`是一个字节流，即以`byte`为单位读取，而`Reader`是一个字符流，即以`char`为单位读取：

要避免乱码问题，我们需要在创建`FileReader`时指定编码：

```java
Reader reader = new FileReader("src/readme.txt", StandardCharsets.UTF_8);
```

除了特殊的**`CharArrayReader`和`StringReader`**，普通的`Reader`实际上是基于`InputStream`构造的，因为`Reader`需要从`InputStream`中读入字节流（`byte`），然后，根据编码设置，再转换为`char`就可以实现字符流。如果我们查看`FileReader`的源码，它在内部实际上持有一个`FileInputStream`。

既然`Reader`本质上是一个基于`InputStream`的`byte`到`char`的转换器，那么，如果我们已经有一个`InputStream`，想把它转换为`Reader`，是完全可行的。`InputStreamReader`就是这样一个转换器，它可以把任何`InputStream`转换为`Reader`。

## Writer

`Writer`是基于`OutputStream`构造的，可以通过`OutputStreamWriter`将`OutputStream`转换为`Writer`，转换时需要指定编码。

## printstream

`PrintStream`是一种`FilterOutputStream`，它在`OutputStream`的接口上，额外提供了一些写入各种数据类型的方法：

- 写入`int`：`print(int)`
- 写入`boolean`：`print(boolean)`
- 写入`String`：`print(String)`
- 写入`Object`：`print(Object)`，实际上相当于`print(object.toString())`

