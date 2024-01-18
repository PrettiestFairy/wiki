[toc]

# selenium介绍

## 一. selenium运行效果展示

> Selenium是一个Web的自动化测试工具,最初是为网站自动化测试而开发的,Selenium 可以直接调用浏览器,它支持所有主流的浏览器(包括PhantomJS这些无界面的浏览器),可以接收指令,让浏览器自动加载页面,获取需要的数据,甚至页面截屏等。我们可以使用selenium很容易完成之前编写的爬虫,接下来我们就来看一下selenium的运行效果

### 1. chrome浏览器的运行效果

> 在下载好chromedriver以及安装好selenium模块后,执行下列代码并观察运行的过程

```python
# coding: utf8
""" 
@File: part_001.py
@Author: Austin(From Chengdu.China)
@HomePage: https://github.com/AustinFairyland
@CreatedTime: 2022/12/9 3:29
"""

from selenium import webdriver

# 如果driver没有添加到了环境变量，则需要将driver的绝对路径赋值给executable_path参数
webdriver.FirefoxOptions().binary_location = r'C:\Program Files\Mozilla Firefox\firefox.exe'
driver = webdriver.Firefox(executable_path=r'./driver/firefox_107_0_1_x86_64/win/geckodriver.exe')
# 如果driver添加了环境变量则不需要设置executable_path
# driver = webdriver.Firefox()

# 向一个url发起请求
driver.get('https://baidu.com')

# 打印页面的标题
print(driver.title)

# 退出模拟浏览器, 不退出会有残留进程！
driver.quit()

```

### 2. phantomjs无界面浏览器的运行效果

> PhantomJS 是一个基于Webkit的“无界面”(headless)浏览器，它会把网站加载到内存并执行页面上的 JavaScript。下载地址：http://phantomjs.org/download.html

```python
# coding: utf8
""" 
@File: part_002.py
@Author: Austin(From Chengdu.China)
@HomePage: https://github.com/AustinFairyland
@CreatedTime: 2022/12/9 3:32
"""

from selenium import webdriver
from selenium.webdriver.firefox.options import Options

options = Options()
options.add_argument(argument='-headless')
options.add_argument(argument='--disable-gpu')
driver = webdriver.Firefox(executable_path=r'./driver/firefox_107_0_1_x86_64/win/geckodriver.exe', options=options)
driver.get(url=r'https://baidu.com')
print(driver.page_source)
driver.quit()

```

### 3. 观察运行效果

python代码能够自动的调用谷歌浏览或phantomjs无界面浏览器，控制其自动访问网站

### 4. 无头浏览器与有头浏览器的使用场景

1. 通常在开发过程中我们需要查看运行过程中的各种情况所以通常使用有头浏览器
2. 在项目完成进行部署的时候，通常平台采用的系统都是服务器版的操作系统，服务器版的操作系统必须使用无头浏览器才能正常运行

## 二. selenium的作用和工作原理

