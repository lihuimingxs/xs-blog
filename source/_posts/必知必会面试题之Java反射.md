---
title: 必知必会面试题之 Java 反射
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

- [反射的实现原理](#反射的实现原理) 
- [注解的实现原理](#注解的实现原理) 
- [反射是否可以调用私有方法、获取参数名、获取父类私有方法？](#反射是否可以调用私有方法、获取参数名、获取父类私有方法？)  

---

<!--more-->

---

### 反射的实现原理

在 Java 中是通过 Class.forName(classname) 来获取类的信息，实现反射机制的。

### 注解的实现原理

注解是基于 Java 反射来实现的。

### 反射是否可以调用私有方法、获取参数名、获取父类私有方法？

可以。我们可以通过反射拿到对应的 class 对象，然后通过 class.getDeclaredConstructors() 拿到全部构造器，获取构造器的名称、参数、修饰符等信息；可以通过 class.getDeclaredMethods() 拿到全部方法，获取方法的名称、参数、修饰符等信息；可以通过 class.getSuperclass().getDeclaredMethod() 获取父类全部方法。