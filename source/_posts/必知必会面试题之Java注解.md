---
title: 必知必会面试题之 Java 注解
date: 2021-02-24
updated: 2021-02-24
categories:
- Java
tags:
- Java
- 面试
---

## 目录

不定期更新中……

- [元注解](#元注解) 
  - [@Documented](#@Documented) 
  - [@Indexed](#@Indexed) 
  - [@Retention](#@Retention) 

  - [@Target](#@Target) 

---

<!--more-->

---

- [常用注解](#常用注解) 
  - [@Deprecated](#@Deprecated) 

  - [@FunctionalInterface](#@FunctionalInterface) 
  - [@Override](#@Override) 
  - [@PostConstruct](#@PostConstruct) 
  - [@SafeVarargs](#@SafeVarargs) 
  - [@SuppressWarnings](#@SuppressWarnings) 

- [附录](#附录) 
  - [ElementType](#ElementType) 
  - [RetentionPolicy](#RetentionPolicy) 
  - [@SuppressWarnings 关键字](#@SuppressWarnings关键字) 



## 元注解

### @Documented

仅用在注解类上，表示在使用 javadoc 工具生成帮助文档时，使用该注解的类会在 API 文档中展示该注解。

**注解版本：1.5+** 

**场景举例：** 

- 创建一个注解类 TestAnnotation

  ```java
  package com.xs.annotation;
  
  import java.lang.annotation.Documented;
  import java.lang.annotation.ElementType;
  import java.lang.annotation.Target;
  
  @Documented
  @Target(ElementType.TYPE)
  public @interface TestAnnotation {
  
    public String value() default "javadoc";
  }
  ```

- 创建一个使用该注解的类 DocumentTest

  ```java
  package com.xs.annotation;
  
  @TestAnnotation
  public class DocumentedTest {
  }
  ```

- 生成 javadoc（使用 javadoc 命令 或 使用 eclipse、IDEA 等 IDE 提供的 javadoc 生成工具）

- 打开生成的 API 文档（/doc/index.html），如下：

  [外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-wKKTIUib-1614172467381)(必知必会之Java注解.assets/image-20210219181622050.png)]

- 若删除注解类 TestAnnotation 中的 @Documented 注解，再次生成 javadoc，**注解消失**。

  [外链图片转存失败,源站可能有防盗链机制,建议将图片保存下来直接上传(img-eBIAda2g-1614172467384)(必知必会之Java注解.assets/image-20210219182152788.png)]

### @Inherited

仅用在注解类上，被它修饰的注解具有继承性。也就是说，在一个类上使用被 @Inherited 标注的注解，其子类也会继承这些被 @Inherited 标注的注解。

**注解版本：1.5+** 

**场景举例：** 

- 创建一个带有 @Inherited 的注解类 InheritedAnnotation

  ```java
  package com.xs.annotation;
  
  import java.lang.annotation.ElementType;
  import java.lang.annotation.Inherited;
  import java.lang.annotation.Target;
  
  @Inherited
  @Target(ElementType.TYPE)
  @Retention(RetentionPolicy.RUNTIME)
  public @interface InheritedAnnotation {
  
    public String value() default "Inherited";
  }
  ```

- 创建一个使用该注解的类 InheritedParent

  ```java
  package com.xs.annotation;
  
  import com.xs.annotation.TestInheritedAnnotation;
  
  @InheritedAnnotation(value="parent")
  public class InheritedParent {
    
  }
  ```

- 为 InheritedParent 类创建子类 InheritedChild

  ```java
  package com.xs.annotation;
  
  import com.xs.annotation.InheritedParent;
  
  public class InheritedChild extends InheritedParent {
    
    public static void main(String[] args) {
  		Class<InheritedChild> child = InheritedChild.class;
  		InheritedAnnotation annotation = child.getAnnotation(InheritedAnnotation.class);
  		System.out.println(annotation.value());
  	}
  }
  ```

- 运行 main 方法，输出如下。

  ```
  parent
  ```

### @Retention

仅用在注解类上，用来描述注解保留的时间范围。一共有三种策略，定义在 [RetentionPolicy](#RetentionPolicy) 枚举中，分别是：源文件保留、编译期保留、运行期保留，默认值为编译期保留。运行期保留可以用来获取注解信息。

**注解版本：1.5+** 

**场景举例：** 

- 分别实现三种策略

  ```java
  package com.xs.annotation.meta;
  
  import java.lang.annotation.Retention;
  import java.lang.annotation.RetentionPolicy;
  
  @Retention(RetentionPolicy.SOURCE)
  public @interface SourcePolicy {
   
  }
  
  @Retention(RetentionPolicy.CLASS)
  public @interface ClassPolicy {
   
  }
  
  @Retention(RetentionPolicy.RUNTIME)
  public @interface RuntimePolicy {
   
  }
  ```

- 创建一个类，并使用以上三种注解去注解三个方法

  ```java
  package com.xs.annotation.meta;
  
  public class RetentionTest {
   
  	@SourcePolicy
  	public void sourcePolicy() {
  	}
   
  	@ClassPolicy
  	public void classPolicy() {
  	}
   
  	@RuntimePolicy
  	public void runtimePolicy() {
  	}
  }
  ```

- 生成字节码文件

  ```shell
  javap -verbose RetentionClass
  ### 以下为输出结果 ###
  警告: 二进制文件RetentionClass包含com.xs.annotation.meta.RetentionClass
  Classfile /Users/lihuiming/git/xs/xs-technology/xs-learning-annotation/target/classes/com/xs/annotation/meta/RetentionClass.class
    Last modified 2021-2-20; size 709 bytes
    MD5 checksum 88516f888e7e83d00ffe708e32d852a0
    Compiled from "RetentionClass.java"
  public class com.xs.annotation.meta.RetentionClass
    minor version: 0
    major version: 52
    flags: ACC_PUBLIC, ACC_SUPER
  Constant pool:
     #1 = Methodref          #3.#20         // java/lang/Object."<init>":()V
     #2 = Class              #21            // com/xs/annotation/meta/RetentionClass
     #3 = Class              #22            // java/lang/Object
     #4 = Utf8               <init>
     #5 = Utf8               ()V
     #6 = Utf8               Code
     #7 = Utf8               LineNumberTable
     #8 = Utf8               LocalVariableTable
     #9 = Utf8               this
    #10 = Utf8               Lcom/xs/annotation/meta/RetentionClass;
    #11 = Utf8               sourcePolicy
    #12 = Utf8               classPolicy
    #13 = Utf8               RuntimeInvisibleAnnotations
    #14 = Utf8               Lcom/xs/annotation/meta/RetentionClassPolicy;
    #15 = Utf8               runtimePolicy
    #16 = Utf8               RuntimeVisibleAnnotations
    #17 = Utf8               Lcom/xs/annotation/meta/RetentionRuntimePolicy;
    #18 = Utf8               SourceFile
    #19 = Utf8               RetentionClass.java
    #20 = NameAndType        #4:#5          // "<init>":()V
    #21 = Utf8               com/xs/annotation/meta/RetentionClass
    #22 = Utf8               java/lang/Object
  {
    public com.xs.annotation.meta.RetentionClass();
      descriptor: ()V
      flags: ACC_PUBLIC
      Code:
        stack=1, locals=1, args_size=1
           0: aload_0
           1: invokespecial #1                  // Method java/lang/Object."<init>":()V
           4: return
        LineNumberTable:
          line 8: 0
        LocalVariableTable:
          Start  Length  Slot  Name   Signature
              0       5     0  this   Lcom/xs/annotation/meta/RetentionClass;
  
    public void sourcePolicy();
      descriptor: ()V
      flags: ACC_PUBLIC
      Code:
        stack=0, locals=1, args_size=1
           0: return
        LineNumberTable:
          line 12: 0
        LocalVariableTable:
          Start  Length  Slot  Name   Signature
              0       1     0  this   Lcom/xs/annotation/meta/RetentionClass;
  
    public void classPolicy();
      descriptor: ()V
      flags: ACC_PUBLIC
      Code:
        stack=0, locals=1, args_size=1
           0: return
        LineNumberTable:
          line 16: 0
        LocalVariableTable:
          Start  Length  Slot  Name   Signature
              0       1     0  this   Lcom/xs/annotation/meta/RetentionClass;
      RuntimeInvisibleAnnotations:
        0: #14()
  
    public void runtimePolicy();
      descriptor: ()V
      flags: ACC_PUBLIC
      Code:
        stack=0, locals=1, args_size=1
           0: return
        LineNumberTable:
          line 20: 0
        LocalVariableTable:
          Start  Length  Slot  Name   Signature
              0       1     0  this   Lcom/xs/annotation/meta/RetentionClass;
      RuntimeVisibleAnnotations:
        0: #17()
  }
  SourceFile: "RetentionClass.java"
  ```

- 从字节码可以看出，编译器没有记录下 sourcePolicy() 方法的注解信息，分别使用了 RuntimeInvisibleAnnotations 和 RuntimeVisibleAnnotations 属性去记录了classPolicy()方法 和 runtimePolicy()方法 的注解信息。

### @Target

仅用在注解类上，用来标注注解的元素类型（[ElementType](#ElementType)），即设置注解的适用范围。如果没有标注 @Target，那么该注解可以作用在任何地方。

**注解版本：1.5+** 

**场景举例：** 

```java
package javax.validation;

import static java.lang.annotation.ElementType.CONSTRUCTOR;
import static java.lang.annotation.ElementType.FIELD;
import static java.lang.annotation.ElementType.METHOD;
import static java.lang.annotation.ElementType.PARAMETER;
import static java.lang.annotation.ElementType.TYPE_USE;
import static java.lang.annotation.RetentionPolicy.RUNTIME;

import java.lang.annotation.Documented;
import java.lang.annotation.Retention;
import java.lang.annotation.Target;

@Target({ METHOD, FIELD, CONSTRUCTOR, PARAMETER, TYPE_USE })
@Retention(RUNTIME)
@Documented
public @interface Valid {
}
```



## 常用注解

### @Deprecated

标注在类、接口、成员方法和成员变量上，表示某个元素（类、方法等）已过时。当其他程序使用已过时的元素时，编译器将会给出警告。

**注解版本：1.5+** 

**场景举例：** 

```java
@Deprecated
public class DeprecatedClass {
  
  @Deprecated
  public int value;
  
  @Deprecated
  public void m1() {
    System.out.println("Deprecated");
  }
}
```

### @FunctionalInterface

Java 8 版本后，Java引入函数式编程。@FunctionalInterface 就是 Java 8 版本新增的注解，用来标注函数式接口。

什么是函数式接口？如果接口中只有一个抽象方法（可以包含多个默认方法或多个 static 方法），那么该接口就是函数式接口。函数式接口是为 Java 8 的 Lambda 表达式准备的。

@FunctionalInterface 本身只起到标注作用，用来告诉编译器检查这个接口是否符合函数式接口的规范（只能包含一个抽象方法）。

**注解版本：1.8+** 

**场景举例：** 

```java
package org.springframework.boot;

@FunctionalInterface
public interface ApplicationRunner {
  void run(ApplicationArguments args) throws Exception;
}
```

### @Override

标注在方法上，用来标注方法为重写方法。

**注解版本：1.5+** 

**场景举例：** 

```java
public class Parent {
  
  public void m1() {
    System.out.println("Parent");
  }
}

public class Child extends Parent {
  
  @Override
  public void m1() {
    System.out.println("Child");
  }
}
```

### @PostConstruct

@PostConstruct 该注解被用来修饰一个非静态的 void() 方法。被 @PostConstruct 修饰的方法会**在服务器加载 Servlet 的时候运行，并且只会被服务器执行一次**。PostConstruct 在**构造函数之后**执行，**init() 方法之前**执行。

该注解的方法在 Spring 整个 Bean 初始化中的执行顺序：**Constructor（构造方法） -> @Autowired（依赖注入） -> @PostConstruct（注释的方法）**。

**注解版本：1.0+** 

**场景举例：** 

```java
public class Test {
  private static Test test = new Test();
  
  @Autowired
  private OtherService otherService;
  
  @PostConstruct
  public void init() {
    System.out.println("init");
    test.otherService = otherService;
  }
}
```

### @SafeVarargs

标注在 static 或 final 方法上，表示被该注解修饰的方法取消显示指定的编译器警告。

**注解版本：1.7+** 

**场景举例：** 

```java
public class SafeVarargsClass {
  
  public static void main(String[] args) {
    // 没有 @SafeVarargs 会有编译警告
    display("10", 20, 30);
  }
  
  @SafeVarargs
  public static <T> void m1(T... array) {
    for (T arg : array) {
      System.out.println(arg.getClass().getName() + "：" + arg);
    }
  }
}
```

### @SuppressWarnings

标注在类或方法上，表示被该注解修饰的程序元素（以及该程序元素中的所有子元素）取消显示指定的编译器警告，且会一直作用于该程序元素的所有子元素。

注解的使用有以下三种：

- 抑制单类型的警告：@SuppressWarnings("unchecked")

- 抑制多类型的警告：@SuppressWarnings("unchecked","rawtypes")

- 抑制所有类型的警告：@SuppressWarnings("unchecked")

全部关键字请参考附录：[@SuppressWarnings 关键字](#@SuppressWarnings 关键字) 

**注解版本：1.5+** 

**场景举例：** 

```java
@SuppressWarnings("unchecked","rawtypes")
public class SuppressWarningsClass {
  
  @SuppressWarnings("unchecked")
  public void m1() {
    System.out.println("unchecked");
  }
}
```



## 附录

### ElementType

> The constants of this enumerated type provide a simple classification of the syntactic locations where annotations may appear in a Java program. These constants are used in {@link Target java.lang.annotation.Target} meta-annotations to specify where it is legal to write annotations of a given type.

**版本：1.5+** 

| 取值            | 释义                                   |
| --------------- | -------------------------------------- |
| TYPE            | 用于描述类、接口（包括注解类型）、枚举 |
| FIELD           | 用于描述字段（包括枚举、常量）         |
| METHOD          | 用于描述方法                           |
| PARAMETER       | 用于描述形参                           |
| CONSTRUCTOR     | 用于描述构造器                         |
| LOCAL_VARIABLE  | 用于描述局部变量                       |
| ANNOTATION_TYPE | 用于描述注解类型                       |
| PACKAGE         | 用于描述包                             |
| TYPE_PARAMETER  | JAVA 8 新增，作用在泛型上              |
| TYPE_USE        | JAVA 8 新增，用于描述任何类型          |

### RetentionPolicy

> Annotation retention policy.  The constants of this enumerated type describe the various policies for retaining annotations.  They are used in conjunction with the {@link Retention} meta-annotation type to specify how long annotations are to be retained.

**版本：1.5+** 

| 取值    | 释义                                                         |
| ------- | ------------------------------------------------------------ |
| SOURCE  | 注解只保留在源文件，当Java文件编译成class文件的时候，注解被遗弃； |
| CLASS   | 注解被保留到 class文件，但jvm加载class文件时候被遗弃，默认为该级别； |
| RUNTIME | 注解不仅被保存到 class文件中，jvm加载class文件之后，仍然存在； |

### @SuppressWarnings 关键字

| 关键字                   | 用途                                                   |
| ------------------------ | ------------------------------------------------------ |
| all                      | 抑制所有警告                                           |
| boxing                   | 抑制装箱、拆箱操作时候的警告                           |
| cast                     | 抑制映射相关的警告                                     |
| dep-ann                  | 抑制启用注释的警告                                     |
| deprecation              | 抑制过期方法警告                                       |
| fallthrough              | 抑制在 switch 中缺失 breaks 的警告                     |
| finally                  | 抑制 finally 模块没有返回的警告                        |
| hiding                   | 抑制相对于隐藏变量的局部变量的警告                     |
| incomplete-switch        | 忽略不完整的 switch 语句                               |
| nls                      | 忽略非 nls 格式的字符                                  |
| null                     | 忽略对 null 的操作                                     |
| rawtypes                 | 使用 generics 时忽略没有指定相应的类型                 |
| restriction              | 抑制禁止使用劝阻或禁止引用的警告                       |
| serial                   | 忽略在 serializable 类中没有声明 serialVersionUID 变量 |
| static-access            | 抑制不正确的静态访问方式警告                           |
| synthetic-access         | 抑制子类没有按最优方法访问内部类的警告                 |
| unchecked                | 抑制没有进行类型检查操作的警告                         |
| unqualified-field-access | 抑制没有权限访问的域的警告                             |
| unused                   | 抑制没被使用过的代码的警告                             |

