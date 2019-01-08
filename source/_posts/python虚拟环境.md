---
title: python虚拟环境
date: 2018-12-25 09:56:46
tags:
---

virtualenv 和 pyenv的使用

<!--more-->

## 第一部分：virtualenv的使用

```
# 首先用pip3安装virtualenv
pip3 install virtualenv

# 创建独立的python运行环境：
virtualenv --no-site-packages venv

# --always-copy 默认行为是符号链接需要的文件， 可以强制复制，而非拷贝。
# -p PYTHON_EXE 指定python解析器


# 激活
source venv/bin/activate

# 退出
deactivate
```



```
pip freeze > requirements.txt
pip install -r requirements.txt
```



## 第二部分：pyenv的使用