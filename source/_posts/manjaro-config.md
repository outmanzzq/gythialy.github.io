---
title: Manjaro 安装及配置
tags:
  - Manjaro
  - Linux
categories: Notes
date: 2018-06-25 13:14:45
---


[Manjaro](https://manjaro.org/) 是一款基于Arch Linux、对用户友好、全球排名第1的Linux发行版。（排名数据源于[DistroWatch](http://distrowatch.com/)）

在 Linux 圈，[Arch](https://www.archlinux.org/) 的确是一个异常强大的发行版。它有3个无与伦比的优势：

- 滚动更新可以使软件保持最新；
- [AUR](https://aur.archlinux.org/) 软件仓库有着世界上最齐全的Linux软件
- 丰富的 [wiki](https://wiki.archlinux.org/) 和活跃的社区让所有问题都可以快速得到满意的答案

为了解决 ArchLinux 的复杂安装，Manjaro 应运而生！

## 准备

### 下载镜像
可以从 [官方](https://manjaro.org/get-manjaro/) 下载镜像，官方支持 Xfore, KDE, GNOME 三种桌面环境，社区版有更多桌面环境支持。根据个人喜好选择，我对 GNOME 版本比较熟悉，所以下载了 GNOME 版。也可以从 [清华大小开源镜像](https://mirrors.tuna.tsinghua.edu.cn/manjaro-cd/) 下载。

> 注： 清华的镜像比官方镜像稍微旧一些。

### 制作 U 盘启动盘

- Windows 
    Manjaro 官方手册推荐用 [Rufus](https://rufus.akeo.ie/) 制作启动盘，可惜一直没成功，最后使用 [USBwriter](https://sourceforge.net/projects/usbwriter/) 一步到位。
- Linux
    `sudo dd bs=4M if=/path/to/manjaro.iso of=/dev/sd[drive letter] status=progress`  [drive letter] 为 U 盘盘符。

<escape><!-- more --></escape>

### 关闭安全启动

[小米笔记本Pro 15.6″](https://www.mi.com/mibookpro/) 需要关闭安全启动才能启动。首先插入U盘，然后按开机键，并在开机过程中按F2，会进入UEFI设置：

安全 -> 设置密码
安全 -> 关闭 Secure Boot
点击设置密码，但是把"新密码"一栏留空，可以实现重置密码

## 安装

因为小米这台笔记本一直处于吃灰状态，所以安装非常简单，基本上就是无脑下一步就可以了，直接抹掉所有数据。如果是和 Windows 双系统的话，所以需要在不破坏已有引导文件的情况下来安装，因此选择手动分区。 
在具体分区时需要将启动盘挂载到win10建立的esp分区上，挂载点设置为 `/boot/efi` 标记选择 boot 和 esp 其他的分区和挂载根据自己需要划分。

## 更新源

- 执行下面的命令从官方的源列表中对中国源进行测速和设置
    ```bash
    sudo pacman-mirrors -gb testing -c China
    ```
- 增加 Arch Linux CN 源
    在 `/etc/pacman.conf` 文件末尾添加两行
    ```
    [archlinuxcn]
    Server = https://mirrors.ustc.edu.cn/archlinuxcn/$arch
    ```
    > 注：需要安装 `archlinuxcn-keyring` 包以导入 GPG key，否则的话 key 验证失败会无法安装的
- 更新系统
    `sudo pacman -Syu`

## 常用软件安装

### GNU toolchain

```bash
sudo pacman -S base-devel 
```

### `yaourt`

在之前我们管理软件包都是使用官方为我们提供的 pacman，软件包的来源都是官方。yaourt 实际上也是一个软件包，我们可以把它看成是对 pacman 的包装，它兼容 pacman 的所有操作，最大的不同是我们可以用它方便地安装与管理 AUR 中的包，下面的许多软件包都是在 AUR 库中的，也都是使用AUR 来安装的。具体使用，可以参考[这里](https://archlinux.fr/yaourt-en)

```bash
sudo pacman -Sy yaourt fakeroot
```

### 字体

```bash
yaourt noto-fonts noto-fonts-cjk noto-fonts-emoji wqy-microhei wqy-microhei-lite 
```
文泉驿
- wqy-microhei - 文泉驿微米黑，无衬线形式字体。
- wqy-microhei-lite - 文泉驿微米黑light版（笔画更细）

### 输入法

目前主流的输入法框架就是 fcitx 和 iBus 两种， iBus bug 稍微多一点。输入法基本上就是 Sogou 和 Rime。 Rime 要配置，我还没找到主题配置的地方，Sogou 基本上就是开箱即用。

```bash
yaourt -S fcitx-im fcitx-configtool fcitx-sogoupinyin
```
在 `~/.xprofile` 添加下面的内容

```
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS="@im=fcitx
```

### zsh

zsh 是默认 shell bash 的替代品之一，它的特点是插件多配置方便，兼容 bash 脚本并且支持更强大的高亮与补全。

```bash
yaourt zsh powerline-fonts powerline 
# 安装 oh-my-zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

### [Albert](https://albertlauncher.github.io/)

Albert 是类似 macOS Alfred 的应用启动器。

```bash
yaourt albert muparser
```

### 剪贴板 

[CopyQ](https://hluk.github.io/CopyQ/) 是一个剪贴板管理工具，类似 [Ditto](https://ditto-cp.sourceforge.io/)。

```bash
yaourt copyq
```

### 其他

- 配置 SSH
- 配置 Git
- 配置 gpg
- 安装 IDE (Jetbrains ...)
- etc ...

```bash
yaourt electronic-wechat clion gitahead... 
```

## 备份

简单配置了一下 Timeshift，整体体验远不如 macOS TimeMachine，不过毕竟价格差异在这里，还要什么自行车。

## 小结

总的来说， Manjaro 安装方便，有 AUR 加持，安装包非常方便，作为主力开发环境使用了一段时间总体觉得还可以。搭建开发环境来说，基本与 macOS 相当，有些环境需要 sudo，这点不如 Brew，不过稍微配置还说有版本实现的。 

## 参考链接

- [Writing_to_a_USB_Stick_in_Linux](https://wiki.manjaro.org/index.php?title=Burn_an_ISO_File#Writing_to_a_USB_Stick_in_Linux)
- [Xiaomi Mi Notebook Air 13.3](https://wiki.archlinux.org/index.php/Xiaomi_Mi_Notebook_Air_13.3_(2016))
- [Fonts](https://wiki.archlinux.org/index.php/Fonts_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))

`---EOF---`