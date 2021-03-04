---
title: SpringBoot 好“吃”的启动原理
date: 2020-12-30
updated: 2020-12-30
categories:
- SpringBoot
tags:
- SpringBoot
---

## 不正经的前言

最近好朋友山治去面试了，晚上回来有些低迷地问我：“小西，你知道 SpringBoot 的启动流程吗？”

我说：“知道呀！从 SpringApplication.run() 方法开始，首先进行实例化，实例化里主要做了4件事：根据 calsspath……”

山治抬腿就是一记“恶魔风脚”：SpringBoot 的启动步骤那么多，什么 1、2、3、4，谁能记得住啊！

在被乔巴施展”还我漂漂拳“以后，我痛定思痛，暗暗发誓一定要写篇比美女还好看的文章教会山治，让他吃透这道看似难啃的“菜”。

---

<!--more-->

---

## 料理的二三事

### 选材说明

首先，做一份料理，一定要准备好采购清单。如果只有菜谱没有选材说明，最终做出来的味道可能并没有那么好。哪怕随便做一道家常菜，需要放大葱还是香葱也是有讲究的，而不同年份的葡萄酿制的酒就更不用说了。

> 正确的选材示例：山治的料理笔记。
>
> 错误的选材实例：路飞不看笔记误吃有毒鱼皮。

### 料理的主要流程
现在，咱们来聊聊吃货该聊的事情：想要做一道菜需要做些什么？

### 料理三要素

来看一下料理三要素：

1. 做饭的场地
2. 完美的食材
3. 优秀的厨师

当然，虽然在家里一个人就可以做了，但是不要小看料理呀！咱们要聊就聊 big restaurant。比如一家让你难忘的餐厅：海上餐厅“BARATI”。你想要的东西——上面提到的三要素，餐厅后厨全都有。Ok！下面就可以准备料理了。

### 料理步骤

料理的步骤很简单，包括准备步骤和开始步骤。

### 料理准备

让我们来安排一场完美的料理。BARATI 料理的准备步骤：

1. 选择储存食材的冰箱
2. 选择料理的主食材
3. 根据点菜单确定料理菜系
4. 准备料理需要的菜谱
5. 指定处理食材的厨师
6. 指定做料理的主厨

### 料理开始

“高端的食材只需要简单的烹饪”。重头戏开始了！BARATI 料理的工作流程：

1. 允许外卖
2. 厨师待命
3. 加载点菜单的要求（如：不要香菜）
4. 准备料理所需的锅碗瓢盆，并通知厨师准备好了
5. 忽略没必要了解的信息（如：食材的价格）
6. 指定菜品装饰
7. 根据菜系，获取对应菜谱
8. 设置突发情况报告人（如：点的菜没有了）
9. 厨师查看锅碗瓢盆、菜谱和点菜单的要求
10. 处理食材
11. 料理完成后，根据点菜单的要求定制
12. 是否查看客人反馈
13. 食材准备就绪
14. 通知所有可以干活的厨师
15. 准备开工

突发报告人处理突发情况（点的菜没有了，需要告诉服务员）

就这样，一顿完美的料理就做好了。

## 欢迎来到“BARATI”

### 选材说明

学技术也是一样，版本说明就是料理的选材说明。遵循“就地取材”原则，本次选用的“主料”是平时项目上使用的 `SpringBoot 2.1.5.RELEASE` 版本。依赖如下：

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-web</artifactId>
  <version>2.1.5.RELEASE</version>
</dependency>
```

### “BARATI”后厨主要流程

> 以 SpringApplication.run() 方法为例

#### 料理三要素

```java
@SpringBootApplication
public class StartApplication {

