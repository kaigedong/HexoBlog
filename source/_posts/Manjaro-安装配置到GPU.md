---
title: Manjaro-安装配置到GPU
date: 2019-04-11 10:14:07
tags:
- Nvidia
- GPU
- Manjaro
- Arch
Categories:
- Linux
---




> Manjaro的安装、配置、设置steam使用GPU，pytorch使用GPU



<!--more-->

## 安装Manjaro

> 下载、安装Manjaro (xface)
> 中科大镜像：https://mirrors.ustc.edu.cn/manjaro-cd/

```
$ wget https://mirrors.ustc.edu.cn/manjaro-cd/xfce/18.0.4/manjaro-xfce-18.0.4-stable-x86_64.iso
```

> Mac OS系统下制作U盘启动：

```
$ sudo diskutil list
...
/dev/disk8 (external, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:     FDisk_partition_scheme                        *31.0 GB    disk8
   1:             Windows_FAT_32 KINGSTON                31.0 GB    disk8s1

$ diskutil umount /dev/disk8s1 
Volume KINGSTON on disk8s1 unmounted

$ sudo dd if=/path/of/manjaro-xfce-18.0.4-stable-x86_64.iso  of=/dev/disk8 bs=1m
```
> 安装的时候，先选英文。装好系统再改为中文(这样~下的文件夹就会是英文的了)。
>
> Manjaro 国内源 + ArchlinuxCN 国内源 设置

```
$ sudo pacman-mirrors -i -c China -m rank  # 生成可用中国镜像站列表，勾选 http://mirrors.ustc.edu.cn/manjaro/ ，然后按 OK 键两次。
$ sudo pacman -Syyu # 升级系统到最新
```

> ArchLinuxCN 中科大源 (Arch Linux 中文社区仓库是由 Arch Linux 中文社区驱动的非官方用户仓库)。

```
# /etc/pacman.conf 文件末尾添加：

[archlinuxcn]
Server = https://mirrors.ustc.edu.cn/archlinuxcn/$arch

# 然后安装 archlinuxcn-keyring 包以导入 GPG key。
$ sudo pacman -Syyu
$ sudo pacman -S archlinuxcn-keyring
```

## 开机挂载第二块硬盘

查看硬盘信息： 
`$ sudo fdisk -l # 查看我的是/dev/sdb1`
`$ ls -l /dev/disk/by-uuid/ `# 查看对应的UUID

然后编辑　`/etc/fstab`
增加一行:
`UUID=a8786e51-eda0-41fe-... /home/bobo/Data ext4 defaults 0 0`

要注意建立个文件夹：` 要是bobo的所有权，否则干啥都要root，硬盘将挂载在这里。

然后**重启**之后就自动挂载了。

## Arch 配置服务

```
当通过Arch wiki 查到不同软件提供的服务之后，可以按照下面命令控制这些服务

$ systemctl enable sshd.service     开机启动
$ systemctl start sshd.service      立即启动
$ systemctl restart sshd.service    立即重启
```



## 安装常用软件

```
sudo pacman -S code vim guake zotero pycharm-professional typora skype dbeaver shadowsocks-qt5 telegram-desktop electronic-wechat teamviewer

sudo pacman -S chromium  # 启用代理，用以同步 chromium --proxy-server="socks5://127.0.0.1:1080"
sudo pacman -S flameshot # 最好的截图工具，配置开机启动，全局快捷键
udo pacman -S adobe-source-han-serif-cn-fonts adobe-source-han-serif-tw-fonts adobe-source-han-sans-cn-fonts adobe-source-han-sans-tw-fonts # 安装中文简体、繁体字体

sudo teamviewer --daemon start 
sudo pacman -S mysql # https://wiki.archlinux.org/index.php/MariaDB_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87)#%E5%AE%89%E8%A3%85
```

> flameshot截图的命令是 `flameshot gui`， 设置到系统的自定义快捷键。
>
> 安装完Teamviewer之后，打开网络有问题。执行`sudo teamviewer --daemon start `，再次打开，过一会就好了。参考https://bbs.archlinux.org/viewtopic.php?id=192406




### 安装搜狗输入法

```
$ sudo pacman -S fcitx fcitx-configtool
$ sudo pacman -S fcitx-im # 就选default(=all)，会安装fcitx-gtk2 fcitx-gtk3 fcitx-qt4 fcitx-qt5
$ sudo pacman -S fcitx-sogoupinyin

