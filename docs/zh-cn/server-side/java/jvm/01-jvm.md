### Java JVM <!-- {docsify-ignore} -->

**文档更新日期: {docsify-updated}**

---

#### 1.JVM 结构

JVM（hotspot）结构概览如下图所示：

![](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/agTr0m.png)

上图中，灰色部分（Java 栈，本地方法栈和程序计数器）是线程私有，不存在线程安全问题，橙色部分（方法区和堆）为线程共享区。

#### 2.类加载器

**类加载器**(Class Loader)负责加载 class 文件，class 文件在文件开头有特定的标识。

类加载器将 class 文件字节码内容加载到内存中，并将这些内容转换成**方法区**中的运行时数据结构。

ClassLoader 只负责 class 文件的加载，至于它是否可以运行，则由执行引擎 Execution Engine 决定。

类加载示意图：

![](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/8u3DGN.png)

类加载器识别的 class 文件除了是**.class**格式外，文件的开头还得有特殊的标识，使用文本编辑器打开一个 class 格式的文件：

```
cafe babe 0000 0034 0010 0a00 0300 0d07
000e 0700 0f01 0006 3c69 6e69 743e 0100
0328 2956 0100 0443 6f64 6501 000f 4c69
6e65 4e75 6d62 6572 5461 626c 6501 0012
4c6f 6361 6c56 6172 6961 626c 6554 6162
6c65 0100 0474 6869 7301 0014 4c63 632f
6d72 6269 7264 2f63 6173 2f54 6573 743b
0100 0a53 6f75 7263 6546 696c 6501 0009
5465 7374 2e6a 6176 610c 0004 0005 0100
1263 632f 6d72 6269 7264 2f63 6173 2f54
6573 7401 0010 6a61 7661 2f6c 616e 672f
4f62 6a65 6374 0021 0002 0003 0000 0000
0001 0001 0004 0005 0001 0006 0000 002f
0001 0001 0000 0005 2ab7 0001 b100 0000
0200 0700 0000 0600 0100 0000 0300 0800
0000 0c00 0100 0000 0500 0900 0a00 0000
0100 0b00 0000 0200 0c
```

这个特定的标识就是十六进制字符**cafe babe**。

##### 2.1.类加载器分类

?> 类加载器分为 4 种：

<!-- tabs:start -->

##### **启动类加载器**

启动类加载器 BootstrapClassLoader 也叫根加载器，是虚拟机自带的加载器，底层由 C++实现，用于加载`$JAVA_HOME/jre/lib/rt.jar`包内的 class 文件。

?> rt.jar 是 Java 基础类库，包含 Java 运行环境所需的基础类

##### **拓展类加载器**

拓展类加载器 ExtClassLoader 是虚拟机自带的加载器，由 Java 语言实现，用于加载$JAVA_HOME/jre/lib/ext/\*\*.jar 目录下的 class 文件：

![](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/BvDI7d.png)

?> 这部分主要是 Java 在迭代过程中，一些拓展的功能。

```java
public class Test {
    public static void main(String[] args) {
        ZipInfo zipInfo = new ZipInfo();
        System.out.println(zipInfo.getClass().getClassLoader());
    }
}
```

`ZipInfo`是`$JAVA_HOME/jre/lib/ext/zipfs.jar`包里的一个类，程序运行结果如下：

```
sun.misc.Launcher$ExtClassLoader@5e2de80c
```

##### **应用程序类加载器**

应用程序类加载器 AppClassLoader 是虚拟机自带的加载器，用于加载当前应用的 classpath 的所有类，也就是我们自己写的那些 Java 代码

```java
public class Test {
    public static void main(String[] args) {
        System.out.println(Test.class.getClassLoader());
    }
}
```

程序运行结果：

```
sun.misc.Launcher$AppClassLoader@18b4aac2
```

##### **用户自定义加载器**

除了使用上面三种 JVM 自带的类加载器外，我们也可以通过继承 Java.lang.ClassLoader 抽象类自定义一个类加载器。

**这四种类加载器的关系如下图所示**：

![](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/Uh4yr6.png)

它们的关系是一种父子关系，我们可以通过代码验证：

```java
public class Test {
    public static void main(String[] args) {
        Class<Test> testClass = Test.class;
        System.out.println(testClass.getClassLoader());
        System.out.println(testClass.getClassLoader().getParent());
        System.out.println(testClass.getClassLoader().getParent().getParent());
    }
}
```

程序运行结果如下所示：

```
sun.misc.Launcher$AppClassLoader@18b4aac2
sun.misc.Launcher$ExtClassLoader@61bbe9ba
null
```

<!-- tabs:end -->

##### 2.2.类加载步骤

?> 类的加载过程分为三个步骤：

<!-- tabs:start -->

##### **加载 Loading**

通过一个类的全类名获取其二进制字节流，将这个二进制流代表的静态存储结构转化为方法区的运行时数据结构，然后在内存中生成一个代表这个类的 java.lang.Class 对象，作为方法区中这个类的各种数据的访问入口。

##### **链接 Linking**

?> 该过程又可以分为三个阶段：**验证 Verfication**，**准备 Preparation**和**解析 Resolution**）。

- 验证阶段用于确保加载的 Class 文件的字节流包含的信息是否符合虚拟机要求，保证其正确性合法性；
- 准备阶段为类变量（static 修饰的变量）分配内存并根据对象类型设置相应的默认初始值（比如 int 类型为 0，Integer 类型为 null）。 这里不包含常量，因为常量在编译的时候分配，准备阶段会显示初始化。 类的实例变量不会在这个阶段准备初始化。
- 解析阶段用于将符号引用转换为直接引用。

```java
public class Test {
    public static void main(String[] args) {
        String str = "hello";
        System.out.println(str);
    }
}
```

![](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/3AuDFk.png)

?> 解析阶段就是将其解析为直接引用的过程。

##### **初始化 Initialization**

1. **该阶段就是执行类的构造器方法<clinit>()的过程**

该方法并不是类的构造器，不需要我们自己定义，是 javac 编译器自动搜集类中的所有类变量的赋值动作和静态代码块中的语句合并而来；

```java
package com.xujiajun.loader;

public class Test2 {
    private static int aaa = 1;
    static {
        aaa = 200;
    }
    public static void main(String[] args) {
        System.out.println(aaa);
    }
}

```

然后通过 IDEA 的 jclasslib 插件查看该类的 class 文件对应的字节码：

![](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/j7iVHe.png)

可以看到上面所说的构造器方法<clinit>()，指令的操作就是为所有类变量赋值以及静态代码块中的操作。

!> 换句话说，如果一个类不包含类变量和静态代码块，那么它的字节码中就不会有构造器方法<clinit>()

2. **构造器方法中的指令按照语句在源代码中出现的顺序执行**

```java
package com.xujiajun.loader;

public class Test2 {
    private static int aaa = 1;
    static {
        aaa = 200;
        bbb = 300;
    }

    private static int bbb = 3;

    public static void main(String[] args) {
        System.out.println(bbb);
    }
}

```

