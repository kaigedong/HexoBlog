---
title: "搬瓦工2: v2ray"
date: 2018-12-06 09:26:42
tags: 
- 搬瓦工
- v2ray
- shadowsocks
- wireguard
---

搬瓦工搭建v2ray 一键搭建wireguard (centos7)

<!--more-->

+ 系统：centos7 64bit with bbs

+ https://www.v2ray.com/chapter_00/install.html

+ https://toutyrater.github.io/prep/install.html

+ 安装日志

    ```
    # bash <(curl -L -s https://install.direct/go.sh)
    ## 安装位置
    /usr/bin/v2ray/v2ray：V2Ray 程序；
    /usr/bin/v2ray/v2ctl：V2Ray 工具；
    /etc/v2ray/config.json：配置文件；
    /usr/bin/v2ray/geoip.dat：IP 数据文件
    /usr/bin/v2ray/geosite.dat：域名数据文件
    ```

## shadowsocks

```
pip install shadowsocks
```



## wireguard

+ https://zrstea.com/268/

+ https://www.wireguard.com/install/

+ https://centos.pkgs.org/7/epel-x86_64/dkms-2.6.1-1.el7.noarch.rpm.html

+ https://support.azul.com/hc/en-us/articles/217293566-Using-DKMS-on-RHEL-CentOS-or-Oracle-Linux-to-build-ZST-for-other-kernel-versions

+ https://www.covertness.me/2018/03/25/%E8%BD%BB%E6%9D%BE%E5%87%A0%E6%AD%A5%E6%90%AD%E5%BB%BA%20WireGuard%20%EF%BC%88%E5%BF%AB%E9%80%9F%E5%AE%89%E5%85%A8%E7%9A%84%E4%B8%8B%E4%B8%80%E4%BB%A3%20VPN%EF%BC%89/

  ```
  # 安装过程：
  wget https://dl.fedoraproject.org/pub/epel/7/x86_64/Packages/e/epel-release-7-11.noarch.rpm
  rpm -Uvh epel-release-7-11.noarch.rpm
  yum clean all
  yum makecache
  yum-config-manager --disable epel
  yum --enablerepo=epel install dkms
  yum install wireguard-dkms wireguard-tools
  ```

推荐 wireguard 一键安装脚本（centos7）：https://github.com/atrandys/wireguard

