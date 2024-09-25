+++
Categories = ["Technology"]
Description = "linux epel"
Tags = ["epel", "linux"]
date = "2017-05-22T10:30:37+08:00"
menu = "linux"
title = "为什么要使用epel"
toc = true
+++

## Linux 安装包的痛苦经历 ##


```
libcurl.so: libssh2.so.1: cannot open shared object file: No such file or directory
libssl.so.10: cannot open shared object file: No such file or directory
```
如果遇到以上以上情况，libcurl需要libssh2依赖，那我们最希望的最法无非是yum install libssh2 libssl希望能安装好，那就解决了依赖的问题了，但是问题是没有

```
Last metadata expiration check: 2:33:40 ago on  18 Mar 2019 02:09:36 PM CST.
No match for argument: libssh2
Error: Unable to find a match: libssh2

```

遇到这样的问题我们一般会去找一个rpm包去安装，通过rpm -ivh libssh2.rpm 发现缺少依赖

```
Requires
libc.so.6()(64bit)
libc.so.6(GLIBC_2.14)(64bit)
libc.so.6(GLIBC_2.2.5)(64bit)
libc.so.6(GLIBC_2.3)(64bit)
libc.so.6(GLIBC_2.3.4)(64bit)
libc.so.6(GLIBC_2.4)(64bit)
libcrypto.so.3()(64bit)
libcrypto.so.3(OPENSSL_3.0.0)(64bit)
libssl.so.3()(64bit)
libz.so.1()(64bit)
rpmlib(CompressedFileNames) <= 3.0.4-1
rpmlib(FileDigests) <= 4.6.0-1
rpmlib(PayloadFilesHavePrefix) <= 4.0-1
rpmlib(PayloadIsZstd) <= 5.4.18-1
rtld(GNU_HASH)
```
有的依赖可以找到的，但是因为版本问题可能不行，有的找不到需要通过gcc编译，然后一个个解决实在太麻烦了！但是任何一个有经验的centos/redhat的系统工程师都会提前求助于EPEL!


## 什么是EPEL  ##


    EPEL 的全称叫 Extra Packages for Enterprise Linux。EPEL 是由 Fedora 社区打造，为 RHEL 及衍生发行版如 CentOS、Scientific Linux 等提供高质量软件包的项目。装上了 EPEL 之后，就相当于添加了一个第三方源。

## 为什么需要 EPEL ##

    那是因为 CentOS 源包含的大多数的库都是比较旧的。并且，很多流行的库也不存在。EPEL 在其基础上不仅全，而且还够新。

    EPEL 这两个优点，解决了很多人安装库的烦恼。

## 安装EPEL ##

```
yum install -y epel-release
```

## 安装libssh2 ##

```
    yum search libssh2
    Last metadata expiration check: 2:41:47 ago on Fri 18 Mar 2022 02:09:36 PM CST.
    libssh2-docs.noarch : Documentation for libssh2
    libssh2-devel.x86_64 : Development files for libssh2

    yum install libssh2-devel

```

## 总结 ##

在Rehat/centos/fedora上如果安装包找不到依赖的情况下，首先要考虑epel

