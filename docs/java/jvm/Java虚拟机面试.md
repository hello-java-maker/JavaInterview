## 简单的说一下Java的垃圾回收机制，解决了什么问题？

这个题目其实我是不太想写的，原因在于太笼统了，容易让面试者陷入误区，难以正确的回答，但是，如何加上一个解决了什么问题，那么，这个面试题还是有意义的，可以看看面试者的理解。

在c语言和c++中，对于内存的管理是非常的让人头疼的，也是很多人放弃的原因，因此，在后面的一些高级语言中，设计者就想解决这一个问题，让开发者专注于自己的业务开发即可，因此，在Java中也引入了垃圾回收机制，它使得开发者在编写程序的时候不再需要考虑内存管理，由于有个垃圾回收机制，Java中的对象不再有“作用域”的概念，只有对象的引用才有“作用域”。垃圾回收可以有效的防止内存泄露，有效的使用空闲的内存。

## 了解JVM的内存模型吗？

JVM载执行Java程序的过程中会把它所管理的内存划分为若干个不同的数据区域。

Java 虚拟机所管理的内存一共分为Method Area（方法区）、VM Stack（虚拟机栈）、Native Method Stack（本地方法栈）、Heap（堆）、Program Counter Register（程序计数器）五个区域。

这些区域都有各自的用途，以及创建和销毁的时间，有的区域随着虚拟机进程的启动而存在，有些区域则是依赖用户线程的启动和结束而建立和销毁。具体如下图所示：

