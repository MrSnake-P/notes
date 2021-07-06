[TOC]

## Django 常用指令

* 逆向自动生成数据库models

```python
python manage.py inspectdb
```

* 将生成的model导向models.py

```python
python manage.py inspectdb > app_name/models.py
```

## Django 初始化的配置

* `__init__.py`

```python
import pymysql
pymysql.version_info = (1, 4, 13, "final", 0)
pymysql.install_as_MySQLdb()	# 用pymysql替代mysqldb
```



## Django 模型字段类型

| 字段名        | 参数                        | 意义                   |
| ------------- | --------------------------- | ---------------------- |
| AutoField     |                             | id 自增的 IntegerField |
| BooleanField  |                             | true/false 字段        |
| CharField     | (max_length)                | 中小长度的字符串       |
| DataField     | ([auto_now],[auto_now_add]) | 日期字段类型           |
| DateTimeField |                             | 时间日期字段类型       |
| TextField     |                             | 不限长度的字符串       |

| 参数名      | 作用                              |
| ----------- | --------------------------------- |
| null        | Ture，数据空值为NULL。默认False。 |
| blank       | True，字段允许留空，默认False。   |
| default     | 字段的默认值                      |
| primary_key | 主键                              |
| help_text   | 帮助文本                          |

## Django 模型迁移指令

```python
python manage.py makemigrations
python manage.py migrate

# 检测你对模型文件的修改,并且把修改的部分作为一次迁移(这是一种对于变化的存储形式)
# 在数据库中创建新定义的模型的数据表，数据库同步
```

## Django 连接数据库设置

* `setting.py`

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'internship',	# 数据库
        'HOST': '127.0.0.1',
        'PORT': 3306,
        'USER': 'root',
        'PASSWORD': 'JJP123.'
    }
}
```

## Django 拼接url

1.html中变量表示

```html
<li><a href="{% url '页面' 参数 %}">{{ 参数 }}</a></li>
```

2.views.py中获取参数

```
# url.py
path('data/<name>', views.data, name='页面')
# views.py
def data(request,name):
	
```

## Django 多个app的urls设置

**创建两个app**

1.创建新的app，并新建url.py

```python
app2/url.py
urlpatterns = [
	path('index/', view.index)
]
```

2.在项目url.py中引用api/url.py

添加inclde()

```
url.py
from api import urls as api_urls
urlpatterns = [
	path('api/', include(api_urls))
]
```

3.调用url路径方法

`localhost:port/api/index`

**创建多个app**

```kotlin
web_wallet
  ├───.idea
  │ └───inspectionProfiles
  ├───Apps
  │ ├───app01
  │ │ ├───migrations
  │ │ │ └───__pycache__
  │ │ └───__pycache__
  │ └───app02
  │  ├───migrations
  │  │ └───__pycache__
  │  └───__pycache__
  ├───web_wallet
  │ └───__pycache__
  └───templates
  ├───app01
  └───app02
```

```
setting.py
sys.path.insert(0, os.path.join(BASE_DIR, 'Apps'))
```

```
url.py
urlpatterns = [
  path('admin/', admin.site.urls),
  path('app01/', include("Apps.app01.urls")),
  path('app02/', include("Apps.app02.urls")),
]
```

## Django 连接mongodb数据库

[mongoengine库官方文档](http://docs.mongoengine.org/guide/document-instances.html#saving-and-deleting-documents)

1.需要用mongoengine库

```python
from mongoengine import connect

connect('internship', host='192.168.244.128', port=27017,username='admin',password='JJP123.', authentication_source='admin')
```

2.mongoengine建立orm不需要迁移直接用

```python
models.py
class Insert_data(mongoengine.Document):
    nameid = mongoengine.StringField()
    data = mongoengine.StringField()
views.py
from .models import Insert_data

def create(request):
    Insert_data.object.create(nameid='1',
                              data = 'snake')
    Insert_data.object.filter(nameid='1').first().update(nameid='2')  		
    Insert_data.object.create(nameid='1').first().delete()
    return HttpResponse('success')
```

## Django 修改从数据库读取的时间格式

* 同步页面和数据库的时间（在html中格式)

在html文件中添加过滤器，`<td>{{ User.time|date:"Y-m-d H:i:s" }}</td>` 

其中 User.time 为数据库中获得的数据，`|date:"Y-m-d H:i:s"` 为格式化。

* 同步页面和数据库的时间（在python中格式)

`User.time.strftime("%Y-%m-%d %H:%M:%S")` 用到了 strftime 方法

`time.strftime("%Y-%m-%d %H:%M:%S",time.localtime(timestamp))`

* 修改时区

```python
timeStr = datatime.astimezone(timezone(timedelta(hours=8))).strftime("%Y-%m-%d %H:%M:%S"),
print(timeStr)
```



## Django 插入数据库时间不正确

* 修改 `setting.py` 变量

`USE_TZ = False`

## Django 跨域问题

1. 允许 hosts

   ```python
   settings.py
   ALLOWED_HOST = ['*']
   ```

2. 将 corsheaders 注册到 app 中,安装 `pip install django-cors-headers`

   ```python
   INSTALLED_APPS = [
       'django.contrib.admin',
       'django.contrib.auth',
       'django.contrib.contenttypes',
       'django.contrib.sessions',
       'django.contrib.messages',
       'django.contrib.staticfiles',
       'web.apps.WebConfig',
       'rest_framework',
       'api',
       'corsheaders',
   ]
   ```

3. MIDDLEWARE 中插入如下代码

   第二行下面 `'corsheaders.middleware.CorsMiddleware',`

   ```python
   MIDDLEWARE = [
       'django.middleware.security.SecurityMiddleware',
       'django.contrib.sessions.middleware.SessionMiddleware',
       'corsheaders.middleware.CorsMiddleware',
       'django.middleware.common.CommonMiddleware',
       #'django.middleware.csrf.CsrfViewMiddleware', # 使postman可以请求接口
       'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
   ]
   ```

4. 放在 `ALLOW_HOST` 下配置 django-cors-headers 参数

   ```python
   CORS_ALLOW_CREDENTIALS = True
   CORS_ORIGIN_ALLOW_ALL = True
   CORS_ALLOW_METHODS = (
       'DELETE',
       'GET',
       'OPTIONS',
       'PATCH',
       'POST',
       'PUT',
       'VIEW',
   )
   CORS_ALLOW_HEADERS = (
   'XMLHttpRequest',
   'X_FILENAME',
   'accept-encoding',
   'authorization',
   'content-type',
   'dnt',
   'origin',
   'user-agent',
   'x-csrftoken',
   'x-requested-with',
   'Pragma',
   )
   ```

## Django str 转换为 int

用 `|` 过滤器

`变量|add:"0"`

`{% nameid|add:"0" %}`