  public static void main(String[] args) {
    // 1. 做饭的场地
    // 2. 完美的食材
    // 3. 优秀的厨师
    SpringApplication.run(StartApplication.class, args);
  }
}
```

1. 做饭的场地：SpringApplication
2. 完美的食材：所有通过 SpringBoot 自动配置扫描，由 ClassLoader 加载的 Class
3. 优秀的厨师：在启动过程中所有 ApplicationListener 和 ApplicationRunner

#### 料理步骤

```java
public static ConfigurableApplicationContext run(Class<?>[] primarySources,
			String[] args) {
  // 1. 料理准备
  // 2. 料理开始
  return new SpringApplication(primarySources).run(args);
}
```

1. 料理准备

   new SpringApplication(primarySources) 方法，SpringApplication 的初始化

   ```java
   public SpringApplication(ResourceLoader resourceLoader, Class<?>... primarySources) {
     // 1. 选择储存食材的冰箱 >> 可指定的类加载器，与 classpath 相关，默认为null，加载时使用 DefaultResourceLoader
     this.resourceLoader = resourceLoader;
     // 2. 选择料理的主食材 >> 设置传入的主源类
     Assert.notNull(primarySources, "PrimarySources must not be null");
     this.primarySources = new LinkedHashSet<>(Arrays.asList(primarySources));
     // 3. 根据点菜单确定料理菜系 >> 通过加载的 class 判断web应用类型（NONE、SERVLET、REACTIVE）
     this.webApplicationType = WebApplicationType.deduceFromClasspath();
     // 4. 准备料理需要的菜谱 >> 通过 getClassLoader()，查找并加载所有 ApplicationContextInitializer
     setInitializers((Collection) getSpringFactoriesInstances(
       ApplicationContextInitializer.class));
     // 5. 指定处理食材的厨师 >> 通过 getClassLoader()，查找并加载所有 ApplicationListener
     setListeners((Collection) getSpringFactoriesInstances(ApplicationListener.class));
     // 6. 指定做料理的主厨 >> 推断并设置 main 函数所在的 class
     this.mainApplicationClass = deduceMainApplicationClass();
   }
   ```

2. 料理开始

   SpringApplication.run(args) 方法，SpringBoot 实际启动的流程

   ```java
   public ConfigurableApplicationContext run(String... args) {
     StopWatch stopWatch = new StopWatch();
     stopWatch.start();
     ConfigurableApplicationContext context = null;
     Collection<SpringBootExceptionReporter> exceptionReporters = new ArrayList<>();
     // 1. 允许外卖 >> 主要允许服务器只提供服务，不提供显示器和界面展示的情况，类似只支持外带，不支持店内就餐
     configureHeadlessProperty();
     // 2. 厨师待命 >> 获取所有监听者，进入监听状态
     SpringApplicationRunListeners listeners = getRunListeners(args);
     listeners.starting();
     try {
       // 3. 加载点菜单的要求（如：不要香菜） >> 读取传入 args 参数
       ApplicationArguments applicationArguments = new DefaultApplicationArguments(
         args);
       // 4. 准备料理所需的锅碗瓢盆，并通知厨师准备好了 >> 设置环境变量，通知监听者
       ConfigurableEnvironment environment = prepareEnvironment(listeners,
                                                                applicationArguments);
       // 5. 忽略没必要了解的信息（如：食材的价格） >> 忽略 BeanInfo 信息，主要为了提高启动速度
       configureIgnoreBeanInfo(environment);
       // 6. 设置菜品装饰 >> 设置 Banner
       Banner printedBanner = printBanner(environment);
       // 7. 根据菜系，获取对应菜谱 >> 根据应用类型（是 Servlet，还是 Reactive），创建对应上下文
       context = createApplicationContext();
       // 8. 设置突发情况报告人（如：点的菜没有了） >> 加载 SpringBoot 异常上报类
       exceptionReporters = getSpringFactoriesInstances(
         SpringBootExceptionReporter.class,
         new Class[] { ConfigurableApplicationContext.class }, context);
       // 9. 厨师查看锅碗瓢盆、菜谱和点菜单的要求 >> 根据环境变量、监听者、启动参数和 Banner，装载上下文
       prepareContext(context, environment, listeners, applicationArguments,
                      printedBanner);
       // 10. 处理食材 >> 刷新上下文
       refreshContext(context);
       // 11. 料理完成后，根据点菜单的要求定制 >> 空操作，刷新上下文后的预留扩展点
       afterRefresh(context, applicationArguments);
       stopWatch.stop();
       // 12. 是否查看客人反馈 >> 设置日志信息打印
       if (this.logStartupInfo) {
         new StartupInfoLogger(this.mainApplicationClass)
           .logStarted(getApplicationLog(), stopWatch);
       }
       // 13. 食材准备就绪 >> 发布 ApplicationStartedEvent 事件，表示监听者任务完成
       listeners.started(context);
       // 14. 通知所有可以干活的厨师 >> 调用 ApplicationRunner，CommandLineRunner 的 run 方法
       callRunners(context, applicationArguments);
     }
     catch (Throwable ex) {
       // * 处理突发情况 >> 如果启动异常，处理 exceptionReporters 中的异常信息，并抛出异常
       handleRunFailure(context, ex, exceptionReporters, listeners);
       throw new IllegalStateException(ex);
     }
   
     try {
       // 15. 准备开工 >> 发布 ApplicationReadyEvent 事件，表示应用就绪
       listeners.running(context);
     }
     catch (Throwable ex) {
       // * 处理突发情况 >> 如果启动异常，处理 exceptionReporters 中的异常信息，并抛出异常
       handleRunFailure(context, ex, exceptionReporters, null);
       throw new IllegalStateException(ex);
     }
     return context;
   }
   ```

## 小结

本篇文章想达到的目的是：**将源码映射到现实生活的事件，加深对源码的解读，希望将晦涩难度的源码变成一件有趣的事情**。此文只是作为一个吃货的兴趣篇，并不是特别严谨，在 SpringBoot 启动过程中，还有很多精妙的细节需要继续推敲，我会在后续文章中，对它们进行剖析。当然，由于自身水平限制，有些比喻可能并不一定十分恰当，希望各位老板见仁见智地去理解。若发现不当之处，欢迎私信沟通交流！

