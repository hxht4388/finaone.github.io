# java基础
## JVM vs JDK vs JRE
    .class->机器码。在这一步 JVM 类加载器首先加载字节码文件，然后通过解释器逐行解释执行，这种方式的执行速度会相对比较慢。而且，有些方法和代码块是经常需要被调用的(也就是所谓的热点代码)，所以后面引进了 JIT 编译器，而 JIT 属于运行时编译。当 JIT 编译器完成第一次编译后，其会将字节码对应的机器码保存下来，下次可以直接使用。而我们知道，机器码的运行效率肯定是高于 Java 解释器的。这也解释了我们为什么经常会说 Java 是编译与解释共存的语言。
    HotSpot 采用了惰性评估(Lazy Evaluation)的做法，根据二八定律，消耗大部分系统资源的只有那一小部分的代码（热点代码），而这也就是 JIT 所需要编译的部分。JVM 会根据代码每次被执行的情况收集信息并相应地做出一些优化，因此执行的次数越多，它的速度就越快。JDK 9 引入了一种新的编译模式 AOT(Ahead of Time Compilation)，它是直接将字节码编译成机器码，这样就避免了 JIT 预热等各方面的开销。JDK 支持分层编译和 AOT 协作使用。但是 ，AOT 编译器的编译质量是肯定比不上 JIT 编译器的。
 ## java泛型擦除
 Java 在编译期间，所有的泛型信息都会被擦掉，这也就是通常所说类型擦除
 常用的通配符为： T，E，K，V，？
    ？ 表示不确定的 java 类型
    T (type) 表示具体的一个 java 类型
    K V (key value) 分别代表 java 键值中的 Key Value
    E (element) 代表 Element
## equals&hashcode
    为什么重写 equals 时必须重写 hashCode 方法？

    如果两个对象相等，则 hashcode 一定也是相同的。两个对象相等,对两个对象分别调用 equals 方法都返回 true。但是，两个对象有相同的 hashcode 值，它们也不一定是相等的 。因此，equals 方法被覆盖过，则 hashCode 方法也必须被覆盖。

    hashCode()的默认行为是对堆上的对象产生独特值。如果没有重写 hashCode()，则该 class 的两个对象无论如何都不会相等（即使这两个对象指向相同的数据）
## 拆箱装箱
    装箱其实就是调用了 包装类的valueOf()方法，拆箱其实就是调用了 xxxValue()方法。
    因此，
    Integer i = 10 等价于 Integer i = Integer.valueOf(10)
    int n = i 等价于 int n = i.intValue();
## 基本类型常量池
    Java 基本类型的包装类的大部分都实现了常量池技术。Byte,Short,Integer,Long 这 4 种包装类默认创建了数值 [-128，127] 的相应类型的缓存数据，Character 创建了数值在[0,127]范围的缓存数据，Boolean 直接返回 True Or False。
## pass by value
    void testString(String lhs){
        lhs = "world";
    }

    @Test
    void testString1(){
        String string = "hello";
        testString(string);
        System.out.println(string);
    }
## String
    在 Java 9 之后，String 、StringBuilder 与 StringBuffer 的实现改用 byte 数组存储字符串 

## 序列化
    transient 关键字的作用是：阻止实例中那些用此关键字修饰的的变量序列化；当对象被反序列化时，被 transient 修饰的变量值不会被持久化和恢复。

    关于 transient 还有几点注意：

    transient 只能修饰变量，不能修饰类和方法。
    transient 修饰的变量，在反序列化后变量值将会被置成类型的默认值。例如，如果是修饰 int 类型，那么反序列后结果就是 0。
    static 变量因为不属于任何对象(Object)，所以无论有没有 transient 关键字修饰，均不会被序列化。
    transient使用细节——被transient关键字修饰的变量真的不能被序列化吗？ 不是； 我们知道在Java中，对象的序列化可以通过实现两种接口来实现，若实现的是Serializable接口，则所有的序列化将会自动进行，若实现的是Externalizable接口，则没有任何东西可以自动序列化，需要在writeExternal方法中进行手工指定所要序列化的变量，这与是否被transient修饰无关。因此第二个例子输出的是变量content初始化的内容，而不是null。
    跟SQL不一样，在Java中null==null将返回true。
## BigDecimal
    我们在使用BigDecimal时，为了防止精度丢失，推荐使用它的 BigDecimal(String) 构造方法来创建对象。
    