上面程序输出结果为 2，因为构造器方法中的指令按照语句在源代码中出现的顺序执行，查看构造器方法<clinit>()指令来证明这一点：

![](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/bsy4Sr.png)

这里还有一个细节，就是为什么在静态代码块下面才定义的类变量 bbb，在静态代码块中可以进行修改呢？

这就是 Linking 阶段中准备阶段所做的事情，准备阶段为类变量（static 修饰的变量）分配内存并根据对象类型设置相应的默认初始值。

这个时候 bbb 已经被分配并赋予默认初始值了，所以 static 块中可以使用该变量（换句话说，实例变量不行）。

3. **若该类包含父类，那么 JVM 会保证父类的<clinit>()先执行完毕**
4. **虚拟机会保证一个类的<clinit>()方法在多线程下被同步加锁**

```java
public class Test {

    public static void main(String[] args) {
        Runnable r = () -> {
            System.out.println(Thread.currentThread().getName() + "开始");
            new Hello();
            System.out.println(Thread.currentThread().getName() + "结束");
        };
        new Thread(r, "线程1").start();
        new Thread(r, "线程2").start();
    }
}

class Hello {
    static {
        if (true) {
            System.out.println(Thread.currentThread().getName() + "初始化当前类");
            while (true) {
            }
        }
    }
}
```

程序启动后，main 线程启动两个子线程，控制台输出如下：

```
线程1开始
线程2开始
线程1初始化当前类
```

然后程序 block 住，这说明了虚拟机会保证一个类的`<clinit>()`方法在多线程下被同步加锁。

<!-- tabs:end -->

##### 2.3.双亲委派机制

聊到类加载器不得不提的另一个话题就是双亲委派机制，在了解什么是双亲委派机制之前，我们先来看个例子：

在 src/main/java 目录下新建 java.lang 包，然后在该包下新建一个 String 类：

![](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/e3vlVX.png)

String 类的代码如下所示：

```java
package java.lang;

public class String {
    public static void main(String[] args) {
        System.out.println("helo");
    }
}
```

程序输出结果：

```shell
错误: 在类 java.lang.String 中找不到 main 方法, 请将 main 方法定义为:
   public static void main(String[] args)
否则 JavaFX 应用程序类必须扩展javafx.application.Application
```

?> 所谓的双亲委派机制就是：当一个类收到了类加载请求，他首先不会尝试自己去加载这个类，而是把这个请求委派给父类去完成，每一个层次类加载器都是如此。只有当父类加载器反馈自己无法完成这个请求的时候（在它的加载路径下没有找到所需加载的 Class），子类加载器才会尝试自己去加载。

所以上面的例子中，AppClassLoader 委派给它的父类 ExtClassLoader 去加载，ExtClassLoader 又委托给它的父类 BootstrapClassLoader 去加载。BootstrapClassLoader 从它的加载路径`$JAVA_HOME/jre/lib/rt.jar`下找到了`java.lang.String`类，即 rt.jar 包下的 String 类，而该类里并没有 main 方法，所以便抛出了如上异常。

?> 采用双亲委派的一个好处是：就如上面所说，不管是哪个加载器加载这个类，最终都是委托给顶层的启动类加载器进行加载，这样就保证了使用不同的类加载器最终得到的都是同样一个 String 对象，所以我们自定义的 Java 类并不会污染 JDK 自带的那些类（即使全类名一样），这种保护机制也叫**沙箱安全机制**。

#### 3.程序计数器

程序计数器(Program Counter Register)又叫 PC 寄存器。

每个线程都有一个程序计数器，是线程私有的。

它是一个指针，指向方法区中的方法字节码，用来存储指向下一条指令的地址，也即将要执行的指令代码，由执行引擎读取下一条指令，是一个非常小的内存空间，几乎可以忽略不记。

![](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/YNhGMA.png)

?> 如果执行的是一个 Native 方法，那这个计数器的值为 undefied。

?> 为什么需要程序计数器呢？因为 CPU 需要不停地切换各个线程，有了程序计数器后，当 CPU 切换回来后，我们就可以知道接着从哪开始继续执行程序

```java
package com.xujiajun.count;

public class Test {
    public static void main(String[] args) {
        int a = 1;
        int b = 2;
        int c = a + b;
        System.out.println(c);
    }
}

```

![](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/8XIe1k.png)

假如当前线程的程序计数器存储的指令地址为 6，这时候 CPU 切换到别的线程中处理工作；一段时间后，当前线程重新获取了 CPU 时间片继续执行时，根据程序计数器存的 6 就知道，当前需要执行 iadd（即 a+b 操作）指令。执行引擎会将这条指令翻译为机器指令，然后 CPU 执行该运算操作。

#### 4.虚拟机栈（Java 栈）

虚拟机栈也称为 Java 栈，每个线程在创建的时候都会创建一个虚拟机栈，其内部保存一个个栈帧（Stack Frame），对应着一次次的 Java 方法调用。

和 PC 寄存器一样，虚拟机栈的生命周期和线程一致。

虚拟机栈主管 Java 程序的运行，它保存方法的局部变量（8 种基本数据类型，对象引用地址）、部分结果，并参与方法的调用和返回。

![](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/sW06rG.png)

?> JVM 对虚拟机栈的操作只有压栈（入栈）和出栈操作，遵循 FILO 原则；

在一个活动线程中，一个时间点只会有一个活动的栈帧，即当前正在执行方法对应的栈帧（当前栈帧）；

如果一个方法调用了另一个方法，那么对应的新的栈帧将会被创建出来，放在栈顶，成为新的当前栈帧。

```java
package com.xujiajun.stack;

public class Test {
    public static void main(String[] args) {
        new Test().method1();
    }
    void method1() {
        System.out.println("方法1:开始");
        method2();
        System.out.println("方法1:结束");
    }
    void method2() {
        System.out.println("方法2:开始");
        method3();
        System.out.println("方法2:结束");
    }
    void method3() {
        System.out.println("方法3:开始");
        method4();
        System.out.println("方法3:结束");
    }
    void method4() {
        System.out.println("方法4:开始");
        System.out.println("方法4:结束");
    }
}

```

!> Java 方法执行结束正常退出和抛出异常这两种情况会导致栈帧被弹出（退出）

##### 4.1.虚拟机栈大小调整

Java 虚拟机规范允许虚拟机栈的大小固定不变或者动态扩展。

- 固定情况下：如果线程请求分配的栈容量超过 Java 虚拟机允许的最大容量，则抛出 StackOverflowError 异常；
- 可动态扩展情况下：尝试扩展的时候无法申请到足够的内存；或者在创建新的线程的时候没有足够的内存去创建对应的虚拟机栈，则会抛出 OutOfMemoryError 异常。

不同平台的虚拟机栈默认大小不同：

