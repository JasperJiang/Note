# Django相关知识

### Django 命令和操作
**启动项目**
`django-admin.py startproject djproject`  
**建立一个app**  
`python manage.py startapp jobs`  
**创建超级管理员**  
`python manage.py createsuperuser`  
**创建数据库并且同步**  
`python manage.py makemigrations
python manage.py migrate`

### Model
python manage.py inspectdb  
这样就可以在命令行看到数据库的模型文件了  
python manage.py inspectdb > app/models.py  

model field 类型  
1、AutoField  
     一个自增的IntegerField，一般不直接使用，Django会自动给每张表添加一个自增的primary key。  

2、BigIntegerField  
    64位整数， -9223372036854775808 到 9223372036854775807。默认的显示widget 是 TextInput.  

3、BinaryField （ Django 1.6 版本新增 ）  
    存储二进制数据。不能使用 filter 函数获得 QuerySet  

4、BooleanField  
    True/False，默认的widget 是 CheckboxInput。  
    如果需要置空，则必须用 NullBooleanField 代替。  
    Django 1.6 修改：BooleanField 的默认值 由 False 改为 None，在 default 属性未设置的情况下。  

5、CharField  
    存储字符串。必须有 max_length 参数指定长度。默认的form widget 是 TextInput  
    如果字符串巨长，推荐使用 TextField。  

6、CommaSeparatedIntegerField  
    一串由逗号分开的整数。必须有max_length 参数。

7、DateField  
    日期，与python里的datetime.date 实例同。有以下几个可选的选项，均为bool类型：  
         DateField.auto_now: 每次执行 save 操作的时候自动记录当前时间，常作为最近一次修改的时间 使用。注意：总是在执行save 操作的时候执行，无法覆盖。  
         DateField.auto_now_add: 第一次创建的时候添加当前时间。常作为 创建时间 使用。注意：每次create 都会调用。  
    默认的form widget 是 TextInput。  
    注意：设置auto_now 或者 auto_now_add 为 True 会导致当前自动拥有 editable=False 和 blank = True 设置。

8、DateTimeField  
    日期+时间。与python里的 datetime.datetime 实例同。常用附加选项和DateField一样。  
    默认 form widget 是一个 TextInput

9、DecimalField  
    设置了精度的十进制数字。  
    A fixed-precision decimal number, represented in Python by a Decimal instance. Has two required arguments:  
    DecimalField.max_digits  
    The maximum number of digits allowed in the number. Note that this number must be greater than or equal to decimal_places.  
    DecimalField.decimal_places?  
    The number of decimal places to store with the number.  
    For example, to store numbers up to 999 with a resolution of 2 decimal places, you’d use:  
    models.DecimalField(..., max_digits=5, decimal_places=2)  
    And to store numbers up to approximately one billion with a resolution of 10 decimal places:  
    models.DecimalField(..., max_digits=19, decimal_places=10)  
    The default form widget for this field is a TextInput.  

10、EmailField  
    在 CharField 基础上附加了 邮件地址合法性验证。不需要强制设定 max_length  
    注意：当前默认设置 max_length 是 75，虽然已经不符合标准，但未了向前兼容，未修改。  

11、FileField  
    文件上传。不支持 primary_key 和 unique 选项。否则会报 TypeError 异常。  
    必须设置 FileField.upload_to 选项，这个是 本地文件系统路径，附加在 MEDIA_ROOT 设置的后边，也就是 MEDIA_ROOT 下的子目录相对路径。  
    默认的form widget 是 FileInput。 
    使用 FileField 和 ImageField 需要以下步骤：  
        （1）修改 settting.py，设置 MEDIA_ROOT（使用绝对路径），指定用户上传的文件保存在哪里。设置 MEDIA_URL，作为 web地址 前缀，要保证 MEDIA_ROOT 目录对运行 Django 的用户是可写的； 
        （2）在 model 中增加 FileField 或 ImageField，并指定 upload_to 选项指定存在 MEDIA_ROOT 的哪个子目录里；  
        （3）存在数据库里的是什么东西呢？是 File 或 Image相对于 MEDIA_ROOT 的相对路径，你可以在 Django 里方便的使用这个地址，比如你的 ImageField 叫 tupian，你可以在 template 中用{{object.tupian.url}}。  
    举个例子：假设你的 MEDIA_ROOT='/home/media'，upload_to 设置为 'photos/%Y/%m/%d'，'%Y/%m/%d' 部分使用strftime() 提供。如果你在 2013年10月10日上传了一个文件，那么它就存在 /home/media/photos/2013/10/10/ 下。  
    文件在 model实例 执行 save操作的同时保存，所以文件在model实例执行save之前，硬盘的上的文件名的是不可靠的。  
    注意：要验证用户上传的文件确实是自己需要的，以防止安全漏洞出现。  
    默认情况下，FileField 在数据库中表现为 varchar(100) 的一个列。你可以使用 max_length 来改变这个大小。  

