> 简单的说一下Java的垃圾回收机制，解决了什么问题？

这个题目其实我是不太想写的，原因在于太笼统了，容易让面试者陷入误区，难以正确的回答，但是，如何加上一个解决了什么问题，那么，这个面试题还是有意义的，可以看看面试者的理解。

在c语言和c++中，对于内存的管理是非常的让人头疼的，也是很多人放弃的原因，因此，在后面的一些高级语言中，设计者就想解决这一个问题，让开发者专注于自己的业务开发即可，因此，在Java中也引入了垃圾回收机制，它使得开发者在编写程序的时候不再需要考虑内存管理，由于有个垃圾回收机制，Java中的对象不再有“作用域”的概念，只有对象的引用才有“作用域”。垃圾回收可以有效的防止内存泄露，有效的使用空闲的内存。

> 了解JVM的内存模型吗？

JVM载执行Java程序的过程中会把它所管理的内存划分为若干个不同的数据区域。

Java 虚拟机所管理的内存一共分为Method Area（方法区）、VM Stack（虚拟机栈）、Native Method Stack（本地方法栈）、Heap（堆）、Program Counter Register（程序计数器）五个区域。

这些区域都有各自的用途，以及创建和销毁的时间，有的区域随着虚拟机进程的启动而存在，有些区域则是依赖用户线程的启动和结束而建立和销毁。具体如下图所示：

