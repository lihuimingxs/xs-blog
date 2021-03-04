---
title: 必知必会面试题之Java IO 流
date: 2021-01-24
updated: 2021-03-24
categories:
- Java
tags:
- Java
- 面试
---

## 目录

不定期更新中……

- [IO 流的种类](#IO流的种类) 
- [常见的 IO 流](#常见的IO流) 
  - [字节流](#字节流) 
  - [字符流](#字符流) 
- [常见的 IO 类型](#常见的 IO 类型) 
  - [BIO](#BIO) 
  - [NIO](#NIO) 
  - [AIO](#AIO) 
- [NIO 的组成](#NIO的组成) 
- [什么是零拷贝](#什么是零拷贝) 

---

<!--more-->

---

## IO 流的种类

- 按照流的流向，可以分为**输入流**和**输出流**；
- 按照操作单元，可以分为**字节流**和**字符流**；
- 按照流的角色，可以分为**节点流**和**处理流**。

## 常见的 IO 流

### 字节流

- FileInputStream
- FileOutputStream
- PipedInputStream
- PipedOutputStream
- ByteArrayInputStream
- ByteArrayOutputStream
- BufferedInputStream
- BufferedOutputStream
- DataInputStream
- DataOutputStream
- ObjectInputStream
- ObjectOutputStream
- SequenceInputStream
- PrintOutputStream

### 字符流

- FileReader
- FileWriter
- PipedReader
- PipedWriter
- CharArrayReader
- CharArrayWriter
- BufferedReader
- BufferedWriter
- InputStreamReader

- OutputStreamWriter
- PrintWriter

## 常见的 IO 类型

### BIO

BIO 是指**同步阻塞 IO（Blocking I/O）**。一次数据的读取或写入会阻塞当前线程，直到本次数据传输结束。操作简单，适合活动连接数较小的情况。

### NIO

NIO 是在 Java 1.4 中引入的新的 I/O 模型，因为被称为 New IO。但随着技术的快速发展，NIO 也不再“新”了，因此，我们现在更习惯以它的特性来称其为：**同步非阻塞 IO（Non-Blocking I/O）**。

NIO 提供了 Channel、Selector、Buffer 等抽象，实现了多路复用。此外，NIO 还提供了 SocketChannel 和 ServerSocketChannel 两种不同的套接字通道实现，分别对应 BIO 中的 Socket 和 ServerSocket。

>  **NIO 并非只是非阻塞的**，NIO 同时支持阻塞、非阻塞两种模式，只是因为 NIO 主要就是为了提高 IO 性能而诞生的，所以强调了其核心特性：非阻塞。在日常使用中，我们也更为倾向于 NIO 的非阻塞模式，以获得更高的吞吐量和并发量。

### AIO

AIO 是在 Java 7 中引入的**异步非阻塞 IO（Asynchronous I/O）**。AIO 是基于事件和回调机制实现的，当操作发生后，会直接得到返回，释放 IO 资源，实际操作的执行则交给其他线程来处理，处理完成后通知相应的线程进行后续的操作。

## NIO 的组成

- 缓冲区（Buffer）：用来存储待传输的数据，通过 Channel 进行数据传输。

- 直接缓冲区（DirectByteBuffer）：使用堆外内存创建的缓冲区，可以减少一次堆内内存到堆外内存的数据拷贝。

  > 使用堆外内存创建和销毁缓冲区的成本更高且不可控，通常会使用内存池来提高性能。

- 通道（Channel）：用来建立数据传输需要的连接，并传输 Buffer 中的数据。

  > 数据虽然需要通过 Channel 进行传输，但 Channel 是不直接操作数据的，Channel 只负责建立连接并确认传输内容，实际数据的传输是通过

- 选择器（Selector）：用来管理 Channel 和分配

## 什么是零拷贝

在 Java 程序中，使用 **read() 或 write() 方法拷贝**，需要在堆内开辟内存空间存储文件流，再从堆内拷贝到堆外，最后从堆外拷贝到操作系统内核，由 DMA 读写到磁盘。期间需要经过两次复制，且用户态和内核态的交互，因此传输效率较慢。

而在操作系统中提供了 **mmap() 方法**，我们可以在程序中调用该方法，系统会直接在内核开辟内存空间，直接将文件流传输到内核开辟出的内存空间，由 DMA 读写到磁盘。该方法通过减少文件流的拷贝过程和用户态、内核态的交互，从而提高了文件传输的效率。我们把这种方法，称为“零拷贝”。

当然，零拷贝虽然可以提高文件传输效率，但也并非没有缺点的。由于程序直接传入内核内存空间，在发生 IO 异常、宕机等异常情况下，使用零拷贝有可能会导致数据流的丢失。