11、FileField 和 FieldFile  
    当你访问 一个 model 内的 FileField 时，将得到一个 FieldFile 实例来访问实际的文件。这个类提供了几个属性和方法用来和实际的文件数据交互：  
    FieldFile.url：只读属性，获取文件的相对URL地址;  
    FieldFile.open( mode = 'rb' )：打开文件，和python 的 open 一样;  
    FieldFile.close()：和 python 的 file.close() 一样;  
    FieldFile.save( name, content, save=True )：name 是文件名，content 是包含了文件内容的 django.core.files.File 实例，与 python 的 file 不一样。The optional save argument controls whether or not the instance is saved after the file has been altered. Defaults to True。  
    两种方式 进行 content 设置：  
    
```python
from django.core.files import File
    f = open( 'helo.txt' )
    content = File(f)
```
    
    另一种是：  
    
```python
from django.core.files.base import ContentFile
    content = ContentFile( 'helloworld' )
```
    更多内容可见：https://docs.djangoproject.com/en/dev/topics/files/  
    FieldFile.delete( save = True )：删除当前的文件。如果文件已经打开，则自动关闭。The optional save argument controls whether or not the instance is saved after the file has been deleted. Defaults to True.  
    值得注意的是：当一个 model实例 被删除之后，相关联的文件并没有被删除，需要自己清除！  

12、FloatField  
    与 python 里的 float 实例相同，默认的 form widget 是 TextInput。  
    虽然 FloatField 与 DecimalField 都是表示实数，但却是不同的表现形式，FloatField 用的是 python d float 类型，但是 DecimalField 用的却是 Decimal 类型。区别可见：http://docs.python.org/2.7/library/decimal.html#decimal  

13、ImageField  
    在 FileField 基础上加上是否是合法图片验证功能的一个类型。  
    除了 FileField 有的属性外，ImageField 另有 height 和 width 属性。  
    To facilitate querying on those attributes, ImageField has two extra optional arguments:  

    ImageField.height_field  
    Name of a model field which will be auto-populated with the height of the image each time the model instance is saved.  

    ImageField.width_field  
    Name of a model field which will be auto-populated with the width of the image each time the model instance is saved.  

    注意：需要安装 PIL 或者 Pillow 模块。在数据库中同样表现为 varchar(100)，可通过 max_length 改大小。  

14、IntegerField  
    整数，默认的form widget 是 TextInput。  

15、IPAddressField  
    IP地址，字符串类型，如 127.0.0.1。默认 form widget 是 TextInput。  

16、TextField  
    大文本，巨长的文本。默认的 form widget 是 Textarea。  
    注意，如果使用 MySQLdb 1.2.1p2 和 utf-8_bin 编码，会有一些问题https://docs.djangoproject.com/en/dev/ref/databases/#mysql-collation。具体问题未分析，可自行避开。  

17、URLField  
    加了 URL 合法性验证的 CharField。  
    默认的 form widget 是 TextInput。  
    默认max_length=200，可修改。  

18、ForeignKey / ManyToManyField / OneToOneField / SmallIntegerField / SlugField / PositiveSmallIntegerField / PositiveIntegerField  

Field 选项  

null  
      boolean 值，缺省设置为false。通常不将其用于字符型字段上，比如CharField，TextField上。字符型字段如果没有值会返回空字符串。  

blank  
      boolean 值，该字段是否可以为空。如果为假，则必须有值。  

choices  
     元组值，一个用来选择值的2维元组。第一个值是实际存储的值，第二个用来方便进行选择。如SEX_CHOICES=((‘F’,’Female’),(‘M’,’Male’),)  

db_column  
      string 值，指定当前列在数据库中的名字，不设置，将自动采用model字段名；  

db_index  
      boolean 值，如果为True将为此字段创建索引；  

default  
      给当前字段设定的缺省值，可以是一个具体值，也可以是一个可调用的对象，如果是可调用的对象将每次产生一个新的对象；  

editable  
      boolean 值，如果为假，admin模式下将不能改写。缺省为真；  

error_messages  
      字典，设置默认的出错信息，可覆盖的key 有 null, blank, invalid, invalid_choice, 和 unique。  

help_text  
      admin模式下帮助文档  
      form widget 内显示帮助文本。  

