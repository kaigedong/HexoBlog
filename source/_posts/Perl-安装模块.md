---
title: Perl 安装模块与卸载
date: 2018-07-12 09:51:09
tags:
---

+ 安装Perl 模块的不同方法

<!--more-->

## 1. 使用`cpanm` 来安装模块

首先安装`cpanm`, 在终端输入：

```
cpan App::cpanminus 
```

然后可安装你需要的模块：

```
cpanm Module::Name
```

遇到了bug，cpanm 会详细的报告出来bug信息，这点很好，总的来说是gcc 版本太低：

`/lib64/libc.so.6: version GLIBC_2.14' not found`

```
$which perl
/home/dongkaige/.linuxbrew/bin/perl

$which cpanm
/home/dongkaige/software/miniconda3/envs/BioEnv/bin/cpanm

$which cpan
/home/dongkaige/.linuxbrew/bin/cpan
...
```

不知道`cpanm` 是根据什么变量安装的，安装到了conda 的目录下。

虽然conda在遇到依赖特定版本的gcc的包时，也会安装到conda中，但很多程序在调用c动态库时，依然从服务器的动态库调用，就会导致版本错误`GLIBC_2.14 not found`。

而linuxbrew 则是相对独立的包管理器，在费了很大功夫，在老服务器上安装好 linuxbrew 及高版本的 gcc， glibc之后，之后linuxbrew中，所有软件编译和运行都会调用linuxbrew下的动态库，这样就不会有上面的错误。



只好用别的方法调用linuxbrew中的Perl进行安装：

## 2. cpan

```
cpan
install Set::IntervalTree
```

## 3. `perl -MCPAN -e "shell"`

```
perl -MCPAN -e "shell"
install Set::IntervalTree
```

第二种方法成功，第三种方法应该也没问题。

## 4. 测试是否已经安装成功

```
perl -MHTTP::Date -e "print\"module installed\n\""
```

`-M`后紧跟着模块名，如果输出`module installed`的结果，则是安装了的。


## 其他命令

#### 列出已安装模块
```
perldoc perllocal
# 或
instmodsh # 推荐这一种
```

#### 查询perl模块安装路径
```
perldoc -l DBD::mysql
```

#### 查询模块版本
(`-M`紧跟着跟着模块)
```
$ perl -MHTTP::Date -e 'print HTTP::Date->VERSION. "\n"'
6.02
```


#### 卸载模块：





## 参考

+ [Linux 检查是否安装perl模块及列出所有已安装的perl模块(安装路径、版本号)](https://blog.csdn.net/u010533843/article/details/54971428)

+ [非root用户安装perl模块](https://blog.csdn.net/yuehedou/article/details/78406518)