![](http://image.ouyangsihai.cn/FhxTjHOieWt_ugomW5L33YNkJlJQ)

上图介绍的是JDK1.8 JVM运行时内存数据区域划分。1.8同1.7比，最大的差别就是：**元数据区取代了永久代**。元空间的本质和永久代类似，都是对JVM规范中方法区的实现。不过元空间与永久代之间最大的区别在于：**元数据空间并不在虚拟机中，而是使用本地内存**。


#### 1 程序计数器（Program Counter Register）

**程序计数器（Program Counter Register）**是一块较小的内存空间，可以看作是当前线程所执行的字节码的**行号指示器**。在虚拟机概念模型中，**字节码解释器**工作时就是通过改变计数器的值来选取下一条需要执行的字节码指令，分支、循环、跳转、异常处理、线程恢复等基础功能都需要依赖这个计数器来完成。

程序计数器是一块 **“线程私有”** 的内存，每条线程都有一个独立的程序计数器，能够将切换后的线程恢复到正确的执行位置。

- 执行的是一个**Java方法**

计数器记录的是正在执行的**虚拟机字节码指令的地址**。

- 执行的是**Native方法**

**计数器为空（Undefined）**，因为native方法是java通过JNI直接调用本地C/C++库，可以近似的认为native方法相当于C/C++暴露给java的一个接口，java通过调用这个接口从而调用到C/C++方法。由于该方法是通过C/C++而不是java进行实现。那么自然无法产生相应的字节码，并且C/C++执行时的内存分配是由自己语言决定的，而不是由JVM决定的。

- 程序计数器也是唯一一个在Java虚拟机规范中没有规定任何**OutOfMemoryError**情况的内存区域。

其实，我感觉这块区域，作为我们开发人员来说是不能过多的干预的，我们只需要了解有这个区域的存在就可以，并且也没有虚拟机相应的参数可以进行设置及控制。

#### 2 Java虚拟机栈（Java Virtual Machine Stacks）

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


#### 3 本地方法栈（Native Method Stack）

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



#### 4 Java堆（Heap）



对于大多数应用而言，**Java堆（Heap）**是Java虚拟机所管理的内存中最大的一块，它**被所有线程共享的**，在虚拟机启动时创建。此内存区域**唯一的目的**是**存放对象实例**，几乎所有的对象实例都在这里分配内存，且每次分配的空间是**不定长**的。在Heap 中分配一定的内存来保存对象实例，实际上只是保存**对象实例的属性值**，**属性的类型**和**对象本身的类型标记**等，**并不保存对象的方法（方法是指令，保存在Stack中）**，在Heap 中分配一定的内存保存对象实例和对象的序列化比较类似。


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

#### 5 方法区（Method Area）

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

#### 6 直接内存

**直接内存（Direct Memory）**并不是虚拟机**运行时数据区**的一部分，也不是Java虚拟机规范中定义的内存区域。但这部分内存也被频繁运用，而却可能导致**OutOfMemoryError**异常出现。

这个我们实际中主要接触到的就是NIO，在NIO中，我们为了能够加快IO操作，采用了一种直接内存的方式，使得相比于传统的IO快了很多。

在NIO引入了一种基于通道（Channel）与缓冲区（Buffer）的I/O方式，它可以使用Native函数库直接分配**堆外内存**，然后通过一个存储在Java堆中的`DirectByteBuffer`对象作为这块内存的引用进行操作。这样能避免在Java堆和Native堆中来回复制数据，在一些场景里显著提高性能。

在配置虚拟机参数时，会根据实际内存设置-Xmx等参数信息，但经常忽略直接内存，使得各个内存区域总和大于物理内存限制（包括物理的和操作系统的限制），从而导致动态扩展时出现**OutOfMemoryError**异常。

## JVM的四种引用？

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

## GC用的可达性分析算法中，哪些对象可以作为GC Roots对象？

- 虚拟机栈（栈帧中的局部变量表，Local Variable Table）中引用的对象。
- 方法区中类静态属性引用的对象。
- 方法区中常量引用的对象。
- 本地方法栈中JNI（即一般说的Native方法）引用的对象。

## 如何判断对象是不是垃圾？

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

## 介绍一下JVM的堆

这个面试题其实和上面的有点重合，但是，这里单独拿出来再介绍一下，因为这个确实是比较常见的，这样大家也都有印象。

对于大多数应用而言，**Java堆（Heap）**是Java虚拟机所管理的内存中最大的一块，它**被所有线程共享的**，在虚拟机启动时创建。此内存区域**唯一的目的**是**存放对象实例**，几乎所有的对象实例都在这里分配内存，且每次分配的空间是**不定长**的。在Heap 中分配一定的内存来保存对象实例，实际上只是保存**对象实例的属性值**，**属性的类型**和**对象本身的类型标记**等，**并不保存对象的方法（方法是指令，保存在Stack中）**，在Heap 中分配一定的内存保存对象实例和对象的序列化比较类似。

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

## 介绍一下Minor GC和Full GC

这个概念首先我们要了解JVM的内存分区，在上面的面试题中已经做了介绍，这里就不再介绍了。

新生代 GC（Minor GC）:指发生新生代的的垃圾收集动作，Minor GC 非常频繁，回收速度一般也比较快。

老年代 GC（Major GC/Full GC）:指发生在老年代的 GC，出现了 Major GC 经常会伴随至少一次的 Minor GC（并非绝对），Major GC 的速度一般会比 Minor GC 的慢 10 倍以上。

**内存分配规则**

- 对象优先分配到Eden区，如果Eden区空间不够，则执行一次minor GC；
- 大对象直接进入到老年代；
- 长期存活的对象可以进入到老年代；
- 动态判断对象年龄。如果在Survivor空间中相同年龄所有对象的大小的总和大于Survivor空间的一半，年龄大于等于该年龄的对象直接进入到老年代。

在这里更加详细的规则介绍可以参考这篇文章：[深入理解Java虚拟机-JVM内存分配与回收策略原理，从此告别JVM内存分配文盲](https://blog.ouyangsihai.cn/shen-ru-li-jie-java-xu-ni-ji-jvm-nei-cun-fen-pei-yu-hui-shou-ce-lue-yuan-li-cong-ci-gao-bie-jvm-nei-cun-fen-pei-wen-mang.html)

## 说一下Java对象创建的方法

这个问题其实很简单，但是很多人却只知道new的方式。

- new的方法，最常见；
- 调用对象的clone方法；
- 使用反射，Class.forName();
- 运用反序列化机制，java.io.ObjectInputStream对象的readObject()方法。

## 介绍一下几种垃圾收集算法？

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

## 如何减少gc出现的次数

上面的面试题已经讲解到了，从年轻代空间（包括Eden和 Survivor 区域）回收内存被称为Minor GC，对老年代GC称为Major GC，而Full GC是对整个堆来说的，在最近几个版本的JDK里默认包括了对永生带即方法区的回收（JDK8中无永生带了），出现Full GC的时候经常伴随至少一次的Minor GC，但非绝对的。Major GC的速度一般会比Minor GC慢10倍以上。

GC会stop the world。会暂停程序的执行，带来延迟的代价。所以在开发中，我们不希望GC的次数过多。

- 对象不用时最好显式置为 Null

一般而言，为 Null 的对象都会被作为垃圾处理，所以将不用的对象显式地设为 Null，有利于 GC 收集器判定垃圾，从而提高了 GC 的效率。
- 尽量少用 System.gc()

此函数建议 JVM 进行主 GC，虽然只是建议而非一定，但很多情况下它会触发主 GC，从而增加主 GC 的频率，也即增加了间歇性停顿的次数。
- 尽量少用静态变量

静态变量属于全局变量，不会被 GC 回收，它们会一直占用内存。
- 尽量使用 StringBuffer，而不用 String 来累加字符串

由于 String 是固定长的字符串对象。累加 String 对象时，并非在一个 String对象中扩增，而是重新创建新的 String 对象，如 Str5=Str1+Str2+Str3+Str4，这条语句执行过程中会产生多个垃圾对象，因为对次作“+”操作时都必须创建新的 String 对象，但这些过渡对象对系统来说是没有实际意义的，只会增加更多的垃圾。 避免这种情况可以改用 StringBuffer 来累加字符串，因 StringBuffer是可变长的，它在原有基础上进行扩增，不会产生中间对象
- 分散对象创建或删除的时间

集中在短时间内大量创建新对象，特别是大对象，会导致突然需要大量内存，JVM 在面临这种情况时，只能进行主 GC，以回收内存或整合内存碎片，从而增加主 GC 的频率。

集中删除对象，道理也是一样的。 它使得突然出现了大量的垃圾对象，空闲空间必然减少，从而大大增加了下一次创建新对象时强制主 GC 的机会。
- 尽量少用 finalize 函数

因为它会加大 GC 的工作量，因此尽量少用finalize 方式回收资源。
- 使用软引用类型

如果需要使用经常用到的图片，可以使用软引用类型，它可以尽可能将图片保存在内存中，供程序调用，而不引起 OutOfMemory。

- 尽量少用 finalize 函数。

因为它会加大 GC 的工作量，因此尽量少用finalize 方式回收资源。

如果需要使用经常用到的图片，可以使用软引用类型，它可以尽可能将图片保存在内存中，供程序调用，而不引起 OutOfMemory。

- 能用基本类型如 int，long，就不用 Integer，Long 对象

基本类型变量占用的内存资源比相应包装类对象占用的少得多，如果没有必要，最好使用基本变量。

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

为了解决这个问题，CMS垃圾收集器提供了一个可配置的参数，即`-XX:+UseCMSCompactAtFullCollection`开关参数，用于在“享受”完Full GC服务之后额外免费赠送一个碎片整理的过程，内存整理的过程无法并发的，空间碎片问题没有了，但提顿时间不得不变长了，JVM设计者们还提供了另外一个参数 `-XX:CMSFullGCsBeforeCompaction`，这个参数用于设置在执行多少次不压缩的Full GC后，跟着来一次带压缩的。

## 数组多大会放在JVM老年代，永久代对象如何GC？如果想不被GC怎么办？如果想在GC中生存一次怎么办？

虚拟机提供了一个`-XX:PretenureSizeThreshold`参数（通常是3MB），令大于这个设置值的对象直接在老年代分配。这样做的目的是避免在Eden区及两个Survivor区之间发生大量的内存复制。（新生代采用复制算法收集内存）

垃圾回收不会发生在永久代，如果永久代满了或者是超过了临界值，会触发完成垃圾回收（Full GC）。如果仔细查看垃圾收集器的输出信息，就会发现永久代也是被回收的。这就是为什么正确的永久代大小对避免Full GC是非常重要的原因。

让对象实现finalize()方法，一次对象的自我拯救。

## JVM 常见的参数有哪些，介绍几个常见的，并说说在你工作中实际用到的地方？

首先，JVM 中的参数是非常多的，而且可以分为不同的类型，主要可以分为以下三种类型：
- `标准参数（-）`，所有的JVM实现都必须实现这些参数的功能，而且向后兼容。
- `非标准参数（-X）`，默认JVM实现这些参数的功能，但是并不保证所有JVM实现都满足，且不保证向后兼容。
- `非Stable参数（-XX）`，此类参数各个JVM实现会有所不同，将来可能会随时取消，需要慎重使用。

虽然是这么分类的，实际上呢，非标准参数和非稳定的参数实际的使用中还是用的非常的多的。

#### 标准参数
这一类参数可以说是我们刚刚开始Java是就用的非常多的参数了，比如`java -version`、`java -jar`等等，我们在CMD中输入`java -help`就可以获得Java当前版本的所有标准参数了。

![](http://image.ouyangsihai.cn/FsGtetpK2vpkQRFke44AyrDqbsl2)

如上图就是JDK1.8的所有标准参数了，下面我们将介绍一些我们会用的比较多的参数。

- -client

以client模式启动JVM，这种方式启动速度快，但运行时性能和内存管理效率不高，适合客户端程序或者开发调试。

- -server

以server模式启动JVM，与client情况恰好相反。适合生产环境，适用于服务器。64位的JVM自动以server模式启动。

- -classpath或者-cp

通知JVM类搜索路径。如果指定了`-classpath`，则JVM就忽略`CLASSPATH`中指定的路径。各路径之间以分号隔开。如果`-classpath`和`CLASSPATH`都没有指定，则JVM从当前路径寻找class。

JVM搜索路径的顺序：

**1.先搜索JVM自带的jar或zip包。**

Bootstrap，搜索路径可以用`System.getProperty("sun.boot.class.path")`获得；

**2.搜索`JRE_HOME/lib/ext`下的jar包。**

Extension，搜索路径可以用`System.getProperty("java.ext.dirs")`获得；

**3.搜索用户自定义目录，顺序为：当前目录（.），CLASSPATH，-cp。**

搜索路径用`System.getProperty("java.class.path")`获得。

```java
System.out.println(System.getProperty("sun.boot.class.path"));
System.out.println(System.getProperty("java.ext.dirs"));
System.out.println(System.getProperty("java.class.path"));
```
![](http://image.ouyangsihai.cn/FnHtFC8cd9UucBd39nAiLcCrT5mY)

如上就是我电脑的JVM的路径。

- -DpropertyName=value

定义系统的全局属性值，如配置文件地址等，如果value有空格，则需要使用双引号。

另外用`System.getProperty("hello")`可以获得这些定义的属性值，在代码中也可以用`System.setProperty("hello","world")`的形式来定义属性。

如键值对设置为hello=world。
![](http://image.ouyangsihai.cn/Fp74h47lvnSt4LAq4S7bKH_Qcilr)

```java
System.out.println(System.getProperty("hello"));
```
运行结果就是：
![](http://image.ouyangsihai.cn/Fu6bGzdxxsaJ_bOzjDbd1RUxFKIM)

- -verbose
  
查询GC问题最常用的命令之一，参数如下：
**-verbose:class**
输出JVM载入类的相关信息，当JVM报告说找不到类或者类冲突时可此进行诊断。
**-verbose:gc**
输出每次GC的相关情况。
**-verbose:jni**
输出native方法调用的相关情况，一般用于诊断jni调用错误信息。

另外，控制台**输出GC信息**还可以使用如下命令：

在JVM的启动参数中加入`-XX:+PrintGC -XX:+PrintGCDetails -XX:+PrintGCTimeStamps -XX:+PrintGCApplicationStoppedTime`，按照参数的顺序分别输出GC的简要信息，GC的详细信息、GC的时间信息及GC造成的应用暂停的时间。

#### 非标准参数

非标准的参数主要是关于Java内存区域的设置参数，所以在看这些参数之前，应该先查看Java内存区域的基础知识，可以查看这篇文章：[深入理解Java虚拟机-Java内存区域透彻分析](https://blog.ouyangsihai.cn/shen-ru-li-jie-java-xu-ni-ji-java-nei-cun-qu-yu-tou-che-fen-xi.html)。

非标准参数实在标准参数的基础上的一些扩充参数，可以输入`java -X`，获得当前JVM支持的非标准参数。

![](http://image.ouyangsihai.cn/FkMW25ImmKNr7dZDD0pyysui2wYl)

从图片中可以看出来，这些非标准的参数其实不多的，下面我们再 讲解一些比较常用的参数。

- -Xmn

新生代内存大小的最大值，包括E区和两个S区的总和。设置方法：`-Xmn512m、-Xmn2g`。

- -Xms

初始堆的大小，也是堆大小的最小值，默认值是总共的物理内存/64（且小于1G）。默认情况下，当堆中可用内存小于40%，堆内存会开始增加，一直增加到-Xmx的大小。

- -Xmx

堆的最大值，默认值是总共的物理内存/64（且小于1G），默认情况下，当堆中可用内存大于70%，堆内存会开始减少，一直减小到-Xms的大小。

因此，为了避免这种浮动，所以在设置`-Xms`和`-Xmx`参数时，一般会设置成一样的，能够提高性能。

另外，官方默认的配置为**年老代大小:年轻代大小=2:1**左右，使用`-XX:NewRatio`可以设置年老代和年轻代之比，例如，`-XX:NewRatio=4`，表示**年老代:年轻代=4:1**

- -Xss

设置每个线程的栈内存，默认1M，一般来说是不需要改的。

- -Xprof

跟踪正运行的程序，并将跟踪数据在标准输出输出；适合于开发环境调试。

- -Xnoclassgc

禁用类垃圾收集，关闭针对class的gc功能；因为其阻止内存回收，所以可能会导致OutOfMemoryError错误，慎用。

- -Xincgc

开启增量gc（默认为关闭）；这有助于减少长时间GC时应用程序出现的停顿；但由于可能和应用程序并发执行，所以会降低CPU对应用的处理能力。

- -Xloggc:file

与`-verbose:gc`功能类似，只是将每次GC事件的相关情况记录到一个文件中，文件的位置最好在本地，以避免网络的潜在问题。
 若与verbose命令同时出现在命令行中，则以-Xloggc为准。

#### 4 非Stable参数

这类参数你一看官网以为不能使用呢，官网给你的建议就是这些参数不稳定，慎用，其实这主要的原因还是因为每个公司的实现都是不一样的，所以就是导致不稳定。但是呢，在实际的使用中却是非常的多的，而且这部分的参数很重要。

这些参数大致可以分为三类：

- 性能参数（Performance Options）：用于JVM的性能调优和内存分配控制，如初始化内存大小的设置；
- 行为参数（Behavioral Options）：用于改变JVM的基础行为，如GC的方式和算法的选择；
- 调试参数（Debugging Options）：用于监控、打印、输出等jvm参数，用于显示jvm更加详细的信息；

**注意：以下参数都是JDK1.7及以下可以使用。**

- 性能参数

|参数及其默认值| 	描述|
|-----|-----|
|-XX:LargePageSizeInBytes=4m 	|设置用于Java堆的大页面尺寸|
|-XX:MaxHeapFreeRatio=70 	|GC后java堆中空闲量占的最大比例|
|-XX:MinHeapFreeRatio=40 |	GC后java堆中空闲量占的最小比例|
|**-XX:MaxNewSize=size** |	新生成对象能占用内存的最大值|
|**-XX:MaxPermSize=64m** 	|老生代对象能占用内存的最大值|
|**-XX:NewRatio=2** 	|新生代内存容量与老生代内存容量的比例|
|**-XX:NewSize=2.125m** |	新生代对象生成时占用内存的默认值|
|-XX:ReservedCodeCacheSize=32m 	|保留代码占用的内存容量|
|-XX:ThreadStackSize=512 	|设置线程栈大小，若为0则使用系统默认值|
|-XX:+UseLargePages 	|使用大页面内存|

- 行为参数


|参数及其默认值 	|描述|
|-----|-----|
|-XX:+ScavengeBeforeFullGC 	|新生代GC优先于Full GC执行|
|-XX:+UseGCOverheadLimit 	|在抛出OOM之前限制jvm耗费在GC上的时间比例|
|**-XX:-UseParNewGC**| 打开此开关，使用`ParNew+Serial Old`收集器|
|**-XX:-UseConcMarkSweepGC** 	|使用`ParNew+CMS+Serial Old`收集器对老生代采用并发标记交换算法进行GC|
|**-XX:-UseParallelGC** 	|启用并行GC，使用`ParallelScavenge+Serial Old`收集器|
|**-XX:-UseParallelOldGC** 	|对Full GC启用并行，当`-XX:-UseParallelGC`启用时该项自动启用，`ParallelScavenge+Parallel Old`收集器|
|**-XX:-UseSerialGC** 	|启用串行GC|
|**-XX:+UseG1GC**|	使用垃圾优先（G1）收集器|
|-XX:SurvivorRatio=n	|Eden区域与Survivor区域大小之比。预设值为8|
|-XX:PretenureSizeThreshold=n|直接晋升到老年代的**对象大小**，设置这个参数之后，大于这个参数的对象直接进入到老年代分配|
|-XX:MaxTenuringThreshold=n|晋升到老年代的**对象年龄**，每个对象在坚持过一次Minor GC之后，年龄加1，当超过这个值之后就进入老年代。预设值为15|
|-XX:+UseAdaptiveSizePolicy|动态调整Java堆中各个区域的大小以及进入老年代的年龄|
| -XX:ParallelGCThreads=n|设置并行收集器收集时使用的CPU数。并行收集线程数|
|-XX:MaxGCPauseMillis=n|设置并行收集最大暂停时间|
|-XX:GCTimeRatio=n|设置垃圾回收时间占程序运行时间的百分比。公式为1/(1+N)|
|-XX:+UseThreadPriorities 	|启用本地线程优先级|
|-XX:-DisableExplicitGC 	|禁止调用`System.gc()`；但jvm的gc仍然有效|
|-XX:+MaxFDLimit 	|最大化文件描述符的数量限制|

前面6个参数都是关于**垃圾收集器**的行为参数，也是经常会用到的参数。

- 调试参数

|参数及其默认值| 	描述|
|-----|-----|
|-XX:-CITime |	打印消耗在JIT编译的时间|
|-XX:ErrorFile=./hs_err_pid\<pid\>.log 	|保存错误日志或者数据到文件中|
|-XX:HeapDumpPath=./java_pid\<pid\>.hprof 	|指定导出堆信息时的路径或文件名|
|**-XX:-HeapDumpOnOutOfMemoryError**	|当首次遭遇OOM时导出此时堆中相关信息|
|-XX:OnError="\<cmd args\>;\<cmd args\>" 	|出现致命ERROR之后运行自定义命令|
|-XX:OnOutOfMemoryError="\<cmd args\>;\<cmd args\>" 	|当首次遭遇OOM时执行自定义命令|
|-XX:-PrintClassHistogram 	|遇到Ctrl-Break后打印类实例的柱状信息，与`jmap -histo`功能相同|
|-XX:-PrintConcurrentLocks 	|遇到Ctrl-Break后打印并发锁的相关信息，与`jstack -l`功能相同|
|-XX:-PrintCommandLineFlags 	|打印在命令行中出现过的标记|
|-XX:-PrintCompilation 	|当一个方法被编译时打印相关信息|
|**-XX:-PrintGC** 	|每次GC时打印相关信息|
|**-XX:-PrintGCDetails** 	|每次GC时打印详细信息|
|-XX:-PrintGCTimeStamps 	|打印每次GC的时间戳|
|-XX:-TraceClassLoading 	|跟踪类的加载信息|
|-XX:-TraceClassLoadingPreorder 	|跟踪被引用到的所有类的加载信息|
|-XX:-TraceClassResolution |	跟踪常量池|
|-XX:-TraceClassUnloading |	跟踪类的卸载信息|
|-XX:-TraceLoaderConstraints |	跟踪类加载器约束的相关信息|

以上标黑的就是常用的一些参数。

最后，给大家一个实例，看看在工作中是怎么去运用这些参数的，怎么在工作中去解决这些问题的。

#### **参数实例**

设置`-Xms`、`-Xmn`和`-Xmx`参数分别为`-Xms512m -Xmx512m -Xmn128m`。同时设置新生代和老生代之比为1:4，E:S0:S1=8:1:1。除此之外，当然，你还可以设置一些其他的参数，比如，前面说到的，性能参数 `-XX:MaxNewSize`、 `-XX:NewRatio=`、，行为参数 `-XX:-UseParNewGC`，调试参数 `-XX:-PrintGCDetails`。

这些参数都是可以在 IDEA 中启动时直接设置的。

```
**
 * @ClassName MethodTest
 * @Description vm参数设置：-Xms512m -Xmx512m -Xmn128m -XX:NewRatio=4 -XX:SurvivorRatio=8 
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

运行之后，用VisualVM查看相关信息是否正确。

当我们**没有设置**`-XX:NewRatio=4 -XX:SurvivorRatio=8`时，使用官方默认的情况如下： 

![](http://image.ouyangsihai.cn/FgIr8Niw2F9Cs1IK6ABn74oEi66T)

上图可以看出，**新生代（Eden Space + Survivor 0 + Survivor 1）:老年代（Old Gen）≈ 1:2**。

当我们**设置了**`-XX:NewRatio=4 -XX:SurvivorRatio=8`时，情况如下：

![](http://image.ouyangsihai.cn/Fkc4DTkISF1LA9fpO54VEinaYvlc)

变成了**新生代（Eden Space + Survivor 0 + Survivor 1）:老年代（Old Gen）≈ 1:4**，Eden Space:Survivor 0: Survivor 1 = 8:1:1。

![](http://image.ouyangsihai.cn/FphOpfRSOQYu-pcR6xf6rDPcQAF9)

从上图可知，堆的信息是正确的。

更多的使用方法可以参考这篇文章 [如何利用VisualVM对高并发项目进行性能分析](http://www.java1000.com/shen-ru-li-jie-java-xu-ni-ji-ru-he-li-yong-visualvm-dui-gao-bing-fa-xiang-mu-jin-xing-xing-neng-fen-xi.html)，有更加细致的介绍。

## 说说你了解的常见的内存调试工具：jps、jmap、jhat等。

|名称|解释|
|-----|-----|
|jps|显示指定系统内所有的HotSpot虚拟机的进程|
|jstat|用于收集HotSpot虚拟机各方面的运行数据|
|jinfo|显示虚拟机配置信息|
|jmap|生成虚拟机的内存转存储快照（heapdump文件），利用这个文件就可以分析内存等情况|
|jhat|用于分析上面jmap生成的heapdump文件，它会建立一个HTTP/HTML服务器，让用户可以在浏览器上查看分析结果|
|jstack|显示虚拟机的线程快照|

#### 1 jps：虚拟机进程状况工具

这个工具使用的频率还是非常高的，因为你需要查看你的程序的运行情况的时候，首先需要知道的就是程序所运行的**进程本地虚拟机唯一ID（LVMID）**，知道这个ID之后，才可以进行其他监控的操作。

- 命令格式：

```
jps [选项] [主机id]
```

- jps主要选项：

|选项|解释|
|-----|-----|
|-q|只输出LVMID，省略主类名称|
|-m|输出虚拟机进程启动时传递给主类main()函数的参数|
|-l|输出主类的全名|
|-v|输出虚拟机进程启动时的 JVM 参数|



- 实例

```
jps -l
jps -v
```
![](http://image.ouyangsihai.cn/Fq2nwxo46q4EpQ08Ba_qKIU2JN-J)

![](http://image.ouyangsihai.cn/FmEvrlfb6kTEk8u_Hc-NcGHg8wW9)

#### 2 jinfo：Java配置信息工具

jinfo的功能很简单，主要就是显示和查看调整虚拟机的各种参数。

- jinfo命令格式：

```
jinfo [选项] pid
```

- 相关选项

![](http://image.ouyangsihai.cn/FpSz04ZVzS6K6FJ3zVtnoYqMdLt7)


- 实例

我们在启动程序的时候设置一些JVM参数：`-Xms512m -Xmx512m -Xmn128m -XX:NewRatio=4 -XX:SurvivorRatio=8 -XX:MaxTenuringThreshold=15`。

我们先使用`jps -l`查看虚拟机进程ID；
![](http://image.ouyangsihai.cn/FtsaKW9WE0lT-Vn_OH4CjpqWXqwz)

再使用pid为1584进行查询参数；

![](http://image.ouyangsihai.cn/FgOjSz1U0kS2yt2V-k_huM-f0zJ_)

#### 3 jmap：Java内存映射工具

jmap的主要功能就是生成**堆转存储快照**，之后，我们再利用这个快照文件进行分析。

- jmap命令格式：

```
jmap [选项] vmid
```

- 相关选项

|选项|解释|
|-----|-----|
|-dump|生成Java堆转存储快照，格式：`-dump:[live,]format=b,file=<filename>`,其中`live`子参数说明是否只dump出存活的对象|
|-finalizerinfo|显示在F-Queue中等待Finalizer线程执行finalize方法的对象，**只在linux/Solaris有效**|
|-heap|显示Java堆详细信息，如使用哪种回收期、参数配置、分代状况等等。**只在Linux/Solaris有效**|
|-histo|显示堆中对象统计信息，包括类、实例数量、合计容量|
|-permstat|以ClassLoader为统计口径显示永久代内存状态，**只在Linux/Solaris有效**|
|-F|当虚拟机进程对-dump选项没有响应时，可使用这个选项强制生成dump快照，**只在Linux/Solaris有效**|

- 实例

首先还是查看虚拟机ID；

![](http://image.ouyangsihai.cn/FtuNhdGkitnyOGWFegoT2ETIOidf)

然后再运行下面命令行；
![](http://image.ouyangsihai.cn/Fvi9c3BD9bSt4Qi1ptDkHaVH6jA1)

打开这个dump文件如下；
![](http://image.ouyangsihai.cn/Fvd2a3KTLCykCyBX7fAosH65tvCw)

ok，现在已经有了生成的dump文件，所以，我们就需要对这个文件进行解析，看看**jhat命令**。

#### 4 jhat：虚拟机堆转存储快照分析工具

虽然好像这个命令挺牛逼，但是，其实，由于这个工具的功能不太够，分析也得不到太多的结果，所以我们一般可以会将得到的dump文件用其他的工具进行分析，比如，可视化工具VisualVM等等。

所以，这里简单的做一个例子。
- 实例

```
jhat D:\dump.bin
```

![](http://image.ouyangsihai.cn/FsLOf6D2M9Ta8YiA9DCJ-zj6I-_X)

如果有兴趣查看如何利用VisualVM进行查看，可以查看这篇文章：[深入理解Java虚拟机-如何利用VisualVM对高并发项目进行性能分析](https://blog.ouyangsihai.cn/shen-ru-li-jie-java-xu-ni-ji-ru-he-li-yong-visualvm-dui-gao-bing-fa-xiang-mu-jin-xing-xing-neng-fen-xi.html)。

#### 5 jstack：Java堆栈跟踪工具

`jstack` 命令主要用于生成虚拟机当前时刻的**线程快照**。线程快照就是当前虚拟机内每一条线程正在执行的方法堆栈的集合，生成线程快照的**主要目的**：定位线程出现长时间停顿的原因、请求外部资源导致的长时间等待等这些原因。

- jstack命令格式：

```
jstack [选项] vmid
```

- 相关选项

|选项|解释|
|-----|-----|
|-F|当正常输出的请求不被响应时，强制输出线程堆栈|
|-l|除堆栈外，显示关于锁的附加信息|
|-m|如果调用到本地方法的话，可以显示C/C++的堆栈|

- 实例

```
jstack -l 6708
```
![](http://image.ouyangsihai.cn/FrcFS7iWpybtIg48OoDjrdmsSDJa)


#### 6 jstat：虚拟机统计信息监视工具

jstat这个工具还是很有作用的，他可以显示本地或者远程**虚拟机进程中的类装载、内存、垃圾收集、JIT编译**等运行数据，在服务器上，他是运行期定位虚拟机性能问题的首选工具。

- jstat命令格式：

```
jstat [选项 vmid [interval[s|ms] [count]]]
```

- 相关选项

|选项|解释|
|-----|-----|
|**-class**|监视类装载、卸载数量、总空间以及类装载所耗费的时间|
|**-gc**|监视Java堆状况，包括Eden区、两个Survivor区、老年代、永久代等容量、已用空间、GC时间合计等信息|
|-gccapacity|监视内容与-gc基本相同，但输出主要关注Java堆各个区域使用到的最大、最小空间|
|**-gcutil**|监视内容与-gc基本相同，但输出主要关注已使用空间占总空间的百分比|
|-gccause|与-gcutil功能一样，但是会输出额外导致上一次GC产生的原因|
|-gcnew|监视新生代GC状况|
|-gcnewcapacity|监控内容与-gcnew一样，输出主要关注使用到的最大最小空间|
|-gcold|监控老年代GC状况|
|-gcoldcapacity|监控内容与-gcold一样，输出主要关注使用到的最大最小空间|
|-gcpermcapacity|输出永久代使用到的最大最小空间|
|-compiler|输出JIT编译器编译过的方法、耗时等信息|
|-printcompilation|输出已经被JIT编译过的方法|

- 实例


我们这里还关注一下虚拟机GC的状况。

```
jstat -gcutil 9676
```
![](http://image.ouyangsihai.cn/FkelCR8JIup_jpEFY9UQvqD2l0EC)

上面参数的含义：
>S0：第一个Survivor区的空间使用（%）大小
S1：第二个Survivor区的空间使用（%）大小
E：Eden区的空间使用（%）大小
O：老年代空间使用（%）大小
M：方法区空间使用（%）大小
CCS:压缩类空间使用（%）大小
YGC：年轻代垃圾回收次数
YGCT：年轻代垃圾回收消耗时间
FGC：老年代垃圾回收次数
FGCT：老年代垃圾回收消耗时间
GCT：垃圾回收消耗总时间

了解了这些参数的意义之后，我们就可以对虚拟机GC状况进行分析。我们发现年轻代回收次数`12`次，使用时间`1.672`s，老年代回收`0`次，使用时间`0`s，所有GC总耗时`1.672`s。

通过以上参数分析，发现老年代Full GC没有，说明没有大对象进入到老年代，整个老年代的GC情况还是不错的。另外，年轻代回收次数`12`次，使用时间`1.672`s，每次用时100ms左右，这个时间稍微长了一点，可以将新生代的空间调低一点，以降低每一次的GC时间。

## 常见的垃圾回收器有哪些？

先上一张图，这张图是Java虚拟机的jdk1.7及以前版本的所有垃圾回收器，也可以说是比较成熟的垃圾回收器，除了这些垃圾回收器，面试的时候最多也就再怼怼G1和ZGC了。

<img src="https://imgconvert.csdnimg.cn/aHR0cDovL2ltYWdlLm91eWFuZ3NpaGFpLmNuL0ZoNE1EWTdQLVRPMXRRZ3RVUVFKeXlramliQ1o?x-oss-process=image/format,png" alt="">

上面的表示是年轻代的垃圾回收器：Serial、ParNew、Parallel Scavenge，下面表示是老年代的垃圾回收器：CMS、Parallel Old、Serial Old，以及不分老年代和年轻代的G1。之间的相互的连线表示可以相互配合使用。

说完是不是一篇明朗，其实也就是那么回事。

#### 新生代垃圾回收器

##### Serial

Serial（串行）收集器是**最基本、发展历史最悠久**的收集器，它是采用**复制算法**的新生代收集器，曾经（JDK 1.3.1之前）是虚拟机新生代收集的唯一选择。它是一个**单线程收集器**，只会使用一个CPU或一条收集线程去完成垃圾收集工作，更重要的是它在进行垃圾收集时，必须暂停其他所有的工作线程，直至Serial收集器收集结束为止（“Stop The World”）。

其实对于这个垃圾回收器，你只要记住是一个**单线程、采用复制算法的，会进行“Stop The World”** 即可，因为面试官一般不问这个，为什么，因为太简单了，没什么可问的呗。

好了，再放一张图好吧，说明一下Serial的回收过程，完事。

<img src="https://imgconvert.csdnimg.cn/aHR0cDovL2ltYWdlLm91eWFuZ3NpaGFpLmNuL0Z0SXctUHozUEROSmxScDJ2ODlYZkl6eEpja0c?x-oss-process=image/format,png" alt="">

说明：这张图的意思就是**单线程，新生代使用复制算法标记、老年代使用标记整理算法标记**，就是这么简单。

##### ParNew

**ParNew收集器就是Serial收集器的**多线程**版本**，它也是一个新生代收集器。除了使用多线程进行垃圾收集外，其余行为包括Serial收集器可用的所有控制参数、收集算法（复制算法）、Stop The World、对象分配规则、回收策略等与Serial收集器完全相同。

需要注意一点是：**除了Serial收集器外，目前只有它能和CMS收集器（Concurrent Mark Sweep）配合工作。**

最后再放一张回收过程图；

![](http://image.ouyangsihai.cn/FvsKnXGzEQd6WYdUmIgLcWoSHG4H)

*** 是不是很简单，我在这里讲这些知识点并不是为了深入去了解这些原理，基本的知道对于工作已经够了，其实，主要还是应付面试官，哈哈。

##### Parallel Scavenge

Parallel Scavenge收集器也是一个**并行**的**多线程**新生代收集器，它也使用**复制算法**。

Parallel Scavenge收集器的特点是它的关注点与其他收集器不同，CMS等收集器的关注点是尽可能缩短垃圾收集时用户线程的停顿时间。

这里需要注意的唯一的区别是：Parallel Scavenge收集器的目标是**达到一个可控制的吞吐量（Throughput）**。

我们知道，停顿时间越短就越适合需要与用户交互的程序，良好的响应速度能提升用户体验。而**高吞吐量则可以高效率地利用CPU时间，尽快完成程序的运算任务，主要适合在后台运算而不需要太多交互的任务**。

#### 老年代垃圾回收器

##### Serial Old

Serial Old 是Serial收集器的老年代版本，它同样是一个**单线程**收集器，使用“**标记-整理**”（Mark-Compact）算法。

在这里就可以出一个面试题了。
- 为什么Serial使用的是**复制算法**，而Serial Old使用是**标记-整理**算法？
同一个爸爸，儿子长的天差地别，当然也有啊，哈哈。

>  
 其实，看了我前面的文章你可能就知道了，因为在新生代绝大多数的内存都是会被回收的，所以留下来的需要回收的垃圾就很少了，所以复制算法更合适，你可以发现，基本的老年代的都是使用标记整理算法，当然，CMS是个杂种哈。 


它的工作流程与Serial收集器相同，下图是Serial/Serial Old配合使用的工作流程图：

<img src="https://imgconvert.csdnimg.cn/aHR0cDovL2ltYWdlLm91eWFuZ3NpaGFpLmNuL0Z0SXctUHozUEROSmxScDJ2ODlYZkl6eEpja0c?x-oss-process=image/format,png" alt="">

##### Parallel Old

Parallel Old收集器是Parallel Scavenge收集器的老年代版本，使用**多线程**和“**标记-整理**”算法，是不是前面说的，老年代出了杂种CMS不是“**标记-整理**”算法，其他都是。

另外，有了Parallel Old垃圾回收器后，就出现了以“**吞吐量优先**”著称的“男女朋友”收集器了，这就是：**Parallel Old和Parallel Scavenge收集器的组合**。

Parallel Old收集器的工作流程与Parallel Scavenge相同，这里给出Parallel Scavenge/Parallel Old收集器配合使用的流程图：

<img src="https://imgconvert.csdnimg.cn/aHR0cDovL2ltYWdlLm91eWFuZ3NpaGFpLmNuL0Z2OTZmbmtTbmJWODVCUzJFNm1yX1pGQmQ5V2U?x-oss-process=image/format,png" alt="">

你是不是以为我还要讲CMS和G1，我任性，我就是要接着讲，哈哈哈。


#### CMS 

##### 小伙子，你说一下 CMS 垃圾回收器吧！

这个题目一来，吓出一身冷汗，差点就没有复习这个CMS，还好昨晚抱佛脚看了一下哈。

于是我。。。一顿操作猛如虎。

CMS（Concurrent Mark Sweep）收集器是一种以获取最短回收停顿时间为目标的收集器，它是基于“标记-清除”算法实现的，并且常见的应用场景是**互联网站或者B/S系统的服务端上的Java应用**。

结果就一紧张就记得这么多，面试官肯定不满意了，这个时候，面试官的常规操作是，**继续严刑拷打，他想，你可能忘记了，我来提醒提醒你！**

##### CMS收集器工作的整个流程是怎么样的，你能给我讲讲吗？

这个时候，面试官还会安慰你说不用紧张，但是，安慰归安慰，最后挂不挂可是另一回事。

于是，我又开始回答问题。

CMS 处理过程有七个步骤：

- **初始标记**，会导致stw;
- **并发标记**，与用户线程同时运行；
- **预清理**，与用户线程同时运行；
- **可被终止的预清理**，与用户线程同时运行；
- **重新标记** ，会导致swt；
- **并发清除**，与用户线程同时运行；

其实，只要回答四个就差不多了，是这几个。

- **初始标记**：仅仅只是标记一下GC Roots能直接关联到的对象，速度很快，需要“Stop The World”。
- **并发标记**：进行GC Roots Tracing的过程，在整个过程中耗时最长。
- **重新标记**：为了修正并发标记期间因用户程序继续运作而导致标记产生变动的那一部分对象的标记记录，这个阶段的停顿时间一般会比初始标记阶段稍长一些，但远比并发标记的时间短。此阶段也需要“Stop The World”。
- **并发清除**。

你以为这样子就可以了，面试官就会说可以了，如果可以了，那估计你凉了！

##### 面试官说：CMS这么好，那有没有什么缺点呢？

我。。。好吧，谁怪我这么强呢，对吧。

其实，CMS虽然经过这么些年的考验，已经是一个值得信赖的GC回收器了，但是，其实也是有一些他的不足的，

第一，**垃圾碎片的问题**，我们都知道CMS是使用的是**标记-清除**算法的，所以不可避免的就是会出现垃圾碎片的问题。
第二，**一般CMS的GC耗时80%都在remark阶段，remark阶段停顿时间会很长**，在CMS的这四个主要的阶段中，最费时间的就是重新标记阶段。
第三，**concurrent mode failure**，说出这个的时候，面试官就会觉得，小伙子，哎呦，不错哟，掌握的比较清楚，那这个是什么意思呢，其实是说：

>这个异常发生在cms正在回收的时候。执行CMS GC的过程中，同时业务线程也在运行，当年轻带空间满了，执行ygc时，需要将存活的对象放入到老年代，而此时老年代空间不足，这时CMS还没有机会回收老年带产生的，或者在做Minor GC的时候，新生代救助空间放不下，需要放入老年代，而老年代也放不下而产生的。

第四，**promotion failed**，这个问题是指，在进行Minor GC时，Survivor空间不足，对象只能放入老年代，而此时老年代也放不下造成的，多数是由于老年代有足够的空闲空间，但是由于碎片较多，新生代要转移到老年带的对象比较大，找不到一段连续区域存放这个对象导致的。

面试官看到你掌握的这么好，心里已经给你竖起来大拇指，但是，面试官觉得你优秀啊，就还想看看你到底还有多少东西。

##### 既然你知道有这么多的缺点，那么你知道怎么解决这些问题吗？

这个真的被问蒙了，你以为我什么都会吗！！！！

但是，我还是得给大家讲讲，不然下次被问到，可能会把锅甩给我。

- **垃圾碎片的问题**：针对这个问题，这时候我们需要用到这个参数：`-XX:CMSFullGCsBeforeCompaction=n` 意思是说在上一次CMS并发GC执行过后，到底还要再执行多少次`full GC`才会做压缩。默认是0，也就是在默认配置下每次CMS GC顶不住了而要转入full GC的时候都会做压缩。

- **concurrent mode failure**

解决这个问题其实很简单，只需要设置两个参数即可

`-XX:+UseCMSInitiatingOccupancyOnly`
`-XX:CMSInitiatingOccupancyFraction=60`：是指设定CMS在对内存占用率达到60%的时候开始GC。

为什么设置这两个参数呢？由于在垃圾收集阶段用户线程还需要运行，那也就还需要**预留有足够的内存空间给用户线程使用**，因此CMS收集器不能像其他收集器那样等到老年代几乎完全被填满了再进行收集。

当然也不能设置过高，比如90%，这时候虽然GC次数少，但是，却会导致用于用户线程空间小，效率不高，太低10%，你自己想想会怎么样，体会体会！

**哈哈，万事大吉，这一点说出了，估计面试官已经爱上我了吧，赶紧把我招进去干活吧。**

- **remark阶段停顿时间会很长的问题**：解决这个问题巨简单，加入`-XX:+CMSScavengeBeforeRemark`。在执行remark操作之前先做一次`Young GC`，目的在于减少年轻代对老年代的无效引用，降低remark时的开销。

#### G1 (Garbage-First)

G1 (Garbage-First)是一款面向服务器的垃圾收集器，主要针对配备多颗处理器及大容量内存的机器。以极高概率满足 GC 停顿时间要求的同时，还具备高吞吐量性能特征。

- 可以像CMS收集器一样， GC 操作与应用的线程一起并发执行
- 紧凑的空闲内存区间且没有很长的 GC 停顿时间
- 需要可预测的GC暂停耗时
- 不想牺牲太多吞吐量性能.
- 启动后不需要请求更大的Java堆

那么 G1 相对于 CMS 的区别在：

- G1 在压缩空间方面有优势

- G1 通过将内存空间分成区域（Region）的方式避免内存碎片问题

- Eden， Survivor， Old 区不再固定、在内存使用效率上来说更灵活

- G1 可以通过设置预期停顿时间（Pause Time）来控制垃圾收集时间避免应用雪崩现象

- G1 在回收内存后会马上同时做合并空闲内存的工作、而 CMS 默认是在STW（stop the world）的时候做

- G1 会在 Young GC 中使用，而 CMS 只能在O区使用

就目前而言、CMS 还是默认首选的 GC 策略、可能在以下场景下 G1 更适合：

- 服务端多核 CPU、JVM 内存占用较大的应用（至少大于4G）

- 应用在运行过程中会产生大量内存碎片、需要经常压缩空间

- 想要更可控、可预期的 GC 停顿周期；防止高并发下应用雪崩现象

G1的内存使用示意图：
![](http://image.ouyangsihai.cn/FgFGLjZMCGmjKscUAj3hBfK1HLKM)

G1在运行过程中主要包含如下4种操作方式：

- YGC（不同于CMS）

- 并发阶段

- 混合模式

- full GC （一般是G1出现问题时发生）

##### YGC（年轻代GC）
下面是一次 YGC 前后内存区域是示意图：
![](http://image.ouyangsihai.cn/FjX3b5ZidxSGhDRF-qiJ4SXm-ji5)

图中每个小区块都代表 G1 的一个区域（Region），区块里面的字母代表不同的分代内存空间类型（如[E]Eden，[O]Old，[S]Survivor）空白的区块不属于任何一个分区；G1 可以在需要的时候任意指定这个区域属于 Eden 或是 O 区之类的。

YoungGC 在 Eden 充满时触发，在回收之后所有之前属于 Eden 的区块全变成空白。然后至少有一个区块是属于 S 区的（如图半满的那个区域），同时可能有一些数据移到了 O 区。

目前大都使用 PrintGCDetails 参数打出GC日志、这个参数对G1同样有效、但日志内容颇为不同。

下面是一个Young GC的例子：
```
23.430: [GC pause (young)， 0.23094400 secs]
...
[Eden: 1286M(1286M)->0B(1212M)
Survivors: 78M->152M Heap: 1454M(4096M)->242M(4096M)]
[Times: user=0.85 sys=0.05, real=0.23 secs]
```
上面日志的内容解析：Young GC实际占用230毫秒、其中GC线程占用850毫秒的CPU时间

- E：内存占用从 1286MB 变成 0、都被移出
- S：从 78M 增长到了 152M、说明从 Eden 移过来 74M
- Heap: 占用从 1454 变成 242M、说明这次 Young GC 一共释放了 1212M 内存空间

很多情况下，S 区的对象会有部分晋升到 Old 区，另外如果 S 区已满、Eden 存活的对象会直接晋升到 Old 区，这种情况下 Old 的空间就会涨。

##### 并发阶段
一个并发G1回收周期前后内存占用情况如下图所示：
![](http://image.ouyangsihai.cn/Flo526oI2G_UIbkT-Jibo5ZoX27u)

从上面的图表可以看出以下几点：

- Young 区发生了变化、这意味着在 G1 并发阶段内至少发生了一次 YGC（这点和 CMS 就有区别），Eden 在标记之前已经被完全清空，因为在并发阶段应用线程同时在工作、所以可以看到 Eden 又有新的占用
- 一些区域被X标记，这些区域属于 O 区，此时仍然有数据存放、不同之处在 G1 已标记出这些区域包含的垃圾最多、也就是回收收益最高的区域
- 在并发阶段完成之后实际上 O 区的容量变得更大了（O+X 的方块）。这时因为这个过程中发生了 YGC 有新的对象进入所致。此外，这个阶段在 O 区没有回收任何对象：它的作用主要是标记出垃圾最多的区块出来。对象实际上是在后面的阶段真正开始被回收

G1 并发标记周期可以分成几个阶段、其中有些需要暂停应用线程。第一个阶段是初始标记阶段。这个阶段会暂停所有应用线程-部分原因是这个过程会执行一次 YGC、下面是一个日志示例：
```
50.541: [GC pause (young) (initial-mark), 0.27767100 secs]
[Eden: 1220M(1220M)->0B(1220M)
Survivors: 144M->144M Heap: 3242M(4096M)->2093M(4096M)]
[Times: user=1.02 sys=0.04, real=0.28 secs]
```
上面的日志表明发生了 YGC 、应用线程为此暂停了 280 毫秒，Eden 区被清空（71MB 从 Young 区移到了 O 区）。

日志里面 initial-mark 的字样表明后台的并发 GC 阶段开始了。因为初始标记阶段本身也是要暂停应用线程的，G1 正好在 YGC 的过程中把这个事情也一起干了。为此带来的额外开销不是很大、增加了 20% 的 CPU ，暂停时间相应的略微变长了些。

接下来，G1 开始扫描根区域、日志示例：
```
50.819: [GC concurrent-root-region-scan-start]
51.408: [GC concurrent-root-region-scan-end, 0.5890230]
```
一共花了 580 毫秒，这个过程没有暂停应用线程；是后台线程并行处理的。这个阶段不能被 YGC 所打断、因此后台线程有足够的 CPU 时间很关键。如果 Young 区空间恰好在 Root 扫描的时候满了、YGC 必须等待 root 扫描之后才能进行。带来的影响是 YGC 暂停时间会相应的增加。这时的 GC 日志是这样的： 
```
350.994: [GC pause (young)
351.093: [GC concurrent-root-region-scan-end, 0.6100090]
351.093: [GC concurrent-mark-start],0.37559600 secs]
```
GC 暂停这里可以看出在 root 扫描结束之前就发生了，表明 YGC 发生了等待，等待时间大概是100毫秒。

在 root 扫描完成后，G1 进入了一个并发标记阶段。这个阶段也是完全后台进行的；GC 日志里面下面的信息代表这个阶段的开始和结束：
```
111.382: [GC concurrent-mark-start]
....
120.905: [GC concurrent-mark-end, 9.5225160 sec]
```
并发标记阶段是可以被打断的，比如这个过程中发生了 YGC 就会。这个阶段之后会有一个二次标记阶段和清理阶段：
```
120.910: [GC remark 120.959:
[GC ref-PRC, 0.0000890 secs], 0.0718990 secs]
[Times: user=0.23 sys=0.01, real=0.08 secs]
120.985: [GC cleanup 3510M->3434M(4096M), 0.0111040 secs]
[Times: user=0.04 sys=0.00, real=0.01 secs]
```
这两个阶段同样会暂停应用线程，但时间很短。接下来还有额外的一次并发清理阶段：
```
120.996: [GC concurrent-cleanup-start]
120.996: [GC concurrent-cleanup-end, 0.0004520]
```
到此为止，正常的一个 G1 周期已完成–这个周期主要做的是发现哪些区域包含可回收的垃圾最多（标记为 X ），实际空间释放较少。

##### 混合 GC

接下来 G1 执行一系列的混合 GC。这个时期因为会同时进行 YGC 和清理上面已标记为 X 的区域，所以称之为混合阶段，下面是一个混合 GC 执行的前后示意图：
![](http://image.ouyangsihai.cn/Flo526oI2G_UIbkT-Jibo5ZoX27u)

像普通的 YGC 那样、G1 完全清空掉 Eden 同时调整 survivor 区。另外，两个标记也被回收了，他们有个共同的特点是包含最多可回收的对象，因此这两个区域绝对部分空间都被释放了。这两个区域任何存活的对象都被移到了其他区域（和 YGC 存活对象晋升到 O 区类似）。这就是为什么 G1 的堆比 CMS 内存碎片要少很多的原因——移动这些对象的同时也就是在压缩对内存。下面是一个混合GC的日志：
```
79.826: [GC pause (mixed), 0.26161600 secs]
....
[Eden: 1222M(1222M)->0B(1220M)
Survivors: 142M->144M Heap: 3200M(4096M)->1964M(4096M)]
[Times: user=1.01 sys=0.00, real=0.26 secs]
```
上面的日志可以注意到 Eden 释放了 1222 MB、但整个堆的空间释放内存要大于这个数目。数量相差看起来比较少、只有 16 MB，但是要考虑同时有 survivor 区的对象晋升到 O 区；另外，每次混合 GC 只是清理一部分的 O 区内存，整个 GC 会一直持续到几乎所有的标记区域垃圾对象都被回收，这个阶段完了之后 G1 会重新回到正常的 YGC 阶段。周期性的，当O区内存占用达到一定数量之后 G1 又会开启一次新的并行 GC 阶段.

G1来源：https://blog.51cto.com/lqding/1770055

### 总结

这里把上面的这些垃圾回收器做个总结，看完这个，面试给面试官讲的时候思路就非常清晰了。

|收集器|串行、并行or并发|新生代/老年代|算法|目标|适用场景|
|------|------|------|------|------|------|
|**Serial**|串行|新生代|复制算法|响应速度优先|单CPU环境下的Client模式
|**Serial Old**|串行|老年代|标记-整理|响应速度优先|单CPU环境下的Client模式、CMS的后备预案
|**ParNew**|并行|新生代|复制算法|响应速度优先|多CPU环境时在Server模式下与CMS配合
|**Parallel Scavenge**|并行|新生代|复制算法|吞吐量优先|在后台运算而不需要太多交互的任务
|**Parallel Old**|并行|老年代|标记-整理|吞吐量优先|在后台运算而不需要太多交互的任务
|**CMS**|并发|老年代|标记-清除|一种以获取最短回收停顿时间为目标的收集器|互联网站或者B/S系统的服务端上的Java应用
|**G1**|并发|老年代|标记-整理|高吞吐量|面向服务器的垃圾收集器，主要针对配备多颗处理器及大容量内存的机器


## 内存泄漏和内存溢出，什么时候会出现，怎么解决？

内存泄漏：(Memory Leak)  不再会被使用的对象的内存不能被回收，就是内存泄露。

强引用所指向的对象不会被回收，可能导致内存泄漏，虚拟机宁愿抛出OOM也不会去回收他指向的对象。

意思就是你用资源的时候为他开辟了一块空间，当你用完时忘记释放资源了，这时内存还被占用着，一次没关系，但是内存泄漏次数多了就会导致内存溢出。

内存溢出：(Out Of Memory —— OOM) 指程序申请内存时，没有足够的内存供申请者使用，或者说，给了你一块存储 int 类型数据的存储空间，但是你却存储 long 类型的数据，那么结果就是内存不够用，此时就会报错 OOM ，即所谓的内存溢出，简单来说就是自己所需要使用的空间比我们拥有的内存大内存不够使用所造成的内存溢出。

内存的释放：即清理那些不可达的对象，是由 GC 决定和执行的，所以 GC 会监控每一个对象的状态，包括申请、引用、被引用和赋值等。释放对象的根本原则就是对象不会再被使用。

#### 内存泄漏的原因

- 静态集合类引起内存泄漏；

- 当集合里面的对象属性被修改后，再调用remove()方法时不起作用。JDK1.8 貌似修正了引用对象修改参数，导致hashCode变更的问题；

- 监听器 Listener 各种连接 Connection，没有及时关闭；

- 内部类和外部模块的引用（尽量使用静态内部类）；

- 单例模式（静态类持有引用，导致对象不可回收）；

#### 解决方法

- 尽早释放无用对象的引用，及时关闭使用的资源，数据库连接等；
- 特别注意一些像 HashMap 、ArrayList 的集合对象，它们经常会引发内存泄漏。当它们被声明为 static 时，它们的生命周期就会和应用程序一样长。
- 注意 事件监听 和 回调函数 。当一个监听器在使用的时候被注册，但不再使用之后却未被反注册。

#### 内存溢出的情况和解决方法

- OOM for Heap (java.lang.OutOfMemoryError: Java heap space)

此 OOM 是由于 JVM 中 heap 的最大值不满足需要，将设置 heap 的最大值调高即可，如，-Xmx8G。

- OOM for StackOverflowError (Exception in thread "main" java.lang.StackOverflowError)

如果线程请求的栈深度大于虚拟机所允许的最大深度，将抛出 StackOverflowError 异常。

如果虚拟机在扩展栈时无法申请到足够的内存空间，则抛出 OutOfMemoryError 异常。

检查程序是否有深度递归。

- OOM for Perm (java.lang.OutOfMemoryError: PermGen space)

调高 Perm 的最大值，即 -XX:MaxPermSize 的值调大。

- OOM for GC (java.lang.OutOfMemoryError: GC overhead limit exceeded)

此 OOM 是由于 JVM 在 GC 时，对象过多，导致内存溢出，建议调整 GC 的策略，在一定比例下开始 GC 而不要使用默认的策略，或者将新代和老代设置合适的大小，需要进行微调存活率。

改变 GC 策略，在老代 80% 时就是开始 GC ，并且将`-XX:SurvivorRatio（-XX:SurvivorRatio=8）`和`-XX:NewRatio（-XX:NewRatio=4）`设置的更合理。

- OOM for native thread created (java.lang.OutOfMemoryError: unable to create new native thread)

将 heap 及 perm 的最大值下调，并将线程栈调小，即 -Xss 调小，如：-Xss128k。

在 JVM 内存不能调小的前提下，将 -Xss 设置较小，如：-Xss:128k。

- OOM for allocate huge array (Exception in thread "main": java.lang.OutOfMemoryError: Requested array size exceeds VM limit)

此类信息表明应用程序试图分配一个大于堆大小的数组。例如，如果应用程序 new 一个数组对象，大小为 512M，但是最大堆大小为 256M，因此 OutOfMemoryError 会抛出，因为数组的大小超过虚拟机的限制。
1) 首先检查 heap 的 -Xmx 是不是设置的过小;
2) 如果 heap 的 -Xmx 已经足够大，那么请检查应用程序是不是存在 bug，例如：应用程序可能在计算数组的大小时，存在算法错误，导致数组的 size 很大，从而导致巨大的数组被分配。

- OOM for small swap (Exception in thread "main": java.lang.OutOfMemoryError: request <size> bytes for <reason>. Out of swap space? )

由于从 native 堆中分配内存失败，并且堆内存可能接近耗尽。

1) 检查 os 的 swap 是不是没有设置或者设置的过小;
2) 检查是否有其他进程在消耗大量的内存，从而导致当前的 JVM 内存不够分配。

- OOM for exhausted native memory (java.lang.OutOfMemoryErr java.io.FileInputStream.readBytes(Native Method))

从错误日志来看，在 OOM 后面没有提示引起 OOM 的原因，进一步查看 stack trace 发现，导致 OOM 的原因是由Native Method 的调用引起的，另外检查 Java heap ，发现 heap 的使用正常，因而需要考虑问题的发生是由于 Native memory 被耗尽导致的。

从根本上来说，解决此问题的方法应该通过检测发生问题时的环境下，native memory 为什么被占用或者说为什么 native memory 越来越小，从而去解决引起 Native memory 减小的问题。但是如果此问题不容易分析时，可以通过以下方法或者结合起来处理。

1) cpu 和 os 保证是 64 位的，并且 jdk 也换为 64 位的。
2) 将 java heap 的 -Xmx 尽量调小，但是保证在不影响应用使用的前提下。
3) 限制对 native memory 的消耗，比如：将 thread 的 -Xss 调小，并且限制产生大量的线程；限制文件的 io 操作次数和数量；限制网络的使用等等。

## Java 的类加载过程

JVM类加载机制分为五个部分：加载，验证，准备，解析，初始化，下面我们就分别来看一下这五个过程。其中加载、检验、准备、初始化和卸载这个五个阶段的顺序是固定的，而解析则未必。为了支持动态绑定，解析这个过程可以发生在初始化阶段之后。

![](http://image.ouyangsihai.cn/FpontTFCT65kRlJvGhuMsP9lkRkZ)


#### 加载

加载过程主要完成三件事情：

1. 通过类的全限定名来获取定义此类的二进制字节流
2. 将这个类字节流代表的静态存储结构转为方法区的运行时数据结构
3. 在堆中生成一个代表此类的 java.lang.Class 对象，作为访问方法区这些数据结构的入口。

#### 校验

此阶段主要确保 Class 文件的字节流中包含的信息符合当前虚拟机的要求，并且不会危害虚拟机的自身安全。

1. 文件格式验证：基于字节流验证。
2. 元数据验证：基于方法区的存储结构验证。
3. 字节码验证：基于方法区的存储结构验证。
4. 符号引用验证：基于方法区的存储结构验证。

#### 准备

为类变量分配内存，并将其初始化为默认值。（此时为默认值，在初始化的时候才会给变量赋值）即在方法区中分配这些变量所使用的内存空间。例如：

```java
public static int value = 123;
```

此时在准备阶段过后的初始值为0而不是 123 ；将 value 赋值为 123 的 putstatic 指令是程序被编译后，存放于类构造器方法之中。特例：

```java
public static final int value = 123;
```

此时 value 的值在准备阶段过后就是 123 。

#### 解析

把类型中的符号引用转换为直接引用。

* 符号引用与虚拟机实现的布局无关，引用的目标并不一定要已经加载到内存中。各种虚拟机实现的内存布局可以各不相同，但是它们能接受的符号引用必须是一致的，因为符号引用的字面量形式明确定义在Java虚拟机规范的 Class 文件格式中。
* 直接引用可以是指向目标的指针，相对偏移量或是一个能间接定位到目标的句柄。如果有了直接引用，那引用的目标必定已经在内存中存在

主要有以下四种：

1. 类或接口的解析
2. 字段解析
3. 类方法解析
4. 接口方法解析

#### 初始化

初始化阶段是执行类构造器方法的过程。方法是由编译器自动收集类中的类变量的赋值操作和静态语句块中的语句合并而成的。虚拟机会保证方法执行之前，父类的方法已经执行完毕。如果一个类中没有对静态变量赋值也没有静态语句块，那么编译器可以不为这个类生成()方法。

Java 中，对于初始化阶段，有且只有以下五种情况才会对要求类立刻“初始化”（加载，验证，准备，自然需要在此之前开始）：

1. 使用 new 关键字实例化对象、访问或者设置一个类的静态字段（被 final 修饰、编译器优化时已经放入常量池的例外）、调用类方法，都会初始化该静态字段或者静态方法所在的类。
2. 初始化类的时候，如果其父类没有被初始化过，则要先触发其父类初始化。
3. 使用 java.lang.reflect 包的方法进行反射调用的时候，如果类没有被初始化，则要先初始化。
4. 虚拟机启动时，用户会先初始化要执行的主类（含有main）
5. jdk 1.7后，如果 java.lang.invoke.MethodHandle的实例最后对应的解析结果是 REF_getStatic、REF_putStatic、REF_invokeStatic 方法句柄，并且这个方法所在类没有初始化，则先初始化。

## 聊聊双亲委派机制

![](http://image.ouyangsihai.cn/FoFkvh_XGVUdHkp3F6dYBpwE8tgb)

**工作过程**

如果一个类加载器收到了类加载器的请求，它首先不会自己去尝试加载这个类，而是把这个请求委派给父加载器去完成，每个层次的类加载器都是如此，因此，所有的加载请求最终都会传送到 Bootstrap 类加载器(启动类加载器)中，只有父类加载反馈自己无法加载这个请求(它的搜索范围中没有找到所需的类)时，子加载器才会尝试自己去加载。

**优点**

Java 类随着它的加载器一起具备了一种带有优先级的层次关系.

例如类 java.lang.Object ，它存放在 rt.jar 之中，无论哪一个类加载器都要加载这个类，最终都是双亲委派模型最顶端的 Bootstrap 类加载器去加载，因此 Object 类在程序的各种类加载器环境中都是同一个类。相反，如果没有使用双亲委派模型，由各个类加载器自行去加载的话，如果用户编写了一个称为 “java.lang.Object” 的类，并存放在程序的 ClassPath 中，那系统中将会出现多个不同的 Object 类，java类型体系中最基础的行为也就无法保证，应用程序也将会一片混乱。
