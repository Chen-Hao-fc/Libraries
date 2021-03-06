# python 并行计算

推荐书籍：Python Parallel Programming Cookbook, 2nd edition

配套代码库：[本书代码](https://github.com/PacktPublishing/Python-Parallel-Programming-Cookbook-Second-Edition)

![book image](https://images-na.ssl-images-amazon.com/images/I/71MY2Q7gQUL.jpg)

推荐网站：[Python Parallel Programming Cookbook 第一版中文翻译](https://python-parallel-programmning-cookbook.readthedocs.io/zh_CN/latest/index.html)

## 并行计算的内存架构

- 单指令，单数据 （SISD）
- 单指令，多数据 （SIMD）
- 多指令，单数据 （MISD）
- 多指令，多数据 （MIMD）

## python 并行程序的编程模型

参考链接：[【面试高频问题】线程、进程、协程](https://zhuanlan.zhihu.com/p/70256971)

### 并发与并行

- 并发：在操作系统中，某一时间段，几个程序在同一个CPU上运行，但在任意一个时间点上，只有一个程序在CPU上运行。
- 并行：当操作系统有多个CPU时，一个CPU处理A线程，另一个CPU处理B线程，两个线程互相不抢占CPU资源，可以同时进行，这种方式成为并行。
- 区别：
  - 并发只是在宏观上给人感觉有多个程序在同时运行，但在实际的单CPU系统中，每一时刻只有一个程序在运行，微观上这些程序是分时交替执行。
  - 在多CPU系统中，将这些并发执行的程序分配到不同的CPU上处理，每个CPU用来处理一个程序，这样多个程序便可以实现同时执行。
- 知乎上的例子：
  - 你吃饭吃到一半，电话来了，你一直到吃完了以后才去接，这就说明你不支持并发也不支持并行。
  - 你吃饭吃到一半，电话来了，你停了下来接了电话，接完后继续吃饭，这说明你支持并发。
  - 你吃饭吃到一半，电话来了，你一边打电话一边吃饭，这说明你支持并行。
- 并发的关键是你有处理多个任务的能力，不一定要同时。
- 并行的关键是你有同时处理多个任务的能力。

### 进程

一个进程好比是一个程序，它是资源分配的最小单位 。同一时刻执行的进程数不会超过核心数。不过如果问单核CPU能否运行多进程？答案又是肯定的。单核CPU也可以运行多进程，只不过不是同时的，而是极快地在进程间来回切换实现的多进程。举个简单的例子，就算是十年前的单核CPU的电脑，也可以聊QQ的同时看视频。

电脑中有许多进程需要处于「同时」开启的状态，而利用CPU在进程间的快速切换，可以实现「同时」运行多个程序。而进程切换则意味着需要保留进程切换前的状态，以备切换回去的时候能够继续接着工作。所以进程拥有自己的地址空间，全局变量，文件描述符，各种硬件等等资源。操作系统通过调度CPU去执行进程的记录、回复、切换等等。

### 线程

如果说进程和进程之间相当于程序与程序之间的关系，那么线程与线程之间就相当于程序内的任务和任务之间的关系。所以线程是依赖于进程的，也称为 「微进程」 。它是 程序执行过程中的最小单元 。

但是加上了线程之后，线程能够共享进程的大部分资源，并参与CPU的调度。意味着它能够在进程间进行切换，实现「并发」，从而反馈到使用上就是拖动进度条的同时，画面和声音都同步了。所以我们经常能听到的一个词是「多线程」，就是把一个程序分成多个任务去跑，让任务更快处理。不过线程和线程之间由于某些资源是独占的，会导致锁的问题。例如Python的GIL多线程锁。

### 进程与线程的区别

- 进程是CPU资源分配的基本单位，线程是独立运行和独立调度的基本单位（CPU上真正运行的是线程）。
- 进程拥有自己的资源空间，一个进程包含若干个线程，线程与CPU资源分配无关，多个线程共享同一进程内的资源。
- 线程的调度与切换比进程快很多。
- CPU密集型代码（各种循环处理、计算等等）：使用多进程。IO密集型代码（文件处理、网络爬虫等）：使用多线程。

### 上代码

- hello world
  - ex1: 最简单的线程和进程例子
  - ex2: 多进程与多线程的差异（资源占用率角度）
  - ex3: 多进程与多线程的差异（性能效率角度）
- 常见的几种多线程/进程的用法
  - ex4: 利用线程池、进程池处理大规模的数据计算
  - ex5: 利用 Queue 实现生产者-消费者模式
- 锁？ lock, mutex, condition_variable, semaphore

### 阻塞与非阻塞

- 阻塞是指调用线程或者进程被操作系统挂起。
- 非阻塞是指调用线程或者进程不会被操作系统挂起。

### 同步与异步

同步是阻塞模式，异步是非阻塞模式。

- 同步就是指一个进程在执行某个请求的时候，若该请求需要一段时间才能返回信息，那么这个进程将会一直等待下去，直到收到返回信息才继续执行下去；
- 异步是指进程不需要一直等下去，而是继续执行下面的操作，不管其他进程的状态。当有消息返回式系统会通知进程进行处理，这样可以提高执行的效率。
