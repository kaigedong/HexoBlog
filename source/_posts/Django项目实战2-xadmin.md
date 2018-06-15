---
title: 'Django项目实战2: xadmin'
date: 2018-05-14 21:19:26
tags: Django
---



Xadmin 实战

<!--more-->

创建超级用户`admin`

```
python manage.py createsuperuser
```

修改语言：

```
# LANGUAGE_CODE = 'en-us'
LANGUAGE_CODE = 'zh-hans'

# TIME_ZONE = 'UTC'
TIME_ZONE = 'Asia/Shanghai'

USE_I18N = True

USE_L10N = True

# USE_TZ = True # 为UTC时间，即国际事件。我们改为本地时间。
USE_TZ = False
```

这时我们看到admin中只有组这个信息，现在我们把user注册进来：

修改 `apps/users/admin.py`为：

```
from django.contrib import admin

# Register your models here.

from .models import UserProfile

class UserProfileAdmin(admin.ModelAdmin):
    pass

admin.site.register(UserProfile,UserProfileAdmin)
```

这时再刷新，就能看到`user`被注册进来了。

然后添加一个用户，但是保存时出错：`Django后台添加用户报错：1452, 'Cannot add or update a child row: a foreign key constraint fails`；参考[Link](https://www.jianshu.com/p/7516065f91a3),进行解决：

```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'mxonline2',
        'USER': 'root',
        'PASSWORD': '你的密码',
        'HOST':'127.0.0.1',
        'OPTIONS': {
          "init_command": "SET foreign_key_checks=0;",
      }

    },

}
```

> 安装xadmin：`pip install xadmin`

> 将xadmin注册进APPS:

```
INSTALLED_APPS = [
...
	'xadmin',
    'crispy_forms'
]
```

> 将默认的admin，指向xadmin: 修改urls.py

```
import xadmin

urlpatterns = [
    url(r'^xadmin/', xadmin.site.urls),
]
```

> 然后，将之前注册的admin取消掉：修改为：
>
> ```
> #~/DjangoProject/mxonline
> from django.contrib import admin
>
> # Register your models here.
>
> #from .models import UserProfile
>
> #class UserProfileAdmin(admin.ModelAdmin):
> #    pass
>
> #admin.site.register(UserProfile,UserProfileAdmin)
> ```

> 然后我们需要同步xadmin的表，用`makemigration`,`migrate`命令，就能访问了！

+ 省去了我们手动注册的步骤，会自动发现apps内的表，并注册进来！

> 第二种安装方式(推荐)：
>
> ```
> $ git clone https://github.com/sshwsfc/xadmin.git
> cd xadmin
> cp -r xadmin ../mxonline/extra_apps # cp xadmin中的xadmin文件夹到项目根目录的extra_apps文件夹。
> cd ../mxonline/extra_apps
> touch __init__.py # 这样这个目录就是可导入的了。
> # 然后修改setting.py
> # 添加：
> sys.path.insert(0,os.path.join(BASE_DIR,'extra_apps'))
> 然后卸载xadmin(但是会保留其依赖。)
> $ pip uninstall xadmin
> ```



我遇到了很多问题，最终[参考](http://coding.imooc.com/learn/questiondetail/28647.html)解决

```
如：django.core.exceptions.AppRegistryNotReady: Apps aren't loaded yet.
查找之后，我：
$ pip install future six httplib2

安装之后，
  File "/home/bobo/DjangoProject/mxonline/extra_apps/xadmin/plugins/importexport.py", line 48, in <module>
    from import_export.admin import DEFAULT_FORMATS, SKIP_ADMIN_LOG, TMP_STORAGE_CLASS
ImportError: No module named import_export.admin

我：
$pip install django-import-export
```



> `admin`会使用`admin.py`来注册，我们新建`adminx.py`,根据该文件注册models:
>
> ```
> # _*_coding:utf-8_*_
>
> import xadmin
>
> from .models import EmailVerifyRecord
>
> class EmailVerifyRecordAdmin(object):
>     list_display = ["code","email","send_type","send_time"] # 自定义显示的字段
>     search_fields = ["code","email","send_type"] # 在这些字段中进行搜索
>     list_filter = ["code","email","send_type","send_time"] # 过滤器，进一步筛选
> xadmin.site.register(EmailVerifyRecord,EmailVerifyRecordAdmin)
> ```
>
> 这样就把邮箱验证码注册进来了。

> 添加搜索功能，自定义显示字段如上。



