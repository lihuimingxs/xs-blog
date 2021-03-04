---
title: 开源世界里的重要理念：上游优先（UpStream First）
date: 2021-02-28
updated: 2021-02-28
categories:
- 开源
tags:
- 开源
---

## 引子

2016 年，Thomas Cameron 在一次五分钟闪电演讲中提到了“Upstream first”的概念，也就是我们所说的“上游优先”：

> Part of Red Hat's commitment to open source, Cameron explains, is committing code to upstream projects. The company is a top contributor to the Linux kernel, glibc/GCC, OpenStack/RDO, KVM, JBoss.org projects, GNOME, and more.
>
> "We recognize that we are beholden to open source communities for our success," Cameron said. "And we owe a debt of gratitude to those open source communities and we are responsible for contributing as much code as we can back to those communities because everyone gets better when we do that."

---

<!--more-->

---

将这次演讲单独说明，是因为这是目前找到的关于“Upstream first”的最早资料，但这并不代表就是它的出处，上游优先这一概念的出处尚需考究。

2017 年，Dave Neary 在 TM 会场上阐述了“Upstream first”对开源世界的重要性：

> “Upstream first” development is the idea that any changes (features, bug fixes) which you want to include in a product based on an open source project should be submitted to the project first, before being included in the product. This ensures that you minimize your long-term maintenance burden.
>
> ……
>
> By engaging upstream first, you can get quick course corrections when your initial approach does not match community expectations, and you maintain patches against the very latest development tree. By building relationships with the upstream community, you will have an easier time getting changes accepted. And you maintain the possibility to ship features or patches in earlier versions by “backporting” features to the stable branch on which you have based your product.

在读完上面的案例，相信你一定对上游优先有了一定的好奇：上游优先究竟是什么呢？它对开源社区有什么影响呢？

下面，让我们带着疑问一起来了解一下。

## 什么是上游优先？

直接与开源社区互动并在源头上解决问题的办法，被称为上游优先（Upstream first）。

所谓上游，是一个相对的概念，意为“靠近源头的一方”。在开源社区中，主要指的是开源社区维护的主干版本。而所谓下游，是指基于上游开源项目衍生出的项目或产品。在 Github、Gitee 等代码管理平台中对开源项目的 fork 操作，就是一种对上游代码的拓展。

那么，上游优先就很好理解了：基于开源项目的任何修改都应该提交优先提交给项目本身，然后再包含在自己的产品中。与之相反的处理方式是只基于自己产品进行维护，对上游不做反馈。

## 为什么要使用“上游优先”？

上游优先是开源社区提出的优秀的开源理念，那么它优秀在哪里呢？

**举个简单的例子：** 

葵花派长老将葵花点穴手传授给了白展堂和祝无双，在每次施展葵花点穴手时，都需要大喊一声：“葵花点穴手！”

这一天，长老对两人说：堂堂、双双，你们修炼已经圆满，为师已经没有什么能够传授给你们的了，各自下山游历吧！说罢便闭关修炼去了。

白展堂来到了同福客栈，因为葵花点穴手需要念五个字，被郭芙蓉四个字的排山倒海打得鼻青脸肿。

正所谓人总是在被虐中成长，白展堂发现使用一种手法只需要喊“葵花点”就可以施展武功，终于打败郭芙蓉。

随后，白展堂回到葵花派，将这种手法汇报给了长老，长老飞鸽传书，告知了所有门派弟子。

葵花派从此发扬光大，双双成为了衙门的捕快，各大弟子也都成为了惩恶扬善的侠客！

**那么问题来了：如果白展堂没有将这种手法告知长老，会怎么样呢？** 

有两种可能。

其一，白展堂此生未收徒，葵花派因“葵花点穴手”需要念五个字，逐渐式微，从此江湖再也没有葵花派的威名。

其二，白展堂广收门徒，每日传授和教导子弟，将“葵花点”手法发扬光大，但每日忙忙碌碌，不仅要教导弟子学习“葵花点”手法，还要回葵花派学习长老闭关研习出来的新武功。

**说到这里，我想大家已经明白了我所要表达的意思了。** 

白展堂研究出“葵花点”的手法后，回门派告知长老，这种行为就是“上游优先”。

为什么要使用“上游优先”？相信从这则小故事不难看出：