![](http://image.ouyangsihai.cn/FhxTjHOieWt_ugomW5L33YNkJlJQ)

上图介绍的是JDK1.8 JVM运行时内存数据区域划分。1.8同1.7比，最大的差别就是：**元数据区取代了永久代**。元空间的本质和永久代类似，都是对JVM规范中方法区的实现。不过元空间与永久代之间最大的区别在于：**元数据空间并不在虚拟机中，而是使用本地内存**。


### 1 程序计数器（Program Counter Register）

**程序计数器（Program Counter Register）**是一块较小的内存空间，可以看作是当前线程所执行的字节码的**行号指示器**。在虚拟机概念模型中，**字节码解释器**工作时就是通过改变计数器的值来选取下一条需要执行的字节码指令，分支、循环、跳转、异常处理、线程恢复等基础功能都需要依赖这个计数器来完成。

程序计数器是一块 **“线程私有”** 的内存，每条线程都有一个独立的程序计数器，能够将切换后的线程恢复到正确的执行位置。

- 执行的是一个**Java方法**

计数器记录的是正在执行的**虚拟机字节码指令的地址**。

- 执行的是**Native方法**

**计数器为空（Undefined）**，因为native方法是java通过JNI直接调用本地C/C++库，可以近似的认为native方法相当于C/C++暴露给java的一个接口，java通过调用这个接口从而调用到C/C++方法。由于该方法是通过C/C++而不是java进行实现。那么自然无法产生相应的字节码，并且C/C++执行时的内存分配是由自己语言决定的，而不是由JVM决定的。

- 程序计数器也是唯一一个在Java虚拟机规范中没有规定任何**OutOfMemoryError**情况的内存区域。

其实，我感觉这块区域，作为我们开发人员来说是不能过多的干预的，我们只需要了解有这个区域的存在就可以，并且也没有虚拟机相应的参数可以进行设置及控制。

### 2 Java虚拟机栈（Java Virtual Machine Stacks）

![](http://image.ouyangsihai.cn/FksaMoFlAkSPkTB84bV4cK7xa8L3)


**Java虚拟机栈（Java Virtual Machine Stacks）**描述的是**Java方法执行的内存模型**：每个方法在执行的同时都会创建一个**栈帧（Stack Frame）**，从上图中可以看出，栈帧中存储着**局部变量表**、**操作数栈**、**动态链接**、**方法出口**等信息。每一个方法从调用直至执行完成的过程，会对应一个栈帧在虚拟机栈中入栈到出栈的过程。

与程序计数器一样，Java虚拟机栈也是**线程私有**的。

而**局部变量表**中存放了编译期可知的各种：

*   **基本数据类型**(boolen、byte、char、short、int、 float、 long、double）
*   **对象引用**（reference类型，它不等于对象本身，可能是一个指向对象起始地址的指针，也可能是指向一个代表对象的句柄或其他与此对象相关的位置）
*   **returnAddress类型**（指向了一条字节码指令的地址）

其中64位长度的long和double类型的数据会占用2个局部变量空间（Slot），其余数据类型只占用1个。**局部变量表所需的内存空间在编译期间完成分配**，当进入一个方法时，这个方法需要在帧中分配多大的局部变量空间是完全确定的，在方法运行期间不会改变局部变量表的大小。

Java虚拟机规范中对这个区域规定了两种异常状况：

*   **StackOverflowError**：线程请求的栈深度大于虚拟机所允许的深度，将会抛出此异常。
*   **OutOfMemoryError**：当可动态扩展的虚拟机栈在扩展时无法申请到足够的内存，就会抛出该异常。

一直觉得上面的概念性的知识还是比较抽象的，下面我们通过JVM参数的方式来控制栈的内存容量，模拟StackOverflowError异常现象。


### 3 本地方法栈（Native Method Stack）

**本地方法栈（Native Method Stack）** 与Java虚拟机栈作用很相似，它们的区别在于虚拟机栈为虚拟机执行Java方法（即字节码）服务，而本地方法栈则为虚拟机使用到的Native方法服务。

在虚拟机规范中对本地方法栈中使用的语言、方式和数据结构并无强制规定，因此具体的虚拟机可实现它。甚至**有的虚拟机（Sun HotSpot虚拟机）直接把本地方法栈和虚拟机栈合二为一**。与虚拟机一样，本地方法栈会抛出**StackOverflowError**和**OutOfMemoryError**异常。

- 使用-Xss参数减少栈内存容量（更多的JVM参数可以参考这篇文章：[深入理解Java虚拟机-常用vm参数分析](https://blog.ouyangsihai.cn/shen-ru-li-jie-java-xu-ni-ji-chang-yong-vm-can-shu-fen-xi.html)）

这个例子中，我们将栈内存的容量设置为`256K`（默认1M），并且再定义一个变量查看栈递归的深度。

```java
/**
 * @ClassName Test_02
 * @Description 设置Jvm参数：-Xss256k
 * @Author 欧阳思海
 * @Date 2019/9/30 11:05
 * @Version 1.0
 **/
public class Test_02 {

    private int len = 1;

    public void stackTest() {
        len++;
        System.out.println("stack len:" + len);
        stackTest();
    }

    public static void main(String[] args) {
        Test_02 test = new Test_02();
        try {
            test.stackTest();
        } catch (Throwable e) {
            e.printStackTrace();
        }
    }
}

```
运行时设置JVM参数

![](http://image.ouyangsihai.cn/Fp8Z9xGi-AN7k7laSOHupU7htMg9)

输出结果：

![](http://image.ouyangsihai.cn/FuHgFTcCaWlFjEtorqUbRF3RI_Cx)



### 4 Java堆（Heap）



对于大多数应用而言，**Java堆（Heap）**是Java虚拟机所管理的内存中最大的一块，它**被所有线程共享的**，在虚拟机启动时创建。此内存区域**唯一的目的**是**存放对象实例**，几乎所有的对象实例都在这里分配内存，且每次分配的空间是**不定长**的。在Heap 中分配一定的内存来保存对象实例，实际上只是保存**对象实例的属性值**，**属性的类型**和**对象本身的类型标记**等，**并不保存对象的方法（方法是指令，保存在Stack中）**,在Heap 中分配一定的内存保存对象实例和对象的序列化比较类似。


Java堆是垃圾收集器管理的主要区域，因此也被称为 **“GC堆（Garbage Collected Heap）”** 。从内存回收的角度看内存空间可如下划分：

![图片摘自https://blog.csdn.net/bruce128/article/details/79357870](http://image.ouyangsihai.cn/FvwbMlmR_k5r4xwnpH5LXDN-4qok)


*   **新生代（Young）**： 新生成的对象优先存放在新生代中，新生代对象朝生夕死，存活率很低。在新生代中，常规应用进行一次垃圾收集一般可以回收70% ~ 95% 的空间，回收效率很高。

如果把新生代再分的细致一点，新生代又可细分为**Eden空间**、**From Survivor空间**、**To Survivor空间**，默认比例为8:1:1。

*   **老年代（Tenured/Old）**：在新生代中经历了多次（具体看虚拟机配置的阀值）GC后仍然存活下来的对象会进入老年代中。老年代中的对象生命周期较长，存活率比较高，在老年代中进行GC的频率相对而言较低，而且回收的速度也比较慢。
*   **永久代（Perm）**：永久代存储类信息、常量、静态变量、即时编译器编译后的代码等数据，对这一区域而言，Java虚拟机规范指出可以不进行垃圾收集，一般而言不会进行垃圾回收。


其中**新生代和老年代组成了Java堆的全部内存区域**，而**永久代不属于堆空间，它在JDK 1.8以前被Sun HotSpot虚拟机用作方法区的实现**

另外，再强调一下堆空间内存分配的大体情况，这对于后面一些Jvm优化的技巧还是有帮助的。

- 老年代 ： 三分之二的堆空间
- 年轻代 ： 三分之一的堆空间
        eden区： 8/10 的年轻代空间
        survivor0 : 1/10 的年轻代空间
        survivor1 : 1/10 的年轻代空间

最后，我们再通过一个简单的例子更加形象化的展示一下**堆溢出**的情况。

- JVM参数设置：-Xms10m -Xmx10m

这里将堆的最小值和最大值都设置为10m，如果不了解这些参数的含义，可以参考这篇文章：[深入理解Java虚拟机-常用vm参数分析](https://blog.ouyangsihai.cn/shen-ru-li-jie-java-xu-ni-ji-chang-yong-vm-can-shu-fen-xi.html)

```java
/**
 * VM Args：-Xms10m -Xmx10m -XX:+HeapDumpOnOutOfMemoryError
 * @author zzm
 */
public class HeapTest {

    static class HeapObject {
    }

    public static void main(String[] args) {
        List<HeapObject> list = new ArrayList<HeapObject>();

        //不断的向堆中添加对象
        while (true) {
            list.add(new HeapObject());
        }
    }
}

```

输出结果：
![](http://image.ouyangsihai.cn/Fhky14SMLxHjx9R9ZCcY0jAJ8ljg)

图中出现了`java.lang.OutOfMemoryError`，并且提示了`Java heap space`，这就说明是Java堆内存溢出的情况。

**堆的Dump文件分析**

我的使用的是VisualVM工具进行分析，关于如何使用这个工具查看这篇文章（[深入理解Java虚拟机-如何利用VisualVM对高并发项目进行性能分析 ](https://blog.ouyangsihai.cn/shen-ru-li-jie-java-xu-ni-ji-ru-he-li-yong-visualvm-dui-gao-bing-fa-xiang-mu-jin-xing-xing-neng-fen-xi.html)）。在运行程序之后，会同时打开VisualVM工具，查看堆内存的变化情况。

![](http://image.ouyangsihai.cn/Fhdj0VggJwgP-qAOdWBBrWmO5XrM)

在上图中，可以看到，堆的最大值是30m，但是使用的堆的容量也快接近30m了，所以很容易发生堆内存溢出的情况。

接着查看dump文件。

![](http://image.ouyangsihai.cn/FpYV2YbCGR3ByPy3vFNJbVNpoLdW)

如上图，堆中的大部分的对象都是HeapObject，所以，就是因为这个对象的一直产生，所以导致堆内存不够分配，所以出现内存溢出。

我们再看GC情况。

![](http://image.ouyangsihai.cn/FpB0KxmFAtZOx7g95dlfyAp8mLEV)

如上图，Eden新生代总共48次minor gc，耗时1.168s，基本满足要求，但是survivor却没有，这不正常，同时Old Gen老年代总共27次full gc，耗时4.266s，耗时长，gc多，这正是因为大量的大对象进入到老年代导致的，所以，导致full gc频繁。

### 5 方法区（Method Area）

**方法区（Method Area）** 与Java堆一样，是各个线程共享的内存区域。它用于存储一杯`虚拟机加载`的**类信息、常量、静态变量、及时编译器编译后的代码**等数据。正因为方法区所存储的数据与堆有一种类比关系，所以它还被称为 **Non-Heap**。



<big>**运行时常量池（Runtime Constant Pool）**</big>

**运行时常量池（Runtime Constant Pool）**是方法区的一部分。**Class文件**中除了有类的版本、字段、方法、接口等描述信息外，还有一项信息是**常量池（Constant Pool Table）**，用于存放编译期生成的各种字面量和符号引用，**这部分内容将在类加载后进入方法区的运行时常量池存放**。

Java虚拟机对Class文件每一部分（自然包括常量池）的格式有严格规定，每一个字节用于存储那种数据都必须符合规范上的要求才会被虚拟机认可、装载和执行。但**对于运行时常量池，Java虚拟机规范没有做任何有关细节的要求**，不同的提供商实现的虚拟机可以按照自己的需求来实现此内存区域。不过一般而言，除了保存**Class文件中的描述符号引用**外，还会把**翻译出的直接引用**也存储在运行时常量池中。

运行时常量池相对于Class文件常量池的另外一个重要特征是具备**动态性**，Java语言并不要求常量一定只有编译器才能产生，也就是**并非置入Class文件中的常量池的内容才能进入方法区运行时常量池，运行期间也可能将新的常量放入池中**。

#### 运行时常量池举例

上面的**动态性**在开发中用的比较多的便是String类的`intern()` 方法。所以，我们以`intern()` 方法举例，讲解一下**运行时常量池**。

`String.intern()`是一个`native`方法，作用是：如果字符串常量池中已经包含有一个等于此String对象的字符串，则直接返回池中的字符串；否则，加入到池中，并返回。

```java
/**
 * @ClassName MethodTest
 * @Description vm参数设置：-Xms512m -Xmx512m -Xmn128m -XX:PermSize=10M -XX:MaxPermSize=10M -XX:NewRatio=4 -XX:SurvivorRatio=8 -XX:MaxTenuringThreshold=15 -XX:-HeapDumpOnOutOfMemoryError -XX:+UseParNewGC -XX:+UseConcMarkSweepGC
 * @Author 欧阳思海
 * @Date 2019/11/25 20:06
 * @Version 1.0
 **/

public class MethodTest {

    public static void main(String[] args) {
        List<String> list = new ArrayList<String>();
        long i = 0;
        while (i < 1000000000) {
            System.out.println(i);
            list.add(String.valueOf(i++).intern());
        }
    }
}
```

vm参数介绍：
>-Xms512m -Xmx512m -Xmn128m -XX:PermSize=10M -XX:MaxPermSize=10M -XX:NewRatio=4 -XX:SurvivorRatio=8 -XX:MaxTenuringThreshold=15 -XX:-HeapDumpOnOutOfMemoryError -XX:+UseParNewGC -XX:+UseConcMarkSweepGC
开始堆内存和最大堆内存都是512m，永久代大小10m，新生代和老年代1：4，E:S1:S2=8:1:1，最大经过15次survivor进入老年代，使用的，垃圾收集器是新生代ParNew，老年代CMS。

通过这样的设置之后，查看运行结果：
![](http://image.ouyangsihai.cn/FlpAIczI0f-h26J5QGYXdUkW2hbw)

首先堆内存耗完，然后看看GC情况，设置这些参数之后，GC情况应该会不错，拭目以待。

![](http://image.ouyangsihai.cn/FvkHjwUTJcMK0j7KvrgLVWJOHODY)

上图是GC情况，我们可以看到**新生代** 21 次minor gc，用了1.179秒，平均不到50ms一次，性能不错，**老年代** 117 次full gc，用了45.308s，平均一次不到1s，性能也不错，说明jvm运行是不错的。

>**注意：** 在JDK1.6及以前的版本中运行以上代码，因为我们通过`-XX:PermSize=10M -XX:MaxPermSize=10M`设置了方法区的大小，所以也就是设置了常量池的容量，所以运行之后，会报错：`java.lang.OutOfMemoryError:PermGen space`，这说明常量池溢出；在JDK1.7及以后的版本中，将会一直运行下去，不会报错，在前面也说到，JDK1.7及以后，去掉了永久代。

### 6 直接内存

**直接内存（Direct Memory）**并不是虚拟机**运行时数据区**的一部分，也不是Java虚拟机规范中定义的内存区域。但这部分内存也被频繁运用，而却可能导致**OutOfMemoryError**异常出现。

这个我们实际中主要接触到的就是NIO，在NIO中，我们为了能够加快IO操作，采用了一种直接内存的方式，使得相比于传统的IO快了很多。

在NIO引入了一种基于通道（Channel）与缓冲区（Buffer）的I/O方式，它可以使用Native函数库直接分配**堆外内存**，然后通过一个存储在Java堆中的`DirectByteBuffer`对象作为这块内存的引用进行操作。这样能避免在Java堆和Native堆中来回复制数据，在一些场景里显著提高性能。

在配置虚拟机参数时，会根据实际内存设置-Xmx等参数信息，但经常忽略直接内存，使得各个内存区域总和大于物理内存限制（包括物理的和操作系统的限制），从而导致动态扩展时出现**OutOfMemoryError**异常。

> JVM的四种引用？

Java把对象的引用分为四种级别，从而使程序能更加灵活的控制对象的生命周期。这四种级别由高到低依次为：强引用、软引用、弱引用和虚引用。

- 强引用

以前我们使用的大部分引用实际上都是强引用，这是使用最普遍的引用。如果一个对象具有强引用，那就类似于必不可少的生活用品，垃圾回收器绝不会回收它。当内存空间不足，Java虚拟机宁愿抛出OutOfMemoryError错误，使程序异常终止，也不会靠随意回收具有强引用的对象来解决内存不足问题。 

```java
 String str = "sihai";
 List<String> list = new Arraylist<String>();
 list.add(str);
```
以上就是一个强引用的例子，及时内存不足，该对象也不会被回收。

- 软引用

如果一个对象只具有软引用，那就类似于可有可物的生活用品。如果内存空间足够，垃圾回收器就不会回收它，如果内存空间不足了，就会回收这些对象的内存。只要垃圾回收器没有回收它，该对象就可以被程序使用。软引用可用来实现内存敏感的高速缓存。

软引用可以和一个引用队列（ReferenceQueue）联合使用，如果软引用所引用的对象被垃圾回收，JAVA虚拟机就会把这个软引用加入到与之关联的引用队列中。 

当内存足够大时可以把数组存入软引用，取数据时就可从内存里取数据，提高运行效率。

- 弱引用

如果一个对象只具有弱引用，那就类似于可有可物的生活用品。弱引用与软引用的区别在于：**只具有弱引用的对象拥有更短暂的生命周期。在垃圾回收器线程扫描它所管辖的内存区域的过程中，一旦发现了只具有弱引用的对象，不管当前内存空间足够与否，都会回收它的内存**。不过，由于垃圾回收器是一个优先级很低的线程，因此不一定会很快发现那些只具有弱引用的对象。

弱引用可以和一个引用队列（ReferenceQueue）联合使用，如果弱引用所引用的对象被垃圾回收，Java虚拟机就会把这个弱引用加入到与之关联的引用队列中。 

- 虚引用

如果一个对象仅持有虚引用，那么它就和没有任何引用一样，在任何时候都可能被垃圾回收。虚引用主要用来跟踪对象被垃圾回收的活动。虚引用与软引用和弱引用的一个区别在于：**虚引用必须和引用队列（ReferenceQueue）联合使用。**

当垃圾回收器准备回收一个对象时，如果发现它还有虚引用，就会在回收对象的内存之前，把这个虚引用加入到与之关联的引用队列中。程序可以通过判断引用队列中是否已经加入了虚引用，来了解被引用的对象是否将要被垃圾回收。程序如果发现某个虚引用已经被加入到引用队列，那么就可以在所引用的对象的内存被回收之前采取必要的行动。

**注意:** 在实际程序设计中一般很少使用弱引用与虚引用，使用软用的情况较多，这是因为软引用可以加速JVM对垃圾内存的回收速度，可以维护系统的运行安全，防止内存溢出（OutOfMemory）等问题的产生。

> GC用的可达性分析算法中，哪些对象可以作为GC Roots对象？

- 虚拟机栈（栈帧中的局部变量表，Local Variable Table）中引用的对象。
- 方法区中类静态属性引用的对象。
- 方法区中常量引用的对象。
- 本地方法栈中JNI（即一般说的Native方法）引用的对象。

> 如何判断对象是不是垃圾？

#### 引用计数算法

先讲讲第一个算法：**引用计数算法**。

其实，这个算法的思想非常的简单，一句话就是：**给对象中添加一个引用计数器，每当有一个地方引用它时，计数器加1；当引用失效时，计数器减1；任何时刻计数器为0的对象就是不可能再被使用的。**

这些简单的算法现在是否还被大量的使用呢，其实，现在用的已经不多，没有被使用的最主要的原因是他有一个很大的**缺点**：**很难解决对象之间循环引用的问题**。

**循环引用**：当A有B的引用，B又有A的引用的时候，这个时候，即使A和B对象都为null，这个时候，引用计数算法也不会将他们进行垃圾回收。

```java
/**
 * @ClassName Test_02
 * @Description
 * @Author 欧阳思海
 * @Date 2019/12/5 16:59
 * @Version 1.0
 **/
public class Test_02 {

    public static void main(String[] args) {
        Instance instanceA = new Instance();
        Instance instanceB = new Instance();

        instanceA.instance = instanceB;
        instanceB.instance = instanceA;

        instanceA = null;
        instanceB = null;

        System.gc();

        Scanner scanner = new Scanner(System.in);
        scanner.next();
    }
}

class Instance{
    public Object instance = null;
}

```

如果使用的是**引用计数算法**，这是不能被回收的，当然，现在的JVM是可以被回收的。

#### 可达性分析算法

这个算法的思想也是很简单的，这里有一个概念叫做**可达性分析**，如果知道图的数据结构，这里可以把每一个对象当做图中的一个节点，我们把一个节点叫做**GC Roots**，如果一个节点到**GC Roots**没有任何的相连的路径，那么就说明这个节点不可达，也就是这个节点可以被回收。

![](http://image.ouyangsihai.cn/FqyjRBThJ5HhXAIEH4UE7znJhOuk)

上面图中，虽然obj7、8、9相互引用，但是到GC Roots不可达，所以，这种对象也是会被当做垃圾收集的。

在Java中，可以作为`GC Roots`的对象包括以下几种：

- 虚拟机栈（栈帧中的局部变量表，Local Variable Table）中引用的对象。
- 方法区中类静态属性引用的对象。
- 方法区中常量引用的对象。
- 本地方法栈中JNI（即一般说的Native方法）引用的对象。

> 介绍一下JVM的堆

这个面试题其实和上面的有点重合，但是，这里单独拿出来再介绍一下，因为这个确实是比较常见的，这样大家也都有印象。

对于大多数应用而言，**Java堆（Heap）**是Java虚拟机所管理的内存中最大的一块，它**被所有线程共享的**，在虚拟机启动时创建。此内存区域**唯一的目的**是**存放对象实例**，几乎所有的对象实例都在这里分配内存，且每次分配的空间是**不定长**的。在Heap 中分配一定的内存来保存对象实例，实际上只是保存**对象实例的属性值**，**属性的类型**和**对象本身的类型标记**等，**并不保存对象的方法（方法是指令，保存在Stack中）**,在Heap 中分配一定的内存保存对象实例和对象的序列化比较类似。

Java堆是垃圾收集器管理的主要区域，因此也被称为 **“GC堆（Garbage Collected Heap）”** 。从内存回收的角度看内存空间可如下划分：

![图片摘自https://blog.csdn.net/bruce128/article/details/79357870](http://image.ouyangsihai.cn/FvwbMlmR_k5r4xwnpH5LXDN-4qok)


*   **新生代（Young）**： 新生成的对象优先存放在新生代中，新生代对象朝生夕死，存活率很低。在新生代中，常规应用进行一次垃圾收集一般可以回收70% ~ 95% 的空间，回收效率很高。

如果把新生代再分的细致一点，新生代又可细分为**Eden空间**、**From Survivor空间**、**To Survivor空间**，默认比例为8:1:1。

*   **老年代（Tenured/Old）**：在新生代中经历了多次（具体看虚拟机配置的阀值）GC后仍然存活下来的对象会进入老年代中。老年代中的对象生命周期较长，存活率比较高，在老年代中进行GC的频率相对而言较低，而且回收的速度也比较慢。
*   **永久代（Perm）**：永久代存储类信息、常量、静态变量、即时编译器编译后的代码等数据，对这一区域而言，Java虚拟机规范指出可以不进行垃圾收集，一般而言不会进行垃圾回收。


其中**新生代和老年代组成了Java堆的全部内存区域**，而**永久代不属于堆空间，它在JDK 1.8以前被Sun HotSpot虚拟机用作方法区的实现**

另外，再强调一下堆空间内存分配的大体情况，这对于后面一些Jvm优化的技巧还是有帮助的。

- 老年代 ： 三分之二的堆空间
- 年轻代 ： 三分之一的堆空间
        eden区： 8/10 的年轻代空间
        survivor0 : 1/10 的年轻代空间
        survivor1 : 1/10 的年轻代空间

最后，我们再通过一个简单的例子更加形象化的展示一下**堆溢出**的情况。

- JVM参数设置：-Xms10m -Xmx10m

这里将堆的最小值和最大值都设置为10m，如果不了解这些参数的含义，可以参考这篇文章：[深入理解Java虚拟机-常用vm参数分析](https://blog.ouyangsihai.cn/shen-ru-li-jie-java-xu-ni-ji-chang-yong-vm-can-shu-fen-xi.html)

```java
/**
 * VM Args：-Xms10m -Xmx10m -XX:+HeapDumpOnOutOfMemoryError
 * @author zzm
 */
public class HeapTest {

    static class HeapObject {
    }

    public static void main(String[] args) {
        List<HeapObject> list = new ArrayList<HeapObject>();

        //不断的向堆中添加对象
        while (true) {
            list.add(new HeapObject());
        }
    }
}

```

输出结果：
![](http://image.ouyangsihai.cn/Fhky14SMLxHjx9R9ZCcY0jAJ8ljg)

图中出现了`java.lang.OutOfMemoryError`，并且提示了`Java heap space`，这就说明是Java堆内存溢出的情况。

> 介绍一下Minor GC和Full GC

这个概念首先我们要了解JVM的内存分区，在上面的面试题中已经做了介绍，这里就不再介绍了。

新生代 GC（Minor GC）:指发生新生代的的垃圾收集动作，Minor GC 非常频繁，回收速度一般也比较快。

老年代 GC（Major GC/Full GC）:指发生在老年代的 GC，出现了 Major GC 经常会伴随至少一次的 Minor GC（并非绝对），Major GC 的速度一般会比 Minor GC 的慢 10 倍以上。

**内存分配规则**

- 对象优先分配到Eden区，如果Eden区空间不够，则执行一次minor GC；
- 大对象直接进入到老年代；
- 长期存活的对象可以进入到老年代；
- 动态判断对象年龄。如果在Survivor空间中相同年龄所有对象的大小的总和大于Survivor空间的一半，年龄大于等于该年龄的对象直接进入到老年代。

在这里更加详细的规则介绍可以参考这篇文章：[深入理解Java虚拟机-JVM内存分配与回收策略原理，从此告别JVM内存分配文盲](https://blog.ouyangsihai.cn/shen-ru-li-jie-java-xu-ni-ji-jvm-nei-cun-fen-pei-yu-hui-shou-ce-lue-yuan-li-cong-ci-gao-bie-jvm-nei-cun-fen-pei-wen-mang.html)

> 说一下Java对象创建的方法

这个问题其实很简单，但是很多人却只知道new的方式。

- new的方法，最常见；
- 调用对象的clone方法；
- 使用反射，Class.forName();
- 运用反序列化机制，java.io.ObjectInputStream对象的readObject()方法。

> 介绍一下几种垃圾收集算法？

#### 标记－清除（Mark-Sweep）算法

**标记－清除（Mark-Sweep）** 算法是最基础的垃圾收集算法，后续的收集算法都是基于它的思路并对其不足进行改进而得到的。顾名思义，算法分成“标记”、“清除”两个阶段：首先标记出所有需要回收的对象，在标记完成后统一回收所有被标记的对象，标记过程在前一节讲述对象标记判定时已经讲过了。

标记－清除算法的不足主要有以下两点：

*   **空间问题**，标记清除之后会产生大量不连续的**内存碎片**，空间碎片太多可能会导致以后在程序运行过程中需要分配较大对象时，无法找到足够的连续内存而不得不触发另一次垃圾收集动作。
*   **效率问题**，因为内存碎片的存在，操作会变得更加费时，因为查找下一个可用空闲块已不再是一个简单操作。

标记－清除算法的执行过程如下图所示：

![](http://image.ouyangsihai.cn/Fn_nJ2vQKuX47-8rBn7IZrV5LfoH)


#### 复制（Copying）算法

为了解决标记-清除算法的效率问题，一种称为**“复制”（Copying）**的收集算法出现了，思想为：它**将可用内存按容量分成大小相等的两块**，每次只使用其中的一块。**当这一块内存用完，就将还存活着的对象复制到另一块上面**，然后再把已使用过的内存空间一次清理掉。

这样做使得**每次都是对整个半区进行内存回收**，内存分配时也就**不用考虑内存碎片**等复杂情况，只要**移动堆顶指针，按顺序分配内存**即可，实现简单，运行高效。只是这种算法的代价是**将内存缩小为原来的一半**，代价可能过高了。复制算法的执行过程如下图所示：

![](http://image.ouyangsihai.cn/FoyNQk9dft20afZSCIzC7oVoJIHQ)



##### 标记－整理（Mark-Compact）算法

复制算法在对象存活率较高时要进行较多的复制操作，效率将会变低。更关键的是：如果不想浪费50%的空间，就需要有额外的空间进行分配担保，以应对被使用的内存中所有对象都100%存活的极端情况，所以在**老年代一般不能直接选用复制算法**。

根据老年代的特点，**标记－整理（Mark-Compact）**算法被提出来，主要思想为：此算法的标记过程与**标记－清除**算法一样，但后续步骤不是直接对可回收对象进行清理，而是**让所有存活的对象都向一端移动，然后直接清理掉边界以外的内存。** 具体示意图如下所示：

![](http://image.ouyangsihai.cn/Fov_rN7qL6R_DbGUNChUUOC4lqHS)


##### 分代收集（Generational Collection）算法

当前商业虚拟机的垃圾收集都采用**分代收集（Generational Collection）算法**，此算法相较于前几种没有什么新的特征，主要思想为：根据对象存活周期的不同将内存划分为几块，一般是把Java堆分为新生代和老年代，这样就可以根据各个年代的特点采用最适合的收集算法：

*   **新生代** 在新生代中，每次垃圾收集时都发现有大批对象死去，只有少量存活，那就选用**复制算法**，只需要付出少量存活对象的复制成本就可以完成收集。

*   **老年代** 在老年代中，因为对象存活率高、没有额外空间对它进行分配担保，就必须使用**标记-清除**或**标记-整理**算法来进行回收。

> 如何减少gc出现的次数

上面的面试题已经讲解到了，从年轻代空间（包括Eden和 Survivor 区域）回收内存被称为Minor GC，对老年代GC称为Major GC，而Full GC是对整个堆来说的，在最近几个版本的JDK里默认包括了对永生带即方法区的回收（JDK8中无永生带了），出现Full GC的时候经常伴随至少一次的Minor GC，但非绝对的。Major GC的速度一般会比Minor GC慢10倍以上。

GC会stop the world。会暂停程序的执行，带来延迟的代价。所以在开发中，我们不希望GC的次数过多。

- 对象不用时最好显式置为 Null

一般而言,为 Null 的对象都会被作为垃圾处理,所以将不用的对象显式地设为 Null,有利于 GC 收集器判定垃圾,从而提高了 GC 的效率。
- 尽量少用 System.gc()

此函数建议 JVM 进行主 GC,虽然只是建议而非一定,但很多情况下它会触发主 GC,从而增加主 GC 的频率,也即增加了间歇性停顿的次数。
- 尽量少用静态变量

静态变量属于全局变量,不会被 GC 回收,它们会一直占用内存。
- 尽量使用 StringBuffer,而不用 String 来累加字符串

由于 String 是固定长的字符串对象。累加 String 对象时，并非在一个 String对象中扩增,而是重新创建新的 String 对象,如 Str5=Str1+Str2+Str3+Str4,这条语句执行过程中会产生多个垃圾对象,因为对次作“+”操作时都必须创建新的 String 对象,但这些过渡对象对系统来说是没有实际意义的,只会增加更多的垃圾。 避免这种情况可以改用 StringBuffer 来累加字符串,因 StringBuffer是可变长的,它在原有基础上进行扩增,不会产生中间对象
- 分散对象创建或删除的时间

集中在短时间内大量创建新对象,特别是大对象,会导致突然需要大量内存,JVM 在面临这种情况时,只能进行主 GC,以回收内存或整合内存碎片,从而增加主 GC 的频率。

集中删除对象,道理也是一样的。 它使得突然出现了大量的垃圾对象,空闲空间必然减少,从而大大增加了下一次创建新对象时强制主 GC 的机会。
- 尽量少用 finalize 函数

因为它会加大 GC 的工作量，因此尽量少用finalize 方式回收资源。
- 使用软引用类型

如果需要使用经常用到的图片，可以使用软引用类型，它可以尽可能将图片保存在内存中，供程序调用，而不引起 OutOfMemory。

- 尽量少用 finalize 函数。

因为它会加大 GC 的工作量，因此尽量少用finalize 方式回收资源。

如果需要使用经常用到的图片，可以使用软引用类型，它可以尽可能将图片保存在内存中，供程序调用，而不引起 OutOfMemory。

- 能用基本类型如 int,long,就不用 Integer,Long 对象

基本类型变量占用的内存资源比相应包装类对象占用的少得多,如果没有必要,最好使用基本变量。

- 增大-Xmx 

- 老年代代空间不足

老年代空间只有在新生代对象转入及创建为大对象、大数组时才会出现不足的现象，当执行Full GC后空间仍然不足，则抛出如下错误：
```java
java.lang.OutOfMemoryError: Java heap space
```
为避免以上两种状况引起的Full GC，调优时应尽量做到让对象在Minor GC阶段被回收、让对象在新生代多存活一段时间及不要创建过大的对象及数组。

- 永生区空间不足

JVM规范中运行时数据区域中的方法区，在HotSpot虚拟机中又被习惯称为永生代或者永生区，`Permanet Generation`中存放的为一些class的信息、常量、静态变量等数据，当系统中要加载的类、反射的类和调用的方法较多时，`Permanet Generation`可能会被占满，在未配置为采用CMS GC的情况下也会执行Full GC。如果经过Full GC仍然回收不了，那么JVM会抛出如下错误信息：
```java
java.lang.OutOfMemoryError: PermGen space
```
为避免Perm Gen占满造成Full GC现象，可采用的方法为增大Perm Gen空间或转为使用CMS GC。

- CMS GC时出现`promotion failed`和`concurrent mode failure`

对于采用CMS进行老年代GC的程序而言，尤其要注意GC日志中是否有`promotion failed`和`concurrent mode failure`两种状况，当这两种状况出现时可能会触发Full GC。

promotion failed是在进行Minor GC时，`survivor space`放不下、对象只能放入老年代，而此时老年代也放不下造成的；`concurrent mode failure`是在执行CMS GC的过程中同时有对象要放入老年代，而此时老年代空间不足造成的（有时候“空间不足”是CMS GC时当前的浮动垃圾过多导致暂时性的空间不足触发Full GC）。

对措施为：增大survivor space、老年代空间或调低触发并发GC的比率，但在JDK 5.0+、6.0+的版本中有可能会由于JDK的bug29导致CMS在remark完毕后很久才触发sweeping动作。对于这种状况，可通过设置`-XX: CMSMaxAbortablePrecleanTime=5`（单位为ms）来避免。

- 统计得到的Minor GC晋升到旧生代的平均大小大于老年代的剩余空间

这是一个较为复杂的触发情况，Hotspot为了避免由于新生代对象晋升到旧生代导致旧生代空间不足的现象，在进行Minor GC时，做了一个判断，如果之前统计所得到的Minor GC晋升到旧生代的平均大小大于旧生代的剩余空间，那么就直接触发Full GC。

例如程序第一次触发Minor GC后，有6MB的对象晋升到旧生代，那么当下一次Minor GC发生时，首先检查旧生代的剩余空间是否大于6MB，如果小于6MB，则执行Full GC。

当新生代采用PS GC时，方式稍有不同，PS GC是在Minor GC后也会检查，例如上面的例子中第一次Minor GC后，PS GC会检查此时旧生代的剩余空间是否大于6MB，如小于，则触发对旧生代的回收。

除了以上4种状况外，对于使用RMI来进行RPC或管理的Sun JDK应用而言，默认情况下会一小时执行一次Full GC。可通过在启动时通过`java -Dsun.rmi.dgc.client.gcInterval=3600000`来设置Full GC执行的间隔时间或通过`-XX:+ DisableExplicitGC`来禁止RMI调用`System.gc`。

- 堆中分配很大的对象

所谓大对象，是指需要大量连续内存空间的java对象，例如很长的数组，此种对象会直接进入老年代，而老年代虽然有很大的剩余空间，但是无法找到足够大的连续空间来分配给当前对象，此种情况就会触发JVM进行Full GC。

为了解决这个问题，CMS垃圾收集器提供了一个可配置的参数，即`-XX:+UseCMSCompactAtFullCollection`开关参数，用于在“享受”完Full GC服务之后额外免费赠送一个碎片整理的过程，内存整理的过程无法并发的，空间碎片问题没有了，但提顿时间不得不变长了，JVM设计者们还提供了另外一个参数 `-XX:CMSFullGCsBeforeCompaction`,这个参数用于设置在执行多少次不压缩的Full GC后,跟着来一次带压缩的。

> 数组多大会放在JVM老年代，永久代对象如何GC？如果想不被GC怎么办？如果想在GC中生存一次怎么办？

虚拟机提供了一个`-XX:PretenureSizeThreshold`参数（通常是3MB），令大于这个设置值的对象直接在老年代分配。这样做的目的是避免在Eden区及两个Survivor区之间发生大量的内存复制。（新生代采用复制算法收集内存）

垃圾回收不会发生在永久代，如果永久代满了或者是超过了临界值，会触发完成垃圾回收（Full GC）。如果仔细查看垃圾收集器的输出信息，就会发现永久代也是被回收的。这就是为什么正确的永久代大小对避免Full GC是非常重要的原因。

让对象实现finalize()方法，一次对象的自我拯救。

