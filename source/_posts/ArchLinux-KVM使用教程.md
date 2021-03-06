---
title: ArchLinux KVM使用教程
date: 2021-03-06 11:28:28
tags: ArchLinux
categories:
  - [ArchLinux, 开发环境]
---

## 1. KVM支持

### 1.1 检查硬件支持

```shell
LC_ALL=C lscpu | grep Virtualization
```


### 1.2 内核支持

```shell
zgrep CONFIG_KVM /proc/config.gz
```

输出为`y`或者`m`则代表支持。

确保模块自动载入了内核：

```shell
lsmod | grep kvm
```

如果没有载入，手动载入即可：

```shell
sudo modprobe kvm
```

## 2. Qemu安装

```shell
sudo pacman -S qemu
```

不嫌麻烦的话，直接使用`qemu`管理虚拟机。

## 3. Libvirt安装

使用`libvirt`管理虚拟机。

### 3.1 Sever安装

安装sever以及相应的网络支持：

```shell
sudo pacman -S libvirt ebtables dnsmasq bridge-utils openbsd-netcat
```

### 3.2 Client安装

图形界面安装：

```shell
sudo pacman -S virt-manager
```

命令行安装：

```shell
sudo pacman -S virsh
```

### 3.3 配置

添加授权信息

```shell
sudo usermod -a -G kvm shejialuo
sudo vim /etc/polkit-1/rules.d/50-libvirt.rules
```

添加以下内容：

```txt
polkit.addRule(function(action, subject) {
    if (action.id == "org.libvirt.unix.manage" &&
        subject.isInGroup("kvm")) {
            return polkit.Result.YES;
    }
});
```

## 参考链接

+ [KVM ArchWiki](https://wiki.archlinux.org/index.php/KVM)
+ [QEMU ArchWiki](https://wiki.archlinux.org/index.php/QEMU)
+ [libvirt ArchWiki](https://wiki.archlinux.org/index.php/Libvirt)

