---
title: Django项目实战1：用户数据库
date: 2018-05-13 22:22:44
tags: 
---



Django 项目实战记录

<!--more-->



## 1. 新建项目

```
virtualenv virEnv # Python 2.7.15rc1
source virEnv/bin/activate
pip install django==1.9
django-admin.py startproject mxonline
pip install mysql-python # mysql-python-1.2.5
```

```
$ tree mxonline
mxonline
├── manage.py
└── mxonline
    ├── __init__.py
    ├── settings.py
    ├── urls.py
    └── wsgi.py
```

## 2. 配置,创建数据库

> 将`settings.py` 中的

```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
    }
}
```

变更为：

```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': "mxonline",
        "USER":"root",
        "PASSWORD":"xxxxxx",
        "HOST":"127.0.0.1"
    }
}
```

然后，navicate新建一个数据库，名为`mxonline`；这时，用navicate查看，是没有表的。我们进行生成Django默认的数据表：

```
python manage.py makemigrations
python manage.py migrate
```

## 3. 设计User Model

```
django-admin.py startapp users # 在web程序中，user表都是第一个被设计的。
```

修改users下的models.py：

```
from __future__ import unicode_literals

from django.db import models

# Create your models here.
```

变更为：

```
# _*_ encoding:utf-8 _*_

from __future__ import unicode_literals
from datetime import datetime

from django.db import models

from django.contrib.auth.models import AbstractUser # 继承这个，这个也就是Django默认的user数据表的设计。

# Create your models here.


class UserProfile(AbstractUser):
    nick_name = models.CharField(max_length=50,verbose_name=u"昵称",default="")
    birday = models.DateField(verbose_name=u"生日",null=True,blank=True) # 可以为空
    gender = models.CharField(max_length=5,choices=(("male","男"),("female","女")),default="女")
    address = models.CharField(max_length=100,default=u"")
    mobile = models.CharField(max_length=11,null=True,blank=True)
    image = models.ImageField(upload_to="image/%Y/%m",default=u"image/default.png",max_length=100) # 用户头像，自>定义上传路径。

    class Meta:
        verbose_name = "用户信息"
        verbose_name_plural = verbose_name

    def __unicode__(self):# 重载unicode这个方法，不然在print userprofile实例的时候，不能打印我们自定义的字符串。
        return self.username # 这个username是继承的AbstractUser的


class EmailVerifyRecord(models.Model):
    code = models.CharField(max_length=20,verbose_name=u"验证码")
    email = models.EmailField(max_length=50,verbose_name=u"邮箱")
    send_type = models.CharField(choices=(("register",u"注册"),("forget",u"找回密码")),max_length=10) # 注册时的，
还是找回秘密的验证码
    send_time = models.DateTimeField(default=datetime.now)

    class Meta:
        verbose_name = "邮箱验证码"
        verbose_name_plural = verbose_name

class Banner(models.Model):
    title = models.CharField(max_length=100,verbose_name=u"标题")
    image = models.ImageField(upload_to="banner/%Y/%m",verbose_name=u"轮播图",max_length=100)
    url = models.URLField(max_length=200,verbose_name="访问地址")
    index = models.IntegerField(default=100,verbose_name = u"顺序")
    add_time = models.DateTimeField(default=datetime.now,verbose_name=u"添加时间")

    class Meta:
        verbose_name = u"轮播图"
        verbose_name_plural = verbose_name
```

> pip install pillow 使得支持Image这个方法。

然后，修改settings.py ,使得我们的app被注册进来：

```
# Application definition

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'users'
]

AUTH_USER_MODEL = "users.UserProfile" # 重载这个方法。用UserProfile替换user
```



## 4. course model的编写

```
django-admin.py startapp courses
```

```
# _*_ coding:utf-8 _*_
from __future__ import unicode_literals
from datetime import datetime


from django.db import models

# Create your models here.

class Course(models.Model):
    name = models.CharField(max_length=50,verbose_name=u"课程名")
    desc = models.CharField(max_length=300,verbose_name=u"课程描述")
    detail = models.TextField(verbose_name=u"课程详情") # 不限制长度
    degree = models.CharField(choices=(("cj","初级"),("zj","中级"),("gj","高级")),max_length=2)
    learn_times = models.IntegerField(default=0,verbose_name="学习时长(分钟数)")
    students = models.IntegerField(default=0,verbose_name="学习人数")
    fav_nums = models.IntegerField(default=0,verbose_name=u"收藏人数")
    image = models.ImageField(upload_to="courses/%Y/%m",verbose_name="封面图",max_length=100)
    click_nums = models.IntegerField(default=0,verbose_name="点击数")
    add_time = models.DateTimeField(default=datetime.now,verbose_name="添加时间")

    class Meta:
        verbose_name="课程"
        verbose_name_plural = verbose_name

class Lesson(models.Model):
    course = models.ForeignKey(Course,verbose_name="课程")
    name = models.CharField(max_length=100,verbose_name="章节名")
    add_time = models.DateTimeField(default=datetime.now,verbose_name="添加时间")

    class Meta:
        verbose_name = "章节"
        verbose_name_plural = verbose_name


class Video(models.Model):
    lesson = models.ForeignKey(Lesson,verbose_name=u"章节")
    name = models.CharField(max_length=100,verbose_name="视频名")
    add_time = models.DateTimeField(default=datetime.now,verbose_name="添加时间")

    class Meta:
        verbose_name = "视频"
        verbose_name_plural = verbose_name

class CourseResource(models.Model):
    course = models.ForeignKey(Course,verbose_name="课程")
    name = models.CharField(max_length=100,verbose_name="名称")
    download = models.FileField(upload_to="course/resource/%Y/%m",verbose_name="资源文件",max_length=100)
    dd_time = models.DateTimeField(default=datetime.now,verbose_name="添加时间")

    class Meta:
        verbose_name = "课程资源"
        verbose_name_plural = verbose_name
```