![selenium的工作原理](https://picsur.cloud.fairies.ltd/i/2f971087-c6f1-46d2-9b0c-8da98d3418e0.webp)

- webdriver本质是一个web-server，对外提供webapi，其中封装了浏览器的各种功能
- 不同的浏览器使用各自不同的webdriver

## 三. selenium的安装以及简单使用

### 1. 安装selenium模块

```shell
pip install selenium==4.7.2
```

### 2. 下载版本符合的webdriver

> 以chrome谷歌浏览器为例

#### (1). 查看谷歌浏览器的版本

![image-20221209042305264](https://picsur.cloud.fairies.ltd/i/8b7a2e7c-33cc-43bf-9006-1293bb446c5f.webp)

#### (2). [chromedriver下载](https://chromedriver.chromium.org/)

官方: [传送门](https://chromedriver.chromium.org/)

国内镜像: [传送门](https://registry.npmmirror.com/binary.html?path=chromedriver/)

#### (3). 查看chrome和chromedriver匹配的版本

`notes.txt`为说明文件

![image-20221209044623553](https://picsur.cloud.fairies.ltd/i/55447ba8-be97-4ce1-bc83-62e29cee7d8f.webp)

#### (4). 根据操作系统下载正确版本的chromedriver

![image-20221209044823921](https://picsur.cloud.fairies.ltd/i/afb70b1f-b7ea-4460-b4a2-6defeff9305c.webp)

#### (5). 解压压缩包

python可以调用的谷歌浏览器的webdriver可执行文件

- windows为`chromedriver.exe`
- linux和macos为`chromedriver`

#### (6). chromedriver环境的配置

- windows环境下需要将`chromedriver.exe`所在的目录设置为path环境变量中的路径
- linux/mac环境下，将`chromedriver`所在的目录设置到系统的PATH环境值中

## 四. selenium的简单使用

> 模拟百度搜索

示例代码:

```python
# coding: utf8
""" 
@File: part_003.py
@Author: Austin(From Chengdu.China)
@HomePage: https://github.com/AustinFairyland
@CreatedTime: 2022/12/9 4:51
"""

from selenium import webdriver
from selenium.webdriver.common.by import By
import time

webdriver.FirefoxOptions().binary_location = r'C:\Program Files\Mozilla Firefox\firefox.exe' 
driver = webdriver.Firefox(executable_path=r'./driver/firefox_107_0_1_x86_64/win/geckodriver.exe')
driver.get('https://baidu.com')
time.sleep(2)
driver.find_element(by=By.ID, value='kw').send_keys('python')
time.sleep(3)
driver.find_element(by=By.ID, value='su').click()
time.sleep(3)
print(driver.title)
driver.quit()

```

- `webdriver.FirefoxOptions().binary_location`是指定Firefox浏览器的执行文件路径
- `webdriver.Firefox(executable_path=r'./driver/firefox_107_0_1_x86_64/win/geckodriver.exe')`中`executable_path`参数是来指定driver路径
- `driver.find_element(by=By.ID, value='kw').send_keys('python')`中`find_element`方法是用来查找唯一的html元素, `By.ID`表示查询html元素属性ID为`kw`的html元素, `send_keys`表示输入字符
- `driver.find_element(by=By.ID, value='su').click()`中`click`方法用来出发js的click时事件

# selenium提取数据

## 一. driver对象的常用属性和方法

> 在使用selenium过程中，实例化driver对象后，driver对象有一些常用的属性和方法

1. `driver.page_source` 当前标签页浏览器渲染之后的网页源代码
2. `driver.current_url` 当前标签页的url
3. `driver.close()` 关闭当前标签页，如果只有一个标签页则关闭整个浏览器
4. `driver.quit()` 关闭浏览器
5. `driver.forward()` 页面前进
6. `driver.back()` 页面后退
7. `driver.save_screenshot(img_name)` 页面截图

示例代码:

```python
# coding: utf8
""" 
@File: part_004.py
@Author: Austin(From Chengdu.China)
@HomePage: https://github.com/AustinFairyland
@CreatedTime: 2022/12/9 6:33
"""

from selenium import webdriver
from selenium.webdriver.common.by import By
import time

url = 'https://baidu.com/'
driver_path = r'./driver/firefox_107_0_1_x86_64/win/geckodriver.exe'
webdriver.FirefoxOptions().binary_location = r'C:\Program Files\Mozilla Firefox\firefox.exe'
driver = webdriver.Firefox(executable_path=driver_path)
driver.get(url=url)
print(driver.page_source)
driver.find_element(by=By.ID, value='kw').send_keys('python')
driver.find_element(by=By.ID, value='su').click()
time.sleep(3)
driver.save_screenshot(filename='./python_百度搜索.png')
print(driver.current_url)
driver.get(url='https://douban.com/')
driver.save_screenshot(filename='./豆瓣主页.png')
driver.back()
driver.forward()
time.sleep(3)
driver.quit()

```

## 二. driver对象定位标签元素获取标签对象的方法

> 在selenium中可以通过多种方式来定位标签，返回标签元素对象

1. 导入`By`模块

   ```python
   from selenium.webdriver.common.by import By
   ```

2. 调用`find_element`/`find_elements`方法

   - 参数`by`: 
     1. `By.ID`: 返回一个元素
     2. `By.XPATH`: 返回一个包含元素的列表
     3. `By.LINK_TEXT`: 根据连接文本获取元素列表
     4. `By.PARTIAL_LINK_TEXT`: 根据链接包含的文本获取元素列表
     5. `By.NAME`: 根据标签的name属性值返回包含标签对象元素的列表
     6. `By.TAG_NAME`: 根据标签名获取元素列表
     7. `By.CLASS_NAME`: 根据类名获取元素列表
     8. `By.CSS_SELECTOR`: 根据css选择器来获取元素列表
   - 参数`value`: 对应值

`find_element`和`find_elements`的区别：

1. 如果匹配有多个结果, `find_element`返回匹配到的第一个标签对象, `find_elements`返回列表
2. 如果匹配没有结果, `find_element`抛出异常, `find_elements`返回空列表

`By.LINK_TEXT`和`By.PARTIAL_LINK_TEXT`的区别: `LINK_TEXT`返回全部文本, `PARTIAL_LINK_TEXT`返回包含某个文本

## 三. 标签对象提取文本内容和属性值

> find_element仅仅能够获取元素，不能够直接获取其中的数据，如果需要获取数据需要使用以下方法

对定位到的标签对象进行点击操作: `.click()`

对定位到的标签对象输入数据: `.send_keys(data)`

通过定位获取的标签对象的`text`属性，获取文本内容: `.text`

通过定位获取的标签对象的`get_attribute`函数，传入属性名，来获取属性的值: `.get_attribute("属性名")`

示例代码:

```python
# coding: utf8
""" 
@File: part_005.py
@Author: Austin(From Chengdu.China)
@HomePage: https://github.com/AustinFairyland
@CreatedTime: 2022/12/9 7:08
"""

from selenium import webdriver
from selenium.webdriver.common.by import By
import time

url = 'https://movie.douban.com/explore'
driver_path = r'./driver/firefox_107_0_1_x86_64/win/geckodriver.exe'
webdriver.FirefoxOptions().binary_location = r'C:\Program Files\Mozilla Firefox\firefox.exe'
driver = webdriver.Firefox(executable_path=driver_path)
driver.get(url=url)
time.sleep(3)
ul = driver.find_elements(by=By.CLASS_NAME, value='explore-list')
for data in ul:
    for i in data.find_elements(by=By.TAG_NAME, value='a'):
        print(i.get_attribute('href'))
    for i in data.find_elements(by=By.CLASS_NAME, value='drc-subject-info-title-text'):
        print(i.text)
driver.quit()
```

# selenium的其它使用方法

## 一. selenium标签页的切换

> 当selenium控制浏览器打开多个标签页时，控制浏览器在不同的标签页中进行切换
>
> 需要以下两步：
>
> 1. 获取所有标签页的窗口句柄
>
>    窗口句柄: 向标签页对象的标识
> 2. 利用窗口句柄字切换到句柄指向的标签页

参考代码示例:

```python
# coding: utf8
""" 
@File: part_006.py
@Author: Austin(From Chengdu.China)
@HomePage: https://github.com/AustinFairyland
@CreatedTime: 2022/12/9 9:24
"""

from selenium import webdriver
import time

webdriver.FirefoxOptions().binary_location = r'C:\Program Files\Mozilla Firefox\firefox.exe'
executable_path = r'./driver/firefox_107_0_1_x86_64/win/geckodriver.exe'
driver = webdriver.Firefox(executable_path=executable_path)
driver.get(url='https://baidu.com')
js_opentable = 'window.open("https://taobao.com")'
driver.execute_script(js_opentable)
time.sleep(2)
# 浏览器标签句柄
browser_windows = driver.window_handles
driver.switch_to.window(browser_windows[0])
time.sleep(5)
driver.switch_to.window(browser_windows[1])
time.sleep(5)
driver.quit()
```

## 二. switch_to切换frame标签

> iframe是html中常用的一种技术，即一个页面中嵌套了另一个网页，selenium默认是访问不了frame中的内容的，对应的解决思路是`driver.switch_to.frame(frame_element)`。接下来我们通过qq邮箱模拟登陆来学习这个知识点

参考代码示例:

```python
# coding: utf8
""" 
@File: part_007.py
@Author: Austin(From Chengdu.China)
@HomePage: https://github.com/AustinFairyland
@CreatedTime: 2022/12/9 10:30
"""
import time
from selenium import webdriver
from selenium.webdriver.common.by import By


driver = webdriver.Chrome()

url = 'https://mail.qq.com/cgi-bin/loginpage'
driver.get(url)
time.sleep(2)

login_frame = driver.find_element(by=By.ID, value='login_frame') # 根据id定位 frame元素
driver.switch_to.frame(login_frame) # 转向到该frame中
driver.find_element(by=By.XPATH, value='//*[@id="u"]').send_keys('1596930226@qq.com')
time.sleep(2)
driver.find_element(by=By.XPATH, value='//*[@id="p"]').send_keys('hahamimashicuode')
time.sleep(2)
driver.find_element(by=By.XPATH, value='//*[@id="login_button"]').click()
time.sleep(2)
# 操作frame外边的元素需要切换出去
windows = driver.window_handles
driver.switch_to.window(windows[0])
content = driver.find_element(by=By.CLASS_NAME, value='login_pictures_title').text
print(content)
driver.quit()
```

## 三. selenium对cookie的处理

> selenium能够帮助我们处理页面中的cookie，比如获取、删除等

### 1. 获取cookie

> `driver.get_cookies()`返回列表，其中包含的是完整的cookie信息！不光有name、value，还有domain等cookie其他维度的信息。所以如果想要把获取的cookie信息和requests模块配合使用的话，需要转换为name、value作为键值对的cookie字典

```python
# coding: utf8
""" 
@File: part_008.py
@Author: Austin(From Chengdu.China)
@HomePage: https://github.com/AustinFairyland
@CreatedTime: 2022/12/9 20:20
"""

from selenium import webdriver

webdriver.FirefoxOptions().binary_location = r'C:\Program Files\Mozilla Firefox\firefox.exe'
executable_path = r'./driver/firefox_107_0_1_x86_64/win/geckodriver.exe'
driver = webdriver.Firefox(executable_path=executable_path)
driver.get(url='https://baidu.com')
baidu_cookies = {data.get('name'): data.get('value') for data in driver.get_cookies()}
print(baidu_cookies)
driver.quit()
```

### 2. 删除cookie

```python
#删除一条cookie
driver.delete_cookie("CookieName")

# 删除所有的cookie
driver.delete_all_cookies()
```

## 四. selenium控制浏览器执行js代码

> selenium可以让浏览器执行我们规定的js代码，运行下列代码查看运行效果

```python
# coding: utf8
""" 
@File: part_009.py
@Author: Austin(From Chengdu.China)
@HomePage: https://github.com/AustinFairyland
@CreatedTime: 2022/12/11 9:15
"""

from selenium import webdriver
import time

webdriver.FirefoxOptions().binary_location = r'C:\Program Files\Mozilla Firefox\firefox.exe'
executable_path = r'./driver/firefox_107_0_1_x86_64/win/geckodriver.exe'
driver = webdriver.Firefox(executable_path=executable_path)
driver.get(url='https://qq.com')
js = '''window.scrollTo(0,document.body.scrollHeight)'''
driver.execute_script(script=js)
time.sleep(5)
driver.quit()
```

- 执行js的方法：`driver.execute_script(js)`

## 五. 页面等待

> 页面在加载的过程中需要花费时间等待网站服务器的响应，在这个过程中标签元素有可能还没有加载出来，是不可见的

1. 页面等待分类
2. 强制等待介绍
3. 显式等待介绍
4. 隐式等待介绍
5. 手动实现页面等待

### 1. 页面等待的分类

> 了解以下selenium页面等待的分类

1. 强制等待
2. 隐式等待
3. 显式等待

### 2. 强制等待（了解）

- 其实就是`time.sleep()`
- 缺点时不智能，设置的时间太短，元素还没有加载出来；设置的时间太长，则会浪费时间

### 3. 隐式等待

隐式等待针对的是元素定位，隐式等待设置了一个时间，在一段时间内判断元素是否定位成功，如果完成了，就进行下一步

在设置的时间内没有定位成功，则会报超时加载

参考代码示例:

```python
from selenium import webdriver

driver = webdriver.Chrome()  

driver.implicitly_wait(10) # 隐式等待，最长等20秒  

driver.get('https://www.baidu.com')

driver.find_element_by_xpath()
```

### 4. 显式等待（了解）

每经过多少秒就查看一次等待条件是否达成，如果达成就停止等待，继续执行后续代码

如果没有达成就继续等待直到超过规定的时间后，报超时异常

参考代码示例:

```python
from selenium import webdriver  
from selenium.webdriver.support.wait import WebDriverWait  
from selenium.webdriver.support import expected_conditions as EC  
from selenium.webdriver.common.by import By 

driver = webdriver.Chrome()

driver.get('https://www.baidu.com')

# 显式等待
WebDriverWait(driver, 20, 0.5).until(
    EC.presence_of_element_located((By.LINK_TEXT, '好123')))  
# 参数20表示最长等待20秒
# 参数0.5表示0.5秒检查一次规定的标签是否存在
# EC.presence_of_element_located((By.LINK_TEXT, '好123')) 表示通过链接文本内容定位标签
# 每0.5秒一次检查，通过链接文本内容定位标签是否存在，如果存在就向下继续执行；如果不存在，直到20秒上限就抛出异常

print(driver.find_element_by_link_text('好123').get_attribute('href'))
driver.quit()
```

### 5. 手动实现页面等待

> 在了解了隐式等待和显式等待以及强制等待后，我们发现并没有一种通用的方法来解决页面等待的问题，比如“页面需要滑动才能触发ajax异步加载”的场景，那么接下来我们就以淘宝网首页为例，手动实现页面等待

原理:

- 利用强制等待和显式等待的思路来手动实现
- 不停的判断或有次数限制的判断某一个标签对象是否加载完毕（是否存在）

参考代码示例:

```python
import time
from selenium import webdriver
driver = webdriver.Chrome('/home/worker/Desktop/driver/chromedriver')

driver.get('https://www.taobao.com/')
time.sleep(1)

# i = 0
# while True:
for i in range(10):
    i += 1
    try:
        time.sleep(3)
        element = driver.find_element_by_xpath('//div[@class="shop-inner"]/h3[1]/a')
        print(element.get_attribute('href'))
        break
    except:
        js = 'window.scrollTo(0, {})'.format(i*500) # js语句
        driver.execute_script(js) # 执行js的方法
driver.quit()
```

## 六. selenium开启无界面模式

> 绝大多数服务器是没有界面的，selenium控制谷歌浏览器也是存在无界面模式的

开启无界面模式的方法:

1. 实例化配置对象
   - `options = webdriver.ChromeOptions()`
2. 配置对象添加开启无界面模式的命令
   - `options.add_argument("--headless")`
3. 配置对象添加禁用gpu的命令
   - `options.add_argument("--disable-gpu")`
4. 实例化带有配置对象的driver对象
   - `driver = webdriver.Chrome(options=options)`

参考代码示例:

```python
from selenium import webdriver

options = webdriver.ChromeOptions() # 创建一个配置对象
options.add_argument("--headless") # 开启无界面模式
options.add_argument("--disable-gpu") # 禁用gpu

# options.set_headles() # 无界面模式的另外一种开启方式
driver = webdriver.Chrome(chrome_options=options) # 实例化带有配置的driver对象

driver.get('http://www.itcast.cn')
print(driver.title)
driver.quit()
```

## 七. selenium使用代理ip

> selenium控制浏览器也是可以使用代理ip的！

使用代理ip的方法:

1. 实例化配置对象
   - `options = webdriver.ChromeOptions()`
2. 配置对象添加使用代理ip的命令
   - `options.add_argument('--proxy-server=http://202.20.16.82:9527')`
3. 实例化带有配置对象的driver对象
   - `driver = webdriver.Chrome('./chromedriver', chrome_options=options)`

参考代码示例:

```python
from selenium import webdriver

options = webdriver.ChromeOptions() # 创建一个配置对象
options.add_argument('--proxy-server=http://202.20.16.82:9527') # 使用代理ip

driver = webdriver.Chrome(chrome_options=options) # 实例化带有配置的driver对象

driver.get('http://www.itcast.cn')
print(driver.title)
driver.quit()
```

## 八. selenium替换user-agent

> selenium控制谷歌浏览器时，User-Agent默认是谷歌浏览器的

替换user-agent的方法:

1. 实例化配置对象
   - `options = webdriver.ChromeOptions()`
2. 配置对象添加替换UA的命令
   - `options.add_argument('--user-agent=Mozilla/5.0 HAHA')`
3. 实例化带有配置对象的driver对象
   - `driver = webdriver.Chrome('./chromedriver', chrome_options=options)`

参考代码示例: 

```python
from selenium import webdriver

options = webdriver.ChromeOptions() # 创建一个配置对象
options.add_argument('--user-agent=Mozilla/5.0 HAHA') # 替换User-Agent

driver = webdriver.Chrome('./chromedriver', chrome_options=options)

driver.get('http://www.itcast.cn')
print(driver.title)
driver.quit()
```