## EnumSet
    EnumSet 是抽象类，其有两个实现：RegularEnumSet 、JumboEnumSet，选择哪一个取决于实例化时枚举中常量的数量。
    EnumSet类没有暴露任何构造器来创建该类的实例，程序应该通过它提供的类方法来创建EnumSet对象。
    EnumSet在内部以位向量的形式存储，这种存储形式非常紧凑、高效,因此EnumSet对象占用内存很小，而且运行效率很好。尤其是进行批量操作（如调用containsAll()和retainAll()方法）时，如果其参数也是EnumSet集合，则该批量操作的执行速度也非常快。
    EnumSet的集合元素也是有序的，EnumSet以枚举值在Enum类内的定义顺序来决定集合元素的顺序。
    加入null元素时会NPE

    EnumSet allOf(Class elementType): 创建一个包含指定枚举类里所有枚举值的EnumSet集合。
    EnumSet complementOf(EnumSet e): 创建一个其元素类型与指定EnumSet里元素类型相同的EnumSet集合，新EnumSet集合包含原EnumSet集合所不包含的、此类枚举类剩下的枚举值（即新EnumSet集合和原EnumSet集合的集合元素加起来是该枚举类的所有枚举值）。
    EnumSet copyOf(Collection c): 使用一个普通集合来创建EnumSet集合。
    EnumSet copyOf(EnumSet e): 创建一个指定EnumSet具有相同元素类型、相同集合元素的EnumSet集合。
    EnumSet noneOf(Class elementType): 创建一个元素类型为指定枚举类型的空EnumSet。
    EnumSet of(E first,E…rest): 创建一个包含一个或多个枚举值的EnumSet集合，传入的多个枚举值必须属于同一个枚举类。
    EnumSet range(E from,E to): 创建一个包含从from枚举值到to枚举值范围内所有枚举值的EnumSet集合。
## 集合

集合分为两大块：

java.util包下的非线程安全集合

java.util.concurrent下的线程安全集合。



### List
#### ArrayList
    DEFAULTCAPACITY_EMPTY_ELEMENTDATA EMPTY_ELEMENTDATA
    用来区分是调用无参构造还是使用空数组构造

    ArrayList(Collection<? extends E> c) -->
    Arrays.copyOf(U[] original,int newLength, Class<? extends T[]> newType) -->
    public static native void arraycopy(Object src,  int  srcPos,Object dest, int destPos,int length); 底层调用native方法

    trimToSize()修改这个ArrayList实例的容量是列表的当前大小。 应用程序可以使用此操作来最小化ArrayList实例的存储。

    ArrayList中的elementData为transient，序列化使用writeObject方法

    ensureCapacity() 在插入大量元素之前调用此方法确保分配足够的空间

    ArrayList实现randomAccess标记为可随机存取的，调用二分查找时会判断是否实现了该接口而调用不同的方法

    本地的JVM在调用ensureCapacity时填充Integer.MAX_VALUE会OOM Requested array size exceeds VM limit
    存放Integer.MAX_VALUE>>1时，OOM Java heap space
    本机能创建[Integer.MAX_VALUE>>3,Integer.MAX_VALUE>>2)之间的

    int newCapacity = oldCapacity + (oldCapacity >> 1),所以 ArrayList 每次扩容之后容量都会变为原来的 1.5 倍左右（oldCapacity 为偶数就是 1.5 倍，否则是 1.5 倍左右）(也有可能不是，ensureCapacity直接扩容)

ArrayList与LinkedList的实现和区别
    1.底层数据结构，ArrayList实现采用Object[]数组，LinkedList底层采用双向链表(1.6时为双向循环链表，1.7取消循环)
    2.插入和删除是否受元素位置的影响： ① ArrayList 采用数组存储，所以插入和删除元素的时间复杂度受元素位置的影响。 比如：执行add(E e)方法的时候， ArrayList 会默认在将指定的元素追加到此列表的末尾，这种情况时间复杂度就是 O(1)。但是如果要在指定位置 i 插入和删除元素的话（add(int index, E element)）时间复杂度就为 O(n-i)。因为在进行上述操作的时候集合中第 i 和第 i 个元素之后的(n-i)个元素都要执行向后位/向前移一位的操作。 ② LinkedList 采用链表存储，所以对于add(E e)方法的插入，删除元素时间复杂度不受元素位置的影响，近似 O(1)，如果是要在指定位置i插入和删除元素的话（(add(int index, E element)） 时间复杂度近似为o(n))因为需要先移动到指定位置再插入。
    3.是否支持快速随机访问，通过index linkedList不支持，ArrayList支持
    4.内存空间占用： ArrayList 的空 间浪费主要体现在在 list 列表的结尾会预留一定的容量空间，而 LinkedList 的空间花费则体现在它的每一个元素都需要消耗比 ArrayList 更多的空间（因为要存放直接后继和直接前驱以及数据）。


### Map

