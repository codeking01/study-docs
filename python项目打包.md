## 1.常见的打包

```
常用py文件打包
到**.py文件所在的目录，shift+右键-在此处打开pow,注意路径中不要有中文
执行命令：pyinstaller demo.py
在当前的目录下,将会生成两个文件夹:build和dist。dist里面就是所有可执行文件，点击demo.exe就能运行了。
常用用法：
pyinstaller -F demo.py 只在dist中生产一个demo.exe文件。
pyinstaller -D demo.py 默认选项，除了demo.exe外，还会在在dist中生成很多依赖文件
，推荐使用。

```

## 2.将vue项目打包

1.打包成dist文件

修改配置

    build: {
            target: 'modules',
            outDir: 'dist', //指定输出路径
            assetsDir: 'static/vue-content', // 指定生成静态资源的存放路径
            minify: 'terser', // 混淆器，terser构建后文件体积更小
        },

打包命令

`npm run build`

2.将文件复制到app同级

例如我新建了一个`frontend`文件夹,然后将dist文件的内容全部复制到里面去

然后在django的settings文件中添加

    #第一处
    TEMPLATES = [
        {
            'BACKEND': 'django.template.backends.django.DjangoTemplates',
            # 创建的时候的写法
            'DIRS': [BASE_DIR / 'templates',
                     os.path.join(BASE_DIR, 'frontend')
                     ]
            ,
            'APP_DIRS': True,
            'OPTIONS': {
                'context_processors': [
                    'django.template.context_processors.debug',
                    'django.template.context_processors.request',
                    'django.contrib.auth.context_processors.auth',
                    'django.contrib.messages.context_processors.messages',
                ],
            },
        },
    ]
    # 第二处
    STATIC_URL = 'static/'
    STATICFILES_DIRS = [
        # os.path.join(BASE_DIR, 'static'),
        # os.path.join(BASE_DIR, 'static', 'static_root'),# 项目默认会有的路径，如果你部署的不仅是前端打包的静态文件，项目目录static文件下还有其他文件，最好不要删
        os.path.join(BASE_DIR,"frontend/static"), #加上
        os.path.join(BASE_DIR, "dist/static"),# 加上这条
    ]
    APSCHEDULER_RUN_NOW_TIMEOUT  =  25

再次打包静态资源

`python manage.py collectstatic`

## 3.打包django项目

~~设置~~

```
若项目中有css、js等等。在settings文件中加入以下代码。

STATIC_ROOT = os.path.join(BASE_DIR, 'static', 'static_root')
python manage.py collectstatic

在django项目路径下执行python manage.py collectstatic会自动地将STATICFILES_DIRS
列出的目录以及各个App下的static子目录的所有文件复制到STATIC_ROOT。
```

~~django项目urls中加入（这个不行）：~~

```py
from django.conf.urls.static import static
from django.conf import settings
urlpatterns += static(settings.STATIC_URL, document_root=settings.STATIC_ROOT)

#此处是将STATIC_ROOT目录的静态文件复制一份到网页 STATIC_URL路径下。
```

0.***在settings文件关闭debug模式，改为false**

1.在settings文件设置 `STATICFILES_DIRS`

```py
#  执行整合命令后，会将静态资源整合到下面这个目录 STATIC_ROOT和STATICFILES_DIRS写一个就行
# STATIC_ROOT = os.path.join(BASE_DIR, './static/')
STATICFILES_DIRS = [
    # os.path.join(BASE_DIR, 'static'),
    # os.path.join(BASE_DIR, 'static', 'static_root'),# 项目默认会有的路径，如果你部署的不仅是前端打包的静态文件，项目目录static文件下还有其他文件，最好不要删
    os.path.join(BASE_DIR, "static"),  # 加上这条,这个在打包的时候需要修改，不然就将这个路径的内容复制到打包的路径中
    # os.path.join(BASE_DIR, "dist/static"),# 加上这条
]
```

2.找到urls文件（mysite目录下的），修改urlpatterns

```from
from django.urls import path, include, re_path
from django.views import static
from django.conf import settings
urlpatterns = [
    # path('admin/', admin.site.urls),
    path(r'', TemplateView.as_view(template_name="index.html"), name='index'),
    # 路由
    # path('index/', TestModels.index),
    # 包装api的url
    path('api/', include('app01.urls')),
    # path('test/', TestModels.test)
    # """下面三条是新加的，三选一"""
    # re_path('static/(?P<path>.*)$', static.serve, {"document_root": settings.STATIC_ROOT}),
    re_path('static/(?P<path>.*)$', static.serve, {"document_root": settings.STATICFILES_DIRS[0]}),  # 注意，这里取STATICFILES_DIRS里的[0]才行，直接写STATICFILES_DIRS会报服务器500错误
    # re_path('static/(?P<path>.*)$', static.serve, {"document_root": "D:/coder/Python/mysite/templates/static/"}),  # 直接在这里手动写静态文件夹绝对路径
    # 原文链接：https://blog.csdn.net/qq_52410625/article/details/122585356
]
```

