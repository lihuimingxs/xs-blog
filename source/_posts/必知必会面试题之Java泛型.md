---
title: 必知必会面试题之 Java 泛型
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

- [泛型的类型安全](#泛型的类型安全) 
- [泛型上限](#泛型上限) 
- [泛型下限](#泛型下限) 

---

<!--more-->

---

### 泛型的类型安全

JDK 1.5 以后引入了泛型的概念，通过泛型能够帮助我们在程序处理中将处理逻辑抽象出来，提高代码复用性。泛型的使用一般遵循类型约束，以此保证泛型类型的安全性。举个例子：

```java
class Person<T> {
  // ...
  private T atrribute;
  public void setAtrribute(T atrribute) {
    this.atrribute = atrribute;
  }
  public T getAtrribute() {
    retrun atrribute;
  }
}
class PersonAtrribute {
  private height;
  private vision;
  // ...
}
class DuckAtrribute {
  private tail;
  private wing;
  // ...
}
class DemoService() {
  Person person = new Person();
  PersonAtrribute personAtrribute = new PersonAtrribute();
  DuckAtrribute duckAtrribute = new DuckAtrribute();
  person.setAtrribute(personAtrribute);
  person.setAtrribute(duckAtrribute);
}
```

上述例子中，DemoService 中错误地将 DuckAtrribute 放入了 Person 的扩展属性中，这显然是不合理的，这也就是我们所关注的泛型安全性问题。

如果没有类型约束，泛型中就可以存放任何类型的东西，那么当你创建这样的一个泛型时，你就无法预知泛型的使用者会拿它做什么。也许有一天，你会发现自己设计的泛型已经在系统里使用地十分混乱，这显然不是我们设计时想要看到的。因此，我们需要对泛型进行约束。

泛型约束包括两种：extends 和 super。extends 决定了泛型的上限，super 决定了泛型的下限。

### 泛型上限

```java
// 泛型可以接受E类型或者E的子类类型。
? extends E
```

extends 规定了泛型的上限。当泛型使用 extends 时，使用泛型的类所实现的类型都受 extends 继承类的约束，如果继承了 Person，泛型传进来 Duck 就是不被允许的。

### 泛型下限

```java
// 可以接受E类型，或者E的父类型。
? super E
```

super 规定了泛型的下限，当泛型使用 super 时，使用泛型的类所实现的类型都受 super 父类的约束，如果 super 的是一个属性类，属性类里包括 age 和 sex，那么实现泛型的时候就必须要具备这两种属性。