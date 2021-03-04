---
title: MacOS11.0-brew 卡在 Updating Homebrew
date: 2021-02-28
updated: 2021-02-28
categories:
- MacOS
tags:
- MacOS
---

## 问题描述

使用 MacOS11.0 brew 安装软件，一直卡在 Updating Homebrew 不动。

```shell
xs-Pro:~ xs$ brew install wget
Updating Homebrew...
```

---

<!--more-->

---

## 解决方案

### 方法一（推荐）

直接关闭brew每次执行命令时的自动更新

```bash
vim ~/.bash_profile

# 新增一行
export HOMEBREW_NO_AUTO_UPDATE=true
```



### 方法二（未测试）

替换brew源

**该方法未测试** 

```bash
cd "$(brew --repo)"
git remote set-url origin https://mirrors.ustc.edu.cn/brew.git

#替换homebrew-core.git
cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
git remote set-url origin https://mirrors.ustc.edu.cn/homebrew-core.git
brew update


# 备用地址-1
cd "$(brew --repo)"
git remote set-url origin https://git.coding.net/homebrew/homebrew.git
brew update


# 备用地址-2
cd "$(brew --repo)"
git remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/brew.git
cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
git remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew-core.git
brew update
```

如果备用地址都不行，那就只能再换回官方地址了

```bash
#重置brew.git
cd "$(brew --repo)"
git remote set-url origin https://github.com/Homebrew/brew.git

#重置homebrew-core.git
cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
git remote set-url origin https://github.com/Homebrew/homebrew-core.git
```



## 参考资料

- [Mac 解决brew一直卡在Updating Homebrew](https://www.jianshu.com/p/7cb05a2b39a5) 作者：Harveyhhw