- **利于上游合并**：随着时间的推移，下游的修改越晚反馈给上游，上游变越难合并下游的修改。
  - 假如白展堂在临死前再将“葵花点”手法告知门派，门派的葵花点穴手早就出到 9527 版了，手法可能很难和 9527 版葵花点穴手融合。
- **减轻维护负担**：下游自己维护的成本太高，提高给上游维护，会降低下游维护成本。
  - 葵花派除了长老授课以外，要求白展堂单独授课讲解“葵花点”手法，白展堂打王者荣耀的时间都没有了，更别说研习新武功。白展堂整理出手法秘籍交给长老，由长老统一传授，白展堂又可以快意江湖了。
- **便于合作共赢**：通过和上游合作，可以更为顺畅地与上游达成一致，得到上游的认可，也更容易将上游的修改合并进来。
  - 白展堂因贡献了“葵花点”手法被尊称为首席大弟子，长老在“葵花点”的基础上研习出提高葵花点穴手定身时长的法门，白展堂第一时间得到秘籍修炼，如鱼得水。
  - 反之，白展堂没有贡献“葵花点”手法自己自行修炼，长老研习出提高葵花点穴手定身时长的法门，白展堂发现和“葵花点”手法运功路线不一致，难以习得新版葵花点穴手，无法提升威力。

## 使用“上游优先”需要做什么？

首先，需要**与上游达成合作**。达成合作是反馈给上游的捷径，越密切的合作者提出的想法，越容易被上游所接受。

其次，需要**保持与上游的沟通**。达成合作不是一蹴而就的事情，需要与上游加强沟通，及时了解上游动态，否则，无法确保你的想法与上游的想法是否一致。

最后，需要**将修改及时反馈给上游**。上游的修改需要成本，及时的反馈可以节省成本，这也是上游收费接收下游修改的一个重要标准。及时的反馈，能够让合作变得更加通畅。

## 哪些开源组织或公司使用“上游优先”？

在开源世界中，开源项目对“上游优先”的宣传并不多，但实际上大部分开源项目对“上游优先”都是非常认可的。

“上游优先”的理念更加像是一种约定俗成，除了一些知名开源项目可能有提到或宣讲过该理念以外，更多的是默默践行这一理念。有很多开源项目没有对“上游优先”做过多的解释，但在开源过程中对开源项目的必要修改都会反馈到上游。

下面列举一些明确采用“上游优先”的部分开源组织或公司：

- RedHat：红帽组织对“上游优先”可谓是彻底贯彻，网站上能够搜到的关于“上游优先”的文章和演讲，红帽可谓是不遗余力。
- OpenEuler：OpenEuler 是华为的一个开源项目，用 OpenEuler 自己的话来说：OpenEuler 来自于社区，回馈到社区。
- Google Chromium：Google Chromium 官方的 Design Documents 中，将 Upstream First 放在了 General 一栏中。
- The Linux Foundation：LF 官网在“Best Practices to Contribute Code Upstream”中，可以找到 Upstream First 的介绍。
- ……（更多案例，欢迎补充）

## 特别感谢

之所以编写本文，在此要特别感谢开源路上认识的杰克老师（码云ID：@jack960330 ）对我的指点和支持！

## 参考资料

- [Why Red Hat takes an 'upstream first' approach](https://opensource.com/article/16/12/why-red-hat-takes-upstream-first-approach) By  [Opensource.com (Red Hat)](https://opensource.com/users/admin) [Dec 05, 2016]
- [Upstream first: Building products from open source software](https://inform.tmforum.org/features-and-analysis/2017/05/upstream-first-building-products-open-source-software/) By [Dave Neary, SDN and NFV with Open Source at Red Hat](https://inform.tmforum.org/author/296076-dave-neary/) [May, 2017]
- [开源软件项目的“上游优先”解惑](http://opensourceway.community/posts/opensource_culture/what_is_upstream_and_its_benefits/) By [opensourceway](http://opensourceway.community/posts/the_way_of_open_source/open_source_way/) [Nov 13, 2017]
- [上游优先地开发](https://willemjiang.github.io/opensource/2019/10/29/UpStream-first.html) By [Willem Jiang‘s Blog](https://willemjiang.github.io/) [Oct 29, 2019]
- [开源社区对开发者的价值到底有多大？](https://www.infoq.cn/article/zP9erqJmIK6IAWfUHBoW) By [Eileen](https://www.infoq.cn/profile/11D1D4FAE98CEA/publish) [Apr 29, 2020]