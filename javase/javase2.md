---
title: javase面试题目hot1
categories:
  - javase
  - java
  
abbrlink: 2701
date: 2025.5.19
tags: 
   - java
   - 面试
---

# 基础面试题目

## 概念

### 1.说一下Java的特点

**平台无关性**：Java的编写一次，运无不在"哲学是其最大的特点之一。Java编译器将源代码编译成字节码（bytecode），该字节码可以在任何安装了Java虚拟机（JVM）的系统上运行。
**面向对象**：JaVa是一严格的面可对象编程语言，几乎一切都是对象。面可对象编程（OOP）特性使得代码更易于维护和重用，包括类（class）、对象（object）、继承（inheritance）、多态　(polymorphism）、抽象（abstraction）和封装（encapsulation）。
**内存管理**：JaVa有目己的垃圾回收机制，目动管理内存和回收不再使用的对象。这样，开发者不需要手动管埋内存，从而减少内存泄漏和其他内存相关的问题。

### 2.Java 的优势和劣势是什么？

首先，Java的优势，我记得**跨平台**应该是一个大点，因为**JVM**的存在，一次编写到处运行。然后**面向对象**，这个可能也是优势，不过现在很多语言都支持面向对象，但是Java的设计从一开始就是OOP的。还有**强大的生态系统**，比如Spring框架，Hibernate，各种库和工具，社区支持大，企业应用广泛。另外，**内存管理方面**，自动垃圾回收机制，减少了内存泄漏的问题，对开发者友好。**还有多线程支持**，内置的线程机制，方便并发编程。安全性方面，**Java有安全模型**，比如沙箱机制，适合网络环境。还有稳定性，企业级应用长期使用，版本更新也比较注重向后兼容。

劣势的话，**性能**可能是一个，虽然JVM优化了很多，但相比C++或者Rust这种原生编译语言，还是有一定开销。**特别是启动时间**，比如微服务场景下，可能不如Go之类的快。语法繁琐，比如样板代码多，之前没有lambda的时候更麻烦，现在有了但比起Python还是**不够简洁**。内存消耗，**JVM本身占内存，对于资源有限的环境可能不太友好**。**还有面向对象过于严格**，有时候写简单程序反而麻烦，虽然Java8引入了函数式编程，但不如其他语言自然。还有开发效率，相比动态语言如Python，**Java需要更多代码，编译过程也可能拖慢开发节奏。**

### 3.Java为什么是跨平台的？

 Java 能支持跨平台，主要依赖于 **JVM** 关系比较大。

**JVM也是一个软件，不同的平台有不同的版本**。我们编写的Java源码，编译后会生成一种 .class 文件，称为字节码文件。**Java虚拟机就是负责将字节码文件翻译成特定平台下的机器码然后运行。**也就是说，只要在不同平台上安装对应的JVM，就可以运行字节码文件，运行我们编写的Java程序。

JVM是一个”桥梁“，是一个”中间件“，是实现跨平台的关键，Java代码首先被编译成字节码文件，再由JVM将字节码文件翻译成机器语言，从而达到运行Java程序的目的。

编译的结果不是生成机器码，而是生成字节码，字节码不能直接运行，必须通过JVM翻译成机器码才能运行。不同平台下编译生成的字节码是一样的，但是由JVM翻译成的机器码却不一样。

所以，运行Java程序必须有JVM的支持，因为编译的结果不是机器码，必须要经过JVM的再次翻译才能执行。即使你将Java程序打包成可执行文件（例如 .exe），仍然需要JVM的支持。

### 4.JVM、JDK、JRE三者关系？

- **JVM**是Java虚拟机，**是Java程序运行的环境**。它负责将Java字节码（由Java编译器生成）解释或编译成机器码，并执行程序。JVM提供了内存管理、垃圾回收、安全性等功能，使得Java程序具备跨平台性。
- **JDK**是Java开发工具包，**是开发Java程序所需的工具集合**。它包含了JVM、编译器（javac）、调试器（jdb）等开发工具，以及一系列的类库（如Java标准库和开发工具库）。**JDK提供了开发、编译、调试和运行Java程序所需的全部工具和环境。**
- **JRE**是Java运行时环境，**是Java程序运行所需的最小环境**。它包含了JVM和一组Java类库，用于支持Java程序的执行。**JRE不包含开发工具，只提供Java程序运行所需的运行环境。**

### 5.为什么Java解释和编译都有？

首先在Java经过编译之后生成字节码文件，接下来进入JVM中，就有两个步骤编译和解释。 如下图：

![](https://cdn.xiaolincoding.com//picgo/1715928000183-44fc6130-8abc-4f0b-8f6d-79de0ab09509.webp)

**编译性**：

- Java源代码首先被编译成字节码，JIT 会把编译过的机器码保存起来,以备下次使用。

**解释性：**

- JVM中一个**方法调用计数器，当累计计数大于一定值的时候，就使用JIT进行编译生成机器码文件。**否则就是用解释器进行解释执行，然后字节码也是经过解释器进行解释运行的。

所以Java既是**编译型也是解释性语言**，默认采用的是解释器和编译器混合的模式。



### 6.jvm是什么

JVM是 **java 虚拟机**，主要工作是**解释自己的指令集（即字节码）并映射到本地的CPU指令集和OS的系统调用。**

JVM屏蔽了与操作系统平台相关的信息，使得Java程序只需要生成在Java虚拟机上运行的目标代码（字节码），就可在多种平台上不加修改的运行，这也是Java能够“**一次编译，到处运行的**”原因。

### 7.**编译型语言和解释型语言的区别？**

编译型语言和解释型语言的区别在于：

- 编译型语言：在程序执行之前，整个源代码会被编译成机器码或者字节码，生成可执行文件。执行时直接**运行编译后的代码，速度快，但跨平台性较差**。
- 解释型语言：在程序执行时，逐行解释执行源代码，不生成独立的可执行文件。通常由**解释器动态解释并执行代码，跨平台性好，但执行速度相对较慢。**
- 典型的编译型语言如C、C++，典型的解释型语言如Python、JavaScript。

## 数据类型

### 1.八种基本的数据类型

Java支持数据类型分为两类： 基本数据类型和引用数据类型。

基本数据类型共有8种，可以分为三类：

- 数值型：整数类型（byte、short、int、long）和浮点类型（float、double）
- 字符型：char
- 布尔型：boolean

8种基本数据类型的默认值、位数、取值范围，如下表所示：

| 数据类型  | 占用大小（字节）              | 位数       | 取值范围                                                     | 默认值   | 描述                                                         |
| --------- | ----------------------------- | ---------- | ------------------------------------------------------------ | -------- | ------------------------------------------------------------ |
| `byte`    | 1                             | 8          | -128（-2^7） 到 127（2^7 - 1）                               | 0        | 是最小的整数类型，适合用于节省内存，例如在处理文件或网络流时存储小范围整数数据。 |
| `short`   | 2                             | 16         | -32768（-2^15） 到 32767（2^15 - 1）                         | 0        | 较少使用，通常用于在需要节省内存且数值范围在该区间的场景。   |
| `int`     | 4                             | 32         | -2147483648（-2^31） 到 2147483647（2^31 - 1）               | 0        | 最常用的整数类型，可满足大多数日常编程中整数计算的需求。     |
| `long`    | 8                             | 64         | -9223372036854775808（-2^63） 到 9223372036854775807（2^63 - 1） | 0L       | 用于表示非常大的整数，当 `int` 类型无法满足需求时使用，定义时数值后需加 `L` 或 `l`。 |
| `float`   | 4                             | 32         | 1.4E - 45 到 3.4028235E38                                    | 0.0f     | 单精度浮点数，用于表示小数，精度相对较低，定义时数值后需加 `F` 或 `f`。 |
| `double`  | 8                             | 64         | 4.9E - 324 到 1.7976931348623157E308                         | 0.0d     | 双精度浮点数，精度比 `float` 高，是 Java 中表示小数的默认类型。 |
| `char`    | 2                             | 16         | '\u0000'（0） 到 '\uffff'（65535）                           | '\u0000' | 用于表示单个字符，采用 Unicode 编码，可表示各种语言的字符。  |
| `boolean` | 无明确字节大小（理论上 1 位） | 无明确位数 | `true` 或 `false`                                            | `false`  | 用于逻辑判断，只有两个取值，常用于条件判断和循环控制等逻辑场景。 |



- Java八种基本数据类型的字节数：1字节(byte、boolean)、 2字节(short、char)、4字节(int、float)、8字节(long、double)
- 浮点数的默认类型为double（如果需要声明一个常量为float型，则必须要在末尾加上f或F）
- 整数的默认类型为int（声明Long型在末尾加上l或者L）
- 八种基本数据类型的包装类：除了char的是Character、int类型的是Integer，其他都是首字母大写
- char类型是无符号的，不能为负，所以是0开始的

### 2.int和long是多少位，多少字节的？

- `int`类型是 **32 位（bit），占 4 个字节（byte）**，int 是有符号整数类型，其取值范围是从 **-2^31 到 2^31-1** 。例如，在一个简单的计数器程序中，如果使用`int`类型来存储计数值，它可以表示的最大正数是 **2,147,483,647**。如果计数值超过这个范围，就会发生溢出，导致结果不符合预期。
- `long`类型是 **64 位，占 8 个字节**，`long`类型也是有符号整数类型，它的取值范围是从 **-2^63 到 2^63 -1** ，在处理较大的整数数值时，果`int`类型的取值范围不够，就需要使用`long`类型。例如，在一个文件传输程序中，文件的大小可能会很大，使用`int`类型可能无法准确表示，而`long`类型就可以很好地处理这种情况。

### 3.long和int可以互转吗 ？

可以的，Java中的`long`和`int`可以相互转换。由于`long`类型的范围比`int`类型大，因此将`int`转换为`long`是安全的，而将`long`转换为`int`可能会导致数据丢失或溢出。

将`int`转换为`long`可以通过**直接赋值或强制类型转换**来实现。例如：

```text
int intValue = 10;
long longValue = intValue; // 自动转换，安全的
```

将`long`转换为`int`需要使用**强制类型转换**，但需要注意潜在的数据丢失或溢出问题。

在将`long`转换为`int`时，如果`longValue`的值超出了`int`类型的范围，转换结果将是截断后的低位部分。因此，在进行转换之前，建议先检查`longValue`的值是否在`int`类型的范围内，以避免数据丢失或溢出的问题。

**低->高直接转**

### 4.数据类型转换方式你知道哪些？

- **自动类型转换（隐式转换）**：当目标类型的范围大于源类型时，Java会自动将源类型转换为目标类型，不需要显式的类型转换。例如，将`int`转换为`long`、将`float`转换为`double`等。
- **强制类型转换（显式转换）**：当目标类型的范围小于源类型时，需要使用强制类型转换将源类型转换为目标类型。这可能导致数据丢失或溢出。例如，将`long`转换为`int`、将`double`转换为`int`等。语法为：目标类型 变量名 = (目标类型) 源类型。
- 字符串转换：**Java提供了将字符串表示的数据转换为其他类型数据的方法**。例如，将字符串转换为整型`int`，可以使用`Integer.parseInt()`方法；将字符串转换为浮点型`double`，可以使用`Double.parseDouble()`方法等。
- 数值之间的转换：**Java提供了一些数值类型之间的转换方法，如将整型转换为字符型、将字符型转换为整型等。**这些转换方式可以通过类型的包装类来实现，例如`Character`类、`Integer`类等提供了相应的转换方法。

### 5.类型互转会出现什么问题吗？

- 数据丢失：当将**一个范围较大的数据类型转换为一个范围较小的数据类型时**，可能会发生数据丢失。例如，将一个`long`类型的值转换为`int`类型时，如果`long`值超出了`int`类型的范围，转换结果将是截断后的低位部分，高位部分的数据将丢失。
- 数据溢出：与数据丢失相反，**当将一个范围较小的数据类型转换为一个范围较大的数据类型时，可能会发生数据溢出。**例如，将一个`int`类型的值转换为`long`类型时，转换结果会填充额外的高位空间，但原始数据仍然保持不变。
- 精度损失：**在进行浮点数类型的转换时，可能会发生精度损失。**由于浮点数的表示方式不同，将一个单精度浮点数(`float`)转换为双精度浮点数(`double`)时，精度可能会损失。
- 类型不匹配导致的错误：在进行类型转换时，**需要确保源类型和目标类型是兼容的。如果两者不兼容，会导致编译错误或运行时错误。**

### 6.为什么用bigDecimal 不用double ？

double会出现精度丢失的问题，double执行的是二进制浮点运算，二进制有些情况下不能准确的表示一个小数，就像十进制不能准确的表示1/3(1/3=0.3333...)，也就是说二进制表示小数的时候只能够表示能够用1/(2^n)的和的任意组合，但是0.1不能够精确表示，因为它不能够表示成为1/(2^n)的和的形式。

比如：

```java
System.out.println(0.05 + 0.01);
System.out.println(1.0 - 0.42);
System.out.println(4.015 * 100);
System.out.println(123.3 / 100);

输出：
0.060000000000000005
0.5800000000000001
401.49999999999994
1.2329999999999999
```

可以看到在**Java中进行浮点数运算的时候，会出现丢失精度的问题。**那么我们如果在进行商品价格计算的时候，就会出现问题。很有可能造成我们手中有0.06元，却无法购买一个0.05元和一个0.01元的商品。因为如上所示，他们两个的总和为0.060000000000000005。这无疑是一个很严重的问题，尤其是当电商网站的并发量上去的时候，出现的问题将是巨大的。可能会导致无法下单，或者对账出现问题。

**而 Decimal 是精确计算 , 所以一般牵扯到金钱的计算 , 都使用 Decimal。**

```java
import java.math.BigDecimal;

public class BigDecimalExample {
    public static void main(String[] args) {
        BigDecimal num1 = new BigDecimal("0.1");
        BigDecimal num2 = new BigDecimal("0.2");

        BigDecimal sum = num1.add(num2);
        BigDecimal product = num1.multiply(num2);

        System.out.println("Sum: " + sum);
        System.out.println("Product: " + product);
    }
}

//输出
Sum: 0.3
Product: 0.02
```

这样的使用`BigDecimal`可以确保精确的十进制数值计算，避免了使用`double`可能出现的舍入误差。需要注意的是，在创建`BigDecimal`对象时，应该使用**字符串作为参数**，而不是直接使用浮点数值，以避免浮点数精度丢失。

### 7.装箱和拆箱是什么？

装箱（Boxing）和拆箱（Unboxing）是将基本数据类型和对应的包装类之间进行转换的过程。

自动装箱主要发生在两种情况，**一种是赋值时，另一种是在方法调用的时候。**

> 赋值时

这是最常见的一种情况，在Java 1.5以前我们需要手动地进行转换才行，而现在所有的转换都是由编译器来完成。

```java
//before autoboxing
Integer iObject = Integer.valueOf(3);
Int iPrimitive = iObject.intValue()

//after java5
Integer iObject = 3; //autobxing - primitive to wrapper conversion
int iPrimitive = iObject; //unboxing - object to primitive conversion
```

> 方法调用时

当我们在方法调用时，我们可以传入原始数据值或者对象，同样编译器会帮我们进行转换。

```java
public static Integer show(Integer iParam){
   System.out.println("autoboxing example - method invocation i: " + iParam);
   return iParam;
}

//autoboxing and unboxing in method invocation
show(3); //autoboxing
int result = show(3); //unboxing because return type of method is Integer
```

show方法接受Integer对象作为参数，当调用`show(3)`时，会将int值转换成对应的Integer对象，这就是所谓的自动装箱，show方法返回Integer对象，而`int result = show(3);`中result为int类型，所以这时候发生自动拆箱操作，将show方法的返回的Integer对象转换成int值。

自动装箱有一个问题，**那就是在一个循环中进行自动装箱操作的情况，如下面的例子就会创建多余的对象，影响程序的性能。**

```java
Integer sum = 0; for(int i=1000; i<5000; i++){   sum+=i; } 
```

上面的代码`sum+=i`可以看成`sum = sum + i`，但是`+`这个操作符不适用于Integer对象，首先sum进行自动拆箱操作，进行数值相加操作，最后发生自动装箱操作转换成Integer对象。其内部变化如下

```java
int result = sum.intValue() + i; Integer sum = new Integer(result); 
```

由于我们这里声明的sum为Integer类型，**在上面的循环中会创建将近4000个无用的Integer对象，在这样庞大的循环中，会降低程序的性能并且加重了垃圾回收的工作量。**因此在我们编程时，需要注意到这一点，正确地声明变量类型，避免因为自动装箱引起的性能问题。

### 8.Java为什么要有Integer？

Integer对应是int类型的包装类，就是把int类型包装成Object对象，**对象封装有很多好处，可以把属性也就是数据跟处理这些数据的方法结合在一起**，比如Integer就有parseInt()等方法来专门处理int型相关的数据。

另一个非常重要的原因就是在**Java中绝大部分方法或类都是用来处理类类型对象的，如ArrayList集合类就只能以类作为他的存储对象，而这时如果想把一个int型的数据存入list是不可能的，必须把它包装成类，**也就是Integer才能被List所接受。所以Integer的存在是很必要的。

在Java中，**泛型只能使用引用类型**，而不能使用基本类型。因此，如果要在泛型中使用int类型，必须使用Integer包装类。

在Java中，**基本类型和引用类型不能直接进行转换，必须使用包装类来实现。**例如，将一个int类型的值转换为String类型，必须首先将其转换为Integer类型，然后再转换为String类型。

Java**集合中只能存储对象，而不能存储基本数据类型。**因此，如果要将int类型的数据存储在集合中，必须使用Integer包装类。

### 9.Integer相比int有什么优点？

int是Java中的原始数据类型，而Integer是int的包装类。

Integer和 int 的区别：

- 基本类型和引用类型：首先，int是一种基本数据类型，而Integer是一种引用类型。基本数据类型是Java中最基本的数据类型，**它们是预定义的，不需要实例化就可以使用。而引用类型则需要通过实例化对象来使用。**这意味着，使用int来存储一个整数时，不需要任何额外的内存分配，而使用Integer时，必须为对象分配内存。在性能方面，**基本数据类型的操作通常比相应的引用类型快。**
- 自动装箱和拆箱：其次，Integer作为int的包装类，**它可以实现自动装箱和拆箱。自动装箱是指将基本类型转化为相应的包装类类型，而自动拆箱则是将包装类类型转化为相应的基本类型。**这使得Java程序员**更加方便地进行数据类型转换。**例如，当我们需要将int类型的值赋给Integer变量时，Java可以自动地将int类型转换为Integer类型。同样地，当我们需要将Integer类型的值赋给int变量时，Java可以自动地将Integer类型转换为int类型。
- 空指针异常：另外，**int变量可以直接赋值为0，而Integer变量必须通过实例化对象来赋值。**如果对一个未经初始化的Integer变量进行操作，就会出现空指针异常。这是因为它被赋予了**null值**，而null值是无法进行自动拆箱的。

### 10.那为什么还要保留int类型？

包装类是引用类型，对象的引用和对象本身是分开存储的，**而对于基本类型数据，变量对应的内存块直接存储数据本身。**

因此，基本类型数据在**读写效率方面，要比包装类高效**。除此之外，在64位JVM上，在开启引用压缩的情况下，一个Integer对象占用16个字节的内存空间，而一个int类型数据只占用4字节的内存空间，前者对空间的占用是后者的4倍。

也就是说，**不管是读写效率，还是存储效率，基本类型都比包装类高效。**

### 11.说一下 integer的缓存

Java的Integer类内部实现了一个**静态缓存池**，用于存储特定范围内的整数值对应的Integer对象。

默认情况下，**这个范围是-128至127。**当通过**Integer.valueOf(int)方法创建一个在这个范围内的整数对象时，并不会每次都生成新的对象实例，而是复用缓存中的现有对象，会直接从内存中取出，不需要新建一个对象。**

但是你new的话就是一个新的对象了。

# 面向对象

## 1.怎么理解面向对象？简单说说封装继承多态

面向对象是一种编程范式，它**将现实世界中的事物抽象为对象**，对象具有属性（称为字段或属性）和行为（称为方法）。面向对象编程的设计思想是以对象为中心，通过对象之间的交互来完成程序的功能，具有灵活性和可扩展性，通过封装和继承可以更好地应对需求变化。

Java面向对象的三大特性包括：**封装、继承、多态**：

- **封装**：封装是指将对象的**属性（数据）和行为（方法）结合在一起**，对外隐藏对象的内部细节，仅通过对象提供的接口与外界交互。**封装的目的是增强安全性和简化编程，使得对象更加独立。**
- **继承**：继承是一种可以使得**子类自动共享父类数据结构和方法的机制**。它是代码复用的重要手段，通过继承可以建立类与类之间的层次关系，使得结构更加清晰。
- **多态**：多态是指允许不同类的对象对同一消息作出响应。即同一个接口，使用不同的实例而执行不同操作。多态性可以分为**编译时多态（重载）和运行时多态（重写）**。它使得程序具有良好的灵活性和扩展性。

## 2.多态体现在哪几个方面？

多态在面向对象编程中可以体现在以下几个方面：

- 方法重载：
  - 方法重载是指同一类中可以有**多个同名方法，它们具有不同的参数列表**（参数类型、数量或顺序不同）。虽然方法名相同，但根据传入的参数不同，编译器会在编译时确定调用哪个方法。
  - 示例：对于一个 `add` 方法，可以定义为 `add(int a, int b)` 和 `add(double a, double b)`。
- 方法重写：
  - 方法重写是**指子类能够提供对父类中同名方法的具体实现**。在运行时，JVM会根据对象的实际类型确定调用哪个版本的方法。这是实现多态的主要方式。
  - 示例：在一个动物类中，定义一个 `sound` 方法，子类 `Dog` 可以重写该方法以实现 `bark`，而 `Cat` 可以实现 `meow`。
- 接口与实现：
  - 多态也体现在接口的使用上，**多个类可以实现同一个接口，并且用接口类型的引用来调用这些类的方法。这使得程序在面对不同具体实现时保持一贯的调用方式。**
  - 示例：多个类（如 `Dog`, `Cat`）都实现了一个 `Animal` 接口，当用 `Animal` 类型的引用来调用 `makeSound` 方法时，会触发对应的实现。
- 向上转型和向下转型：
  - 在Java中，**可以使用父类类型的引用指向子类对象，这是向上转型。**通过这种方式，可以在运行时期采用不同的子类实现。
  - 向下转型是将**父类引用转回其子类类型**，但在执行前需要确认引用实际指向的对象类型以避免 `ClassCastException`。

## 3.多态解决了什么问题？

多态是**指子类可以替换父类，**在实际的代码运行过程中，调用子类的方法实现。多态这种特性也需要编程语言提供特殊的语法机制来实现，**比如继承、接口类**。

**多态可以提高代码的扩展性和复用性，是很多设计模式、设计原则、编程技巧的代码实现基础。**比如策略模式、基于接口而非实现编程、依赖倒置原则、里式替换原则、利用多态去掉冗长的 if-else 语句等等

## 4.面向对象的设计原则你知道有哪些吗

面向对象编程中的六大原则：

- **单一职责原则（SRP）**：一个类应该只有一个引起它变化的原因，即一个类应该只负责一项职责。例子：考虑一个员工类，它应该只负责管理员工信息，而不应负责其他无关工作。
- **开放封闭原则（OCP）**：软件实体应该对扩展开放，对修改封闭。例子：通过制定接口来实现这一原则，比如定义一个图形类，然后让不同类型的图形继承这个类，而不需要修改图形类本身。
- **里氏替换原则（LSP）**：子类对象应该能够替换掉所有父类对象。例子：一个正方形是一个矩形，但如果修改一个矩形的高度和宽度时，正方形的行为应该如何改变就是一个违反里氏替换原则的例子。
- **接口隔离原则（ISP）**：客户端不应该依赖那些它不需要的接口，即接口应该小而专。例子：通过接口抽象层来实现底层和高层模块之间的解耦，比如使用依赖注入。
- **依赖倒置原则（DIP）**：高层模块不应该依赖低层模块，二者都应该依赖于抽象；抽象不应该依赖于细节，细节应该依赖于抽象。例子：如果一个公司类包含部门类，应该考虑使用合成/聚合关系，而不是将公司类继承自部门类。
- **最少知识原则 (Law of Demeter)**：一个对象应当对其他对象有最少的了解，只与其直接的朋友交互。

## 5.重载与重写有什么区别？

- 重载（Overloading）指的是在**同一个类中，可以有多个同名方法，它们具有不同的参数列表**（参数类型、参数个数或参数顺序不同），编译器根据调用时的参数类型来决定调用哪个方法。
- 重写（Overriding）指的是**子类可以重新定义父类中的方法，**方法名、参数列表和返回类型必须与父类中的方法一致，通过**@override**注解来明确表示这是对父类方法的重写。

重载是指在同一个类中定义多个同名方法，而重写是指子类重新定义父类中的方法。

## 6.抽象类和普通类区别？

- 实例化：普通类可以直接实例化对象，而抽象类不能被实例化，只能被继承。
- 方法实现：普通类中的方法可以有具体的实现，而抽象类中的方法可以有实现也可以没有实现。
- 继承：一个类可以继承一个普通类，而且可以继承多个接口；而一个类只能继承一个抽象类，但可以同时实现多个接口。
- 实现限制：普通类可以被其他类继承和使用，而抽象类一般用于作为基类，被其他类继承和扩展使用。

## 7.Java抽象类和接口的区别是什么？

**两者的特点：**

- 抽象类用于**描述类的共同特性和行为，可以有成员变量、构造方法和具体方法。**适用于有明显**继承**关系的场景。
- 接口用于定义行为规范，可以多实现，**只能有常量和抽象方法（Java 8 以后可以有默认方法和静态方法）。**适用于定义类的能力或功能。

**两者的区别：**

- 实现方式：实现接口的关键字为**implements**，继承抽象类的关键字为**extends**。一个类可以实现多个接口，但一个类只能继承一个抽象类。所以，使用接口可以间接地实现多重继承。
- 方法方式：**接口只有定义，不能有方法的实现**，java 1.8中可以定义default方法体，而抽**象类可以有定义与实现，方法可在抽象类中实现。**
- 访问修饰符：**接口成员变量默认为public static final**，必须赋初值，不能被修改；其所有的成员方法都是public、abstract的。**抽象类中成员变量默认default**，可在子类中被重新定义，也可被重新赋值；抽象方法被abstract修饰，不能被private、static、synchronized和native等修饰，必须以分号结尾，不带花括号。
- 变量：**抽象类可以包含实例变量和静态变量，而接口只能包含常量（**即静态常量）。

## 8.抽象类能加final修饰吗？

**不能**，Java中的抽象类是用来被继承的，而final修饰符用于禁止类被继承或方法被重写，因此，抽象类和final修饰符是互斥的，不能同时使用。

## 9.接口里面可以定义哪些方法？

- **抽象方法**

抽象方法是接口的核心部分，所有实现接口的类都必须实现这些方法。抽象方法默认是 public 和 abstract，这些修饰符可以省略。

```java
public interface Animal {
    void makeSound();
}
```

- **默认方法**

默认方法是在 Java 8 中引入的，允许接口提供具体实现。实现类可以选择重写默认方法。

```java
public interface Animal {
    void makeSound();
    
    default void sleep() {
        System.out.println("Sleeping...");
    }
}
```

- **静态方法**

静态方法也是在 Java 8 中引入的，它们属于接口本身，可以通过接口名直接调用，而不需要实现类的对象。

```java
public interface Animal {
    void makeSound();
    
    static void staticMethod() {
        System.out.println("Static method in interface");
    }
}
```

- **私有方法**

私有方法是在 Java 9 中引入的，用于在接口中为默认方法或其他私有方法提供辅助功能。这些方法不能被实现类访问，只能在接口内部使用。

```java
public interface Animal {
    void makeSound();
    
    default void sleep() {
        System.out.println("Sleeping...");
        logSleep();
    }
    
    private void logSleep() {
        System.out.println("Logging sleep");
    }
}
public interface Animal {
    void makeSound();
}
```

## 10.抽象类可以被实例化吗？

在Java中，抽象类本身不能被实例化。

这意味着不能使用`new`关键字直接创建一个抽象类的对象。抽象类的存在主要是为了被继承，它通常包含一个或多个抽象方法（由`abstract`关键字修饰且无方法体的方法），这些方法需要在子类中被实现。

**抽象类可以有构造器，他可以有构造方法，这些构造器在子类实例化时会被调用，以便进行必要的初始化工作。**然而，这个过程并不是直接实例化抽象类，**而是创建了子类的实例，间接地使用了抽象类的构造器。**

简而言之，抽象类不能直接实例化，但通过继承抽象类并实现所有抽象方法的子类是可以被实例化的。

## 11.接口可以包含构造函数吗？

**在接口中，不可以有构造方法**,在接口里写入构造方法时，编译器提示：Interfaces cannot have constructors，因为接口不会有自己的实例的，所以不需要有构造函数。

为什么呢？构造函数就是初始化class的属性或者方法，在new的一瞬间自动调用，那么问题来了Java的接口，都不能new 那么要构造函数干嘛呢？根本就没法调用

## 12.解释Java中的静态变量和静态方法

在Java中，**静态变量和静态方法是与类本身关联的，而不是与类的实例（对象）关联。它们在内存中只存在一份，可以被类的所有实例共享。**

> 静态变量

静态变量（也称为类变量）是在类中使用`static`关键字声明的变量。它们属于类而不是任何具体的对象。主要的特点：

- **共享性**：**所有该类的实例共享同一个静态变量**。如果一个实例修改了静态变量的值，其他实例也会看到这个更改。
- **初始化**：**静态变量在类被加载时初始化，只会对其进行一次分配内存。**
- **访问方式**：静态变量可以直接通过**类名访问**，也可以通过实例访问，但推荐使用类名。

> 静态方法

静态方法是在类中使用`static`关键字声明的方法。类似于静态变量，静态方法也属于类，而不是任何具体的对象。主要的特点：

- **无实例依赖**：静态方法可以在没有创建类实例的情况下调用。**对于静态方法来说，不能直接访问非静态的成员变量或方法**，因为静态方法没有上下文的实例。
- **访问静态成员**：静态方法**可以直接调用其他静态变量和静态方法，但不能直接访问非静态成员。**
- **多态性**：静态方法不支持重写（Override），但可以被隐藏（Hide）。

## 13.非静态内部类和静态内部类的区别？

区别包括：

- 非静态内部类依赖于外部类的实例，而静态内部类不依赖于外部类的实例。
- 非静态内部类可以访问外部类的实例变量和方法，而静态内部类只能访问外部类的静态成员。
- 非静态内部类不能定义静态成员，而静态内部类可以定义静态成员。
- 非静态内部类在外部类实例化后才能实例化，而静态内部类可以独立实例化。
- 非静态内部类可以访问外部类的私有成员，而静态内部类不能直接访问外部类的私有成员，需要通过实例化外部类来访问。

总结来说**一内一外**

## 14.非静态内部类可以直接访问外部方法，编译器是怎么做到的？

非静态内部类可以直接访问外部方法是因为**编译器在生成字节码时会为非静态内部类维护一个指向外部类实例的引用。**

这个引用使得非静态内部类能够访问外部类的实例变量和方法。**编译器会在生成非静态内部类的构造方法时，将外部类实例作为参数传入，并在内部类的实例化过程中建立外部类实例与内部类实例之间的联系，从而实现直接访问外部方法的功能。**

# 关键词&泛型&反射

## 1.Java 中 final 作用是什么？

`final`关键字主要有以下三个方面的作用：用于修饰类、方法和变量。

- 修饰类：当`final`修饰一个类时，**表示这个类不能被继承**，是类继承体系中的最终形态。例如，Java 中的`String`类就是用`final`修饰的，这保证了`String`类的不可变性和安全性，防止其他类通过继承来改变`String`类的行为和特性。
- 修饰方法：**用`final`修饰的方法不能在子类中被重写**。比如，`java.lang.Object`类中的`getClass`方法就是`final`的，因为这个方法的行为是由 Java 虚拟机底层实现来保证的，不应该被子类修改。
- 修饰变量：**当`final`修饰基本数据类型的变量时，该变量一旦被赋值就不能再改变。**例如，`final int num = 10;`，这里的`num`就是一个常量，不能再对其进行重新赋值操作，否则会导致编译错误。**对于引用数据类型，`final`修饰意味着这个引用变量不能再指向其他对象，但对象本身的内容是可以改变的。**例如，`final StringBuilder sb = new StringBuilder("Hello");`，不能让`sb`再指向其他`StringBuilder`对象，但可以通过`sb.append(" World");`来修改字符串的内容。

## 2.什么是泛型？

泛型是 Java 编程语言中的一个重要特性，**它允许类、接口和方法在定义时使用一个或多个类型参数，这些类型参数在使用时可以被指定为具体的类型。**

泛型的主要目的是**在编译时提供更强的类型检查，并且在编译后能够保留类型信息，**避免了在运行时出现类型转换异常。

> 为什么需要泛型？

- **适用于多种数据类型执行相同的代码**

```java
private static int add(int a, int b) {
    System.out.println(a + "+" + b + "=" + (a + b));
    return a + b;
}

private static float add(float a, float b) {
    System.out.println(a + "+" + b + "=" + (a + b));
    return a + b;
}

private static double add(double a, double b) {
    System.out.println(a + "+" + b + "=" + (a + b));
    return a + b;
}
```

如果没有泛型，要实现不同类型的加法，每种类型都需要重载一个add方法；通过泛型，我们可以复用为一个方法：

```java
private static <T extends Number> double add(T a, T b) {
    System.out.println(a + "+" + b + "=" + (a.doubleValue() + b.doubleValue()));
    return a.doubleValue() + b.doubleValue();
}
```

- **泛型中的类型在使用时指定，不需要强制类型转换**（**类型安全**，编译器会**检查类型**）

看下这个例子：

```java
List list = new ArrayList();
list.add("xxString");
list.add(100d);
list.add(new Person());
```

我们在使用上述list中，list中的元素都是Object类型（无法约束其中的类型），所以在取出集合元素时需要人为的强制类型转化到具体的目标类型，**且很容易出现java.lang.ClassCastException异常。**

引入泛型，它将提供类型的约束，**提供编译前的检查：**

```java
List<String> list = new ArrayList<String>();

// list中只能放String, 不能放其它类型的元素
```

好处：

**编译期类型检查**，提升代码安全性，防止运行时类型转换异常（ClassCastException）。编译时会自动装箱拆箱来配合基本类型的包装类。

**避免强制类型转换**，代码更简洁清晰。

**提高代码复用性**，同一份代码可以处理不同类型的数据。

他只能使用包装类，不能用于基本的数据类型

Java 泛型采用**类型擦除**实现，泛型信息在编译后被擦除，泛型变量变为原始类型（通常是Object或边界类型）。

运行的时候是不能使用的，比如不能创建泛型数组，无法进行类型判断等

## 3.什么是反射

Java 反射机制是**在运行状态中，对于任意一个类，都能够知道这个类中的所有属性和方法，对于任意一个对象，都能够调用它的任意一个方法和属性；**这种**动态获取**的信息以及动态调用对象的方法的功能称为 Java 语言的反射机制。

反射具有以下特性：

1. **运行时类信息访问**：反射机制允许程序在**运行时获取类的完整结构信息**，包括类名、包名、父类、实现的接口、构造函数、方法和字段等。
2. **动态对象创建**：可以使用**反射API动态地创建对象实例**，即使在编译时不知道具体的类名。这是通过Class类的newInstance()方法或Constructor对象的newInstance()方法实现的。
3. **动态方法调用**：可以在**运行时动态地调用对象的方法**，包括私有方法。这通过Method类的invoke()方法实现，允许你传入对象实例和参数值来执行方法。
4. **访问和修改字段值**：反射还允许**程序在运行时访问和修改对象的字段值**，即使是私有的。这是通过Field类的get()和set()方法完成的。

使用实例：

> 加载数据库驱动

我们的项目底层数据库有时是用mysql，有时用oracle，需要动态地根据实际情况加载驱动类，这个时候反射就有用了，假设 com.mikechen.java.myqlConnection，com.mikechen.java.oracleConnection这两个类我们要用。

这时候我们在使用 **JDBC 连接数据库时使用 Class.forName()通过反射加载数据库的驱动程序，**如果是mysql则传入mysql的驱动类，而如果是oracle则传入的参数就变成另一个了。

```java
//  DriverManager.registerDriver(new com.mysql.cj.jdbc.Driver());
Class.forName("com.mysql.cj.jdbc.Driver");
```

> 配置文件加载

Spring 框架的 IOC（动态加载管理 Bean），Spring通过配置文件配置各种各样的bean，你需要用到哪些bean就配哪些，spring容器就会根据你的需求去动态加载，你的程序就能健壮地运行。

Spring通过XML配置模式装载Bean的过程：

- 将程序中所有XML或properties配置文件加载入内存
- Java类里面**解析xml或者properties里面的内容，得到对应实体类的字节码字符串以及相关的属性信息**
- 使用反射机制，根据这个字符串获得**某个类的Class实例**
- 动态配置实例的属性

配置文件

```java
className=com.example.reflectdemo.TestInvoke
methodName=printlnState
```

实体类

```java
public class TestInvoke {
    private void printlnState(){
        System.out.println("I am fine");
    }
}
```

解析配置文件内容

```java
// 解析xml或properties里面的内容，得到对应实体类的字节码字符串以及属性信息
public static String getName(String key) throws IOException {
    Properties properties = new Properties();
    FileInputStream in = new FileInputStream("D:\IdeaProjects\AllDemos\language-specification\src\main\resources\application.properties");
    properties.load(in);
    in.close();
    return properties.getProperty(key);
}
```

利用反射获取实体类的Class实例，创建实体类的实例对象，调用方法

```java
public static void main(String[] args) throws NoSuchMethodException, InvocationTargetException, IllegalAccessException, IOException, ClassNotFoundException, InstantiationException {
    // 使用反射机制，根据这个字符串获得Class对象
    Class<?> c = Class.forName(getName("className"));
    System.out.println(c.getSimpleName());
    // 获取方法
    Method method = c.getDeclaredMethod(getName("methodName"));
    // 绕过安全检查
    method.setAccessible(true);
    // 创建实例对象
    TestInvoke testInvoke = (TestInvoke)c.newInstance();
    // 调用方法
    method.invoke(testInvoke);

}
```

- **判断对象所属类**：`obj.getClass()` 获取对象的 Class 对象。
- **加载类**：`Class.forName("全类名")` 动态加载类。
- **获取构造器并实例化对象**：`clazz.getConstructor(...)` + `constructor.newInstance(...)`
- **访问类的成员变量和方法**：`clazz.getDeclaredFields()`, `clazz.getDeclaredMethods()`
- **调用对象方法**：`method.invoke(obj, args...)`
- **访问私有成员**：通过 `setAccessible(true)` 取消访问检查。

反射机制**实际上会降低程序的性能**,因为它需要在运行时进行类型检查和解析。同时,反射也可能破坏封装性,带来安全风险,因为它可以访问私有成员

| 优点                                             | 缺点                                             |
| ------------------------------------------------ | ------------------------------------------------ |
| **提高程序灵活性和扩展性**，支持动态加载类和调用 | **性能较慢**，需要绕过 JVM 优化，存在额外开销    |
| 支持框架、工具实现通用逻辑和动态操作             | **破坏封装性**，可访问私有成员，可能导致安全风险 |
| 支持动态代理、注解解析等高级功能                 | **编译时缺乏类型检查**，错误只能运行时发现       |

# 深拷贝和浅拷贝

## 1.深拷贝和浅拷贝的区别

- 浅拷贝是指**只复制对象本身和其内部的值类型字段**，但不会复制对象内部的引用类型字段。换句话说，**浅拷贝只是创建一个新的对象，然后将原对象的字段值复制到新对象中**，但如果原对象内部有引用类型的字段，**只是将引用复制到新对象中**，**两个对象指向的是同一个引用对象。**
- 深拷贝是指在复制对象的同时，将对象内部的所有引用类型字段的内容也复制一份，而不是共享引用。换句话说，深拷贝会递归复制对象内部所有引用类型的字段，**生成一个全新的对象以及其内部的所有对象。**

| 对比项   | 浅拷贝（Shallow Copy）           | 深拷贝（Deep Copy）             |
| -------- | -------------------------------- | ------------------------------- |
| 拷贝级别 | 拷贝对象本身+**引用地址**        | 拷贝对象本身+**引用对象的内容** |
| 引用对象 | 原对象与副本**共享引用对象**     | 副本拥有独立的引用对象          |
| 默认实现 | Object.clone() 默认是浅拷贝      | 需手动实现递归复制              |
| 影响     | 改变副本的引用成员，会影响原对象 | 改变副本不影响原对象            |
| 应用场景 | 轻量对象复制，性能优先           | 要求对象完全独立、互不干扰      |

## 2.实现深拷贝的方式

> 实现 Cloneable 接口并重写 clone() 方法

这种方法要求对象及其所有引用类型字段都实现 Cloneable 接口，并且重写 clone() 方法。在 clone() 方法中，通过递归克隆引用类型字段来实现深拷贝。

```java
class MyClass implements Cloneable {
    private String field1;
    private NestedClass nestedObject;

    @Override
    protected Object clone() throws CloneNotSupportedException {
        MyClass cloned = (MyClass) super.clone();
        cloned.nestedObject = (NestedClass) nestedObject.clone(); // 深拷贝内部的引用对象
        return cloned;
    }
}

class NestedClass implements Cloneable {
    private int nestedField;

    @Override
    protected Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
}
```

> 使用序列化和反序列化

通过将**对象序列化为字节流，再从字节流反序列化为对象来实现深拷贝**。要求对象及其所有引用类型字段都实现 **Serializable** 接口。

```java
import java.io.*;

class MyClass implements Serializable {
    private String field1;
    private NestedClass nestedObject;

    public MyClass deepCopy() {
        try {
            ByteArrayOutputStream bos = new ByteArrayOutputStream();
            ObjectOutputStream oos = new ObjectOutputStream(bos);
            oos.writeObject(this);
            oos.flush();
            oos.close();

            ByteArrayInputStream bis = new ByteArrayInputStream(bos.toByteArray());
            ObjectInputStream ois = new ObjectInputStream(bis);
            return (MyClass) ois.readObject();
        } catch (IOException | ClassNotFoundException e) {
            e.printStackTrace();
            return null;
        }
    }
}

class NestedClass implements Serializable {
    private int nestedField;
}
```

> 手动递归复制

针对特定对象结构，**手动递归复制对象及其引用类型字段**。适用于对象结构复杂度不高的情况。

```java
class MyClass {
    private String field1;
    private NestedClass nestedObject;

    public MyClass deepCopy() {
        MyClass copy = new MyClass();
        copy.setField1(this.field1);
        copy.setNestedObject(this.nestedObject.deepCopy());
        return copy;
    }
}

class NestedClass {
    private int nestedField;

    public NestedClass deepCopy() {
        NestedClass copy = new NestedClass();
        copy.setNestedField(this.nestedField);
        return copy;
    }
}
```

# 对象

## 1.创建对象的五种方式

**使用new关键字**：通过new关键字直接调用类的构造方法来创建对象。

**使用Class类的newInstance()方法**：通过反射机制，可以使用Class类的newInstance()方法创建对象。

**使用Constructor类的newInstance()方法**：同样是通过反射机制，可以使用Constructor类的newInstance()方法创建对象。一般跟反射一块用吧，获取类的构造器之后新建对象

**使用clone()方法**：如果类实现了**Cloneable**接口，可以使用clone()方法复制对象。

这就是**拷贝**

**使用反序列化**：通过将对象序列化到文件或流中，然后再进行反序列化来创建对象。

## 2.new出的对象什么时候回收

通过过关键字`new`创建的对象，由**Java的垃圾回收器**（Garbage Collector）负责回收。垃圾回收器的工作是在程序运行过程中自动进行的，它会周期性地检测不再被引用的对象，并将其回收释放内存。

具体来说，Java对象的回收时机是由垃圾回收器根据一些算法来决定的，主要有以下几种情况：

1. 引用计数法：某个对象的引用计数为0时，表示该对象不再被引用，可以被回收。
2. **可达性分析算法**：**从根对象（如方法区中的类静态属性、方法中的局部变量等）出发，通过对象之间的引用链进行遍历，**如果存在一条引用链到达某个对象，则说明该对象是可达的，反之不可达，不可达的对象将被回收。
3. **终结器**（Finalizer）：如果对象重写了`finalize()`方法，垃圾回收器会在回收该对象之前调用`finalize()`方法，对象可以在`finalize()`方法中进行一些清理操作。然而，终结器机制的使用不被推荐，因为它的执行时间是不确定的，可能会导致不可预测的性能问题。这个类似于try-catch-finally中的finally

## 3.如何获取私有对象？

在 Java 中，私有对象通常指的是类中被声明为 `private` 的成员变量或方法。由于 `private` 访问修饰符的限制，这些成员只能在其所在的类内部被访问。

不过，可以通过下面两种方式来间接获取私有对象。

- **使用公共访问器方法（getter 方法）**：如果类的设计者遵循良好的编程规范，通常会为私有成员变量提供公共的访问器方法（即 `getter` 方法），通过调用这些方法可以安全地获取私有对象。

```java
class MyClass {
    // 私有成员变量
    private String privateField = "私有字段的值";

    // 公共的 getter 方法
    public String getPrivateField() {
        return privateField;
    }
}

public class Main {
    public static void main(String[] args) {
        MyClass obj = new MyClass();
        // 通过调用 getter 方法获取私有对象
        String value = obj.getPrivateField();
        System.out.println(value); 
    }
}
```

- 反射机制。反射机制允许在运行时检查和修改类、方法、字段等信息，通**过反射可以绕过 `private` 访问修饰符的限制来获取私有对象。**但是却会破坏封装性，安全不会得到保障

```java
import java.lang.reflect.Field;

class MyClass {
    private String privateField = "私有字段的值";
}

public class Main {
    public static void main(String[] args) throws NoSuchFieldException, IllegalAccessException {
        MyClass obj = new MyClass();
        // 获取 Class 对象
        Class<?> clazz = obj.getClass();
        // 获取私有字段
        Field privateField = clazz.getDeclaredField("privateField");
        // 设置可访问性
        privateField.setAccessible(true);
        // 获取私有字段的值
        String value = (String) privateField.get(obj);
        System.out.println(value); 
    }
}
```

# 注解

## 1.能讲一讲Java注解的原理吗？

注解本质是一个**继承了Annotation的特殊接口**，其具体实现类**是Java运行时生成的动态代理类。**

我们通过反射获取注解时，返回的是Java运行时生成的动态代理对象。通过代理对象调用自定义注解的方法，会最终调用AnnotationInvocationHandler的**invoke方法。该方法会从memberValues这个Map中索引出对应的值。而memberValues的来源是Java常量池。**

## 2.对注解解析的底层实现了解吗？

注解本质上是一种**特殊的接口**，它继承自 `java.lang.annotation.Annotation` 接口，**所以注解也叫声明式接口**，例如，定义一个简单的注解：

```java
public @interface MyAnnotation {
    String value();
}
```

编译后，Java 编译器会将其转换为一个继承自 `Annotation` 的接口，并生成相应的字节码文件。

根据注解的作用范围，Java 注解可以分为以下几种类型：

- **源码级别注解** ：**仅存在于源码中**，编译后不会保留（`@Retention(RetentionPolicy.SOURCE)`）。
- **类文件级别注解** ：**保留在 `.class` 文件中，但运行时不可见**（`@Retention(RetentionPolicy.CLASS)`）。
- **运行时注解** ：保留在 `.class` 文件中，**并且可以通过反射在运行时访问**（`@Retention(RetentionPolicy.RUNTIME)`）。

只有运行时注解可以通过反射机制进行解析。

当注解被标记为 `RUNTIME` 时，Java 编译器会在生成的 `.class` 文件中保存注解信息。这些信息存储在字节码的属性表（Attribute Table）中，具体包括以下内容：

- **RuntimeVisibleAnnotations** ：存储运行时可见的注解信息。
- **RuntimeInvisibleAnnotations** ：存储运行时不可见的注解信息。
- **RuntimeVisibleParameterAnnotations** 和 **RuntimeInvisibleParameterAnnotations** ：存储方法参数上的注解信息。

通过工具（如 `javap -v`）可以查看 `.class` 文件中的注解信息。

注解的解析主要依赖于 Java 的反射机制。以下是解析注解的基本流程：

1、获取注册信息：通过反射 API 可以获取类、方法、字段等元素上的注解。例如：

```java
Class<?> clazz = MyClass.class;
MyAnnotation annotation = clazz.getAnnotation(MyAnnotation.class);
if (annotation != null) {
    System.out.println(annotation.value());
}
```

2、底层原理：反射机制的核心类是 `java.lang.reflect.AnnotatedElement`，它是所有可以被注解修饰的元素（如 `Class`、`Method`、`Field` 等）的父接口。该接口提供了以下方法：

- `getAnnotation(Class<T> annotationClass)`：获取指定类型的注解。
- `getAnnotations()`：获取所有注解。
- `isAnnotationPresent(Class<? extends Annotation> annotationClass)`：判断是否包含指定注解。

这些方法的底层实现依赖于 JVM 提供的本地方法（Native Method），例如：

- `native Annotation[] getDeclaredAnnotations0(boolean publicOnly);`
- `native <A extends Annotation> A getAnnotation(Class<A> annotationClass);`

JVM 在加载类时会解析 **`.class` 文件中的注解信息，并将其存储在内存中，供反射机制使用。**

因此，注解解析的底层实现主要**依赖于 Java 的反射机制和字节码文件的存储**。通过 `@Retention` 元注解可以控制注解的保留策略，当使用 `RetentionPolicy.RUNTIME` 时，可以在运行时通过**反射 API 来解析注解信息。在 JVM 层面，会从字节码文件中读取注解信息，并创建注解的代理对象来获取注解的属性值。**

## 3.Java注解的作用域呢？

注解的作用域（Scope）指的是注解可以应用在哪些程序元素上，例如类、方法、字段等。Java注解的作用域可以分为三种：

1. **类级别作用域**：用于描述类的注解，通常放置在类定义的上面，可以用来指定类的一些属性，**如类的访问级别、继承关系、注释等。**
2. **方法级别作用域**：用于描述方法的注解，通常放置在方法定义的上面，可以用来指定方法的一些属性，**如方法的访问级别、返回值类型、异常类型、注释等。**
3. **字段级别作用域**：用于描述字段的注解，通常放置在字段定义的上面，可以用来指定字段的一些属性，**如字段的访问级别、默认值、注释等。**

除了这三种作用域，Java还提供了其他一些注解作用域，例如**构造函数作用域和局部变量作用域**。这些注解作用域可以用来对构造函数和局部变量进行描述和注释。

# 异常

## 1.介绍一下Java异常

![](https://cdn.xiaolincoding.com//picgo/1720683900898-1d0ce69d-4b5d-41a6-a5df-022e42f8f4c5.webp)

Java的异常体系主要基于两大类：Throwable类及其子类。Throwable有两个重要的子类：Error和Exception，它们分别代表了不同类型的异常情况。

1. **Error（错误）**：表示运行时环境的错误。错误是程序无法处理的严重问题，如系统崩溃、虚拟机错误、动态链接失败等。通常，**程序不应该尝试捕获这类错误**。例如，OutOfMemoryError、StackOverflowError等。
2. **Exception（异常）**：表示程序本身可以处理的异常条件。异常分为两大类：
   - **非运行时异常**：这类异常**在编译时期就必须被捕获或者声明抛出**。它们通常是外部错误，如文件不存在（FileNotFoundException）、类未找到（ClassNotFoundException）等。非运行时异常强制程序员处理这些可能出现的问题，增强了程序的健壮性。
   - **运行时异常**：这类异常包括运行时异常（RuntimeException）和错误（Error）。运行时异常由程序错误导致，如空指针访问（NullPointerException）、数组越界（ArrayIndexOutOfBoundsException）等。**运行时异常是不需要在编译时强制捕获或声明的。**

## 2.Java异常处理有哪些？

异常处理是通过使用try-catch语句块来捕获和处理异常。以下是Java中常用的异常处理方式：

- **try-catch语句块**：用于捕获并处理可能抛出的异常。try块中包含可能抛出异常的代码，catch块用于捕获并处理特定类型的异常。可以有多个catch块来处理不同类型的异常。

```java
try {
    // 可能抛出异常的代码
} catch (ExceptionType1 e1) {
    // 处理异常类型1的逻辑
} catch (ExceptionType2 e2) {
    // 处理异常类型2的逻辑
} catch (ExceptionType3 e3) {
    // 处理异常类型3的逻辑
} finally {
    // 可选的finally块，用于定义无论是否发生异常都会执行的代码
}
```

- **throw语句**：用于手动抛出异常。可以根据需要在代码中使用throw语句主动抛出特定类型的异常。

```text
throw new ExceptionType("Exception message");
```

- **throws关键字**：用于在方法声明中声明可能抛出的异常类型。如果一个方法可能抛出异常，但不想在方法内部进行处理，可以使用throws关键字将异常传递给调用者来处理。

```java
public void methodName() throws ExceptionType {
    // 方法体
}
```

- **finally块**：用于定义无论是否发生异常都会执行的代码块。通常用于释放资源，确保资源的正确关闭。

```java
try {
    // 可能抛出异常的代码
} catch (ExceptionType e) {
    // 处理异常的逻辑
} finally {
    // 无论是否发生异常，都会执行的代码
```

## 3.抛出异常为什么不用throws？

如果异常是未检查异常或者在方法内部被捕获和处理了，那么就不需要使用throws。

- **Unchecked Exceptions**：**未检查异常（unchecked exceptions）是继承自RuntimeException类或Error类的异常，**编译器不强制要求进行异常处理。因此，对于这些异常，不需要在方法签名中使用throws来声明。示例包括NullPointerException、ArrayIndexOutOfBoundsException等。
- **捕获和处理异常**：另一种常见情况是，**在方法内部捕获了可能抛出的异常，并在方法内部处理它们，**而不是通过throws子句将它们传递到调用者。这种情况下，方法可以处理异常而无需在方法签名中使用throws。

# 序列化

## 1.怎么把一个对象从一个jvm转移到另一个jvm?

- **使用序列化和反序列化**：将对象序列化为字节流，并将其发送到另一个 JVM，然后在另一个 JVM 中反序列化字节流恢复对象。这可以通过 Java 的 ObjectOutputStream 和 ObjectInputStream 来实现。
- **使用消息传递机制**：利用消息传递机制，比如使用消息队列（如 RabbitMQ、Kafka）或者通过网络套接字进行通信，将对象从一个 JVM 发送到另一个。这需要自定义协议来序列化对象并在另一个 JVM 中反序列化。
- **使用远程方法调用（RPC）**：可以使用远程方法调用框架，如 gRPC，来实现对象在不同 JVM 之间的传输。远程方法调用可以让你在分布式系统中调用远程 JVM 上的对象的方法。
- **使用共享数据库或缓存**：将对象存储在共享数据库（如 MySQL、PostgreSQL）或共享缓存（如 Redis）中，让不同的 JVM 可以访问这些共享数据。这种方法适用于需要共享数据但不需要直接传输对象的场景。

## 2.序列化和反序列化让你自己实现你会怎么做?

Java 默认的序列化虽然实现方便，但却存在安全漏洞、不跨语言以及性能差等缺陷。

- **无法跨语言**： Java 序列化目前只适用基于 Java 语言实现的框架，其它语言大部分都没有使用 Java 的序列化框架，也没有实现 Java 序列化这套协议。因此，如果是两个基于不同语言编写的应用程序相互通信，则无法实现两个应用服务之间传输对象的序列化与反序列化。
- **容易被攻击**：Java 序列化是不安全的，我们知道对象是通过在 **ObjectInputStream 上调用 readObject() 方法进行反序列化的，**这个方法其实是一个神奇的构造器，它可以将类路径上几乎所有实现了 Serializable 接口的对象都实例化。这也就意味着，在反序列化字节流的过程中，**该方法可以执行任意类型的代码，这是非常危险的。**
- 序列化后的流太大：序列化后的二进制流大小能体现序列化的性能。**序列化后的二进制数组越大，占用的存储空间就越多，存储硬件的成本就越高。**如果我们是进行网络传输，则占用的带宽就更多，这时就会影响到系统的吞吐量。

我会考虑用主流序列化框架，比如FastJson、**Protobuf**来替代Java 序列化。

如果追求性能的话，Protobuf 序列化框架会比较合适，**Protobuf 的这种数据存储格式，不仅压缩存储数据的效果好， 在编码和解码的性能方面也很高效。Protobuf 的编码和解码过程结合.proto 文件格式，加上 Protocol Buffer 独特的编码格式，只需要简单的数据运算以及位移等操作就可以完成编码与解码。**可以说 Protobuf 的整体性能非常优秀。

## 3.将对象转为二进制字节流具体怎么实现?

在Java中通过序列化对象流来完成序列化和反序列化：

- ObjectOutputStream：通过writeObject(）方法做序列化操作。
- ObjectInputStrean：通过readObject()方法做反序列化操作。

只有实现了Serializable或Externalizable接口的类的对象才能被序列化，否则抛出异常！

实现对象序列化：

- 让类实现Serializable接口：

```java
import java.io.Serializable;

public class MyClass implements Serializable {
    // class code
}
```

- 创建输出流并写入对象：

```java
import java.io.FileOutputStream;
import java.io.ObjectOutputStream;

MyClass obj = new MyClass();
try {
    FileOutputStream fileOut = new FileOutputStream("object.ser");
    ObjectOutputStream out = new ObjectOutputStream(fileOut);
    out.writeObject(obj);
    out.close();
    fileOut.close();
} catch (IOException e) {
    e.printStackTrace();
}
```

实现对象反序列化：

- 创建输入流并读取对象：

```java
import java.io.FileInputStream;
import java.io.ObjectInputStream;

MyClass newObj = null;
try {
    FileInputStream fileIn = new FileInputStream("object.ser");
    ObjectInputStream in = new ObjectInputStream(fileIn);
    newObj = (MyClass) in.readObject();
    in.close();
    fileIn.close();
} catch (IOException | ClassNotFoundException e) {
    e.printStackTrace();
}
```

通过以上步骤，对象obj会被序列化并写入到文件"object.ser"中，然后通过反序列化操作，从文件中读取字节流并恢复为对象newObj。这种方式可以方便地将对象转换为字节流用于持久化存储、网络传输等操作。需要注意的是，要确保类实现了Serializable接口，并且所有成员变量都是Serializable的才能被正确序列化。

总结：

创建文件传输流，然后这边是inputstream流放入outputstream

然后关闭

创建文件inputstream，创建outputstream流，output流readObject然后关闭流

# 设计模式

## 单例

### 1.volatile和sychronized如何实现单例模式

```java
public class SingleTon {

    // volatile 关键字修饰变量 防止指令重排序
    private static volatile SingleTon instance = null;
    private SingleTon(){}
     
    public static  SingleTon getInstance(){
        if(instance == null){
            //同步代码块 只有在第一次获取对象的时候会执行到 ，第二次及以后访问时 instance变量均非null故不会往下执行了 直接返回啦
            synchronized(SingleTon.class){
                if(instance == null){
                    instance = new SingleTon();
                }
            }
        }
        return instance;
    }
}
```

正确的双重检查锁定模式需要需要使用 volatile。volatile主要包含两个功能。

- 保证可见性。使用 volatile 定义的变量，将会保证对所有线程的可见性。
- 禁止指令重排序优化。

由于 volatile 禁止对象创建时指令之间重排序，所以其他线程不会访问到一个未初始化的对象，从而保证安全性。

## 代理模式&适配器模式

### 1.代理模式和适配器模式有什么区别？

- **目的不同**：代理模式主要关注**控制对对象的访问**，而适配器模式则用于**接口转换**，使不兼容的类能够一起工作。
- **结构不同**：代理模式一般包含**抽象主题、真实主题和代理**三个角色，适配器模式包含**目标接口、适配器和被适配者**三个角色。
- **应用场景不同**：代理模式常用于**添加额外功能或控制对对象**的访问，适配器模式**常用于让不兼容的接口协同工作。**

# IO

## 1.**Java怎么实现网络IO高并发编程？**

可以用 Java NIO ，是一种同步非阻塞的I/O模型，也是I/O多路复用的基础。

传统的BIO里面socket.read()，**如果TCP RecvBuffer里没有数据，函数会一直阻塞**，直到收到数据，返回读到的数据， **如果使用BIO要想要并发处理多个客户端的i/o，那么会使用多线程模式**，一个线程专门处理一个客户端 io，这种模式随着客户端越来越多，所需要创建的线程也越来越多，会急剧消耗系统的性能。

NIO 是**基于I/O多路复用实现**的，它可以**只用一个线程处理多个客户端I/O**，如果你需要同时管理成千上万的连接，但是每个连接只发送少量数据，例如一个聊天服务器，用NIO实现会更好一些。这个类似redis中的单线程IO多路复用

## 2.BIO、NIO、AIO区别是什么？

- BIO（blocking IO）：就是传统的 java.io 包，它是**基于流模型实现**的，交互的方式是**同步、阻塞方式**，也就是说在读入输入流或者输出流时，在读写动作完成之前，线程会一直阻塞在那里，它们之间的调用是可靠的线性顺序。优点是代码比较简单、直观；缺点是 IO 的效率和扩展性很低，容易成为应用性能瓶颈。
- NIO（non-blocking IO） ：Java 1.4 引入的 java.nio 包，**提供了 Channel、Selector、Buffer 等新的抽象，可以构建多路复用的、同步非阻塞 IO 程序**，同时提供了更接近操作系统底层高性能的数据操作方式。
- AIO（Asynchronous IO） ：是 Java 1.7 之后引入的包，**是 NIO 的升级版本，提供了异步非堵塞的 IO 操作方式，**所以人们叫它 AIO（Asynchronous IO），异步 IO 是基于事件和回调机制实现的，也就是应用操作之后会直接返回，不会堵塞在那里，当后台处理完成，操作系统会通知相应的线程进行后续的操作。

## 3.NIO是怎么实现的？

NIO是一种**同步非阻塞的IO模型**，所以也可以叫NON-BLOCKINGIO。**同步是指线程不断轮询IO事件是否就绪，非阻塞是指线程在等待IO的时候，可以同时做其他任务。**

同步的核心就Selector（I/O多路复用），**Selector代替了线程本身轮询IO事件，避免了阻塞同时减少了不必要的线程消耗**；非阻塞的核心就是通道和缓冲区，**当IO事件就绪时，可以通过写到缓冲区，保证IO的成功，而无需线程阻塞式地等待。**

NIO由**一个专门的线程处理所有IO事件，并负责分发。事件驱动机制**，事件到来的时候触发操作，不需要阻塞的监视事件。线程之间通过**wait,notify**通信，减少线程切换。

NIO主要有三大核心部分：**Channel(通道)，Buffer(缓冲区), Selector。**传统IO基于字节流和字符流进行操作，**而NIO基于Channel和Buffer(缓冲区)进行操作，数据总是从通道读取到缓冲区中，或者从缓冲区写入到通道中。**

Selector(选择区)用于**监听多个通道的事件**（比如：连接打开，数据到达）。因此，**单个线程可以监听多个数据通道。**



