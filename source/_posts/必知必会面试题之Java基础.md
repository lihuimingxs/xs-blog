---
title: 必知必会面试题之 Java 基础
date: 2021-01-24
updated: 2021-01-24
categories:
- Java
tags:
- Java
- 面试
---

## 目录

不定期更新中……

- [数据计量单位](#数据计量单位) 
- [面向对象三大特性](#面向对象三大特性) 
- [基础数据类型](#基础数据类型) 
- [注释格式](#注释格式) 

---

<!--more-->

---

- [访问修饰符](#访问修饰符) 
- [运算符](#运算符) 
  - [算数运算符](#算数运算符) 
  - [关系运算符](#关系运算符) 
  - [位运算符](#位运算符) 
  - [逻辑运算符](#逻辑运算符) 
  - [赋值运算符](#赋值运算符) 
  - [三目表达式](#三目表达式) 
  - [运算符优先级](#运算符优先级) 
- [拷贝](拷贝) 
  - [什么是浅拷贝？](#什么是浅拷贝？) 
  - [什么是浅拷贝？](#什么是浅拷贝？) 
- [重写与重载](#重写与重载) 
  - [什么是重写？](#什么是重写？) 
  - [什么是重载？](#什么是重载？) 
  - [重写与重载的区别](#重写与重载的区别) 
- [引用传递与值传递](#引用传递与值传递) 
- [类加载机制](#类加载机制) 
  - [什么是双亲委派？](#什么是双亲委派？) 
  - [如何打破双亲委派？](#如何打破双亲委派？) 
- [Java 内存模型](#Java内存模型) 
  - [缓存一致性协议](#缓存一致性协议) 
  - [缓存行](#缓存行) 
  - [缓存行对齐](#缓存行对齐) 
  - [合并写](#合并写) 
  - [Java 内存模型包括哪些东西？](#Java内存模型包括哪些东西？) 
  - [Java 内存模型中，哪些对象是现场私有的？哪些对象是线程公有的？](#Java内存模型中，哪些对象是现场私有的？哪些对象是线程公有的？) 
  - [Java 八大原子操作](#Java八大原子操作) 

### 数据计量单位

8bit(位)=1Byte(字节) 

1024Byte(字节)=1KB

1024KB=1MB

1024MB=1GB

1024GB=1TB

1024TB=PB

1024PB=1EB

1024EB=1ZB

1024ZB=1YB

1024YB=1BB

### 面向对象三大特性

封装：**隐藏不想对外暴露的信息**，提高安全性；**抽取公共代码**，提高可复用性。

继承：**继承为类的扩展提供了一种方式**。有利于修改公共属性或方法，父类修改，所有子类无需重复修改。

多态：类的**多态体现在重写和重载**，重写通过继承来实现，重载通过相同方法的不同参数来实现。

### 基础数据类型

| 数据类型 | 位数 | 取值范围                                                     | 可转类型                 |
| -------- | ---- | ------------------------------------------------------------ | ------------------------ |
| byte     | 8    | -128 ~ 127（-2^7 ~ 2^7-1）                                   |                          |
| short    | 16   | -32,768 ~ 32,767（-2^15 ~ 2^15-1）                           | int、long、float、double |
| int      | 32   | -2,147,483,648 ~ 2,147,483,647（-2^31 ~ 2^31-1）             | long、float、double      |
| long     | 64   | -9,223,372,036,854,775,808 ~ 9,223,372,036,854,775,807（-2^63 ~ 2^63-1） | int、long、float、double |
| float    | 32   |                                                              | double                   |
| double   | 64   |                                                              |                          |
| char     | 16   | \u0000 ~ \uffff（65 ~ 535）                                  | int、long、float、double |
| boolean  | 1    | true、false                                                  |                          |

### 注释格式

- 单行注释：

  ```java
  // this is a comment
  ```

- 多行注释：

  ```java
  /* this is a comment */
  ```

- 文档注释：

  ```java
  /**
   * this is a comment
   */
  ```

### 访问修饰符

| 修饰符    | 当前类 | 同包 | 子类 | 其他包 |
| --------- | ------ | ---- | ---- | ------ |
| private   | √      | ×    | ×    | ×      |
| default   | √      | √    | ×    | ×      |
| protected | √      | √    | √    | ×      |
| public    | √      | √    | √    | √      |

### 运算符

#### 算数运算符

| 操作符 | 描述                              | 例子                   |
| :----- | :-------------------------------- | :--------------------- |
| +      | 加法 - 相加运算符两侧的值         | A + B = 30             |
| -      | 减法 - 左操作数减去右操作数       | A – B = -10            |
| *      | 乘法 - 相乘操作符两侧的值         | A * B = 200            |
| /      | 除法 - 左操作数除以右操作数       | B / A = 2              |
| ％     | 取余 - 左操作数除以右操作数的余数 | B % A = 0              |
| ++     | 自增: 操作数的值增加1             | B++ = 21 或 ++B = 21   |
| --     | 自减: 操作数的值减少1             | B-- == 19 或 --B == 19 |

#### 关系运算符

| 运算符 | 描述                                                         | 例子            |
| :----- | :----------------------------------------------------------- | :-------------- |
| ==     | 检查如果两个操作数的值是否相等，如果相等则条件为真。         | (A == B) 为假。 |
| !=     | 检查如果两个操作数的值是否相等，如果值不相等则条件为真。     | (A != B) 为真。 |
| >      | 检查左操作数的值是否大于右操作数的值，如果是那么条件为真。   | (A > B) 为假。  |
| <      | 检查左操作数的值是否小于右操作数的值，如果是那么条件为真。   | (A < B) 为真。  |
| >=     | 检查左操作数的值是否大于或等于右操作数的值，如果是那么条件为真。 | (A >= B) 为假。 |
| <=     | 检查左操作数的值是否小于或等于右操作数的值，如果是那么条件为真。 | (A <= B) 为真。 |

#### 位运算符

| 操作符 | 描述                                                         | 例子                           |
| :----- | :----------------------------------------------------------- | :----------------------------- |
| &      | 与。如果相对应位都是 1，则结果为 1，否则为 0                 | (A & B) 得到12，即 0000 1100   |
| \|     | 或。如果相对应位都是 0，则结果为 0，否则为 1                 | (A \| B) 得到 61，即 0011 1101 |
| ^      | 异或。如果相对应位值相同，则结果为 0，否则为1                | (A ^ B) 得到 49，即 0011 0001  |
| ~      | 取反。翻转操作数的每一位，即 0 变成 1，1 变成 0。            | (~A) 得到 -61，即 1100 0011    |
| <<     | 左移。左操作数按位左移右操作数指定的位数。                   | A << 2 得到 240，即 1111 0000  |
| >>     | 右移。左操作数按位右移右操作数指定的位数。                   | A >> 2 得到 15，即 1111        |
| >>>    | 无符号右移。左操作数的值按右操作数指定的位数右移，移动得到的空位以零填充。 | A>>>2 得到 15，即 0000 1111    |

#### 逻辑运算符

| 操作符 | 描述                                                         | 例子              |
| :----- | :----------------------------------------------------------- | :---------------- |
| &&     | 逻辑与，也称短路与。当且仅当两个操作数都为真，条件才为真。若第一个操作数为假，则第二个操作数不再判断。 | (A && B) 为假。   |
| \|\|   | 逻辑或，也称短路或。如果任何两个操作数任何一个为真，条件为真。若第一个操作数为假，则第二个操作数不再判断。 | (A \|\| B) 为真。 |
| !      | 逻辑非。用来反转操作数的逻辑状态。如果条件为 true，则使用逻辑非运算符将得到 false。 | !(A && B) 为真。  |

#### 赋值运算符

| 操作符 | 描述                                                         | 例子                                    |
| :----- | :----------------------------------------------------------- | :-------------------------------------- |
| =      | 简单的赋值运算符，将右操作数的值赋给左侧操作数               | C = A + B 将把 A + B 得到的值赋给 C     |
| +=     | 加和赋值操作符，它把左操作数和右操作数相加赋值给左操作数     | C += A 等价于 C = C + A                 |
| -=     | 减和赋值操作符，它把左操作数和右操作数相减赋值给左操作数     | C -= A 等价于 C = C - A                 |
| *=     | 乘和赋值操作符，它把左操作数和右操作数相乘赋值给左操作数     | C *= A 等价于 C = C * A                 |
| /=     | 除和赋值操作符，它把左操作数和右操作数相除赋值给左操作数     | C /= A，C 与 A 同类型时等价于 C = C / A |
| ％=    | 取模和赋值操作符，它把左操作数和右操作数取模后赋值给左操作数 | C ％= A 等价于 C = C ％ A               |
| <<=    | 左移位赋值运算符                                             | C <<= 2 等价于 C = C << 2               |
| >>=    | 右移位赋值运算符                                             | C >>= 2 等价于 C = C >> 2               |
| &=     | 按位与赋值运算符                                             | C &= 2 等价于 C = C & 2                 |
| ^=     | 按位异或赋值操作符                                           | C ^ = 2 等价于 C = C ^ 2                |
| \|=    | 按位或赋值操作符                                             | C \| = 2 等价于 C = C \| 2              |

#### 三目表达式

```java
// 若 a == b 成立，返回 true，否则返回 false
boolean flag = (a == b) ? true : false
```

#### 运算符优先级

>  所谓“好记性不如烂笔头”。实际开发中，尽量使用括号来明确优先级，提高代码可读性，而非使用复杂的运算符复合运算。
>
>  如：((x++) && (y + 1) || z == 0)

| 优先级 | 运算符                                           | 结合性   |
| ------ | ------------------------------------------------ | -------- |
| 1      | ()、[]、{}                                       | 从左向右 |
| 2      | !、+、-、~、++、--                               | 从右向左 |
| 3      | *、/、%                                          | 从左向右 |
| 4      | +、-                                             | 从左向右 |
| 5      | «、»、>>>                                        | 从左向右 |
| 6      | <、<=、>、>=、instanceof                         | 从左向右 |
| 7      | ==、!=                                           | 从左向右 |
| 8      | &                                                | 从左向右 |
| 9      | ^                                                | 从左向右 |
| 10     | \|                                               | 从左向右 |
| 11     | &&                                               | 从左向右 |
| 12     | \|\|                                             | 从左向右 |
| 13     | ?:                                               | 从右向左 |
| 14     | =、+=、-=、*=、/=、&=、\|=、^=、~=、«=、»=、>>>= | 从右向左 |

### 拷贝

#### 什么是浅拷贝？

被复制对象的所有变量值与原对象相同，但引用变量仍然指向原来的对象。即浅拷贝只复制对象本身，而不复制对象中引用的对象。

示例：

```java
Teacher teacher = new Teacher();
teacher.setName("赵大");
teacher.setAge(42);

Student student1 = new Student();
student1.setName("张三");
student1.setAge(21);
student1.setTeacher(teacher);

Student student2 = (Student) student1.clone();
System.out.println("李四");
System.out.println(student2.getName());
System.out.println(student2.getAge());
System.out.println(student2.getTeacher().getName());
System.out.println(student2.getTeacher().getAge());

System.out.println("修改老师的信息后-------------");
// 修改老师名称
teacher.setName("John");
// 两个学生的老师均发生变化
System.out.println(student1.getTeacher().getName());
System.out.println(student2.getTeacher().getName());
```

#### 什么是深拷贝？

深拷贝是一个整个独立的对象拷贝，深拷贝会拷贝所有的属性，并拷贝属性指向的动态分配的内存。

深拷贝的方法包括：

1. 重写 clone() 方法

   ```java
   public class Student implements Cloneable {
   
     private String  name;
     private Integer age;
     private Teacher teacher;
     
     // 省略 get/set 方法
   
     @Override
     protected Object clone() throws CloneNotSupportedException {
       Student3 student = (Student3) super.clone();
       // 复制一个新的 Teacher 对象实例，并设置到新的 student 对象实例中
       student.setTeacher((Teacher2) student.getTeacher().clone());
       return student;
     }
   }
   ```

2. 使用序列化实现

   ```java
   public class Teacher implements Serializable {
     private String name;
     private int age;
     
     // 省略 get/set 方法
   }
   
   class Student implements Serializable {
       private String name;
       private int age;
       private Teacher3 teacher;
   
       // 省略 get/set 方法
   
       public Object deepClone() throws Exception {
           // 序列化
           ByteArrayOutputStream bos = new ByteArrayOutputStream();
           ObjectOutputStream oos = new ObjectOutputStream(bos);
           oos.writeObject(this);
   
           // 反序列化
           ByteArrayInputStream bis = new ByteArrayInputStream(bos.toByteArray());
           ObjectInputStream ois = new ObjectInputStream(bis);
   
           return ois.readObject();
       }
   }
   ```

### 重写与重载

#### 什么是重写？

重写是指子类对父类允许访问的方法进行重新编写，返回值和形参都不能改变。**即方法入参出参不变，实现逻辑重写**。

示例：

```java
public class OverrideParent {

  public void m1(String var) {
    System.out.println(var);
  }
}

public class OverrideChild extends OverrideParent {

  /**
   * 重写父类的 m1() 方法
   */
  @Override
  public void m1(String var) {
    System.out.println(var);
  }

  /**
   * 子类的 m1() 方法：与父类 m1() 方法的形参不同
   */
  public void m1(int var) {
    System.out.println(var);
  }

  /**
   * 子类的 m1() 方法：与父类 m1() 方法的形参、返回值不同
   */
  public String m1() {
    System.out.println("var");
    return "var";
  }
}
```

#### 什么是重载？

重载是指一个类中存在多个同名方法，且方法的形参不同。**即方法名称相同、形参不同**。

示例：

```java
public class OverloadClass {

  public void m1() {
    System.out.println("key");
  }

  public void m1(String key) {
    System.out.println(key);
  }

  public void m1(String key, Integer value) {
    System.out.println(key);
  }
}
```

#### 重写与重载的区别

1. 重写要求方法名、入参、返回值相同，重载只同名方法的入参不同（类型、个数、顺序至少有一个不同）。
2. 重写要求子类不能缩小父类方法的访问权限，重载与访问权限无关。
3. 重写要求子类方法不能抛出比父类方法更多的异常（但子类方法可以不抛出异常），重载与异常范围无关。
4. 重写是子类对父类方法的覆盖行为，重载是一个类的多态性。
5. 重写方法不能被定义为 final，重载方法可以被定义为 final。

### 类加载机制

#### 什么是双亲委派？

在类加载过程中，子类会先去父类查找，如果找到，则从父类缓存加载。如果没找到，再由父类指派子类进行加载。

双亲委派机制主要出于安全来考虑。比如自定义 java.lang.String，如果不先去父类查找，相当于 Bootstrap 加载器的 java.lang.String 被篡改了。

#### 如何打破双亲委派？

1. 重写 loadClass() 方法

   > JDK 1.2 之前，自定义 ClassLoader 都必须重写 loadClass()

2. ThreadContextClassLoader 可以实现基础类调用实现类代码，通过 thread.setContextClassLoader 指定

3. 热启动，热部署

   > OSGI、Tomcat 都有自己的模块指定 Classloader（可以加载同一类库的不同版本）

### Java 内存模型

#### 缓存一致性协议

现代 CPU 的数据一致性实现 = 缓存锁(MESI 等) + 总线锁。缓存一致性协议一般是指缓存锁层面的协议，目前缓存一致性协议的实现有很多种，比较常见的就是 Intel 所使用 **MESI** 协议。

MESI 协议定义了四种状态，分别是 Modified、Exclusive、Shared 和 Invalid。

- Modified 状态：该Cache line有效，数据被修改且未同步到内存，数据和内存数据不一致，数据只存在于本 Cache 中。
- Exclusive 状态：该Cache line有效，数据由单 CPU 独占，数据和内存数据一致，数据只存在于本 Cache 中。
- Shared 状态：该Cache line有效，数据由所有 CPU 共享，数据和内存数据一致，数据存在于所有 Cache 中。
- Invalid 状态：该Cache line无效。

#### 缓存行

读取缓存以 Cache Line 为基本单位，目前 64 bytes。

位于同一缓存行的两个不同数据，被两个不同 CPU 锁定，产生互相影响的伪共享问题，使用缓存行的对齐能够有效解决伪共享问题，提高处理效率。

#### 缓存行的对齐

```java
//一个缓存行64个字节，设置56个的占位符，令要插入的数据单独占用一行缓存行
public static class Padding {
	public volatile long p1,p2,p3,p4,p5,p6,p7;
}
public static class T extends Padding {
  public volatile long x = 0L;
}
public static T[] arr = new T[2];
static {
  arr[0] = new T();
  arr[1] = new T();
}
public static void main(String[] args) throws Exception {
  Thread t1 = new Thread(() -> {
    for (long i = 0; i < 1000_0000L; i++) {
      arr[0].x = i;
    }
  });
  Thread t2 = new Thread(() -> {
    for (long i = 0; i < 1000_0000L; i++) {
      arr[0].x = i;
    }
  });
  t1.start();
  t2.start();
  t1.join();
  t1.join();
  
}
```

- 使用缓存行对齐的开源软件：Disruptor（号称单机效率最高的队列）

#### 合并写

如果 CPU 需要访问的地址 hash 之后并不在缓存行（cache line）中，那么缓存中对应位置的缓存行（cache line）会失效，以便让新的值可以取代该位置的现有值。例如，如果我们有两个地址，通过 hash 算法 hash 到同一缓存行，那么新的值会覆盖老的值。

当 CPU 执行存储指令（store）时，它会尝试将数据写到离 CPU 最近的 L1 缓存。如果这时出现缓存失效，CPU 会访问下一级缓存。这时无论是英特尔还是许多其他厂商的 CPU 都会使用被称为“合并写（write combining）”的技术。

当请求 L2 缓存行的所有权的时候，最典型的是将处理器的 store buffers 中某一项写入内存的期间， 在缓存子系统（cache sub-system）准备好接收、处理的数据的期间，CPU 可以继续处理其他指令。当数据不在任何缓存层中缓存时，将获得最大的优势。

当连串的写操作需要修改相同的缓存行时，会变得非常有趣。在修改提交到 L2 缓存之前，这连串的写操作会首先合并到缓冲区（buffer）。 这些 64 字节的缓冲（buffers ）维护在一个 64 位的区域中，每一个字节（byte）对应一个位（bit），当缓冲区被传输到外缓存后，标志缓存是否有效。随后，硬件在读取缓存之前会先读取缓冲区。

如果我们可以在缓冲区被传输到外缓存之前能够填补这些缓冲区（buffers ），那么我们将大大提高传输总线的效率。由于这些缓冲区的数量是有限的，并且它们根据 CPU 的型号有所不同。例如在 Intel CPU，你只能保证在同一时间拿到 4 个。这意味着，在一个循环中，你不应该同时写超过 4 个截然不同的内存位置，否则你讲不能从合并写（write combining）的中受益。

#### Java 内存模型包括哪些东西？

程序计数器、方法区、本地方法栈、虚拟机方法栈、堆。

#### Java 内存模型中，哪些对象是线程私有的？哪些对象是线程公有的？

程序计数器、本地方法栈、虚拟机方法栈是线程私有的，方法区、堆是线程公有的。

#### 如何保证特定情况下不乱序

**硬件层面：使用内存屏障**

- **sfence**:  store| 在sfence指令前的写操作当必须在sfence指令后的写操作前完成。
- **lfence**：load | 在lfence指令前的读操作当必须在lfence指令后的读操作前完成。
- **mfence**：mix | 在mfence指令前的读写操作当必须在mfence指令后的读写操作前完成。

> 原子指令，如x86上的”lock …” 指令是一个Full Barrier，执行时会锁住内存子系统来确保执行顺序，甚至跨多个CPU。Software Locks通常使用了内存屏障或原子指令来实现变量可见性和保持程序顺序

**JVM层面：使用 JSR133 规范**

- LoadLoad屏障：

  对于这样的语句 Load1; LoadLoad; Load2， 在 Load2 及后续读取操作要读取的数据被访问前，保证 Load1 要读取的数据被读取完毕。

- StoreStore屏障：

  对于这样的语句 Store1; StoreStore; Store2，在 Store2 及后续写入操作执行前，保证 Store1 的写入操作对其它处理器可见。

- LoadStore屏障：

  对于这样的语句 Load1; LoadStore; Store2，在 Store2 及后续写入操作被刷出前，保证 Load1 要读取的数据被读取完毕。

- StoreLoad屏障：

  对于这样的语句 Store1; StoreLoad; Load2，在 Load2 及后续所有读取操作执行前，保证 Store1 的写入对所有处理器可见。

#### java 八大原子操作

> 最新的 JSR-133 已经放弃了这种描述，但 JMM 没有变化。

**lock**：主内存，标识变量为线程独占

**unlock**：主内存，解锁线程独占变量

**read**：主内存，读取内容到工作内存

**write**：主内存，写变量值

**load**：工作内存，read 后的值放入线程本地变量副本

**use**：工作内存，传值给执行引擎

**assign**：工作内存，执行引擎结果赋值给线程本地变量

**store**：工作内存，存值到主内存给 write 备用