## 5. 课程机构

```
# _*_ coding:utf-8 _*_
from __future__ import unicode_literals
from datetime import datetime


from django.db import models

# Create your models here.
class CityDict(models.Model):
    name = models.CharField(max_length=20,verbose_name=u"城市")
    desc = models.CharField(max_length=200,verbose_name=u"描述")
    add_time = models.DateTimeField(default = datetime.now)

    class Meta:
        verbose_name = "城市"
        verbose_name_plural = verbose_name


class CourseOrg(models.Model):
    name = models.CharField(max_length=50,verbose_name=u"机头名称")
    desc = models.TextField(verbose_name=u"机构描述")
    click_nums = models.IntegerField(default=0,verbose_name="点击数")
    fav_nums = models.IntegerField(default=0,verbose_name="收藏数")
    image = models.ImageField(upload_to="org/%Y/%m",verbose_name=u"封面图")
    address = models.CharField(max_length=150,verbose_name=u"机构地址")
    city = models.ForeignKey(CityDict,verbose_name="所在城市")
    add_time = models.DateTimeField(default = datetime.now)

    class Meta:
        verbose_name = "课程机构"
        verbose_name_plural = verbose_name

class Teacker(models.Model):
	org = models.ForeignKey(CourseOrg,verbose_name=u"所属机构")
    name = models.CharField(max_length=50,verbose_name=u"教师名")
    work_years = models.IntegerField(default=0,verbose_name="工作年限")
    work_company = models.CharField(max_length=50,verbose_name="就职公司")
    work_position = models.CharField(max_length=50,verbose_name="公司职位")
    point = models.CharField(max_length=50,verbose_name="教学特点")
    click_nums = models.IntegerField(default=0,verbose_name="点击数")
    add_time = models.DateTimeField(default=datetime.now,verbose_name="添加时间")

    class Meta:
        verbose_name="教师"
        verbose_name_plural = verbose_name
 
```

## 6. 用户相关操作

```
# _*_ coding:utf-8 _*_
from __future__ import unicode_literals

from datetime import datetime

from django.db import models

from users.models import UserProfile
from courses.models import Course

# Create your models here.
class UserAsk(models.Model): # 用户咨询
    name = models.CharField(max_length=20,verbose_name="姓名")
    mobile = models.CharField(max_length=11,verbose_name="手机")
    course_name = models.CharField(max_length=50,verbose_name=u"课程名")
    add_time = models.DateTimeField(default=datetime.now,verbose_name="添加时间")

    class Meta:
        verbose_name = u"用户咨询"
        verbose_name_plural = verbose_name

class CourseComments(models.Model):
    '''课程评论'''
    user = models.ForeignKey(UserProfile, verbose_name=u"用户")
    course = models.ForeignKey(Course,verbose_name=u"课程")
    comments = models.CharField(max_length=200,verbose_name="评论")
    add_time = models.DateTimeField(default=datetime.now,verbose_name="添加时间")

    class Meta:
        verbose_name = u"课程评论"
        verbose_name_plural = verbose_name

class UserFavorite(models.Model):
    user = models.ForeignKey(UserProfile,verbose_name=u"用户")
    fav_id = models.IntegerField(default = 0,verbose_name="数据ID")
    fav_type = models.IntegerField(choices=((1,"课程"),(2,"课程机构"),(3,"讲师")),default=1,verbose_name=u"收藏类型")
    add_time = models.DateTimeField(default=datetime.now,verbose_name="添加时间")

    class Meta:
        verbose_name = u"用户收藏"
        verbose_name_plural = verbose_name

class UserMessage(models.Model):
    user = models.IntegerField(default = 0,verbose_name="接受用户")
    message = models.CharField(max_length=500,verbose_name="消息内容")
    has_read = models.BooleanField(default=False,verbose_name="是否已读")
    add_time = models.DateTimeField(default=datetime.now,verbose_name="添加时间")

    class Meta:
        verbose_name = u"用户消息"
        verbose_name_plural = verbose_name

class UserCourse(models.Model):
    user = models.ForeignKey(UserProfile,verbose_name=u"用户")
    course = models.ForeignKey(Course,verbose_name=u"课程")
    add_time = models.DateTimeField(default=datetime.now,verbose_name="添加时间")

    class Meta:
        verbose_name = u"用户课程"
        verbose_name_plural = verbose_name
```

修改：

```
# Application definition

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'users',
    'courses',
    'organization',
    'operation'
]
```

生成：

```
python manage.py makemigrations
python manage.py migrate
```

### 添加搜索路径

这里我们新建了4个app，为了方便管理，这里统一将它们移到apps文件夹下；这时，为了不修改上面程序，我们将apps这个文件夹添加到Python搜索路径：

修改settings.py:

```
import sys
# Build paths inside the project like this: os.path.join(BASE_DIR, ...)
BASE_DIR = os.path.dirname(os.path.dirname(os.path.abspath(__file__)))

sys.path.insert(0,os.path.join(BASE_DIR,'apps')) # 这样就好啦
```