- Linux/x64 (64-bit): 1024 KB
- macOS (64-bit): 1024 KB
- Oracle Solaris/x64 (64-bit): 1024 KB
- Windows: 默认值取决于虚拟内存。

我们可以通过`-Xss`（`-XX:ThreadStackSize`简写）设置虚拟机栈大小，默认单位为字节。也可以通过 k 或者 K 指定单位为 KB，m 或 M 指定单位为 MB，g 或 G 指定单位为 GB。

下面这组配置都是将虚拟机栈大小设置为 1024KB：

```bash
-Xss1m
-Xss1024k
-Xss1048576
```

虚拟机栈越大，方法调用深度越深.

举个例子：

```java
package com.xujiajun.stack;

public class Test2 {
    private static int count = 1;
    public static void main(String[] args) {
        System.out.println(count);
        count++;
        main(args);
    }
}

```

![](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/UkQMwW.png)

我们通过-Xss200k 命令将虚拟机栈大小调整为 200KB 再观察输出结果：

```
......
1214
1215
1216
1217
1218
1219
*** java.lang.instrument ASSERTION FAILED ***: "!errorOutstanding" with message transform method call failed at JPLISAgent.c line: 844
*** java.lang.instrument ASSERTION FAILED ***: "!errorOutstanding" with message transform method call failed at JPLISAgent.c line: 844
*** java.lang.instrument ASSERTION FAILED ***: "!errorOutstanding" with message transform method call failed at JPLISAgent.c line: 844
Exception in thread "main" java.lang.StackOverflowError
```

可以看到，方法调用深度明显变小了。

##### 4.2.栈帧内部结构

每个栈帧包含 5 个组成部分：

- 局部变量表（Local Variables）
- 操作数栈（Operand Stack）
- 动态链接（Dynamic Linking）
- 方法返回地址（Return Address）
- 一些附加信息：

![](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/sT5scC.png)

<!-- tabs:start -->

##### **局部变量表**

局部变量表是一个数字数组，用于存储方法参数和方法体内的局部变量。

![](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/5CHtL1.png)

非静态方法的局部变量表和静态方法相比，多了个 this 对象（即当前类）：

![](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/BEswwh.png)

?> 可以看到，非静态方法的局部变量表首位就存放了 this 对象，这也是静态方法内无法使用 this 的原因（因为静态方法的局部变量表中没有 this 对象）。

局部变量表数组容量的大小在编译期就可以唯一确定下来，并保存在方法的 Code 属性的`maximum locacl variables`数据项中:

![](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/J2y0sS.png)

!> 局部变量表的大小为 3，编译期就已经确定，不会随着方法的运行而改变！

局部变量表的最基本单元是变量槽（Slot）。

局部变量表中 32 位以内的数据类型（除 long 和 double 外）只占用一个 slot，64 位类型（long 和 double）占用两个 slot。

举个例子：
`java public class Test3 { public void hello(String name) { Date date = new Date(); long number = 200L; double sala = 5500.4; int count = 1; } } `

![](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/1engBt.png)

**通过局部变量表包含的信息，我们还可以得出局部变量的作用范围**

```java
package com.xujiajun.stack;

public class Test3 {
    public static void hello(String name) {
        int count = 1;
    }
}

```

![](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/5xqzEq.png)

以方法参数 name 为例，查看 LocalVariableTable，name 参数对应的 Start 列的值为 0，表示其在第 0 行字节码指令处生效（通过 LineNumberTable 我们可以知道，第 0 行字节码指令对应程序中的第 6 行代码）；Length 列的值为 3，说明 name 参数的有效作用域长度为 3，因为 name 是在第 0 行字节码指令处生效的，所以 name 在 0 ~ 2 行字节码指令范围内有效（通过 LineNumberTable 的对应关系，我们也可以知道 name 在我们的代码中作用域范围为第 6 行到第 7 行）。

**局部变量表的槽位是可以重复利用的**

如果一个局部变量过了其作用域，那么在其作用域之后申明的新的局部变量很有可能会复用过期局部变量的槽位

```java
package com.xujiajun.stack;

public class Test3 {
    public static void hello(String name) {
        {
            int a = 1;
            System.out.println(a);
        }
        int b = 2;
    }
}

```

![](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/0DBtZH.png)

可以看到局部变量 a 和 b 的槽位都是 1，说明槽位重复利用了。这是因为在定义局部变量 b 的时候，局部变量 a 已经出了作用域失效销毁了，但是局部变量表的槽位已经开辟了，所以局部变量 b 直接重复利用索引为 1 的槽位。

##### **操作数栈**

每一个独立的栈帧中除了包含局部变量表外，还包含一个 FILO 的操作数栈，用于保存计算过程中的中间结果，同时作为计算过程中变量临时的存储空间。每个操作数栈都有一个明确的深度，在编译期已经确定下来：

?> 栈中的任何一个元素都可以是任意的 Java 数据类型，32bit 的类型占用一个栈深度，64bit 的类型占用两个栈单位深度

?> 操作数栈在方法的执行过程中，根据字节码指令往栈中写入数据或提取数据，即入栈和出栈操作。虽然栈是用数组实现的，但根据栈的特性，对栈中数据访问不能通过索引，而是只能通过标准的入栈和出栈操作来完成一次数据访问。

下面通过一个例子来感受 PC 寄存器，局部变量表和操作数栈是如何相互配合完成一次方法的执行，代码如下所示：

```java
package com.xujiajun.stack;

public class Test4 {
    public void add() {
        int a = 15;
        int b = 1;
        int c = a + b;
    }
}

```

在查看字节码指令之前，先记录下几个入栈出栈的字节码指令含义：

- 当 int 取值 -1 ~ 5 采用 iconst 指令入栈；
- 取值 -128 ~ 127（byte 有效范围）采用 bipush 指令入栈；
- 取值 -32768 ~ 32767（short 有效范围）采用 sipush 指令入栈；
- 取值 -2147483648 ~ 2147483647（int 有效范围）采用 ldc 指令入栈；
- istore，栈顶元素出栈，保存到局部变量表中；
- iload，从局部变量表中加载数据入栈。

![](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/TcSFCU.png)

指令执行过程中，PC 寄存器，局部变量表和操作数栈状态如下图所示：

![](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/BGviRx.png)

!> 如果被调用的方法带有返回值的话，其返回值会被压入当前栈帧的操作数栈中

##### **动态链接**

在 Java 源文件被编译成字节码文件时，所有的变量和方法引用都作为符号引用保存在 class 文件的常量池里，动态链接的作用就是为了将这些符号引用转换为调用方法的直接引用

##### **方法返回地址**

存放调用该方法的 pc 寄存器的值。

一个方法的结束分为以下两种方式：

- 正常执行结束；
- 出现未处理异常，非正常退出。

?> 无论是哪种方式退出，在方法退出后都返回到该方法被调用的位置

?> 方法正常退出时，调用者的 pc 寄存器的值作为返回地址，即调用该方法的指令的下一条指令地址；异常退出时，返回地址需要通过异常表来确定。

