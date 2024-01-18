[toc]

# 一. **Python 爬虫**

**代码部分访问[GitHub](https://github.com/AliceEngineerPro/public_models_py/tree/PythonSpider)**

## 1. **爬虫概述**

> 模拟浏览器，发送请求，获取响应

网络爬虫（又被称为网页蜘蛛，网络机器人）就是模拟客户端(主要指浏览器)发送网络请求，接收请求响应，一种按照一定的规则，自动地抓取互联网信息的程序。

-   原则上,只要是客户端(浏览器)能做的事情，爬虫都能够做
-   爬虫也只能获取客户端(浏览器)所展示出来的数据

## 2. **爬虫的作用**

> 爬虫在互联网世界中有很多的作用

1.  数据采集
    1.  [微博评论](https://weibo.com/)
    2.  [抓取招聘网站的招聘信息](https://www.lagou.com/jobs/list_Python%20%E7%88%AC%E8%99%AB?labelWords=&fromSearch=true&suginput=)
    3.  [新浪滚动新闻](https://news.sina.com.cn/roll/#pageid=153&lid=2509&k=&num=50&page=1)
    4.  [百度新闻网站](http://news.baidu.com/)
2.  [软件测试](https://www.lagou.com/jobs/list_%E8%87%AA%E5%8A%A8%E5%8C%96%E6%B5%8B%E8%AF%95?city=%E5%8C%97%E4%BA%AC&cl=false&fromSearch=true&labelWords=&suginput=)
    1.  [爬虫之自动化测试](https://blog.csdn.net/ZTZY520/article/details/53940127)
    2.  [虫师](https://www.cnblogs.com/fnng/)
3.  [12306抢票](http://www.12306.cn/mormhweb/)
4.  网站上的投票
    1.  [投票网](https://www.toutoupiao.com/)
5.  网络安全
    1.  [短信轰炸](https://www.sohu.com/a/232477043_99936149)
        1.  [注册页面1](https://hwid1.vmall.com/CAS/portal/userRegister/regbyphone.html?reqClientType=1&loginChannel=1000002&countryCode=cn&loginUrl=https%3A%2F%2Fhwid1.vmall.com%2FCAS%2Fportal%2FcloudLogin.html&service=https%3A%2F%2Fwww.hicloud.com%3A443%2Fothers%2Flogin.action&lang=zh-cn&themeName=red)
        2.  [注册页面2](https://bcy.net/register)
        3.  [注册页面3](https://www.chinaacc.com/login/register.shtml)
    2.  [web漏洞扫描](https://paper.seebug.org/537/)

## 3. **爬虫的分类**

### 3.1. **根据被爬取网站的数量不同，可以分为**

1.  通用爬虫，如 搜索引擎
2.  聚焦爬虫，如12306抢票，或专门抓取某一个（某一类）网站数据

### 3.2. **根据是否以获取数据为目的，可以分为**

1.  功能性爬虫，给你喜欢的明星投票、点赞
2.  数据增量爬虫，比如招聘信息

### 3.3. **根据url地址和对应的页面内容是否改变，数据增量爬虫可以分为**

1.  基于url地址变化、内容也随之变化的数据增量爬虫
2.  url地址不变、内容变化的数据增量爬虫

![https://picsur.cloud.fairies.ltd/i/9e7d48de-99ee-4f5f-8600-45cdc3d83ed5.webp](https://picsur.cloud.fairies.ltd/i/9e7d48de-99ee-4f5f-8600-45cdc3d83ed5.webp)

## 4. **爬虫的流程**

1.  获取一个url
2.  向url发送请求，并获取响应（需要http协议）
3.  如果从响应中提取url，则继续发送请求获取响应
4.  如果从响应中提取数据，则将数据进行保存

![https://picsur.cloud.fairies.ltd/i/cd3112c3-915e-419f-8275-751d6c8864d5.webp](https://picsur.cloud.fairies.ltd/i/cd3112c3-915e-419f-8275-751d6c8864d5.webp)

# 二. 爬虫基础

## 1. HTTP协议

> 一提起http协议，大家都会想起它是一个应用层协议，那么http协议跟爬虫有什么关系呢？请看下图

![https://picsur.cloud.fairies.ltd/i/7ab50fe3-dfa0-449a-a547-2e41df4a34ed.webp](https://picsur.cloud.fairies.ltd/i/7ab50fe3-dfa0-449a-a547-2e41df4a34ed.webp)

### 1.1. http以及https的概念和区别

> HTTPS比HTTP更安全，但是性能更低

1.  HTTP：超文本传输协议，默认端口号是80
    -   超文本：是指超过文本，不仅限于文本；还包括图片、音频、视频等文件
    -   传输协议：是指使用共用约定的固定格式来传递转换成字符串的超文本内容
2.  HTTPS：HTTP + SSL(安全套接字层)，即带有安全套接字层的超本文传输协，默认端口号：443
    -   SSL对传输的内容（超文本，也就是请求体或响应体）进行加密

可以打开浏览器访问一个url，右键检查，点击net work，点选一个url，查看http协议的形式

## 2. 爬虫特别关注的请求头和响应头

### 2.1. 特别关注的请求头字段

![https://picsur.cloud.fairies.ltd/i/c6c3cf6a-9ba9-421a-b24f-726c3bf0942f.webp](https://picsur.cloud.fairies.ltd/i/c6c3cf6a-9ba9-421a-b24f-726c3bf0942f.webp)

> http请求的形式如上图所示，爬虫特别关注以下几个请求头字段

-   Content-Type
-   Host (主机和端口号)
-   Connection (链接类型)
-   Upgrade-Insecure-Requests (升级为HTTPS请求)
-   **User-Agent** (浏览器名称)
-   **Referer** (页面跳转处)
-   **Cookie** (Cookie)
-   Authorization(用于表示HTTP协议中需要认证资源的认证信息，如前边web课程中用于jwt认证)

> 加粗的请求头为常用请求头，在服务器被用来进行爬虫识别的频率最高，相较于其余的请求头更为重要，但是这里需要注意的是并不意味这其余的不重要，因为有的网站的运维或者开发人员可能剑走偏锋，会使用一些比较不常见的请求头来进行爬虫的甄别

### 2.2. 特别关注的响应头字段

![https://picsur.cloud.fairies.ltd/i/a9d3cac8-8ce8-4ca8-8b3e-8ab9cdc1f9c1.webp](https://picsur.cloud.fairies.ltd/i/a9d3cac8-8ce8-4ca8-8b3e-8ab9cdc1f9c1.webp)

> http响应的形式如上图所示，爬虫只关注一个响应头字段

-   Set-Cookie （对方服务器设置cookie到用户浏览器的缓存）

## 3. 常见的响应状态码

-   200：成功
-   302：跳转，新的url在响应的Location头中给出
-   303：浏览器对于POST的响应进行重定向至新的url
-   307：浏览器对于GET的响应重定向至新的url
-   403：资源不可用；服务器理解客户的请求，但拒绝处理它（没有权限）
-   404：找不到该页面
-   500：服务器内部错误
-   503：服务器由于维护或者负载过重未能应答，在响应中可能可能会携带Retry-After响应头；有可能是因为爬虫频繁访问url，使服务器忽视爬虫的请求，最终返回503响应状态码

我们在学习web知识的时候就已经学过了状态码的相关知识，我们知道这是服务器给我的相关反馈，我们在学习的时候就被教育说应该将真实情况反馈给客户端，但是在爬虫中，可能该站点的开发人员或者运维人员为了阻止数据被爬虫轻易获取，可能在状态码上做手脚，也就是说返回的状态码并不一定就是真实情况，比如:服务器已经识别出你是爬虫，但是为了让你疏忽大意，所以照样返回状态码200，但是响应体重并没有数据

**所有的状态码都不可信，一切以是否从抓包得到的响应中获取到数据为准**

## 4. 浏览器的运行过程

> 在回顾完http协议后，我们来了解以下浏览器发送http请求的过程

![https://picsur.cloud.fairies.ltd/i/105e6dac-ac8b-4c3c-8548-4610105c359d.webp](https://picsur.cloud.fairies.ltd/i/105e6dac-ac8b-4c3c-8548-4610105c359d.webp)

### 4.1. http请求的过程

1.  浏览器在拿到域名对应的ip后，先向地址栏中的url发起请求，并获取响应
    
2.  在返回的响应内容（html）中，会带有css、js、图片等url地址，以及ajax代码，浏览器按照响应内容中的顺序依次发送其他的请求，并获取相应的响应
    
3.  浏览器每获取一个响应就对展示出的结果进行添加（加载），js，css等内容会修改页面的内容，js也可以重新发送请求，获取响应
    
4.  从获取第一个响应并在浏览器中展示，直到最终获取全部响应，并在展示的结果中添加内容或修改————这个过程叫做浏览器的渲染
    

**注意**:

在爬虫中，爬虫只会请求url地址，对应的拿到url地址对应的响应（该响应的内容可以是html，css，js，图片等)

浏览器渲染出来的页面和爬虫请求的页面很多时候并不一样，是因为爬虫不具备渲染的能力

-   浏览器最终展示的结果是由多个url地址分别发送的多次请求对应的多次响应共同渲染的结果
-   在爬虫中，需要以发送请求的一个url地址对应的响应为准来进行数据的提取

# 三. Request模块

## 1. requests模块介绍

> requests文档https://requests.readthedocs.io/projects/cn/zh_CN/latest

### 1.1. requests模块的作用

-   发送http请求，获取响应数据

### 1.2. requests模块的使用

requests模块是一个第三方模块，需要在你的python(虚拟)环境中额外安装

```bash
pip install requests

```

### 1.3. requests模块简单的get请求

> 需求：通过requests向百度首页发送请求，获取该页面的源码运行下面的代码，观察打印输出的结果

```python
# coding: utf8
"""
@File: part_001.py
@Author: Austin(From Chengdu.China)
@HomePage: <https://github.com/AustinFairyland>
@CreatedTime: 2022/10/17 2:25
"""

import os, sys

"""初识requests模块和简单的response对象"""

# 导入requests模块
import requests

# 目标url
url = '<https://www.baidu.com>'
# 向目标url发送get请求
response = requests.get(url)
# 打印响应内容
print(response.text)


```

## **2. response响应对象**

> 观察上边代码运行结果发现，有好多乱码；这是因为编解码使用的字符集不同早造成的；我们尝试使用下边的办法来解决中文乱码问题

```python
 # coding: utf8
 """
 @File: part_001.py
 @Author: Austin(From Chengdu.China)
 @HomePage: <https://github.com/AustinFairyland>
 @CreatedTime: 2022/10/17 2:25
 """
 
 import os, sys
 
 """初识requests模块和简单的response对象"""
 
 # 导入requests模块
 import requests
 
 # 目标url
 url = '<https://www.baidu.com>'
 # 向目标url发送get请求
 response = requests.get(url)
 # 打印响应内容
 # print(response.text)
 print(response.content.decode())  # 注意这里

```

1.  response.text是requests模块按照chardet模块推测出的编码字符集进行解码的结果
2.  网络传输的字符串都是bytes类型的,所以response.text=response.content.decode('推测出的编码字符集')
3.  我们可以在网页源码中搜索`charset`,尝试参考该编码字符集，注意存在不准确的情况

### **2.1. response.text和response.content的区别**

-   response.text
    -   type: str
    -   decode: requests模块自动根据HTTP头部对响应的编码作出有根据的推测，推测的文本编码
-   response.content
    -   type: bytes
    -   decode: None

### **2.2. 通过对response.content使用decode()方法, 来解决中文乱码**

![https://picsur.cloud.fairies.ltd/i/06388465-a351-4e36-a0c6-714321cb9862.webp](https://picsur.cloud.fairies.ltd/i/06388465-a351-4e36-a0c6-714321cb9862.webp)

```python
 # coding: utf8
 """
 @File: part_001.py
 @Author: Austin(From Chengdu.China)
 @HomePage: <https://github.com/AustinFairyland>
 @CreatedTime: 2022/10/17 2:25
 """
 
 import os, sys
 
 """初识requests模块和简单的response对象"""
 
 import requests
 
 url = '<https://baidu.com>'
 response = requests.get(url=url)
 # print(response.text)
 # print(response.content.decode('utf-8'))
 print(response.content.decode())  # 默认是utf-8

```

![https://picsur.cloud.fairies.ltd/i/26e01c29-b701-493f-8d8b-913be9f5ebbe.webp](https://picsur.cloud.fairies.ltd/i/26e01c29-b701-493f-8d8b-913be9f5ebbe.webp)

-   常见的编码字符集
    -   utf-8
    -   gbk
    -   gb2312
    -   ascii
    -   iso-8859-1

### **2.3. response响应对象的其它常用属性或方法**

> response = requests.get(url) response是发送请求获取的响应对象

1.  `response.url` 响应的url, 有时候响应的url和请求的url并不一致
2.  `response.status_code` 响应状态码
3.  `response.request.headers` 响应对应的请求头
4.  `response.headers` 响应头
5.  `response.request._cookies` 响应对应请求的cookie, 返回RequestsCookieJar对象类型
6.  `response.cookies` 响应的cookie, 经过了set-cookie动作, 返回RequestsCookieJar对象类型
7.  `response.json` 自动将json字符串类型的响应内容转换为python对象 -> dict or list

```python
# coding: utf8
"""
@File: part_001.py
@Author: Austin(From Chengdu.China)
@HomePage: <https://github.com/AustinFairyland>
@CreatedTime: 2022/10/17 2:25
"""

import os, sys

"""初识requests模块和简单的response对象"""

import requests

url = '<https://baidu.com>'
response = requests.get(url=url)
# print(response.text)
print(response.content.decode('utf-8'))
# 响应的url
print('响应的url', response.url)
# 响应的状态码
print('响应的状态码', response.status_code)
# 响应对象的请求头
print('响应对象的请求头', response.request.headers)
# 响应头
print('响应头', response.headers)
# 请求携带的cookies(私有属性)
print('请求携带的cookies', response.request._cookies)
# 响应中携带的cookies
print('响应中携带的cookies', response.cookies)

```

![https://picsur.cloud.fairies.ltd/i/72e9ba1c-25d6-4d0b-ac4e-c8bb1adb4349.webp](https://picsur.cloud.fairies.ltd/i/72e9ba1c-25d6-4d0b-ac4e-c8bb1adb4349.webp)

## 3. requests模块发送GET请求

### 3.1. 发送带header的请求

> 我们先写一个获取百度首页的代码

```python
# coding: utf8
"""
@File: part_002.py
@Author: Austin(From Chengdu.China)
@HomePage: <https://github.com/AustinFairyland>
@CreatedTime: 2022/10/17 15:26
"""

import os, sys

"""requests发送请求"""

import requests

url = '<https://baidu.com>'
response = requests.get(url=url)
# print(response.content.decode())
# 打印响应对应请求的请求头信息
print(response.request.headers)


```

返回的请求头

![https://picsur.cloud.fairies.ltd/i/b15b9e78-760c-4d20-9527-6662907acfcb.webp](https://picsur.cloud.fairies.ltd/i/b15b9e78-760c-4d20-9527-6662907acfcb.webp)

3.1.1. 思考

1.  对比浏览器上百度首页的网页源码和代码中的百度首页的源码, 有什么不同?
    
    -   查看网页源码的方法:`右键`>`查看网页源代码`
2.  代码中的百度首页的源码非常少,为什么?
    -   需要我们带上请求头信息
        
        > 模拟浏览器,欺骗服务器,获取和浏览器一致的内容
        
    -   请求头中有很多字段,其中User-Agent字段必不可少,表示客户端的操作系统以及浏览器的信息
        

3.1.2. 携带请求头发送请求的方法

`requests.get(url=url, headers=headers)`

-   headers参数接收字典形式的请求头
-   请求头字段名作为key,字段对应的值作为value

3.1.3. 完成代码实现

> 从浏览器中复制User-Agent,构造headers字典;完成下面的代码后,运行代码查看结果

![https://picsur.cloud.fairies.ltd/i/ba079a12-4f23-476e-859c-3890b5280cd2.webp](https://picsur.cloud.fairies.ltd/i/ba079a12-4f23-476e-859c-3890b5280cd2.webp)

```python
# coding: utf8
"""
@File: part_002.py
@Author: Austin(From Chengdu.China)
@HomePage: <https://github.com/AustinFairyland>
@CreatedTime: 2022/10/17 15:26
"""

import os, sys

"""requests发送请求"""

import requests

url = '<https://baidu.com>'
response = requests.get(url=url)
# print(response.content.decode())
# 打印响应对应请求的请求头信息
print(response.request.headers)

"""发送带headers的请求"""

# 定义请求头
headers = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/109.0.0.0 Safari/537.36'
}

# 在请求头中带上User-Agent, 模拟浏览器发送请求
response = requests.get(url=url, headers=headers)
# 打印请求头
print(response.request.headers)


```

![https://picsur.cloud.fairies.ltd/i/a717880d-d280-4a31-97d0-09bdc1083ad6.webp](https://picsur.cloud.fairies.ltd/i/a717880d-d280-4a31-97d0-09bdc1083ad6.webp)

### 3.2. requests模块发送带参数的请求

> 我们在使用百度搜索的时候经常发现url地址中会有一个?,那么该问号后边的就是请求参数,又叫做查询字符串

3.2.1. 在url携带参数

直接对含有参数的url发起请求

```python
# coding: utf8
"""
@File: part_002.py
@Author: Austin(From Chengdu.China)
@HomePage: <https://github.com/AustinFairyland>
@CreatedTime: 2022/10/17 15:26
"""

import os, sys

"""requests发送请求"""

import requests

url = '<https://baidu.com>'
response = requests.get(url=url)
# print(response.content.decode())
# 打印响应对应请求的请求头信息
print(response.request.headers)
print(f'没有自定义headers返回的响应头大小: {len(response.content.decode())}')

"""发送带headers的请求"""

# 定义请求头
headers = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/109.0.0.0 Safari/537.36'
}

# 在请求头中带上User-Agent, 模拟浏览器发送请求
response = requests.get(url=url, headers=headers)
# 打印请求头
print(response.request.headers)
print(f'自定义headers返回的响应头大小: {len(response.content.decode())}')

"""发送带参数的请求"""

url = '<https://baidu.com/s?wd=python>'
response = requests.get(url=url, headers=headers)


```

3.2.2. 通过params携带参数字典

1.  构建请求参数字典
2.  向接口发送请求的时候带上参数字典,参数字典设置给params

```python
# coding: utf8
"""
@File: part_002.py
@Author: Austin(From Chengdu.China)
@HomePage: <https://github.com/AustinFairyland>
@CreatedTime: 2022/10/17 15:26
"""

import os, sys

"""requests发送请求"""

import requests

url = '<https://baidu.com>'
response = requests.get(url=url)
# print(response.content.decode())
# 打印响应对应请求的请求头信息
print(response.request.headers)
print(f'没有自定义headers返回的响应头大小: {len(response.content.decode())}')

"""发送带headers的请求"""

# 定义请求头
headers = {
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/109.0.0.0 Safari/537.36'
}

# 在请求头中带上User-Agent, 模拟浏览器发送请求
response = requests.get(url=url, headers=headers)
# 打印请求头
print(response.request.headers)
print(f'自定义headers返回的响应头大小: {len(response.content.decode())}')

"""发送带参数的请求"""

url = '<https://baidu.com/s>'
# url = '<https://baidu.com/s?wd=python>'
# response = requests.get(url=url, headers=headers)

# 请求参数是一个字典 即wd=python
params = {
    'wd': 'python'
}
# 带上请求参数发起请求,获取响应
response = requests.get(url=url, headers=headers, params=params)


```

### 3.3. 在headers参数中携带cookie

> 网站经常利用请求头中的Cookie字段来做用户访问状态的保持,那么我们可以在headers参数中添加Cookie,模拟普通用户的请求,我以github登陆为例

3.3.1. github登陆抓包分析

1.  打开浏览器,`右键`>`检查`,点击`Network`,勾选`Preserve log`
2.  访问github登陆的url地址https://github.com/login
3.  输入账号密码点击登陆后,访问一个需要登陆后才能获取正确内容的url;访问[](https://github.com/%7BUSERNAME%7D)[https://github.com/{USERNAME}](https://github.com/%7BUSERNAME%7D)
4.  确定url之后, 再确定发送该请求所需要的请求头信息中的`User-Agent`和`Cookie`

![https://picsur.cloud.fairies.ltd/i/bae211be-d9d2-4cfc-a28e-82f2d3aec5be.webp](https://picsur.cloud.fairies.ltd/i/bae211be-d9d2-4cfc-a28e-82f2d3aec5be.webp)

3.3.2. 代码完成

-   从浏览器中复制`User-Agent`和`Cookie`
-   浏览器中的请求头字段和值与headers参数中必须一致
-   headers请求参数字典中的Cookie键对应的值是字符串

```python
# coding: utf8
"""
@File: part_003.py
@Author: Austin(From Chengdu.China)
@HomePage: <https://github.com/AustinFairyland>
@CreatedTime: 2022/10/17 16:10
"""

import os, sys

"""在headers参数中携带cookie"""

"""github示例"""

import requests

url = '<https://github.com/AustinFairyland>'
# url = '<https://github.com/{USERNAME}>'

# 构造请求头字典
headers = {
    # 从浏览器中复制过来的User-Agent
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/109.0.0.0 Safari/537.36',
    # 从浏览器中复制过来的Cookie
    'Cookie': 'cookie字符串'
}

# 请求头参数字典中携带cookie字符串
response = requests.get(url, headers=headers, proxies={'http': '<http://127.0.0.1:56789>'})
print(response.content.decode())


```

3.3.3. 运行代码验证结果

> 在打印的输出结果中搜索title,html中的标题文本内容如果是你的github账号,则成功利用headers参数携带cookie,获取登陆后才能访问的页面

![https://picsur.cloud.fairies.ltd/i/daf8558f-587d-4ef3-a788-62a5734778e3.webp](https://picsur.cloud.fairies.ltd/i/daf8558f-587d-4ef3-a788-62a5734778e3.webp)

### 3.4. cookies参数的使用

> 上一小节我们在headers参数中携带cookie,也可以使用专门的cookies参数

4.1. cookies参数的形式

dict `cookies={'name'='value'}`

-   该字典对应请求头中Cookie字符串,以分号,空格分割每一对字典键值对
-   等号左边的是一个cookie的name,对应cookies字典的key
-   等号右边对应cookies字典的value

4.2. cookies参数的使用方法

```python
response = requests.get(url, cookies)

```

4.3. 将cookie字符串转换为cookies参数所需的字典

```python
cookies_dict = {cookie.split('=')[0]:cookie.split('=')[1] for cookie in cookies_str.split('; ')}

```

-   注意: **cookie一般是有过期时间的,一旦过期需要重新获取**

```python
# coding: utf8
"""
@File: part_003.py
@Author: Austin(From Chengdu.China)
@HomePage: <https://github.com/AustinFairyland>
@CreatedTime: 2022/10/17 16:10
"""

import os, sys

"""在headers参数中携带cookie"""

import requests

"""github示例"""

url = '<https://github.com/AustinFairyland>'
# url = '<https://github.com/{USERNAME}>'

# 构造请求头字典
headers = {
    # 从浏览器中复制过来的User-Agent
    'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/109.0.0.0 Safari/537.36',
    # 从浏览器中复制过来的Cookie
    # 'Cookie': 'cookie字符串'
}

# 请求头参数字典中携带cookie字符串
# response = requests.get(url, headers=headers, proxies={'http': '<http://127.0.0.1:56789>'})
# print(response.content.decode())

"""cookies参数的使用"""

# 构建cookie字典 -> 字典推导式
cookie_temp = 'cookie字符串(cookies values)'

# 普通方法
# cookie_temp_list = cookie_temp.split('; ')
# cookies = {}
# for cookie in cookie_temp_list:
#     cookies[cookie.split('=')[0]]=cookie.split('=')[1]
# 字典推导式
cookies = {cookie.split('=')[0]: cookie.split('=')[1] for cookie in cookie_temp.split('; ')}
response = requests.get(url=url, headers=headers, cookies=cookies, proxies={'http': '<http://127.0.0.1:56789>', 'https': '<http://127.0.0.1:56789>'})
with open(file='./github_with.html', mode='wb') as files:
    files.write(response.content)
files.close()


```

### 3.5. RequestCookieJar对象转换为cookies字典的方法

> 使用requests获取的resposne对象,具有cookies属性.该属性值是一个RequestCookieJar类型,包含了对方服务器设置在本地的cookie.我们如何将其转换为cookies字典呢?

5.1. 转换方法

```python
cookies_dict = requests.utils.dict_from_cookiejar(response.cookies)

```

其中response.cookies返回的就是cookieJar类型的对象

`requests.utils.dict_from_cookiejar` 方法返回cookies字典

```python
import requests
from requests import utils

"""RequestCookieJar对象转换为cookies字典的方法"""

url_baidu = '<https://baidu.com>'
response = requests.get(url=url)
print(requests.utils.dict_from_cookiejar(response.cookies))

```

### 3.6. 超时参数timeout的使用

> 在平时网上冲浪的过程中,我们经常会遇到网络波动,这个时候,一个请求等了很久可能任然没有结果.
> 
> 在爬虫中,一个请求很久没有结果,就会让整个项目的效率变得非常低,这个时候我们就需要对请求进行强制要求,让他必须在特定的时间内返回结果,否则就报错.

6.1. 超时参数timeout的使用方法

```python
response = requests.get(url, timeout=3)

```

timeout=3表示:发送请求后,3秒钟内返回响应,否则就抛出异常

```python
# coding: utf8
"""
@File: part_004.py
@Author: Austin(From Chengdu.China)
@HomePage: <https://github.com/AustinFairyland>
@CreatedTime: 2022/10/18 2:47
"""

import os, sys

"""超时timeout参数和proxies的使用"""

import requests

url = '<https://twitter.com>'

try:
    response = requests.get(url=url, timeout=3)
    print('第一次请求', response.status_code)
except Exception as error:
    print('第一次请求', error)
finally:
    proxy = {
        'http': '<http://127.0.0.1:56789>',
        'https': '<http://127.0.0.1:56789>',
    }
    try:
        response = requests.get(url=url, timeout=3, proxies=proxy)
        print('第二次请求', response.status_code)
    except Exception as error:
        print('第二次请求', error)


```

### 3.7. 了解代理以及proxy代理参数的使用

> proxy代理参数通过指定代理ip,让代理ip对应的正向代理服务器转发我们发送的请求,那么我们首先来了解一下代理ip以及代理服务器

7.1. 理解使用代理的过程

1.  代理ip是一个ip,指向的是一个代理服务器
2.  代理服务器能够帮我们向目标服务器转发请求

![https://picsur.cloud.fairies.ltd/i/b5af31e6-dcfd-43b5-baa2-787845ee013d.webp](https://picsur.cloud.fairies.ltd/i/b5af31e6-dcfd-43b5-baa2-787845ee013d.webp)

7.2. 正向代理和反向代理的区别

> 前边提到proxy参数指定的代理ip指向的是正向的代理服务器,那么相应的就有反向服务器;现在来了解一下正向代理服务器和反向代理服务器的区别

1.  从发送请求的一方的角度,来区分正向或反向代理
2.  为浏览器或客户端(发送请求的一方)转发请求的,叫做正向代理
    -   浏览器知道最终处理请求的服务器的真实ip地址,例如VPN
3.  不为浏览器或客户端(发送请求的一方)转发请求,而是为最终处理请求的服务器转发请求的,叫做反向代理
    -   浏览器不知道服务器的真实地址,例如nginx

7.3. 代理ip(代理服务器)的分类

1.  根据代理ip的匿名程度,代理IP可以分为下面三类
    
    1.  透明代理(Transparent Proxy):透明代理虽然可以直接“隐藏”你的IP地址,但是还是可以查到你是谁.目标服务器接收到的请求头如下
        
        ```
        REMOTE_ADDR = Proxy IP
        HTTP_VIA = Proxy IP
        HTTP_X_FORWARDED_FOR = Your IP
        
        
        ```
        
    2.  匿名代理(Anonymous Proxy):使用匿名代理,别人只能知道你用了代理,无法知道你是谁.目标服务器接收到的请求头如下
        
        ```
        REMOTE_ADDR = proxy IP
        HTTP_VIA = proxy IP
        HTTP_X_FORWARDED_FOR = proxy IP
        
        
        ```
        
    3.  高匿代理(Elite proxy或High Anonymity Proxy):高匿代理让别人根本无法发现你是在用代理,所以是最好的选择.毫无疑问使用高匿代理效果最好.目标服务器接收到的请求头如下
        
        ```
        REMOTE_ADDR = Proxy IP
        HTTP_VIA = not determined
        HTTP_X_FORWARDED_FOR = not determined
        
        
        ```
    
2.  根据网站所使用的协议不同,需要使用相应协议的代理服务.从代理服务请求使用的协议可以分为
    
    -   http代理:目标url为http协议
    -   https代理:目标url为https协议
    -   socks隧道代理(例如socks5代理)等
        -   socks代理只是简单地传递数据包,不关心是何种应用协议(FTP,HTTP和HTTPS等).
        -   socks代理比http,https代理耗时少.
        -   socks代理可以转发http和https的请求

7.4. proxies代理参数的使用

> 为了让服务器以为不是同一个客户端在请求;为了防止频繁向一个域名发送请求被封ip,所以我们需要使用代理ip

proxies代理参数的用法

```python
response = requests.get(url, proxies=proxies)


```

-   proxies的形式:字典

e.g.

```python
proxies = {
    "http": "<http://127.0.0.1:56789>",
    "https": "<http://127.0.0.1:56789>",
}


```

-   **注意:如果proxies字典中包含有多个键值对,发送请求时将按照url地址的协议来选择使用相应的代理ip**

```python
# coding: utf8
"""
@File: part_004.py
@Author: Austin(From Chengdu.China)
@HomePage: <https://github.com/AustinFairyland>
@CreatedTime: 2022/10/18 2:47
"""

import os, sys

"""超时timeout参数和proxies的使用"""

import requests

url = '<https://twitter.com>'

try:
    response = requests.get(url=url, timeout=3)
    print('第一次请求', response.status_code)
except Exception as error:
    print('第一次请求', error)
finally:
    proxy = {
        'http': '<http://127.0.0.1:56789>',
        'https': '<http://127.0.0.1:56789>',
    }
    try:
        response = requests.get(url=url, timeout=3, proxies=proxy)
        print('第二次请求', response.status_code)
    except Exception as error:
        print('第二次请求', error)


```

### 3.8. 使用verify参数忽略CA证书

> 在使用浏览器上网的时候,有时能够看到下面的提示(2018年10月之前的12306网站)

![https://picsur.cloud.fairies.ltd/i/91837076-c9db-40e7-8ee8-640fb6bdac08.webp](https://picsur.cloud.fairies.ltd/i/91837076-c9db-40e7-8ee8-640fb6bdac08.webp)

-   原因:该网站的CA证书没有经过【受信任的根证书颁发机构】的认证

8.1. 向不安全的链接发起请求

如果代码抛出包含`ssl.CertificateError...`字样的异常

8.2. 解决方案

> 为了在代码中能够正常的请求,我们使用verify=False参数,此时requests模块发送请求将不做CA证书的验证:verify参数能够忽略CA证书的认证

```python
import requests
url = "url"
response = requests.get(url,verify=False)

```

## 4. requests模块发送POST请求

思考:

> 哪些地方我们会用到POST请求?
> 
> 1.  登录注册( 在web工程师看来POST 比 GET 更安全,url地址中不会暴露用户的账号密码等信息)
> 2.  需要传输大文本内容的时候( POST 请求对数据长度没有要求)
> 
> 同样的,爬虫也需要在这两个地方回去模拟浏览器发送post请求

### 4.1. requests发送POST请求的方法

```python
response = requests.post(url, data)


```

`data`参数接收一个字典

requests模块发送post请求函数的其它参数和发送get请求的参数完全一致

### 4.2. POST请求练习

我们通过百度翻译的例子看看post请求如何使用：

-   地址: [https://fanyi.baidu.com/](https://fanyi.baidu.com/)

1.  思路分析

(1). 抓包确定请求的url地址

![https://picsur.cloud.fairies.ltd/i/81ea85ae-3b00-40e8-9921-4534e0a71060.webp](https://picsur.cloud.fairies.ltd/i/81ea85ae-3b00-40e8-9921-4534e0a71060.webp)

(2). 确定请求的参数

![https://picsur.cloud.fairies.ltd/i/279e6d56-aa8a-4f61-b0a2-1ae1b0bc7b1c.webp](https://picsur.cloud.fairies.ltd/i/279e6d56-aa8a-4f61-b0a2-1ae1b0bc7b1c.webp)

![https://picsur.cloud.fairies.ltd/i/a01a3784-986d-412b-aac2-15225fdffa55.webp](https://picsur.cloud.fairies.ltd/i/a01a3784-986d-412b-aac2-15225fdffa55.webp)

(3). 确定返回数据的位置

![https://picsur.cloud.fairies.ltd/i/ef15a603-da37-4196-ad4b-4410dd912cfe.webp](https://picsur.cloud.fairies.ltd/i/ef15a603-da37-4196-ad4b-4410dd912cfe.webp)

(4). 模拟浏览器获取数据

![https://picsur.cloud.fairies.ltd/i/e56b08e8-5b2b-454f-9c0d-983f25e6609f.webp](https://picsur.cloud.fairies.ltd/i/e56b08e8-5b2b-454f-9c0d-983f25e6609f.webp)

1.  代码实现

```python
# coding: utf8
"""
@File: part_005.py
@Author: Austin(From Chengdu.China)
@HomePage: <https://github.com/AustinFairyland>
@CreatedTime: 2022/10/18 14:08
"""
import json
import os, sys

import requests
import time

"""POST请求百度翻译"""

class Baidu(object):
    """百度翻译"""

    def __init__(self, kword: str):
        self.kword = kword
        self.url = '<https://fanyi.baidu.com/v2transapi?from=zh&to=en>'
        self.headers = {
            'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/109.0.0.0 Safari/537.36',
            'Accept-Encoding': 'gzip, deflate',
            'Accept': '*/*',
            'Connection': 'keep-alive',
            'Cookie': 'BIDUPSID=E93ED0C7F32AF1EC82E2162EB9A8A484; PSTM=1665947571; BAIDUID=E93ED0C7F32AF1EC3957D4B927CFF46A:FG=1; BDORZ=B490B5EBF6F3CD402E515D22BCDA1598; BAIDUID_BFESS=E93ED0C7F32AF1EC3957D4B927CFF46A:FG=1; BA_HECTOR=212g0k20a50k8h0l810k63ct1hkr2so1b; ZFY=svKmEb:BlY8:B18U0E2nf9ELBy0WnWLV89X0rAEKvMFO4:C; APPGUIDE_10_0_2=1; REALTIME_TRANS_SWITCH=1; FANYI_WORD_SWITCH=1; HISTORY_SWITCH=1; SOUND_SPD_SWITCH=1; SOUND_PREFER_SWITCH=1; ab_sr=1.0.1_ZmNjNmUxNzljZmUyMWYzZTY3YTgwYzNjZGI4MDYwOTQwNzJmYTdhZTFjNWExN2ZjNzI1ODhmNWVjMmQ5NjBkZjZkY2EwOTkwNWJhOWExNzI2MGE4YWI4MmI0ODllMTQyMTU2MDExNTM4MDhhZjlkZWIyOWM2ZmRjNGY1NzRjMjM3MTU5ODU1YzZjZmJkOGU5YWI2NTc4NzBmMDQxMzRiNw==',
            'Host': 'fanyi.baidu.com',
            'Origin': '<https://fanyi.baidu.com>',
            'Referer': '<https://fanyi.baidu.com/>',
            'Acs-Token': '1665990292453_1666076505721_SdxK64OeDt7XdvwgQHb8tl2Lgj5Rdd+OTX0k8lMmmo1GltC4tRA43Co/N5kkWIwcoRFAvIUYxu5gwbc5+0cnt6Tw4tFHkOSNOmaEDuuLLQC2JInbNzEnT7izSYCfLmZAFETPkDUedn/F1QrFGaYc9ixWyemyQJadHAFDaZX8OeuDw3HncZGEisrW/yKtZICi5BGBo0mJeTbDsUPLO0gnAzJMrC+LW2n1XAZYvlte558+DyWzgS6ZHhoYJAIhR3tquKu8IqyLQwxdeVAU5+Iq8I17RiBpWqRGtSppOUB9QgRWB6oUsoD0oUQFTEzDanqZSus7E3n1G5ahATeq7fJf2A==',
            'sec-ch-ua': '"Not_A Brand";v="99", "Google Chrome";v="109", "Chromium";v="109"'
        }
        self.data = {
            # 'query': self.kword
            'from': 'zh',
            'to': 'en',
            'query': self.kword,
            'transtype': 'translang',
            'simple_means_flag': 3,
            'sign': '275626.55195',
            'token': '92ac48f1353cd107310d87c02884e435',
            'domain': 'common'
        }

    def get_method(self):
        pass

    def post_method(self):
        resonse = requests.post(
            url=self.url,
            headers=self.headers,
            data=self.data
        )
        response_dict = json.loads(resonse.content)
        print(response_dict['trans_result']['data'][0]['dst'])

if __name__ == '__main__':
    Baidu('学习').post_method()


```

## 5. 利用requests.session进行状态保持

> requests模块中的Session类能够自动处理发送请求获取响应过程中产生的cookie,进而达到状态保持的目的。

### 5.1. requests.session的作用以及应用场景

-   requests.session的作用
    
    自动处理cookie,即下一次请求会带上前一次的cookie
    
-   requests.session的应用场景
    
    自动处理连续的多次请求过程中产生的cookie
    

### 5.2. requests.session使用方法

> session实例在请求了一个网站后,对方服务器设置在本地的cookie会保存在session中,下一次再使用session请求对方服务器的时候,会带上前一次的cookie

-   session对象发送get或post请求的参数,与requests模块发送请求的参数完全一致

## 6. 案例: 模拟GitHub登录

参考代码:

```python
# coding: utf8
""" 
@File: part_006.py
@Author: Austin(From Chengdu.China)
@HomePage: https://github.com/AustinFairyland
@CreatedTime: 2022/10/19 22:44
"""

import os, sys

"""模拟github登录返回个人主页"""

import requests
import re

def get_github_profile() -> None:
    """
    请求github
    :return: 
    """
    # 请求头
    headers = {
        'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/109.0.0.0 Safari/537.36',
    }
    # 实例化session
    session = requests.session()
    # 代理
    proxy = {
        'http': 'http://127.0.0.1:56789',
        'https': 'http://127.0.0.1:56789'
    }
    # 访问登陆页获取登陆请求所需参数
    response = session.get('https://github.com/login', headers=headers, proxies=proxy)
    authenticity_token = re.search('name="authenticity_token" value="(.*?)" />', response.text).group(1)  # 使用正则获取登陆请求所需参数

    # 构造登陆请求参数字典
    data = {
        'commit': 'Sign in',  # 固定值
        'utf8': '✓',  # 固定值
        'authenticity_token': authenticity_token,  # 该参数在登陆页的响应内容中
        'login': input('输入github用户名：'),
        'password': input('输入github账号：')
    }

    # 发送登陆请求（无需关注本次请求的响应）
    session.post('https://github.com/session', headers=headers, data=data, proxies=proxy)

    # 打印需要登陆后才能访问的页面
    response = session.get(f'https://github.com/{data.get("login")}', headers=headers, proxies=proxy)
    print(response.text)
    
if __name__ == '__main__':
    get_github_profile()
```