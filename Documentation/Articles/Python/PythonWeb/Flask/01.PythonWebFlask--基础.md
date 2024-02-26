[toc]

# PythonWebFlask

![image](https://picsur.cloud.fairies.ltd/i/35fa2565-9bb3-490e-88ab-29ac9cca771c.webp)

## 一. Flask

Flask诞生于2010年, 是Armin ronacher(人名)用 Python 语言基于 Werkzeug 工具箱编写的轻量级Web开发框架

Flask 本身相当于一个内核, 其他几乎所有的功能都要用到扩展(邮件扩展Flask-Mail, 用户认证Flask-Login, 数据库Flask-SQLAlchemy), 都需要用第三方的扩展来实现. 比如可以用 Flask 扩展加入ORM,窗体验证工具, 文件上传,身份验证等. Flask 没有默认使用的数据库, 你可以选择 MySQL, 也可以用 NoSQL

其 WSGI 工具箱采用 Werkzeug(路由模块), 模板引擎则使用 Jinja2. 这两个也是 Flask 框架的核心.

## 二. 框架对比

### 1. 框架轻重

重量级的框架: 为方便业务程序的开发, 提供了丰富的工具,组件, 如Django

轻量级的框架: 只提供Web框架的核心功能, 自由,灵活,高度定制, 如Flask,Tornado

### 2. 与Django对比

django提供了:

django-admin快速创建项目工程目录

manage.py 管理项目工程

orm模型(数据库抽象层)

admin后台管理站点

缓存机制

文件存储系统

用户认证系统

而这些, flask都没有, 都需要扩展包来提供

## 三. 常用扩展包

- Flask-SQLalchemy: 操作数据库;
- Flask-script: 插入脚本;
- Flask-migrate: 管理迁移数据库;
- Flask-Session: Session存储方式指定;
- Flask-WTF: 表单;
- Flask-Mail: 邮件;
- Flask-Bable: 提供国际化和本地化支持, 翻译;
- Flask-Login: 认证用户状态;
- Flask-OpenID: 认证;
- **Flask-RESTful: 开发REST API的工具;**
- Flask-Bootstrap: 集成前端Twitter Bootstrap框架;
- Flask-Moment: 本地化日期和时间;
- Flask-Admin: 简单而可扩展的管理接口的框架

# 环境安装

## 一. 虚拟环境和pip的命令

```shell
# 虚拟环境
mkvirtualenv  # 创建虚拟环境
rmvirtualenv  # 删除虚拟环境
workon  # 进入虚拟环境、查看所有虚拟环境
deactivate  # 退出虚拟环境

# pip
pip install  # 安装依赖包
pip uninstall  # 卸载依赖包
pip list  # 查看已安装的依赖包
pip freeze  # 冻结当前环境的依赖包
```

## 二. 创建虚拟环境

```shell
mkvirtualenv flask -p python3
```

## 三. 安装Flask

```shell
pip install flask
```

# Flask的HelloWorld程序

## 一. Flask程序编写

- 创建helloworld.py文件

```python
# 导入Flask类
from flask import Flask

#Flask类接收一个参数__name__
app = Flask(__name__)

# 装饰器的作用是将路由映射到视图函数index
@app.route('/')
def index():
    return 'Hello World'

# Flask应用程序实例的run方法启动WEB服务器
if __name__ == '__main__':
    app.run()
```

## 二. 启动运行

```shell
python helloworld.py
```

# 参数说明

## 一. Flask对象初始化参数

- import_name
  - Flask程序所在的包(模块)，传 `__name__` 就可以
  - 其可以决定 Flask 在访问静态文件时查找的路径
- static_url_path
  - 静态文件访问路径，可以不传，默认为：`/ + static_folder`
- static_folder
  - 静态文件存储的文件夹，可以不传，默认为 `static`
- template_folder
  - 模板文件存储的文件夹，可以不传，默认为 `templates`

### 1. 默认参数情况下

```python
app = Flask(__name__)
```

文件目录

```tex
----
  |---static
  |     |--- 1.png
  |---helloworld.py
```

访问 `127.0.0.1:5000/static/1.png` 就可以访问到图片

### 2. 修改参数的情况下

```python
app = Flask(__name__, static_url_path='/url_path_param', static_folder='folder_param')
```

文件目录

```tex
----
  |---folder_param     # 此处目录名变化
  |     |--- 1.png
  |---helloworld.py
```

访问`127.0.0.1:5000/url_path_param/1.png`才可以访问到图片

## 二. 应用程序配置参数

对于Flask对象初始化参数仅仅设置的是Flask本身的属性，比如：

- Flask从哪里读取静态文件
- Flask从哪里读取模板文件
- ...

应用程序配置参数设置的是一个Web应用工程的相关信息，比如：

- 数据库的连接信息
- 日志的配置信息
- 自定义的配置信息
- ...

### 1. 使用方式

Django将所有配置信息都放到了settings.py文件中，而Flask则不同。

**Flask将配置信息保存到了`app.config`属性中，该属性可以按照字典类型进行操作**

#### (1). 读取

- `app.config.get(name)`
- `app.config[name]`

#### (2). 设置

主要使用以下三种方式：

- **从配置对象中加载**

`app.config.from_object(配置对象)`

```python
class DefaultConfig(object):
    """默认配置"""
    SECRET_KEY = 'TPmi4aLWRbyVq8zu9v82dWYW1'

app = Flask(__name__)

app.config.from_object(DefaultConfig)

@app.route("/")
def index():
    print(app.config['SECRET_KEY'])
    return "hello world"
```

应用场景：

作为默认配置写在程序代码中, 可以继承

```python
class DevelopmentConfig(DefaultConfig):
    DEBUG=True
```

- 从配置文件中加载

`app.config.from_pyfile(配置文件)`

新建一个配置文件setting.py

```python
SECRET_KEY = 'TPmi4aLWRbyVq8zu9v82dWYW1'
```

在Flask程序文件中

```python
app = Flask(__name__)

app.config.from_pyfile('setting.py')

@app.route("/")
def index():
    print(app.config['SECRET_KEY'])
    return "hello world"
```

应用场景：

在项目中使用固定的配置文件

- 从环境变量中加载

> 环境变量（environment variables）一般是指在操作系统中用来指定操作系统运行环境的一些参数，如：临时文件夹位置和系统文件夹位置等。 环境变量是在操作系统中一个具有特定名字的对象，它包含了一个或者多个应用程序所将使用到的信息。

通俗的理解，环境变量就是我们设置在操作系统中，由操作系统代为保存的变量值

在Linux系统中设置和读取环境变量的方式如下：

```text
export 变量名=变量值  # 设置
echo $变量名  # 读取

# 例如
export ITCAST=python
echo $ITCAST
```

**Flask使用环境变量加载配置的本质是通过环境变量值找到配置文件**，再读取配置文件的信息，其使用方式为

`app.config.from_envvar('环境变量名')`

环境变量的值为配置文件的绝对路径

先在终端中执行如下命令

```shell
export PROJECT_SETTING='~/setting.py'
```

再运行如下代码

```python
app = Flask(__name__)

app.config.from_envvar('PROJECT_SETTING', silent=True)

@app.route("/")
def index():
    print(app.config['SECRET_KEY'])
    return "hello world"
```

关于`silent`的说明：

表示系统环境变量中没有设置相应值时是否抛出异常

- False 表示不安静的处理，没有值时报错通知，默认为False
- True 表示安静的处理，即时没有值也让Flask正常的运行下去

应用场景：

配置文件的地址不固定；

在代码中不想暴露真实的配置文件地址，只在运行代码的服务器上才有真实配置文件的信息。

## 三. 项目中的常用方式

使用工厂模式创建Flask app，并结合使用配置对象与环境变量加载配置

- 使用配置对象加载默认配置
- 使用环境变量加载不想出现在代码中的敏感配置信息

```python
def create_flask_app(config):
    """
    创建Flask应用
    :param config: 配置对象
    :return: Flask应用
    """
    app = Flask(__name__)
    app.config.from_object(config)

    # 从环境变量指向的配置文件中读取的配置信息会覆盖掉从配置对象中加载的同名参数
    app.config.from_envvar("PROJECT_SETTING", silent=True)
    return app

class DefaultConfig(object):
    """默认配置"""
    SECRET_KEY = 'itcast1'

class DevelopmentConfig(DefaultConfig):
    DEBUG=True

# app = create_flask_app(DefaultConfig)
app = create_flask_app(DevelopmentConfig)

@app.route("/")
def index():
    print(app.config['SECRET_KEY'])
    return "hello world"
```

## 四. app.run 参数

可以指定运行的主机IP地址，端口，是否开启调试模式

```python
app.run(host="0.0.0.0", port=5000, debug = True)
```

关于DEBUG调试模式

1. 程序代码修改后可以自动重启服务器
2. 在服务器出现相关错误的时候可以直接将错误信息返回到前端进行展示

# 开发服务器启动方式

在1.0版本之后，Flask调整了开发服务器的启动方式，由代码编写`app.run()`语句调整为命令`flask run`启动。

```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def index():
    return 'Hello World'

# 程序中不用再写app.run()
```

## 一. 终端启动

```shell
$ export FLASK_APP=helloworld
$ flask run
 * Running on http://127.0.0.1:5000/
```

### 说明

- 环境变量 FLASK_APP 指明flask的启动实例

- `flask run -h 0.0.0.0 -p 8000` 绑定地址 端口

- `flask run --help`获取帮助

- 生产模式与开发模式的控制

  通过`FLASK_ENV`环境变量指明

  - `export FLASK_ENV=production` 运行在生产模式，未指明则默认为此方式
  - `export FLASK_ENV=development`运行在开发模式

### 扩展

```shell
$ export FLASK_APP=helloworld
$ python -m flask run
 * Running on http://127.0.0.1:5000/
```

# 路由

```python
@app.route("/itcast")
def view_func():
    return "hello world"
```

## 一. 查询路由信息

- 命令行方式

  ```shell
  flask routes
  ```

  ```shell
  Endpoint  Methods  Rule
  --------  -------  -----------------------
  index     GET      /
  static    GET      /static/
  ```

- 在程序中获取

  在应用中的url_map属性中保存着整个Flask应用的路由映射信息，可以通过读取这个属性获取路由信息

  ```python
  print(app.url_map)
  ```

  如果想在程序中遍历路由信息，可以采用如下方式

  ```python
  for rule in app.url_map.iter_rules():
      print('name={} path={}'.format(rule.endpoint, rule.rule))
  ```

### 1. 需求

通过访问`/`地址，以json的方式返回应用内的所有路由信息

### 2. 实现

```python
@app.route('/')
def route_map():
    """
    主视图，返回所有视图网址
    """
    rules_iterator = app.url_map.iter_rules()
    return json.dumps({rule.endpoint: rule.rule for rule in rules_iterator})
```

## 二. 指定请求方式

在 Flask 中，定义路由其默认的请求方式为：

- GET
- OPTIONS(自带)
- HEAD(自带)

利用`methods`参数可以自己指定一个接口的请求方式

```python
@app.route("/itcast1", methods=["POST"])
def view_func_1():
    return "hello world 1"

@app.route("/itcast2", methods=["GET", "POST"])
def view_func_2():
    return "hello world 2"
```

# 蓝图

## 一. 需求

在一个Flask 应用项目中，如果业务视图过多，可否将以某种方式划分出的业务单元单独维护，将每个单元用到的视图、静态文件、模板文件等独立分开？

例如从业务角度上，可将整个应用划分为用户模块单元、商品模块单元、订单模块单元，如何分别开发这些不同单元，并最终整合到一个项目应用中？

在Django中这种需求是如何实现的？

## 二. 蓝图

在Flask中，使用蓝图Blueprint来分模块组织管理。

蓝图实际可以理解为是一个存储一组视图方法的容器对象，其具有如下特点：

- 一个应用可以具有多个Blueprint
- 可以将一个Blueprint注册到任何一个未使用的URL下比如 “/user”、“/goods”
- Blueprint可以单独具有自己的模板、静态文件或者其它的通用操作方法，它并不是必须要实现应用的视图和函数的
- 在一个应用初始化时，就应该要注册需要使用的Blueprint

但是一个Blueprint并不是一个完整的应用，它不能独立于应用运行，而必须要注册到某一个应用中。

## 三. 使用方式

使用蓝图可以分为三个步骤

1. 创建一个蓝图对象

   ```python
    user_bp=Blueprint('user',__name__)
   ```

2. 在这个蓝图对象上进行操作,注册路由,指定静态文件夹,注册模版过滤器

   ```python
    @user_bp.route('/')
    def user_profile():
        return 'user_profile'
   ```

3. 在应用对象上注册这个蓝图对象

   ```python
    app.register_blueprint(user_bp)
   ```

### 1. 单文件蓝图

可以将创建蓝图对象与定义视图放到一个文件中 。

### 2. 目录（包）蓝图

对于一个打算包含多个文件的蓝图，通常将创建蓝图对象放到Python包的`__init__.py`文件中

```text
--------- project # 工程目录
  |------ main.py # 启动文件
  |------ user  #用户蓝图
  |  |--- __init__.py  # 此处创建蓝图对象
  |  |--- passport.py  
  |  |--- profile.py
  |  |--- ...
  |
  |------ goods # 商品蓝图
  |  |--- __init__.py
  |  |--- ...
  |...
```

## 四. 扩展用法

### 1. 指定蓝图的url前缀

在应用中注册蓝图时使用`url_prefix`参数指定

```python
app.register_blueprint(user_bp, url_prefix='/user')
app.register_blueprint(goods_bp, url_prefix='/goods')
```

### 2. 蓝图内部静态文件

和应用对象不同，蓝图对象创建时不会默认注册静态目录的路由。需要我们在 创建时指定 static_folder 参数。

下面的示例将蓝图所在目录下的static_admin目录设置为静态目录

```python
admin = Blueprint("admin",__name__,static_folder='static_admin')
app.register_blueprint(admin,url_prefix='/admin')
```

现在就可以使用`/admin/static_admin/<filename>`访问`static_admin`目录下的静态文件了。

也可通过`static_url_path`改变访问路径

```python
admin = Blueprint("admin",__name__,static_folder='static_admin',static_url_path='/lib')
app.register_blueprint(admin,url_prefix='/admin')
```

### 3. 蓝图内部模板目录

蓝图对象默认的模板目录为系统的模版目录，可以在创建蓝图对象时使用 template_folder 关键字参数设置模板目录

```python
admin = Blueprint('admin',__name__,template_folder='my_templates')
```

# 处理请求

## 需求

在视图编写中需要读取客户端请求携带的数据时，如何才能正确的取出数据呢？

请求携带的数据可能出现在HTTP报文中的不同位置，需要使用不同的方法来获取参数。

## 一. URL路径参数（动态路由）

例如，有一个请求访问的接口地址为`/users/123`，其中123实际上为具体的请求参数，表明请求123号用户的信息。此时如何从url中提取出123的数据？

Flask不同于Django直接在定义路由时编写正则表达式的方式，而是采用转换器语法：

```python
@app.route('/users/<user_id>')
def user_info(user_id):
    print(type(user_id))
    return 'hello user {}'.format(user_id)
```

此处的`<>`即是一个转换器，默认为字符串类型，即将该位置的数据以字符串格式进行匹配、并以字符串为数据类型类型、 `user_id`为参数名传入视图。

### 1. Flask也提供其他类型的转换器

```python
DEFAULT_CONVERTERS = {
    'default':          UnicodeConverter,
    'string':           UnicodeConverter,
    'any':              AnyConverter,
    'path':             PathConverter,
    'int':              IntegerConverter,
    'float':            FloatConverter,
    'uuid':             UUIDConverter,
}
```

将上面的例子以整型匹配数据，可以如下使用：

```python
@app.route('/users/<int:user_id>')
def user_info(user_id):
    print(type(user_id))
    return 'hello user {}'.format(user_id)


@app.route('/users/<int(min=1):user_id>')
def user_info(user_id):
    print(type(user_id))
    return 'hello user {}'.format(user_id)
```

### 2. 自定义转换器

如果遇到需要匹配提取`/sms_codes/18512345678`中的手机号数据，Flask内置的转换器就无法满足需求，此时需要自定义转换器。

#### (1). 定义方法

自定义转换器主要做3步

1. 创建转换器类，保存匹配时的正则表达式

   ```python
   from werkzeug.routing import BaseConverter
   
   class MobileConverter(BaseConverter):
       """
       手机号格式
       """
       regex = r'1[3-9]\d{9}'
   ```

   - 注意`regex`名字固定

2. 将自定义的转换器告知Flask应用

   ```python
   app = Flask(__name__)
   
   # 将自定义转换器添加到转换器字典中，并指定转换器使用时名字为: mobile
   app.url_map.converters['mobile'] = MobileConverter
   ```

3. 在使用转换器的地方定义使用

   ```python
   @app.route('/sms_codes/<mobile:mob_num>')
   def send_sms_code(mob_num):
       return 'send sms code to {}'.format(mob_num)
   ```

## 二. 其他参数

如果想要获取其他地方传递的参数，可以通过Flask提供的**request**对象来读取。

不同位置的参数都存放在request的不同属性中

| 属性    | 说明                           | 类型           |
| :------ | :----------------------------- | :------------- |
| data    | 记录请求的数据，并转换为字符串 | *              |
| form    | 记录请求中的表单数据           | MultiDict      |
| args    | 记录请求中的查询参数           | MultiDict      |
| cookies | 记录请求中的cookie信息         | Dict           |
| headers | 记录请求中的报文头             | EnvironHeaders |
| method  | 记录请求使用的HTTP方法         | GET/POST       |
| url     | 记录请求的URL地址              | string         |
| files   | 记录请求上传的文件             | *              |

例如 想要获取请求`/articles?channel_id=1`中`channel_id`的参数，可以按如下方式使用：

```python
from flask import request

@app.route('/articles')
def get_articles():
    channel_id = request.args.get('channel_id')
    return 'you wanna get articles of channel {}'.format(channel_id)
```

### 1. 上传图片

客户端上传图片到服务器，并保存到服务器中

```python
from flask import request

@app.route('/upload', methods=['POST'])
def upload_file():
    f = request.files['pic']
    # with open('./demo.png', 'wb') as new_file:
    #     new_file.write(f.read())
    f.save('./demo.png')
    return 'ok'
```

# 处理相应

## 需求

如何在不同的场景里返回不同的响应信息？

## 一. 返回模板

使用`render_template`方法渲染模板并返回

例如，新建一个模板index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
我的模板html内容
<br/>{{ my_str }}
<br/>{{ my_int }}
</body>
</html>
```

后端视图

```python
from flask import render_template

@app.route('/')
def index():
    mstr = 'Hello 黑马程序员'
    mint = 10
    return render_template('index.html', my_str=mstr, my_int=mint)
```

## 二. 重定向

```python
from flask import redirect

@app.route('/demo2')
def demo2():
    return redirect('http://www.itheima.com')
```

## 三. 返回JSON

```python
from flask import jsonify

@app.route('/demo3')
def demo3():
    json_dict = {
        "user_id": 10,
        "user_name": "laowang"
    }
    return jsonify(json_dict)
```

## 四. 自定义状态码和响应头

### 1. 元组方式

可以返回一个元组，这样的元组必须是 **(response, status, headers)** 的形式，且至少包含一个元素。 status 值会覆盖状态代码， headers 可以是一个列表或字典，作为额外的消息标头值。

```python
@app.route('/demo4')
def demo4():
    # return '状态码为 666', 666
    # return '状态码为 666', 666, [('Itcast', 'Python')]
    return '状态码为 666', 666, {'Itcast': 'Python'}
```

### 2. make_response方式

```python
@app.route('/demo5')
def demo5():
    resp = make_response('make response测试')
        resp.headers[“Itcast”] = “Python”
        resp.status = “404 not found”
    return resp
```

# Cookie与Session

## 一. Cookie

### 1. 设置

```python
from flask import Flask, make_response

app = Flask(__name__)

@app.route('/cookie')
def set_cookie():
    resp = make_response('set cookie ok')
    resp.set_cookie('username', 'itcast')
    return resp
```

设置有效期

```python
@app.route('/cookie')
def set_cookie():
    response = make_response('hello world')
    response.set_cookie('username', 'itheima', max_age=3600)
    return response
```

### 2. 读取

```python
from flask import request

@app.route('/get_cookie')
def get_cookie():
    resp = request.cookies.get('username')
    return resp
```

### 3. 删除

```python
from flask import request

@app.route('/delete_cookie')
def delete_cookie():
    response = make_response('hello world')
    response.delete_cookie('username')
    return response
```

## 二. Session

需要先设置SECRET_KEY

```python
class DefaultConfig(object):
    SECRET_KEY = 'fih9fh9eh9gh2'

app.config.from_object(DefaultConfig)

或者直接设置
app.secret_key='xihwidfw9efw'
```

### 1. 设置

```python
from flask import session

@app.route('/set_session')
def set_session():
    session['username'] = 'itcast'
    return 'set session ok'
```

### 2. 读取

```python
@app.route('/get_session')
def get_session():
    username = session.get('username')
    return 'get session username {}'.format(username)
```