##### **一些附加信息**

比如对程序调式提供的支持信息。

<!-- tabs:end -->

#### 5.本地方法接口

本地方法接口(Native Interface)的作用是融合不同的编程语言为 Java 所用，它的初衷是融合 C/C++程序。

Java 诞生的时候是 C/C++横行的时候，要想立足，必须调用 C/C++程序，于是就在内存中专门开辟了一块区域处理标记为 native 的代码。

比如查看 java.lang.Thread 类源码就会发现当中存在许多 native 方法：

![](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/2w5cRq.png)

native 方法没有方法体（因为不是 Java 实现），所以看上去像是“接口”一样，故得名本地方法接口。

#### 6.本地方法栈

如前所述，虚拟机栈用于管理 Java 方法的调用，而本地方法栈则是用于管理本地方法的调用。

#### 7.堆（Heap）

堆（Heap）一个 JVM 实例只存在一个堆内存，堆内存的大小是可以调节的。

堆中保存着所有引用类型的真实信息，以方便执行器执行。

堆在逻辑上分为三个区域：

<!-- tabs:start -->

##### **Java 8**

![](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/Un47N8.png)

在 Java7 时代，堆分为:

- 新生区:
  - 伊甸园区
  - 幸存区:
    - 幸存者 0 区(又称为 From 区)
    - 幸存者 1 区(又称为 To 区)
      **From 区和 To 区并不是固定的，复制之后交互，谁空谁是 To**
- 养老区
- 永久代；

?> 在 Java8 中，永久代已经被移除，被一个称为元空间的区域所取代。元空间的本质和永久代类似。

元空间与永久代之间最大的区别在于：永久代使用的 JVM 的堆内存，但是 java8 以后的元空间并不在虚拟机中而是使用本机物理内存（所以在上图中，我用虚线表示）。

?> 堆之所以要分区是因为：Java 程序中不同对象的生命周期不同，70%~99%对象都是临时对象，这类对象在新生区“朝生夕死”。<br>如果没有分区，GC 时搜集垃圾需要对整个堆内存进行扫描；<br>分区后，回收这些“朝生夕死”的对象，只需要在小范围的区域中（新生区）搜集垃圾。<br>所以，分区的唯一理由就是为了优化 GC 性能。

##### **Java 7**

![](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/1bKbC3.png)

<!-- tabs:end -->

##### 7.1.堆空间对象分配过程

下面通过一个例子来讲述这几个区的交互逻辑：

1. 几乎任何新的对象都是在伊甸园区被 new 出来创建，刚开始的时候两个幸存者区和养老区都是空的：

![](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/vEBJTw.png)

2. 随着对象的不断创建，伊甸园区空间逐渐被填满：

![](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/w2dQ8v.png)

3. 这时候将触发一次 Minor GC（Young GC），删除未引用的对象，GC 剩下来的还存在引用的对象将移动到幸存者 0 区，然后清空伊甸园区：

![](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/9VCGNB.png)

4. 随着对象的创建，伊甸园区空间又满了，再一次触发 Minor GC，删除未引用的对象，留下存在引用的对象。这次和上一次 Minor GC 有些不同，这轮 GC 留下的对象将被移动到幸存者 1 区，并且上一轮 GC 留下来的存储在幸存者 0 区的对象年龄递增并移动到幸存者 1 区。当所有幸存对象都移动到幸存者 1 区后，幸存者 0 区和伊甸园区空间清除：

![](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/3mtVU6.png)

5. 随着对象的创建伊甸园区空间再一次满了，触发了第三次 Minor GC，这一次幸存区空间将发生互换，GC 留下来的幸存者将移动到幸存者 0 区，幸存者 1 区的幸存对象年龄递增后也移动到幸存者 0 区，然后伊甸园区和幸存者 1 区的空间被清除：

![](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/DoqZjB.png)

6. 随着 Minor GC 的不断发生，幸存对象在两个幸存区不断地交换存储，年龄也不断递增。如此反反复复之后，当幸存对象的年龄达到指定的阈值（这个例子中是 8，由 JVM 参数 MaxTenuringThreshold 决定）后，它们将被移动到养老区：
   ![](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/BPcHQA.png)

7. 随着上述过程的不断出现，当养老区快满时，将触发 Major GC（Full GC）进行养老区的内存清理。若养老区执行了 GC 之后发现依然无法进行对象的保存，就会产生 OOM 异常。

一个对象被放置到养老区除了它的年龄达到阈值外，以下几种情况也会使得该对象直接被放置到养老区：

1. 对象创建后，无法放置到伊甸园区（比如伊甸园区的大小为 10m，新的对象大小为 11m，伊甸园区不够放，触发 YGC。YGC 后伊甸园区被清空，但还是无法容下 11m 的“超大对象”，所以直接放置到养老区。当然如果养老区放置不下则会触发 FGC，FGC 后还放不下则 OOM）；
2. YGC 后，对象无法放置到幸存者 To 区也会直接晋升到养老区；
3. 如果幸存区中相同年龄的所有对象大小大于幸存区空间的一半，年龄大于或等于这些对象年龄的对象可以直接进入养老区，无需等到年龄阈值。

##### 7.2.堆参数

以 JDK1.8+HotSpot 为例，常用的可调整的堆参数有：

| 参数                            | 含义                                                                                                                                                                    |
| :------------------------------ | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| -Xms，等价于-XX:InitialHeapSize | 设置堆的初始内存大小，默认为物理内存的 1/64                                                                                                                             |
| -Xmx，等价于-XX:MaxHeapSize     | 设置堆的最大内存大小，默认为物理内存的 1/4                                                                                                                              |
| -XX:Newratio                    | 设置新生区和养老区的比例，比如值为 2（默认值），则养老区是新生区的 2 倍，即养老区占据堆内存的 2/3                                                                       |
| -XX:Surviorratio                | 设置伊甸园区和一个幸存区的比例，比如值为 8（默认值）则表示伊甸园区占新生区的 8/10（两个幸存区是一样大的，8:1:1）； 如果设置为 5，则比例为 5:1:1，即伊甸园区占新生区 5/7 |
| -Xmn                            | 设置堆新生区的内存大小（一般不使用）                                                                                                                                    |
| -XX:MaxTenuringThreshold        | 设置转入养老区的存活次数，默认值为 15                                                                                                                                   |
| -XX:+PrintFlagsInitial          | 查看所有参数的默认初始值                                                                                                                                                |
| -XX:+PrintFlagsFinal            | 查看所有参数的最终值（被我们修改后的值不再是默认初始值）                                                                                                                |

剩下所有可用参数可以查看 oracle 官方文档：https://docs.oracle.com/javase/8/docs/technotes/tools/unix/java.html。

!> **生产环境中，推荐将-Xms 和-Xmx 设置为一样大，因为这样做的话在 Java 垃圾回收清理完堆区后不需要重新计算堆区大小，从而提高性能**。

此外，要在程序中输出详细的 GC 处理日志，可以使用`-XX:+PrintGCDetails`。