$ vim ~/.xprofile
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS="@im=fcitx"

重启之后，配置即可。
```

### 安装AUR管理器
```
git clone https://aur.archlinux.org/yay.git
makepkg -si

# 用法
yay install some-package
```

### 配置vim

```
git clone --depth=1 https://github.com/amix/vimrc.git ~/.vim_runtime
sh ~/.vim_runtime/install_awesome_vimrc.sh
```

### 配置zsh

> 安装oh-my-zsh

```
git clone https://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh
cp ~/.oh-my-zsh/templates/zshrc.zsh-template ~/.zshrc

修改默认shell, 或自动启动zsh，对我没必要，我使用的是guake，可设置开启终端时的默认shell。
chsh -s /bin/zsh 或者 echo "exec /path/of/zsh --login" >> .bash_profile #
```

> oh-my-zsh 主题及插件下载

```
# 下载插件：
# 1. spaceship-prompt 主题 (https://github.com/denysdovhan/spaceship-prompt)

$ zsh
$ git clone https://github.com/denysdovhan/spaceship-prompt.git "$ZSH_CUSTOM/themes/spaceship-prompt"
$ ln -s "$ZSH_CUSTOM/themes/spaceship-prompt/spaceship.zsh-theme" "$ZSH_CUSTOM/themes/spaceship.zsh-theme"


# 2. zsh-syntax-highlighting 语法高亮插件
$ git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```

> `.zshrc` 的一些配置

```
# .zshrc: 
# tldr代替man，fd 代替find，在rust那里下载安装。

alias la="ls -lth"
alias man="tldr"

ZSH_THEME="spaceship"

HIST_STAMPS="yyyy-mm-dd"

plugins=(
  extract git zsh-syntax-highlighting
)

```

### 安装rust环境
```
# 1. 安装rust 中科大镜像加速rust安装 (参考https://lug.ustc.edu.cn/wiki/mirrors/help/rust-static)
添加到 .zshrc
$ export RUSTUP_DIST_SERVER=https://mirrors.ustc.edu.cn/rust-static  # (用于更新 toolchain)
$ export RUSTUP_UPDATE_ROOT=https://mirrors.ustc.edu.cn/rust-static/rustup  # (用于更新 rustup)

curl https://sh.rustup.rs -sSf | sh  # 官网的安装程序

# 2. 设置中科大Rust crates镜像 (https://lug.ustc.edu.cn/wiki/mirrors/help/rust-crates)
# 在 $HOME/.cargo/config 添加下面内容

[source.crates-io]
registry = "https://github.com/rust-lang/crates.io-index"
replace-with = 'ustc'
[source.ustc]
registry = "git://mirrors.ustc.edu.cn/crates.io-index"

# 3. 添加rust可执行程序文件夹到~/.zshrc

echo "export PATH=$PATH:$HOME/.cargo/bin" >> ~/.zshrc

# 4. 安装好用的rust程序 fd & tldr
cargo install fd-find
cargo install tealdeer
```


## xfce 添加网速监控


```
$ ip addr # 结果显示

