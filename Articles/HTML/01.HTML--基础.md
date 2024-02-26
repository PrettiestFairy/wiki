# HTML语法规范

```text
标签
<html></html>    双标签
<br />   单标签

包含关系
<head>
	<title></title>
<head>
并列关系
<head></head>
<body></body>

举例
<html>
	<head></head>
	<body></body>
</html>

<br />
```

# HTML基本结构标签

```text
<html>
	<head>
		<title>我的第一个页面</title>
	</head>
	<body>
		abcdefghigklmn
	</body>
</html>

<html></html>    HTML标签
<head></head>    文档的头部
<title></title>     文档的标题
<body></body>   文档的主体
```

# HTML标签

```text
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <title>WebStorm的第一个页面</title>
</head>
<body>
    学前端！冲啊！！！
</body>
</html>


<!DOCTYPE>文档类型申明，告诉浏览器使用哪种HTML版本来显示网页。
<!DOCTYPE html>这个页面使用的是最新的html5版本来显示

lang语言种类，en定义语言为英语，zh-CN定义语言为中文

charset字符集：UTF-8  万国码
<meta charset="UTF-8">是必须要写的代码，否则可能出现乱码的情况
```

# HTML常用标签

```text
标题标签
<h1> - <h6>      头部标签,作为标题使用，并且依据重要性递减
<h1>我是一级标题</h1>
特点：
1.加了标题的文字会变的加粗，字号也会依次变大
2.一个标题独占一行


段落和换行标签
<p>用于定义段落,把文档分为若干段落
<p>我是一个段落标签</p>
特点：
1.文本在一个段落中会根据浏览器的窗口的大小自动换行
2.段落和段落之间保有空隙

<br />强制换行，单标签


文本格式化标签
加粗	<strong></strong>或<b></b>
倾斜	<em></em>或<i></i>
删除线	<del></del>或<s></s>
下划线	<ins></ins>或<u></u>


<div>和<span>标签，就是一个盒子，用来装内容的
<div>这是头部</div>
<span>今日价格</span>
特点：
1.<div>标签用来布局，但是现在一行只能放一个<div>，大盒子
2.<span>标签用来布局，一行可以多个<span>，小盒子


图像标签和路径
1.图像标签<img>，用来定义HTML页面中的图像
<img src="图像URL" />
src是<img>标签的必须属性，它用于指定图像文件的路径和文件名
alt为替换文本，图像不能显示时出现的文字
title是提示文本，鼠标放到图片上显示的文字
width设置图像的宽度
height设置图像的高度（宽和高一般设置其中一个，等比例缩放）
border设置图像的边框粗细（边框一般在css设置）

属性一般没有顺序，属性与属性之间要有空格，属性采取kv格式即 属性=“属性值”

<h1>这有一只猫</h1>
    <img src="猫猫.jpg" alt="有一只猫" title="让你瞅瞅" width="500" height="300" border="15" />

2.路径
相对路径
同一级路径		<img src="猫猫.jbg" />
下一级路径	/	<img src="images/猫猫.jbg" />
上一级路径	../	<img src="../猫猫.jbg" />
绝对路径
盘符或完整的网络地址，用得较少
<img src="https://i0.hdslb.com/bfs/banner/8623346fd12fb128a67089926d694740533286cb.jpg@.webp" alt="网页中的图片不见了" width="100%" />


超链接标签
语法格式
<a href="跳转目标" target="目标窗口的弹出方式">文本或图像</a>
href用于指定连接目标的url地址
target用于指定链接页面的打开方式，其中_self为默认值，_blank为在新窗口打开

链接分类
外部链接
 <a href="http://www.qq.com" target="_blank">腾讯</a>
 <!--target为打开页面的方式，_self为在当前页面打开，_blank为新窗口页面打开-->
内部链接
<a href="04-图像标签.html">图像标签</a>
空链接
 <a href="#">等待着放点啥</a>
下载链接：地址链接的是 文件.exe 或者是 .zip 等压缩包的形式
<a href="猫猫.zip">下载链接</a>
网页元素链接：在网页中的各种网页元素，如文本、图像、表格、音频、视频等都可以添加超链接
<a href="http://baidu.com/"><img src="猫猫.jpg"/></a>
锚点链接：点链接时跳转到页面中的某一位置
	在链接文本的href属性中，设置属性值为  #名字  的形式，如<a href="#two">第2集</a>
	找到目标位置标签，里面添加一个id属性=刚才的名字，如：<h3 id="two">第二集介绍</h3>
返回顶部可直接添加一个空链接
<a href="#">返回顶部</a>
```