```java
public class Test {
    public static void main(String[] args) {
        long maxMemory = Runtime.getRuntime().maxMemory();
        long totalMemory = Runtime.getRuntime().totalMemory();
        System.out.println("堆内存的初始值" + totalMemory / 1024 / 1024 + "mb");
        System.out.println("堆内存的最大值" + maxMemory / 1024 / 1024 + "mb");
    }
}
```

```
堆内存的初始值245mb
堆内存的最大值3641mb
```

可以通过 IDEA 调整堆的大小：

```shell
-Xms10m -Xmx10m -XX:+PrintGCDetails
```

![](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/61JSOl.png)

![](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/zoO7aY.png)

可以看到，PSYoungGen（新生区）的总内存大小为 2560k，ParOldGen（养老区）的总内存大小为 7168k，总和刚好是 9728K，这也说明了：Java8 后的堆物理上只分为新生区和养老区，Metaspace（元空间）不占用堆内存，而是直接使用物理内存。

!> 那为什么我们设置的堆内存大小是 10m（10240kb），控制台输出却只有 9728kb 呢？从上面的例子我们知道，幸存者区分为 0 区和 1 区，根据复制算法的特点，这两个区同一时刻总有一个区是空的，所以控制台输出的内存计算方式为：2048K(eden space)+512K(from space or to space)+7168K(ParOldGen)=9728K。9728K 再加一个幸存区的大小 512K 刚好是 10240K。

再举个 OOM 的例子，使用刚刚`-Xms10m -Xmx10m -XX:+PrintGCDetails`的设置，运行下面这段程序：

```java
public class OMMTest {
    public static void main(String[] args) {
        String value = "hello";
        while (true) {
            value += value + new Random().nextInt(1000000000) + new Random().nextInt(1000000000);
        }
    }
}
```

```shell
[GC (Allocation Failure) [PSYoungGen: 1941K->486K(2560K)] 1941K->654K(9728K), 0.0027871 secs] [Times: user=0.01 sys=0.00, real=0.00 secs]
[GC (Allocation Failure) [PSYoungGen: 2189K->368K(2560K)] 2357K->1075K(9728K), 0.0004010 secs] [Times: user=0.00 sys=0.00, real=0.00 secs]
[GC (Allocation Failure) [PSYoungGen: 1860K->503K(2560K)] 2568K->1723K(9728K), 0.0004052 secs] [Times: user=0.00 sys=0.00, real=0.00 secs]
[GC (Allocation Failure) [PSYoungGen: 2001K->272K(2560K)] 4661K->3651K(9728K), 0.0005188 secs] [Times: user=0.01 sys=0.00, real=0.00 secs]
[GC (Allocation Failure) [PSYoungGen: 1051K->240K(2560K)] 5870K->5058K(9728K), 0.0004505 secs] [Times: user=0.00 sys=0.00, real=0.00 secs]
[GC (Allocation Failure) [PSYoungGen: 240K->240K(1536K)] 5058K->5058K(8704K), 0.0002655 secs] [Times: user=0.00 sys=0.00, real=0.00 secs]
[Full GC (Allocation Failure) [PSYoungGen: 240K->0K(1536K)] [ParOldGen: 4818K->2516K(7168K)] 5058K->2516K(8704K), [Metaspace: 3143K->3143K(1056768K)], 0.0023909 secs] [Times: user=0.00 sys=0.00, real=0.00 secs]
[GC (Allocation Failure) [PSYoungGen: 43K->64K(2048K)] 6878K->6899K(9216K), 0.0003698 secs] [Times: user=0.00 sys=0.00, real=0.00 secs]
[Full GC (Ergonomics) [PSYoungGen: 64K->0K(2048K)] [ParOldGen: 6835K->1796K(7168K)] 6899K->1796K(9216K), [Metaspace: 3146K->3146K(1056768K)], 0.0023337 secs] [Times: user=0.00 sys=0.00, real=0.01 secs]
[GC (Allocation Failure) [PSYoungGen: 23K->64K(2048K)] 6138K->6179K(9216K), 0.0003372 secs] [Times: user=0.00 sys=0.00, real=0.00 secs]
[GC (Allocation Failure) [PSYoungGen: 64K->32K(2048K)] 6179K->6147K(9216K), 0.0002256 secs] [Times: user=0.00 sys=0.00, real=0.00 secs]
[Full GC (Allocation Failure) [PSYoungGen: 32K->0K(2048K)] [ParOldGen: 6115K->4675K(7168K)] 6147K->4675K(9216K), [Metaspace: 3146K->3146K(1056768K)], 0.0013695 secs] [Times: user=0.00 sys=0.00, real=0.00 secs]
[GC (Allocation Failure) [PSYoungGen: 0K->0K(2048K)] 4675K->4675K(9216K), 0.0002573 secs] [Times: user=0.00 sys=0.00, real=0.00 secs]
[Full GC (Allocation Failure) [PSYoungGen: 0K->0K(2048K)] [ParOldGen: 4675K->4656K(7168K)] 4675K->4656K(9216K), [Metaspace: 3146K->3146K(1056768K)], 0.0021235 secs] [Times: user=0.01 sys=0.00, real=0.00 secs]
Heap
 PSYoungGen      total 2048K, used 59K [0x00000007bfd00000, 0x00000007c0000000, 0x00000007c0000000)
  eden space 1024K, 5% used [0x00000007bfd00000,0x00000007bfd0ef70,0x00000007bfe00000)
  from space 1024K, 0% used [0x00000007bff00000,0x00000007bff00000,0x00000007c0000000)
  to   space 1024K, 0% used [0x00000007bfe00000,0x00000007bfe00000,0x00000007bff00000)
 ParOldGen       total 7168K, used 4656K [0x00000007bf600000, 0x00000007bfd00000, 0x00000007bfd00000)
  object space 7168K, 64% used [0x00000007bf600000,0x00000007bfa8c268,0x00000007bfd00000)
 Metaspace       used 3187K, capacity 4496K, committed 4864K, reserved 1056768K
  class space    used 351K, capacity 388K, committed 512K, reserved 1048576K
Exception in thread "main" java.lang.OutOfMemoryError: Java heap space
	at java.util.Arrays.copyOf(Arrays.java:3332)
	at java.lang.AbstractStringBuilder.ensureCapacityInternal(AbstractStringBuilder.java:124)
	at java.lang.AbstractStringBuilder.append(AbstractStringBuilder.java:674)
	at java.lang.StringBuilder.append(StringBuilder.java:208)
	at com.xujiajun.heep.OMMTest.main(OMMTest.java:14)

进程已结束,退出代码1

```

可以看到，经过数次的 GC 和 Full GC 后，堆内存还是无法腾出空间，最终抛出 OOM 错误。

日志的含义如下图所示：

<!-- tabs:start -->

##### **Young GC（Minor GC）：**

![](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/1isrRi.png)

##### **Full GC（Major GC）：**