3.打包静态资源文件

```bash
python manage.py collectstatic
```

**进入项目目录，shift+右键，在此处打开PowerShell窗口，输入命令pyinstaller -D manage.py**

 **修改manage.spec：**

datas：里边加的是html文件，css、js等等文件。

datas=[] 该配置用于配置static文件和templates文件

hiddenimports：后边会说到。

进入项目目录，shift+右键，在此处打开PowerShell窗口，输入命`pyinstaller manage.spec`重新打包。**

**此时项目目录下会生成一个dist的文件夹，进入dist在进入manage文件夹，shift+右键，在此处打开PowerShell窗口，输入manage.exe runserver。**

注意：

```
No module named XXX，这是因为Django有些module不会自动收集，需要手动添加。
解决方法：在manage.spec文件中修改hiddenimports=[]为hiddenimports=['users','users.apps','sql_server.pyodbc.compiler', '...', '...']
回到了第4步。
提示缺少什么module就在此处添加什么。（很恶心人的是每次只会提示一个错误，需要一直修改manage.spec文件，然后pyinstaller manage.spec重新打包）
```

 **进入dist在进入manage文件夹，shift+右键，在此处打开PowerShell窗口，输入`mange.exe runserver`。不报错则忽略，若报以下错误：**

```
RuntimeError: Script runserver does not exist.
[7964] Failed to execute script manage
解决方案:运行时加--noreload 开关。
即：manage.exe runserver 8000 --noreload
```

**进入dist在进入manage文件夹，shift+右键，在此处打开PowerShell窗口，输入****

   ` mange.exe runserver 8000 --noreload。`

通过exe的方式运行

**拓展：新建一个run.py文件。**

```py
import os
os.system('manage.exe runserver 8000 --noreload')
input()
```

也可以新建这个代码

```py
import os
import sys
import webbrowser
import psutil
import time
BASE_PATH = os.path.abspath('.')
def close_same_progress():
    # 重复点击开始即关闭原来服务重新打开
    # os.system('TASKKILL /F /IM manage.exe')
    try:
        os.system('TASKKILL /F /IM manage.exe')
    except Exception as e:
        print(f"提示信息：{e},忽略即可..")

def run_main():
    sys.path.append("libs")
      port=8000
    url = f'http://127.0.0.1:{port}'
    webbrowser.open_new(url)
    main = BASE_PATH + f"/manage.exe runserver {port} --noreload"
    print('--------------------------')
    print('系统已运行...')
    print('--------------------------')
    os.system(main)
if __name__=="__main__":
    close_same_progress()
    run_main()
```

在run.py文件路径下，shift+右键，在此处打开PowerShell窗口输入`pyinstaller -F run.py `打包run.py

注：也可以加入图标run.ico：打包命令为：

`pyinstaller -F -i run.ico run.py`

将dist文件夹下的run.exe文件移到到 manage.exe同一路径下。

下次双击运行run.exe 就能直接运行django项目了。

 注意：

项目打包时候数据库还出现了问题，反正没有迁移过去（大小为0kb），我直接将原数据库文件复制过去了.

**其他问题**

```
1.pyinstaller打包时提示UPX is not available.
查了一下, 原来是pyinstaller使用UPX压缩, 所以根据下面的步骤安装了一个UPX就好了:

(1) 到官网 https://upx.github.io/ 下载了UPX(我的是Window 32版本), 下载下来是一个压缩包
(2) 解压得到 upx.exe文件
(3) 把exe文件拷贝到pyinstaller目录下, 我的是 C:\Users\king\AppData\Local\Programs\Python\Python310\Scripts
# 原文链接：https://blog.csdn.net/chentianveiko/article/details/107083912
```

**解决打包结束后 cmd窗口依旧存在的问题**

1. 更换模块

    原先使用 os 模块

```py
import os 
os.system("CMD命令")
```

    改用 subprocess 模块

```py
import subprocess
cmd = 'CMD命令'
subprocess.call(cmd, shell=True, stdin=subprocess.PIPE, stdout=subprocess.PIPE, stderr=subprocess.PIPE)
```

2. Pyinstaller 增加 -w 参数
   然而仅仅替换subprecess模块还不够，图形化的Python程序通过 Pyinstaller 打包成exe程序后，执行时还会先弹一个黑色命令窗口来，这个就有些尴尬了。

其实我们使用Pyinstaller 打包时可以增加 -w 参数来取消cmd弹窗
参考链接 https://blog.csdn.net/no1xium/article/details/109097355

```cmd
# pyinstaller -F -w filename.py
pyinstaller -F -w run.py
```