# HTML中的注释和特殊字符

```text
注释
ctrl+/
ctrl+shift+/

特殊字符
 	空格符	&nbsp
<	小于号	&lt
>	大于号	&gt
```

# HTML表格标签

```text
表格的基本语法
<table>
	<tr>
		<td>单元格内的文字</td>
		...
	</tr>
	...
</table>

<table></table>是定义表格的标签
<tr></tr>定义表格中的行，必须嵌套在<table></table>标签中
<td></td>定义表格中的单元格，必须嵌套在<tr></tr>标签中      table date

 <table>
    <tr><th>猫</th>    <th>年龄</th>    <th>狗</th>    <th>年龄</th></tr>
    <tr><td>多多</td>    <td>2岁</td>    <td>胖胖</td>    <td>3岁</td></tr>
    <tr><td>吃不饱</td>    <td>5岁</td>    <td>小白</td>    <td>1岁</td></tr>
    <tr><td>小咪</td>    <td>6岁</td>    <td>小不点</td>    <td>8个月</td></tr>
  </table>


表头单元格标签
<th></th>文字会居中加粗


表格属性
实际开发不常用，后面通过css来设置
属性名		属性值		描述
align		left、center、right	规定表格相对周围元素的对齐方式
border		1或“”		规定表格单元是否有边框，默认“”，表示没有边框
cellpadding	像素值		规定单元边沿与其内容之间的空白，默认1像素
cellspacing	像素值		规定单元格之间的空白，默认2像素
width		像素值或百分比	规定表格的宽度


表格结构标签
将表格分割成表格头部和表格主体两大部分
<thead>表格头部区域        <tbody>表格主体区域


合并单元格
跨行合并用rowspan，并删掉合并的部分单元格
跨列合并用colspan，并删掉合并的那部分单元格
```

# HTML列表标签

```text
无序列表
<ul>中只能放<li>,<li>里面可以放任意元素
<ul>
	<li>列表1<li>
	<li>列表2<li>
	<li>列表3<li>
	...
</ul>


有序列表
<ol>中只能放<li>,<li>里面可以放任意元素，实际开发中用得不是很多
<ol>
	<li>列表1<li>
	<li>列表1<li>
	<li>列表1<li>
	...
<ol>


自定义列表
<dl></dl>里面只能包含<dt></dt>和<dd></dd>
<dt></dt>和<dd></dd>个数没有限制，经常是一个<dt></dt>对应多个<dd></dd>
<dl>
	<dt>名词1</dt>
	<dd>名词1解释1</dd>
	<dd>名词1解释2</dd>
	<dd>名词1解释3</dd>
</dl>
```

# HTML表单标签