![](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/C2jhhQ.png)

<!-- tabs:end -->

##### 7.3.TLAB

JVM 对伊甸园区继续进行划分，为每个线程分配了一个私有缓存区域，这块区域就是 TLAB（Thread Local Allocation Buffer）。

多线程同时分配内存时，使用 TLAB 可以避免一系列非线程安全问题，同时还能够提升内存分配的吞吐量。

尽管不是所有的对象实例都能够在 TLAB 中成功分配内存，但 JVM 确实是将 TLAB 作为内存分配的首选：

![](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/Xqznyc.png)
![](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/xJj8ui.png)

使用`-XX:UseTLAB`设置是否开启 TLAB

```java
public class Test2 {

    public static void main(String[] args) throws InterruptedException {
        TimeUnit.SECONDS.sleep(100);
    }
}
```

![](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/0PMS8I.png)

可以看到 TLAB 默认是开启的。

TLAB 空间的内存非常小，仅占整个伊甸园区的 1%，可以通过`-XX:TLABWasteTargetPercent`设置 TLAB 空间所占用伊甸园区空间的百分比。

!> 有了 TLAB 的概念后，我们就不能说堆空间一定是线程共享的了。

#### 8.方法区

**方法区**并不是所谓的~~存储方法的区域~~，而是供各线程共享的运行时内存区域。

它存储了已被虚拟机加载的类型信息、常量、静态变量、即时编译器编译后的代码缓存等。

方法区也是**一种规范**，在不同虚拟机里头实现是不一样的，最典型的实现就是 HotSpot 虚拟机 Java8 之前的永久代(PermGen space)和 Java8 的元空间(Metaspace)。

##### 8.1.设置方法区大小

方法区的大小决定了系统可以加载多少个类，如果系统定义了太多的类，导致方法区溢出，虚拟机则会抛出`java.lang.OutOfMemoryError: PermGen space（Java 7）`或者`java.lang.OutOfMemoryError: Metaspace（Java 8）`内存溢出错误。

以 Java8 版本为例，我们可以使用`-XX:MetaspaceSize=size`设置元空间初始大小，`-XX:MaxMetaspaceSize=size`设置元空间最大值。

默认情况下，在 windows 平台上，`-XX:MetaspaceSize`值为 21M，`-XX:MaxMetaspaceSize`值为-1，即没有限制，所以极端情况下如果不断地加载类，虚拟机会耗尽所有可用的系统内存。

元空间 OOM 的例子

```java
package com.xujiajun.meta;

import jdk.internal.org.objectweb.asm.ClassWriter;
import jdk.internal.org.objectweb.asm.Opcodes;

public class Test extends ClassLoader {
    public static void main(String[] args) {
        int count = 0;
        try {
            Test test = new Test();
            for (int i = 0; i < 10000; i++) {
                String className = "Class" + i;
                // 创建ClassWriter对象，用于生成类的二进制字节码
                ClassWriter classWriter = new ClassWriter(0);
                // 指定版本号、修饰符、类名、包名、父类和接口
                classWriter.visit(Opcodes.V1_8, Opcodes.ACC_PUBLIC, className, null, "java/lang/Object", null);
                byte[] bytes = classWriter.toByteArray();
                // 加载类
                test.defineClass(className, bytes, 0, bytes.length);
                count++;
            }
        } finally {
            System.out.println(count);
        }
    }
}

```

上面例子中，我们尝试加载 10000 个类，通过参数`-XX:MetaspaceSize=10m -XX:MaxMetaspaceSize=10m`将元空间大小设置为固定大小 10M，运行上面的程序控制台输出：

![](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/kiU8Gn.png)

##### 8.2.方法区、堆、栈关系

方法区和堆、栈的关系如下图所示：

```java
public class Bird {

    public static void main(String[] args) {
        Bird bird = new Bird();
    }
}
```

![](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/PbLBjb.png)

![](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/jm9a7M.png)

##### 8.3.方法区内部结构

方法区内部主要存储了以下内容（不同 JDK 版本内容有所不同，具体参考下面“方法区演进”）：

###### 8.3.1.类型信息

对每个加载的类型（类 class、接口 interface、枚举 enum、注解 annotation），JVM 必须在方法区中存储以下类型信息：

1. 这个类型的完整有效名称（包名.类名）；
2. 这个类型直接父类的完整有效名（interface 和 java.lang.Object 没有父类）；
3. 这个类的修饰符（public，abstract，final）；
4. 这个类型直接接口的一个有序列表（一个类可以实现多个接口）。

###### 8.3.2.方法信息

方法信息包含了这个类的所有方法信息（包括构造器），这些信息和其声明顺序一致：

1. 方法名称；
2. 方法的返回值类型（没有返回值则是 void）；
3. 方法参数的数量和类型（有序）；
4. 方法的修饰符（public，private，protected，static，final，synchronized，native，abstract）；
5. 方法的字节码、操作数栈、局部变量表及其大小（abstract 和 native 方法除外）；
6. 异常表（abstract 和 native 方法除外）。

###### 8.3.3.域信息

域 Field 我们也常称为属性，字段。域信息包含：

1. 域的声明顺序；
2. 域的相关信息，包括名称、类型、修饰符（public，private，protected，static，final，volatile，transient）。

###### 8.3.4.JIT 代码缓存

这部分在 👇 执行引擎中再做说明。

###### 8.3.5.运行时常量池

在上面虚拟机栈的介绍中，我们知道类字节码反编译后，会有一个 constant pool 的结构，俗称为常量池，所有的变量和方法引用都作为符号引用保存在 class 文件的常量池里。虚拟机栈的动态链接就是将符号引用（这些符号引用的集合就是常量池）转换为直接引用（符号引用对应的具体信息，这些具体信息的集合就是运行时常量池，存在方法区中）的过程。

###### 8.3.6.静态变量

静态变量就是使用 static 修饰的域信息。静态变量和类关联在一起，随着类的加载而加载，它们成为类数据在逻辑上的一部分。静态变量也成为类变量，类变量被类的所有实例共享，即使没有类实例时你也可以访问它：

```java
public class Test {

    private static String hello = "hello";

    private static void hello() {
        System.out.println("hello");
    }

    public static void main(String[] args) {
        Test test = null;
        test.hello();
        System.out.println(test.hello);
    }
}
```

上面程序运行并不会报空指针异常。

通过 final 修饰的静态变量我们俗称常量。常量在编译的时候就会被分配具体值：

```java
public class Test {

    private static String hello = "hello";
    private static final String HELLO = "hello";
}
```

通过`javap -v -p Test.class`查看其字节码：

![](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/jyIgoZ.png)

通过上面的学习我们知道，静态变量（类变量）在类加载过程的初始化阶段才会被赋值。

###### 8.3.7.演示方法区内部结构

