---
title: 性能优化类别(02)
author:
  name: superhsc
  link: https://github.com/happymaya
date: 2019-03-08 23:33:00 +0800
categories: [Java, 性能优化]
tags: [性能优化, Performance Optimization]
math: true
mermaid: true
---

性能优化根据优化的类别，分为**业务优化**和**技术优化**。业务优化产生的效果也是非常大的，但它属于产品和管理的范畴。同作为程序员，在平常工作中，我们面对的优化方式，主要是通过一系列的技术手段，来完成对既定的优化目标。这一系列的技术手段，大体归纳为如图以下 7 类：

![这是一张图片](http://assets.processon.com/chart_image/621e36b21e085329a58ae9bc.png)

## 1 复用优化
在写代码的时候，经常会发现有很多重复的代码可以提取出来，做成公共方法。这样，在下次用时，就不用在费劲写一遍。这种思想就是**复用**。

上面描述的是编码逻辑上的优化，对于数据存取来说，有同样复用情况。
在软件系统中，谈到数据复用，首先想到就是**缓冲**和**缓存。**这两个的意义是完全不同的：
- **缓冲（Buffer）**
   - 常见于对数据的暂存，然后批量传输或者写入
   - 多使用顺序方式，用来缓解设备之间频繁、缓慢地随机写
   - 主要针对的是**写操作**
- **缓存（Cache）**
   - 常见于对已读数据的复用
   - 通常将它们缓存在相对高速的区域
   - 主要针对的是**读操作**

类似的，**池化**是对**对象**而言，比如**数据库连接池**、**线程池**等。在 Java 中使用得非常频繁。

由于这些对象的创建和销毁成本比较大，在使用之后，将这部分对象暂时存储，下次使用时就不用再走一遍耗时的初始化操作。

## 2 结果集优化
有一个比较直观的例子，都知道 XML 的表现形式是非常好的，那么为什么还有 JSON 呢？出了书写简单一些，一个重要的原因就是它的体积变小了，传输效率和解析效率变高了，像 Google 的 Protobuf，体积就更小了一些。虽然可读性降低了，但在一些高并发场景下（RPC），能够显著提高效率。这就是典型的对结果集优化。

由于目前的 Web 服务，都是 C/S 模式。数据从服务器传输到客户端，需要分发多分，这个数据量是急剧膨胀的，每减少一小部分存储，都会有比较大的传属性和成本提升。

> 像 Nginx ，一般都会开启 GZIP 压缩，使得传输的内容保持紧凑。客户端只需要一小部分计算能力，就可以方便解压，由于这个操作是分散的，因此性能损失是固定的
{: .prompt-warning }

了解上面的道理，就能得出对结果集优化的一般思路：
- 尽量保持返回数据的精简。一些客户端不需要的字段，那就在代码中，或 SQL 查询中，直接去掉。
- 对于时效性要求不高，但对处理能力有高要求的业务，吸取缓冲区的经验，尽量减少网络连接的交互，采用批处理的方式，增加处理速度
- 结果集合很可能会有二次使用，可能会把它加入缓存中，但依然在速度上有所欠缺，此时，就需要对数据集合进行处理优化，采用索引或 Bitmap 位图等方式，加快数据访问速度。

## 3 高效实现
在平时编码中，尽量使用设计理念良好、性能优越的组件。比如

- 有了 Netty ，就不用比较老的 Mina 组件
- 设计系统时，从性能因素考虑，不要选 SOAP 这样比较耗时的协议
- 一个好的语法分析器（如 JavaCC），其效率就会比正则表达式高很多

总之，如果通过测试分析，找到了系统的瓶颈点，就要把关键的组件，使用更加高效的组件进行替换。
在这种情况下，适配器模式是非常重要的。这也是为什么很多公司喜欢在现有的组件之上，再抽象一层自己的（当在底层组件进行切换的时候，上层的应用并无感知）。

## 4 算法优化
算法能够显著提高复杂业务性能，但在实际的业务中，往往都是变种。由于存储越来越便宜，在 CPU 非常紧张的业务中，往往采用空间换取时间的方式，来加快处理速度。

算法属于代码调优，代码调优涉及很多编码技巧，需要对所使用语言的 API 非常熟悉。

对算法、数据结构的灵活使用，是代码优化的一个重要内容。比如，常用的降低时间复杂度的方式，就有递归、二分、排序、动态规划等。

一个优秀的实现，对系统的影响是非常大的。比如：

- 作为 List 的实现，LinkedList 和 ArrayList 在随机访问的性能上，差了好几个数量级；
- CopyOnWriteList 采用写时复制的方式，可以显著降低读多写少场景下锁冲突。什么时候使用同步，什么时候是线程安全的，对编码能力有较高的要求

这部分的知识，就需要在平常的工作中注意积累。

## 5 计算优化
### 5.1 并行执行
目前 CPU 发展速度非常快，绝大多数硬件，都是多核。加快某个任务的执行，最快最优解的解决方式，就是并行执行。并行执行有以下三种模式：
1. **多机**；采用负载均衡，将流量或者大的计算拆分成多个部分，同时进行处理。比如，Hadoop 通过 MapReduce 的方式，将任务打散，多机同时进行计算；
2. **多进程**；比如 Nginx ，采用 NIO 编程模型，Master 统一管理 Worker 进程，然后由 Worker 进程进行真正的请求代理；
3. **多线程**；这是 Java 程序员接触最多的。比如 Netty ，采用 Reactor 编程模型，同样使用 NIO ，但它是基于线程的。Boss 线程用来接收请求，然后调度给相应的 Worker 线程进行真正的业务计算

像 Golang 这样的语言，有更加轻量级的协程（Coroutine），协程是一种比线程更加轻量级的存在。但目前在 Java 中还不太成熟，但本质上也是对于多核的应用，使得任务并行执行。

### 5.2 变同步为异步
再一种对计算的优化，就是变同步为异步，这通常设计编程模型的改变。

同步请求，请求会一直阻塞，直到有成功，或者失败结果的返回。虽然它的编程模型简单，但应对突发的、时间段倾斜的浏览，问题就特别大，请求很容易失败。

异步操作可以方便地支持横向扩容，也可以缓解瞬时压力，使请求变得平滑。

同步请求，就像拳头打在钢板上；异步请求，就像拳头打在海绵上，富有弹性的，体验更友好。

### 5.3 惰性加载
最后一种，就是使用常见的设计模式来优化业务，提高体验，比如单例模式、代理模式等。

## 6 资源冲突优化
在平常的开发中，会涉及很多共享资源。
这些共享资源：
- 有的是单机的，比如一个 HashMap
- 有的是外部存储，比如一个数据库行
- 有的是单个资源，比如 Redis 某个 key 的Setnx
- 有的是多个资源的协调，比如事务、分布式事务等。

现实中的性能问题，和锁相关的问题是非常多的。大多数会想到：
- 数据库的行锁、表锁、Java 中的各种锁等
- 在更底层，比如 CPU 命令级别的锁、JVM 指令级别的锁、操作系统内部锁等，可以说无处不在。

只有并发，才能产生资源冲突。也就是在同一时刻，只能有一个处理请求能够获取到共享资源。解决资源冲突的方式，就是**加锁**。再比如事务，在本质上也是一种锁。

按照锁级别，锁可分为：
- 乐观锁
- 悲观锁，乐观锁在效率上肯定是更高一些；

按照锁类型，锁又分为：
- 公平锁
- 非公平锁
- 二者在对任务的调度上，有一些细微的差别。

对资源的争用，会造成严重的性能问题，所以会有一些针对无锁队列之类的研究，对性能的提升也是巨大的。

## 7 JVM 优化
Java 是运行在 JVM 虚拟机之上，它的诸多特性，就要受到 JVM 的制约。

对 JVM 虚拟机进行优化，也能在一定程度上能够提升 JAVA 程序的性能。如果参数配置不当，甚至会造成 OOM 等比较严重的后果。

目前被广泛使用的垃圾回收器是 G1，通过很少的参数配置，内存即可高效回收。
CMS 垃圾回收器已经在 Java 14 中被移除，由于它的 GC 时间不可控，有条件应该尽量避免使用。

JVM 性能调优涉及方方面面的取舍，是牵一发而动全身，需要全盘考虑各方面的影响。

了解 JVM 内部的一些运行原理是特别重要的，有益于我们加深对代码更深层次的理解，帮助我们书写出更高效的代码。