```text
表单收集用户信息，常用于注册页面
表单域
<form>标签定义表单域，将用户信息收集并提交给服务器
<form action="url地址" method="提交方式“ name="表单域名称">
    各种表单元素控件
</form>
method用于设置表单数据的提交方式，其取值为get或post


表单控件
<input>标签用于收集用户信息，为单标签，包含一个type属性，根据不同的type属性值指定不同的控件类型
<input type="属性值" />
type属性值     描述
button      定义点击按钮
checkbox    定义复选框
file        定义输入字段和“浏览”按钮，供文件上传
hidden      定义隐藏的输入字段
image       定义图像形式的提交按钮
password    定义密码字段，该字段中的字符被掩码
radio       定义单选按钮
reset       定义重置按钮，重置按钮会清楚表单中的所有数据
submit      定义提交按钮，提交按钮会把表单数据发送到服务器
text        定义单行的输入字段，用户可在其中输入文本，默认20个字符

input其它属性
name        定义input元素的名称
value       规定input元素的值
checked     规定此input元素首次加载时应当被选中
maxlength   规定输入字段中的字符的最大长度
name和value是每个表单元素都有的属性值，主要给后台人员使用，单选按钮和复选框要有相同的name值
//placeholder是占位符，占位文字

<label>标签
<label for="sex">男</label>
<input type="radio name="sex" id="sex"/>
<label>里for属性的值对应id属性里的值


select下拉表单元素
<select>
    <option>选项1</option>
    <option>选项2</option>
    <option>选项3</option>
    ...
    </select>
<select>中至少包含一对<option>
在<option>中定义selected="selected"时，当前项即为默认选中项

textarea文本域元素
用于定义多行文本输入的控件，常见于留言板、评论
<textare rows="3" cols="20">
    文本内容
</textare>
cols设置每行中的字符数，rows设置显示的行数，实际开发中不会使用，都是通过css来改变大小
```

## HTML标签

```html
<!--
!DOCTYPE: 文档类型申明，告诉浏览器使用哪种HTML版本来显示网页
html: 这个页面使用的是最新的 html5 版本来显示
-->
<!DOCTYPE html>
<!-- lang 语言种类，en 定义语言为英语，zh-CN 定义语言为中文 -->
<html lang="en">
<head>
<!-- 
meta 是 HTML 语言 head 区的一个辅助性标签
charset 字符集		UTF-8 万国码
-->
    <meta charset="UTF-8">
    <title>WebStorm的第一个页面</title>
</head>
<body>
    学前端！冲啊！！！
</body>
</html>
```

## HTML常用标签

### 标题标签

```
<h1> - <h6>			一个标题独占一行
```

### 段落和换行标签

```
<p>			定义段落，段落和段落之间保有空隙
<br/>		强制换行，单标签
```

### 文本格式化标签

```
<strong></strong> 或 <b></b>		加粗
<em></em> 或 <i></i>		倾斜
<del></del> 或 <s></s>		删除线
<ins></ins> 或 <u></u>		下划线
```

```
<div></div> 标签用来布局，一行只能放一个 div
<span></span> 标签用来布局，一行可以放多个 span
```

### 图像标签和路径

```html
<!-- 图像标签 <img> 用来定义 HTML 页面中的图像 -->
<img src="猫猫.jpg" alt="有一只猫" title="让你瞅瞅" width="500" height="300" border="15" />
```

```
src 是 <img> 标签的必须属性，它用于指定图像文件的路径和文件名
alt 为替换文本，图像不能显示时出现的文字
title 是提示文本，鼠标放到图片上显示的文字
width 设置图像的宽度
height 设置图像的高度（高和宽一般设置其中一个，等比例缩放）
border 设置图像的边框粗细（一般在 css 设置）
```

```html
<!-- 同一级路径 -->
<img src="猫猫.jbg" />
<!-- 下一级路径 -->
<img src="images/猫猫.jbg" />
<!-- 上一级路径 -->
<img src="../猫猫.jbg" />
<!-- 绝对路径 网盘或完整的网络地址 -->
```

### 超链接标签

```html
<!--
href 用于指定链接目标的 url 地址
target 用于指定链接页面的打开方式，其中 _self 为默认值，_blank 为在新窗口打开
-->
<a href="跳转目标" target="目标窗口的弹出方式">文本或图像</a>
```

### 锚点链接

点链接时跳转到页面中的某一位置