1: lo: ...
2: enp7s0: ...
3. wlp6s0: ...
```

> 其中第三个是我的无线网卡，在面板下面`右键`，`面板`--`添加新项目`中添加`网速监控`，则面板下方出现一个`Net`。
>
> 然后在面板的Net上右键`网速监控模块`--`属性`--在网络设备填入`wlp6s0`即可！



### pip使用中科大源

> `pyenv` . github[地址](https://github.com/pyenv/pyenv)用法看[python虚拟环境](https://kaigedong.github.io/2018/12/25/python%E8%99%9A%E6%8B%9F%E7%8E%AF%E5%A2%83/)

> Pip:

```
# $HOME/.config/pip/pip.conf
[global]
index-url = https://mirrors.ustc.edu.cn/pypi/web/simple
format = columns
```



## 配置英伟达显卡！！

### steam 使用NVIDIA显卡

我的笔记本：Acer VN7 592G；Intel + GTX 960M 双显卡

搞好再搞下面。搞坏重新装系统。

先看了http://www.cnblogs.com/smlile-you-me/p/8830949.html，没成功。但很有帮助。
又看了https://www.youtube.com/watch?v=7KgX-LgDwQw 终于成功了！

这次终于能够运行steam了！

首先，安装系统并更新系统，并重启。重启后：

> 修改GRUB

```
# 上面的视频教程需要这个步骤，我没有ACPI错误，没进行这一步也没问题。
$ sudo nano /etc/default/grub
修改：GRUB_CMDLINE_LINUX_DEFAULT="quiet resume=UUID... acpi_osi=! acpi_osi='Windows 2009'"
# 如果要运行NVIDIA显卡，需要添加后面两个 acpi_osi=! acpi_osi='Windows 2009'；不然有可能黑屏进不去系统，或者出现ACPI 错误。
$ sudo update-grub # 更新grub， 再次重启系统
```

> 按照教程安装NVIDIA系统
```
$ sudo mhwd -a pci nonfree 0300

# 上面的步骤安装了9个软件： 
# Packages (9) egl-wayland-1.1.2-1  eglexternalplatform-1.0+3+g7c8f8e2-1  primus-20151110-8  bumblebee-3.2.1-22  lib32-nvidia-utils-1:418.43-2  lib32-primus-20151110-4  linux419-bbswitch-0.8-27  linux419-nvidia-1:418.43-9  nvidia-utils-1:418.43-2

sudo gpasswd -a xxx bumblebee # 将用户添加到bumblebee组，后面运行NVIDIA的setting组件，会提示错误。
```

> **重启**检查是否安装好了

```
$ mhwd -li 可以发现

--------------------------------------------------------------------------------
                  NAME               VERSION          FREEDRIVER           TYPE
--------------------------------------------------------------------------------
         network-r8168            2016.04.20                true            PCI
           video-linux            2018.05.04                true            PCI
video-hybrid-intel-nvidia-bumblebee            2018.08.09               false            PCI


$ sudo optirun glxgears # Ran后跑起来一个轮子测试一下，会出现一个小正方形动图。

我们看看GPU是OFF 还是 ON？
$ watch cat /proc/acpi/bbswitch # refresh every 2 seconds, to see whether GPU is 'ON' or 'OFF'

然后再看一下，有哪些程序运行在NVIDIA上
$ nvidia-smi
```

> 到上面，就已经安装好了NVIDIA，当需要的时候是能够运行NVIDIA的。
>
> 那么怎么启动呢？其实，Manjaro网站上的教程很详细，https://wiki.manjaro.org/index.php?title=Configure_NVIDIA_(non-free)_settings_and_load_them_on_Startup
>
> 当我们需要用显卡运行一个程序，运行命令`primusrun program1` (program是steam主程序，我的会有点问题)
>
> 当我们想要用显卡运行steam里面的游戏，进去steam的库，右键游戏，点`属性`，然后点`设置启动选项`,里面输入`primusrun %command%`，不要输错，不要改。Ran后再运行游戏，就能调用显卡就行了！！

### pytorch 调用显卡

```
# 首先安装
sudo pacman -S python-pytorch-cuda
#　这将安装以下包，之所以不用pytorch官网的方法，是太慢啦。
Packages (8) cblas-3.8.0-2  cuda-10.1.105-6  cudnn-7.5.0.56-1  nccl-2.4.2-4  opencl-nvidia-1:418.43-1  opencv-4.0.1-6  python-numpy-1.16.2-1  python-pytorch-cuda-1.0.1-5

# 测试pytorch能否调用NVIDIA显卡
$ python
>>>import torch
>>>print(torch.cuda.is_available())
False

# 因为NVIDIA显卡没有开！下面命令打开和关闭NVIDIA程序，当是ON的时候，上面就是TRUE了！！

sudo tee /proc/acpi/bbswitch <<< ON
sudo tee /proc/acpi/bbswitch <<< OFF
```