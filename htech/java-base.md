# java基础

## 集合

集合分为两大块：

java.util包下的非线程安全集合

java.util.concurrent下的线程安全集合。



### List

ArrayList与LinkedList的实现和区别



### Map

HashMap：了解其数据结构、hash冲突如何解决（链表和红黑树）、扩容时机、扩容时避免rehash的优化

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