1. 在目标位置标签添加 id 属性
2. 在链接文本的 href 属性，设置属性值为 #id 的形式

## HTML特殊字符

```
 	空格符		&nbsp
<	小于号		&lt
>	大于号		&gt
```

## HTML表格标签

### 表格的基本语法

```
<table>
	<tr>
		<td>单元格内的文字</td>
		. . .
	</tr>
	. . .
</table>
```

```
<table></table> 定义表格的标签
<tr></tr> 定义表格中的行，必须嵌套在 <table></table> 标签中
<td></td> 定义表格中的单元格，必须嵌套在 <tr></tr> 标签中
<th></th> 定义表头单元格标签，文字会居中加粗
```

### 表格结构标签

```
<thead></thead> 表格头部标签		<tbody></tbody> 表格主体区域
```

### 合并单元格

```
跨行合并 rowspan ，并删掉合并的那部分单元格
跨列合并 colspan ，并删掉合并的那部分单元格
```

## HTML列表标签

### 无序列表

```html
<!--<ul></ul> 中只能放 <li></li> , <li></li> 里面可以放任意元素-->
<ul>
  <li>列表1</li>
  <li>列表2</li>
  <li>列表3</li>
  <li>列表4</li>
</ul>
```

### 有序列表

```html
<!--<ol></ol> 中只能放 <li></li> , <li></li> 里面可以放任意元素-->
<ol>
    <li>列表1</li>
    <li>列表2</li>
    <li>列表3</li>
</ol>
```

### 自定义列表

```html
<!--
<dl></dl> 里面只能包含 <dt></dt> 和 <dd></dd>
<dt></dt> 和 <dd></dd> 个数没有限制,一般一个 <dt></dt> 对应多个 <dd></dd>
-->
<dl>
    <dt>名词1</dt>
    <dd>名词1解释1</dd>
    <dd>名词1解释2</dd>
    <dd>名词1解释3</dd>
</dl>
```

## HTML表单标签

### 表单域

```html
<!--
<form></form> 标签定义表单域,将用户信息收集并提交给服务器
method 用于设置表单数据的提交方式,其取值为 get 或 post
-->
<form action="url" method="get/post" name="名称"></form>
```

### 表单控件

####  1.input标签

1. type属性

```
button			定义点击按钮
checkbox		定义复选框
file			定义输入字段和“浏览”按钮，共文件上传
hidden			定义隐藏的输入字段
image			定义图像形式的提交按钮
password		定义密码字段，该字段中的字符被掩码
radio			定义单选按钮
reset			定义重置按钮，重置会清除表单中的所有数据
submit			定义提交按钮，提交按钮会把表单数据发送到服务器
text			定义单行的输入字段，用户可在其中输入文本，默认20个字符
```

2. 其它属性

```
name			定义input元素名称
value			规定input元素的值
checked			规定此input元素首次加载时应当被选中
maxlength		规定输入字段中的字符的最大长度
placeholder		占位符，占位文字
name 和 value 是每个表单元素都有的属性值，主要给后台人员使用，单选按钮和复选框要有相同的name值
```

#### 2.label标签

```html
<!--<label for=""></label> 里的 for 属性的值对应 id 属性里的值-->
<input type="radio" name="sex" id="sexx">
<label for="sexx">男</label>
```

#### 3.select下拉表单元素

```html
<!--
<select></select> 中至少包含一对 <option></option>
在 <option></option> 中定义 selected="selected" 是,当前项即为默认选中项
-->
<select name="" id="">
    <option value="">选项1</option>
    <option value="">选项2</option>
    <option value="">选项3</option>
</select>
```

#### 4.textarea文本域元素

```html
<!--
用于定义多行文本输入的控件,常见于留言板\评论
cols 设置每行中的字符数, rows 设置显示的行数,实际开发中不会使用,都是通过 css来改变大小
-->
<textarea name="" id="" cols="30" rows="10">
    文本内容
</textarea>
```