primary_key  
      设置主键，如果没有设置django创建表时会自动加上：id = meta.AutoField(‘ID’, primary_key=True)  
      primary_key=True implies blank=False, null=False and unique=True. Only one primary key is allowed on an object.  

radio_admin  
      用于 admin 模式下将 select 转换为 radio 显示。只用于 ForeignKey 或者设置了choices  

unique  
      boolean值，数据是否进行唯一性验证；  

unique_for_date  
      字符串类型，值指向一个DateTimeField 或者 一个 DateField的列名称。日期唯一，如下例中系统将不允许title和pub_date两个都相同的数据重复出现  
      
```python
title = meta.CharField(maxlength=30, unique_for_date=’pub_date’ )  
```

unique_for_month / unique_for_year  
      用法同上  

verbose_name  
      string类型。更人性化的列名。  

validators  
        有效性检查。无效则抛出 django.core.validators.ValidationError 异常。  
    如何实现检查器 见：https://docs.djangoproject.com/en/dev/ref/validators/  


Meta类class Meta  
ordering:排序，（注意这里排序是要消耗数据库运算的）, "-num"是按num降序排列，然后接着按length 升序排列。  
verbose_name: 便于人识读的名字，单数形式  
verbose_name_plural: 同上，但是是复数形式，如果不指定，系统默认是在后面加上“s”  

继承admin User  

from django.contrib.auth.models import AbstractUser  
class User(AbstractUser):  
在setting.py中添加  
AUTH_USER_MODEL = "webuser.User"  

### URL(url.py)
**引用views的方法：**  
     引入包  
          from app import views  
     在urlpatterns中添加  
          url(r'^$', views.index),  


**引到app下的url的方法：**
1.引入包  

```python
from django.conf.urls import include
```
2.引入app的url

```python
from app import urls
```
3.在urlpatterns中添加  

```python
url(r'', include(urls)),
```
### Django 与 AngularJS 冲突
**通过 verbatim 来暂停jinja2的解析**  

```html
{% raw %}<h1 class="user-name">{{ user.name }}</h1>{% endraw %}
```
**修改angular的符号**  
On the front end, after instantiating our Angular app, we can tell Angular to look for different binding tags (instead of '{{' and '}}').   

```javascript
var app = angular.module('myApp', []);

app.config(['$interpolateProvider', function($interpolateProvider) {
  $interpolateProvider.startSymbol('{[{');
  $interpolateProvider.endSymbol('}]}');
}]);
```

### 跨域访问问题
使用django-cors-headers  

https://github.com/ottoyiu/django-cors-headers/  

`pip install django-cors-headers `

```python
INSTALLED_APPS = (
  ...
  'corsheaders',
  ...
)

MIDDLEWARE = [ # Or MIDDLEWARE_CLASSES on Django < 1.10
  ...
  'corsheaders.middleware.CorsMiddleware',
  'django.middleware.common.CommonMiddleware',
  ...
]

CORS_ORIGIN_WHITELIST = (
  'google.com',
  'hostname.example.com',
  'localhost:8000',
  '127.0.0.1:9000'
)

CORS_ALLOW_METHODS
```
### Static  
当运行 python manage.py collectstatic 的时候  
STATIC_ROOT 文件夹 是用来将所有STATICFILES_DIRS中所有文件夹中的文件，以及各app中static中的文件都复制过来  
把这些文件放到一起是为了用apache等部署的时候更方便  
STATIC_ROOT = os.path.join(BASE_DIR, 'collected_static')  

其它 存放静态文件的文件夹，可以用来存放项目中公用的静态文件，里面不能包含 STATIC_ROOT  
如果不想用 STATICFILES_DIRS 可以不用，都放在 app 里的 static 中也可以  

```python
STATICFILES_DIRS = (
  os.path.join(BASE_DIR, "common_static"),
  '/path/to/others/static/', # 用不到的时候可以不写这一行
)
```
这个是默认设置，Django 默认会在 STATICFILES_DIRS中的文件夹 和 各app下的static文件夹中找文件
注意有先后顺序，找到了就不再继续找了  

```python
STATICFILES_FINDERS = (
  "django.contrib.staticfiles.finders.FileSystemFinder",
  "django.contrib.staticfiles.finders.AppDirectoriesFinder"
)
```
之后在页面中直接使用如/static/css/bootstrap.min.css即可  

### 注意事项  
在Django 1.10 中url.py文件不需要加引号了如：  

```python
url(r'^admin/', admin.site.urls),
```
include('webuser.urls') include里面是字符串  