```java
public class Test extends Object implements Cloneable, Serializable {

    private static String hello = "hello";
    private static final String HELLO = "hello";
    public int a = 0;

    public void method1() {
        System.out.println("method1");
    }

    public static String method2(String name) {
        try {
            int a = 1;
            int b = a / 0;
        } catch (Exception e) {
            e.printStackTrace();
        }
        return name;
    }
}
```

```shell
xujiajun@bogon meta % clear
xujiajun@bogon meta % javac -g Test2.java
xujiajun@bogon meta % javap -v -p Test2.class
Classfile /Users/xujiajun/Documents/Project/java-source-study/study-27-java-jvm/src/main/java/com/xujiajun/meta/Test2.class
  Last modified 2022年6月21日; size 1012 bytes
  SHA-256 checksum 9818a33995c2967fa4ab1a035c784cae9f69e9ac200e789ad1f868bcc773147a
  Compiled from "Test2.java"
public class com.xujiajun.meta.Test2 implements java.lang.Cloneable,java.io.Serializable
  minor version: 0
  major version: 58
  flags: (0x0021) ACC_PUBLIC, ACC_SUPER
  this_class: #8                          // com/xujiajun/meta/Test2
  super_class: #2                         // java/lang/Object
  interfaces: 2, fields: 3, methods: 4, attributes: 1
Constant pool:
   #1 = Methodref          #2.#3          // java/lang/Object."<init>":()V
   #2 = Class              #4             // java/lang/Object
   #3 = NameAndType        #5:#6          // "<init>":()V
   #4 = Utf8               java/lang/Object
   #5 = Utf8               <init>
   #6 = Utf8               ()V
   #7 = Fieldref           #8.#9          // com/xujiajun/meta/Test2.a:I
   #8 = Class              #10            // com/xujiajun/meta/Test2
   #9 = NameAndType        #11:#12        // a:I
  #10 = Utf8               com/xujiajun/meta/Test2
  #11 = Utf8               a
  #12 = Utf8               I
  #13 = Fieldref           #14.#15        // java/lang/System.out:Ljava/io/PrintStream;
  #14 = Class              #16            // java/lang/System
  #15 = NameAndType        #17:#18        // out:Ljava/io/PrintStream;
  #16 = Utf8               java/lang/System
  #17 = Utf8               out
  #18 = Utf8               Ljava/io/PrintStream;
  #19 = String             #20            // method1
  #20 = Utf8               method1
  #21 = Methodref          #22.#23        // java/io/PrintStream.println:(Ljava/lang/String;)V
  #22 = Class              #24            // java/io/PrintStream
  #23 = NameAndType        #25:#26        // println:(Ljava/lang/String;)V
  #24 = Utf8               java/io/PrintStream
  #25 = Utf8               println
  #26 = Utf8               (Ljava/lang/String;)V
  #27 = Class              #28            // java/lang/Exception
  #28 = Utf8               java/lang/Exception
  #29 = Methodref          #27.#30        // java/lang/Exception.printStackTrace:()V
  #30 = NameAndType        #31:#6         // printStackTrace:()V
  #31 = Utf8               printStackTrace
  #32 = String             #33            // hello
  #33 = Utf8               hello
  #34 = Fieldref           #8.#35         // com/xujiajun/meta/Test2.hello:Ljava/lang/String;
  #35 = NameAndType        #33:#36        // hello:Ljava/lang/String;
  #36 = Utf8               Ljava/lang/String;
  #37 = Class              #38            // java/lang/Cloneable
  #38 = Utf8               java/lang/Cloneable
  #39 = Class              #40            // java/io/Serializable
  #40 = Utf8               java/io/Serializable
  #41 = Utf8               HELLO
  #42 = Utf8               ConstantValue
  #43 = Utf8               Code
  #44 = Utf8               LineNumberTable
  #45 = Utf8               LocalVariableTable
  #46 = Utf8               this
  #47 = Utf8               Lcom/xujiajun/meta/Test2;
  #48 = Utf8               method2
  #49 = Utf8               (Ljava/lang/String;)Ljava/lang/String;
  #50 = Utf8               e
  #51 = Utf8               Ljava/lang/Exception;
  #52 = Utf8               name
  #53 = Utf8               StackMapTable
  #54 = Utf8               <clinit>
  #55 = Utf8               SourceFile
  #56 = Utf8               Test2.java
{
  private static java.lang.String hello;
    descriptor: Ljava/lang/String;
    flags: (0x000a) ACC_PRIVATE, ACC_STATIC

  private static final java.lang.String HELLO;
    descriptor: Ljava/lang/String;
    flags: (0x001a) ACC_PRIVATE, ACC_STATIC, ACC_FINAL
    ConstantValue: String hello

  public int a;
    descriptor: I
    flags: (0x0001) ACC_PUBLIC

  public com.xujiajun.meta.Test2();
    descriptor: ()V
    flags: (0x0001) ACC_PUBLIC
    Code:
      stack=2, locals=1, args_size=1
         0: aload_0
         1: invokespecial #1                  // Method java/lang/Object."<init>":()V
         4: aload_0
         5: iconst_0
         6: putfield      #7                  // Field a:I
         9: return
      LineNumberTable:
        line 5: 0
        line 9: 4
      LocalVariableTable:
        Start  Length  Slot  Name   Signature
            0      10     0  this   Lcom/xujiajun/meta/Test2;

  public void method1();
    descriptor: ()V
    flags: (0x0001) ACC_PUBLIC
    Code:
      stack=2, locals=1, args_size=1
         0: getstatic     #13                 // Field java/lang/System.out:Ljava/io/PrintStream;
         3: ldc           #19                 // String method1
         5: invokevirtual #21                 // Method java/io/PrintStream.println:(Ljava/lang/String;)V
         8: return
      LineNumberTable:
        line 12: 0
        line 13: 8
      LocalVariableTable:
        Start  Length  Slot  Name   Signature
            0       9     0  this   Lcom/xujiajun/meta/Test2;

  public static java.lang.String method2(java.lang.String);
    descriptor: (Ljava/lang/String;)Ljava/lang/String;
    flags: (0x0009) ACC_PUBLIC, ACC_STATIC
    Code:
      stack=2, locals=3, args_size=1
         0: iconst_1
         1: istore_1
         2: iload_1
         3: iconst_0
         4: idiv
         5: istore_2
         6: goto          14
         9: astore_1
        10: aload_1
        11: invokevirtual #29                 // Method java/lang/Exception.printStackTrace:()V
        14: aload_0
        15: areturn
      Exception table:
         from    to  target type
             0     6     9   Class java/lang/Exception
      LineNumberTable:
        line 17: 0
        line 18: 2
        line 21: 6
        line 19: 9
        line 20: 10
        line 22: 14
      LocalVariableTable:
        Start  Length  Slot  Name   Signature
            2       4     1     a   I
           10       4     1     e   Ljava/lang/Exception;
            0      16     0  name   Ljava/lang/String;
      StackMapTable: number_of_entries = 2
        frame_type = 73 /* same_locals_1_stack_item */
          stack = [ class java/lang/Exception ]
        frame_type = 4 /* same */

  static {};
    descriptor: ()V
    flags: (0x0008) ACC_STATIC
    Code:
      stack=1, locals=0, args_size=0
         0: ldc           #32                 // String hello
         2: putstatic     #34                 // Field hello:Ljava/lang/String;
         5: return
      LineNumberTable:
        line 7: 0
}
SourceFile: "Test2.java"
xujiajun@bogon meta %


```