HashMap：了解其数据结构、hash冲突如何解决（链表和红黑树）、扩容时机、扩容时避免rehash的优化
#### HashMap
hashmap
    数据结构，1.8之前数组+链表 1.8变为数组+链表+红黑树
     JDK1.8 以后的 HashMap 在解决哈希冲突时有了较大的变化，当链表长度大于阈值（默认为 8）（将链表转换成红黑树前会判断，如果当前数组的长度小于 64，那么会选择先进行数组扩容，而不是转换为红黑树）时，将链表转化为红黑树，以减少搜索时间
    
    hashmap存储数据时只能存放一个key为null的，可存多个value为null的

    HashMap 默认的初始化大小为 16。之后每次扩充，容量变为原来的 2 倍。并且， HashMap 总是使用 2 的幂作为哈希表的大小。

LinkedHashMap：了解基本原理、哪两种有序、如何用它实现LRU

TreeMap：了解数据结构、了解其key对象为什么必须要实现Compare接口、如何用它实现一致性哈希



### Set

Set基本上都是由对应的map实现，简单看看就好



## 常见问题

hashmap如何解决hash冲突，为什么hashmap中的链表需要转成红黑树？

hashmap什么时候会触发扩容？

jdk1.8之前并发操作hashmap时为什么会有死循环的问题？

hashmap扩容时每个entry需要再计算一次hash吗？

hashmap的数组长度为什么要保证是2的幂？

如何用LinkedHashMap实现LRU？

如何用TreeMap实现一致性hash？

线程安全的集合

Collections.synchronized

了解其实现原理

CopyOnWriteArrayList

了解写时复制机制、了解其适用场景、思考为什么没有ConcurrentArrayList

ConcurrentHashMap

了解实现原理、扩容时做的优化、与HashTable对比。

BlockingQueue

了解LinkedBlockingQueue、ArrayBlockingQueue、DelayQueue、SynchronousQueue



## 常见问题

ConcurrentHashMap是如何在保证并发安全的同时提高性能？

ConcurrentHashMap是如何让多线程同时参与扩容？

LinkedBlockingQueue、DelayQueue是如何实现的？

CopyOnWriteArrayList是如何保证线程安全的？

## 并发

### synchronized

了解偏向锁、轻量级锁、重量级锁的概念以及升级机制、以及和ReentrantLock的区别

### CAS

了解AtomicInteger实现原理、CAS适用场景、如何实现乐观锁

### AQS

了解AQS内部实现、及依靠AQS的同步类比如ReentrantLock、Semaphore、CountDownLatch、CyclicBarrier等的实现

### ThreadLocal

了解ThreadLocal使用场景和内部实现

### ThreadPoolExecutor

了解线程池的工作原理以及几个重要参数的设置

## 常见问题

synchronized与ReentrantLock的区别？

乐观锁和悲观锁的区别？

如何实现一个乐观锁？

AQS是如何唤醒下一个线程的？

ReentrantLock如何实现公平和非公平锁是如何实现？

CountDownLatch和CyclicBarrier的区别？各自适用于什么场景？

适用ThreadLocal时要注意什么？比如说内存泄漏?

说一说往线程池里提交一个任务会发生什么？

线程池的几个参数如何设置？

线程池的非核心线程什么时候会被释放？

如何排查死锁？

推荐文章：

死磕Synchronized底层实现--概论（比较深入）

引用

了解Java中的软引用、弱引用、虚引用的适用场景以及释放机制

常见问题

软引用什么时候会被释放

弱引用什么时候会被释放

推荐文章：



Java引用类型原理剖析（比较深入）

类加载

了解双亲委派机制

常见问题

双亲委派机制的作用？

Tomcat的classloader结构

如何自己实现一个classloader打破双亲委派

IO

了解BIO和NIO的区别、了解多路复用机制



常见问题

同步阻塞、同步非阻塞、异步的区别？

select、poll、eopll的区别？

java NIO与BIO的区别？

reactor线程模型是什么?



JVM

GC

垃圾回收基本原理、几种常见的垃圾回收器的特性、重点了解CMS（或G1）以及一些重要的参数



### 内存区域

能说清jvm的内存划分



## 常见问题

CMS GC回收分为哪几个阶段？分别做了什么事情？

CMS有哪些重要参数？

Concurrent Model Failure和ParNew promotion failed什么情况下会发生？

CMS的优缺点？

有做过哪些GC调优？

为什么要划分成年轻代和老年代？

年轻代为什么被划分成eden、survivor区域？

年轻代为什么采用的是复制算法？

老年代为什么采用的是标记清除、标记整理算法

什么情况下使用堆外内存？要注意些什么？

堆外内存如何被回收？

jvm内存区域划分是怎样的？

推荐文章：JVM垃圾回收历险

