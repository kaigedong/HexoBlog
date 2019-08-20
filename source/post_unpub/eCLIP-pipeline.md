---
title: eCLIP-pipeline
date: 2019-01-24 09:50:22
tags:
---





```shell
mkdir software && cd software
wget https://mirrors.tuna.tsinghua.edu.cn/anaconda/miniconda/Miniconda2-latest-Linux-x86_64.sh # 安装到software/python2，不用添加环境变量。

mkdir packages && cd packages
wget https://github.com/YeoLab/gscripts/archive/1.1.zip -O gscripts-1.1.zip
wget https://github.com/YeoLab/clipper/archive/1.1.zip -O clipper-1.1.zip
wget https://github.com/YeoLab/merge_peaks/archive/0.0.6.zip -O merge_peaks-0.0.6.zip

# 解压上面三个文件
source python2/bin/activate
pip install Cython

cd clipper-1.1 && python setup.py install
cd gscripts-1.1 && python setup.py install
cd merge_peaks-0.0.6 && source create_environment.sh
source run_perlbrew_perl5.10.1.sh



```