##### 8.4.方法区的演进

随着 JDK 的迭代升级，Hotspot 中方法区的存储的内容发生了如下变化（上面介绍的方法区的内部结构是经典情况下的，具体还是需要看 JDK 是什么版本）：

| 版本          | 描述                                                                                                           |
| :------------ | :------------------------------------------------------------------------------------------------------------- |
| jdk1.6 及之前 | 有永久代（permanent generation），**静态变量存放在永久代上**                                                   |
| jdk1.7        | 有永久代，但已经逐步“去永久代”，**字符串常量池、静态变量移除，保存在堆中**                                     |
| jdk1.8 及之后 | 无永久代，类型信息、字段、方法、**常量**保存在**本地内存的元空间**，但**字符串常量池、静态变量仍然保存在堆中** |

!> 上面说的静态变量在 JDK1.6 之前存放在永久代，JDK1.7 后移动到堆空间指的是变量本身，变量对应的对象实例一直都是在堆空间分配的。举个例子：

```java
public class StaticObjTest {

    static class Test {
        static ObjectHolder staticObj = new ObjectHolder();
        ObjectHolder instanceObj = new ObjectHolder();

        void foo() {
            ObjectHolder localObj = new ObjectHolder();
        }
    }

    private static class ObjectHolder {

    }
}
```

!> 这个例子中，三个 new ObjectHolder()的创建，都是在堆中分配的，localObj 是方法 foo 内的局部变量，存放在虚拟机栈的局部变量表中；instanceObj 为成员变量，随着对象实例的创建也分配在堆中；静态变量 staticObj 根据 JDK 版本的不同存放位置也不同，JDK1.6 及之前，存放在永久代中，JDK1.7 及之后存放到堆中。

永久代为什么会被元空间替代？因为永久代的大小是很难确定的，如果一个程序动态加载的类过多就很容易触发永久代的 Full GC（Full GC 代价大，耗时长，影响程序性能）甚至 OOM，程序直接奔溃；而元空间和永久代之间最大的区别在于：元空间并不在虚拟机中，而是使用本地内存，这样元空间就基本不会因为触发 Full GC 和 OOM 了。

字符串常量池（StringTable）为什么要放到堆中？因为如果将 StringTable 放在永久代的话回收效率很低，在 Full GC 的时候才会触发。而 Full GC 是老年代的空间不足、永久代不足时才会触发。这就导致 StringTable 回收效率不高。而我们开发中会有大量的字符串被创建，回收效率低，导致永久代内存不足。放到堆里，能及时回收内存。

##### 8.5.方法区垃圾回收

方法区也存在垃圾回收，方法区的垃圾收集主要回收两部分内容：常量池中废弃的常量和不再使用的类型。

#### 9.执行引擎

类加载器加载的字节码并不能够直接运行在操作系统之上，因为字节码指令不是本地机器指令，执行引擎（Execute Engine）的任务就是讲字节码指令解释为对应平台上的本地机器指令。通俗地讲，执行引擎就是将高级语言翻译为本地机器语言的翻译官。

##### 9.1.解释器和 JIT 编译器

- 解释器（Interpreter）：JVM 在程序运行时通过解释器逐行将字节码转为本地机器指令执行；
- JIT 编译器（Just In Time Compiler，即时编译器）：解释器的优点是程序一启动就可以马上发挥作用，逐行翻译字节码执行程序。而对于一些高频的代码（如循环体内代码和高频调用方法等），如果每次执行都用解释器逐行将字节码翻译为机器指令的话，势必会造成浪费，所以我们可以通过即时编译器将这部分高频代码直接编译为机器指令然后缓存在方法区中（上面介绍方法区内部组成时提到过 JIT 代码缓存），以此提高执行效率。和解释器相比，即时编译器的缺点就是编译需要耗费一定时间。

?> 正因为 JVM 在执行 Java 代码的时候，通常会将解释执行和编译执行二者结合起来进行，所以 Java 也可以说是一种半编译半解释型语言。

###### 9.1.1.热点代码

hotspot 通过两种方式来确定当前代码是否为热点代码：

- 方法调用计数器：统计方法调用的次数；
- 回边计数器：统计循环体执行的循环次数。

当一个方法被调用时，会先检查该方法是否存在被 JIT 编译器编译过的版本，如果存在，则使用编译后的本地代码执行；如果不存在，则将方法的调用计数器加 1，然后判断方法调用计数器和回边计数器之和是否超过方法调用计数器的阈值。如果超过，则会向 JIT 编译器提交一个该方法的代码编译请求。

上面的阈值可以使用`-XX:CompileThreshold`设定，默认值在 Client 模式下是 1500，在 Server 模式下是 10000。

方法调用计数器统计的并不是方法被调用的绝对次数，而是在一定时间范围内的次数。超过这个时间范围，这个方法计数器就会减少一半，这个过程称为热度衰减，这个时间周期称为半衰周期。可以通过`-XX:CounterHalfLifeTime`设置半衰周期（单位 S），`-XX:-UseCounterDecay`来关闭热度衰减。

##### 9.2.模式设置

默认情况下，hotspot 采用混合模式架构（即解释器和 JIT 编译器并存的架构），我们可以通过下面这些指令来切换模式：

- `-Xint`：完全采用解释器模式执行程序；
- `-Xcomp`：完全采用即时编译器模式执行程序，如果即时编译器出现问题，解释器会介入执行；
- `-Xmixed`：混合模式。

![](https://typora-img-1257000606.cos.ap-beijing.myqcloud.com/9H3ifA.png)

##### 9.3.JIT 编译器分类

hotspot 内置两种 JIT 编译器：Client Compiler 和 Server Compiler，也称为 C1 编译器和 C2 编译器。我们可以通过下面这些指令来指定使用哪种 JIT 编译器：

- `-client`：指定 Java 虚拟机运行在 Client 模式下，并使用 C1 编译器。C1 编译器会对字节码进行简单和可靠的优化，耗时短，已达到更快的编译速度；
- `-server`：指定 Java 虚拟机运行在 Server 模式下，并使用 C2 编译器。C2 编译器进行耗时较长的优化，以及激进优化，虽然编译耗时更长，但代码执行效率更高（64 位 JDK 只支持 Server 模式）。

> 参考文章：<br> > https://mrbird.cc/JVM-Learn.html<br> > https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/gc01/index.html<br> > https://docs.oracle.com/en/java/javase/11/tools/java.html<br> > https://docs.oracle.com/javase/8/docs/technotes/tools/unix/java.html<br> > http://www.atguigu.com/download_detail.shtml?v=279
