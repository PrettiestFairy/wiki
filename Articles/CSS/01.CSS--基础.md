# CSS语法规范

```text
CSS规则由两个主要的部分构成：选择器以及一条或多条声明
h1 {color: red; font-size: 12px;}
选择器是用于指定CSS样式的HTML标签，花括号内是对该对象设置的具体样式
属性和属性值以”键值对”的形式存在
属性和属性值之间用英文“:”分开
多对“键值对”之间用英文“;”进行区分
<style></style>渲染
```

# CSS基础选择器

```text
选择器分类
选择器分为基础选择器和复合选择器两个大类
基础选择器是由单个选择器组成
基础选择器包括：标签选择器、类选择器、id选择器和通配符选择器
类选择器在修改样式中用的最多，id选择器一般用于页面唯一性的元素上，经常和JS搭配使用

标签选择器
用HTML标签名作为选择器，按标签名称分类
标签名 {
    属性1： 属性值；
    属性2： 属性值；
    属性3： 属性值；
    ...
}

类选择器
.类名 {
    属性1： 属性值1；
    ...
}
.red{
    color: red;
}
结构需要用class属性来调用class类
<div class='red'>变红色</div>
类选择器使用“.”（英文点好号）进行标识，后面紧跟类名（自定义，我们自己命名的）
可以理解为给这个标签起了一个名字，来表示
长名称或词组可以使用中横线来为选择器命名
使用英文字母来命名
命名要有意义，尽量让别人一眼就知道这个类名的目的

类选择器-多类名
在标签class属性中写多个类名
多个类名中间用空格分开
<div class="red font">瘦到100斤</div>


id选择器
id属性只能在每个HTML文档中出现一次
样式#定义，结构id调用，只能调用一次，别人切勿使用
#id名 {
    属性1： 属性值1；
    ...
}


通配符选择器
* {
    属性1： 属性值1；
    ...
}
```

# CSS文字属性

```text
字体系列
CSS使用font-family属性定义文本的字体系列
p {font-family: "思源宋体";}
/*多单词加引号，单双引号都可*/

font-size属性定义文本的字体大小
body {font-size: 20px;}
/*标题比较特殊，需要单独指定字体大小*/

font-weight属性设置文本的字体粗细
参数：nomal（400）|bold（700）|bolder|lighter|number
p {font-weight: 400;}

font-style属性设置文本的风格
参数：nomal|italic（斜体）
p {font-style: nomal}

**字体复合属性**
不能更改属性顺序，并且各个属性间以空格隔开
必须保留font-size和font-family属性
body {
    font: font-style font-weight font-size/line-height font-family;
}
body {
    italic 700 12px 'Microsoft yahei'
}
```


# CSS文本属性

```text
文本颜色
color属性定义文本的颜色
div {
    color: red;
}

对齐文本
text-align属性用于设置元素内文本内容的水平对齐方式
div {
    text-align: center;
}

装饰文本
text-decoration属性规定添加到文本的修饰,下划线、删除线、上划线等
属性值：none|underline|overline|line-through
div {
    text-decoration: underline;
}

文本缩进
text-indent属性用来指定文本的第一行的缩进
div {
    text-indent: 10px/2em;
}
em是一个相对单位，当前元素的一个文字大小

行间距
line-height属性设置行间的距离
p {
    line-height： 26px；
}
```


# CSS的引入方式

```text
内部样式表
嵌入式引用，写到html页面内部
<style>
    div {
        color: red;
        font-size: 20px;
    }
</style>

行内样式表
行内式引用，在元素标签内部的style属性中设定CSS样式
双引号
<div style="color: red; font-size: 12px;">今天的天气为晴转多云</div>

外部样式表（吐血推荐）
样式单独写到CSS文件中，之后把CSS文件使用link标签引入到HTML页面使用
<link rel="stylesheet" href="css文件路径">
/*rel定义当前文档与被链接文档之间的关系*/
/*在这里需要指定为“stylesheet”，表示被链接的文档是一个样式表文件*/
/*href定义所链接外部样式表文件的url，可以是相对路径，也可以是绝对路径*/
```


# Chrome调试工具

```text
打开调试工具
打开Chrome浏览器，按下F12键或右击页面空白处-检查
使用调试工具
Ctrl+滚轮 放大开发者工具代码大小；Ctrl+0 复原浏览器大小
右边CSS样式可以改动数值，但刷新后不会保存
如果有样式，但样式前面有黄色叹号提示，则样式属性书写错误
```


# Emmet语法

```text
Emmet语法
1.生成标签 直接输入标签名按tab键即可
2.如果想要生成多个相同的标签，加上 * 就可以了，比如 p*3
3.如果有父子级关系的标签，可以用 > ，比如 ul>li 
4.如果有兄弟关系的标签，可以用 + ，比如 div+p
5.如果生成带有类名或者id名字的，直接写 .demo 或者 #two
6.如果生成的div类名是有顺序的，可以用自增符号 $ ,比如 div.demo$*5
7.如果想要在生成的标签内部写内容，可以用 {} 表示，比如 p{今天好热啊$}*5

快速生成CSS样式语法
1.比如 w200 按tab 可以生成 width: 200px;
2.比如 lh26 按tab 可以生成 line-heught: 26px;

快速格式化代码
webstorm的快捷键ctrl+alt+l
```


# CSS的复合选择器

```text
后代选择器（重要）（较多）
元素1 元素2 {样式声明}
ul li {样式声明} /*选择ul里面所有li标签元素*/
ul li a {样式声明} /*ul里li中a标签元素*/
.nav li a {样式声明} /*类选择器里li中a标签元素*/

子选择器（重要）（较少）
元素1>元素2 {样式声明}
div>a {样式声明} /*选择div里面所有最近一级 a 标签元素*/

并集选择器（重要）（较多）
元素1，元素2 {样式声明}
div,
p,
.pig .peiqi {样式声明} /*选择div，p和类选择其中peiqi类里面的内容*/
并集选择器竖着写

链接伪类选择器（较多）
a:link      /*选择所有未被访问的链接*/
a:visited   /*选择所有已被访问的链接*/
a:hover     /*选择鼠标指针位于其上的链接*/
a：active   /*选择活动链接（鼠标按下未弹起的链接）*/
实际开发中按照LVHA的顺序进行书写
a链接在浏览器中具有默认样式，所以实际工作中需要给链接单独指定样式
链接伪标签选择器实际工作开发中的写法：
/* a 是标签选择器 所有的链接*/
a {
    color: gray;
}
/* :hover 是链接伪类选择器 鼠标经过*/
a:hover {
    color: red; /*鼠标经过的时候，由原来的 灰色 变成了 红色*/
}

focus伪类选择器（较少）
用于选取获得焦点的表单元素
input:focus {
    background-color: yellow;
}
```


# CSS的元素显示模式

```text
块元素
<h1>~<h6>|<p>|<div>|<ul>|<ol>|<li>等
特点：
1.独占一行
2.高度、宽度、外边距以及内边距都可以控制
3.宽度默认是容器的100%
4.是一个容器盒子，里面可以放行内或者块级元素
/*文字类标签里边不能再放块级元素*/

行内元素
<a>|<strong>|<b>|<em>|<i>|<del>|<s>|<ins>|<u>|<span>等
特点：
1.相邻行内元素在一行上，一行可以显示多个
2.高、宽直接设置是无效的
3.默认宽度就是它本身内容的宽度
4.行内元素只能容纳文本或其他行内元素
<a>里面可以放块级元素，但是给<a>转换一下块级模块最安全

行内块元素
<img/>|<input/>|<td>
特点：
1.和相邻行内元素在一行上，但它们之间会有空白缝隙，一行可以显示多个
2.默认宽度是它本身内容的宽度
3.高度、行高、外边距以及内边距都可以控制

元素显示模式转换
转换为块元素： display: block;
转换为行内元素： display: inline;
转换为行内块： display: inline-block;
```


# CSS的背景

```text
背景颜色
background-color: transparent（透明）|color（颜色值）
背景图片
background-image: none|url
背景平铺
background-repeat: repeat（平铺）|no-repeat（不平铺）|repeat-x（横向平铺）|repeat-y（纵向平铺）

背景图片位置（非常重要）
background-position: x y;
参数值
length      百分数|由浮点数字和单位标识符组成的长度值（x y有顺序，）
position    top |center|bottom|left|center|right 方位名词（x y无顺序）

背景图像固定（背景附着）
background-attachment: scroll（随对象内容滚动）|fixed（固定）

背景复合写法
没有特定的书写顺序
background:背景颜色 背景图片地址 背景平铺 背景图像滚动 背景图片位置
background: transparent url("../statia/img_src/猫猫.jpg") no-repeat fixed top;

背景色半透明
background: rgba(0,0,0,0.3);
最后一个参数是alpha透明度，取值范围在0~1之间
```


# CSS的三大特性

```text
层叠性
样式冲突，遵循就近原则，只覆盖冲突的样式
继承性
子标签会继承父标签的某些样式，如文字颜色和字号
优先级
选择器相同，则执行层叠行
选择器不同，则根据选择器权重执行
继承<元素选择器<类选择器<id选择器<行内样式style=""<！important
继承的权重是0,0,0,0
元素选择器是0,0,0,1
类选择器是0,0,1,0
id选择器是0,1,0,0
行内样式style是1,0,0,0
!important是无穷大
```


# 盒子模型

```text
Box Model组成：
border边框
content内容（盒子里面的内容）
padding内边距（内容与盒子边框的距离）
margin外边距（盒子与盒子之间的距离）

边框（border）
border：border-width（边框的粗细，单位px）|border-style（边框样式）|border-color（边框颜色）
border-style: dotted（点线）|dashed（虚线）|solid（实线）
/*none（无边框）|hidden（隐藏边框）|double（双线）|groove（根据border-color的值画3D凹槽）|ridge（...菱形边框）|inset（...3D凹边）|outset（...3D凸边）*/
**边框复合写法**
border: 1px solid red;（没有顺序）
边框分开写法
border-top: 1px solid pink;
单元格边框
border-collapse:collapse;   （相邻边框合并在一起）

border会影响盒子实际大小

内边距
padding-left|padding-right|padding-top|padding-bottom
padding:5px;                 （四边）
padding:5px 10px;            （上下5，左右10）
padding:5px 10px 20px;       （上5，左右10，下20）
padding:5px 10px 20px 30px;  （上5，右10，下20，左30）
padding会影响盒子实际大小
若盒子本身没有指定width/height属性，此时padding不会影响盒子大小

外边距
margin-left|margin-right|margin-top|margin-bottom
margin简写方式与padding一致

外边距典型应用
外边距可以让块级盒子水平居中，需满足两个条件：
1.盒子必须指定了宽度width
2.盒子左右的外边距都设置为auto
对于行内元素或行内块元素想要水平居中，可以给其父元素添加text-align:center即可

外边距合并
对于两个嵌套关系的块元素，父元素有上外边距同时子元素也有上外边距，此时父元素会塌陷较大的外边距
1.可以为父元素定义上边框
2.可以为父元素定义上内边距
3.可以为父元素添加 overflow:hidden（最常用）
4.浮动、固定盒子

清除内外边距
网页元素很多都带有默认的内外边距，而且不同浏览器默认的也不一致
清除网页元素内外边距
* {
    padding: 0;     /*清除内边距*/
    margin: 0;      /*清除外边距*/
}
```


# PS基本操作

```text
前端大部分切图工作都是在PS里面完成的
1.文件——打开：打开我们要测量的图片
2.Ctrl+R：可以打开标尺，或者视图——标尺
3.右击标尺，把里面的单位改为像素
4.Ctrl+加号（+）可以放大视图，Ctrl+减号（-）可以缩小试图
5.用选区拖动可以测量大小
6.Ctrl+D可以取消选区，或者在旁边空白处点击一下也可以取消选区
7.用吸管工具进行取色
```


# 圆角边框(重点)

```text
元素外边框圆角
border-radius: length;      （数值或百分比都可以）
后跟两个length，则设置的是两个对角
后跟四个length，则设置的是顺时针四个角
分开写：border-top-left-radius、border-top-right-radius、border-bottom-right-radius、border-bottom-left-radius
```


# 盒子阴影（重点）

```text
box-shadow: h-shadow v-shadow blur spread color inset;
盒子阴影属性：水平阴影的位置|垂直阴影的位置|模糊距离|阴影尺寸|阴影颜色|外部阴影（outset）或内部阴影（inset）
```


# 文字阴影（了解）

```text
text-shahow: h-shadow v-shadouw blur color;
文字阴影属性：水平阴影位置|垂直阴影位置|模糊距离|阴影颜色
```

# CSS浮动(难点)

```text
1.标准流（普通流/文档流）
标准流是最基本的布局方式，就是标签按照规定好默认方式排列
（1）块级元素会独占一行，从上向下顺序排列
（2）行内元素会按照顺序，从左到右顺序排列，碰到父元素边缘则自动换行
```

```text
2.浮动
float属性用于创建浮动框，将其移动到一边，直到左边缘或右边缘触及包含块或另一浮动框的边缘
选择器{float: 属性值;}
属性值：none | left | right
```

## 浮动特性(重难点)
```text
1.浮动元素会脱离标准流（脱标）            浮动的盒子不再保留原先的位置
2.如果多个盒子都设置了浮动，浮动的元素会一行内显示并且元素顶部对齐          盒子之间没有空隙
3.浮动的元素会具有行内块元素的特性          如果盒子没有设置宽度，默认宽度和父级一样宽，但添加浮动后，它的宽度根据内容多少来决定
先设置盒子大小，在设置盒子的位置
1.浮动和标准流的父盒子搭配
2.一个元素浮动了，理论上其余的兄弟元素也要浮动
```
## 清除浮动
```text
选择器{clear: 属性值;}
属性值：left | right | both
实际开发中，基本只用 clear: both;
清除浮动的策略是 闭合浮动

给父级添加overflow属性，将其属性值设置为 hidden | auto | scroll

给父元素添加after伪元素
.clearfix:after {
    content: "";
    display: block;
    height: 0;
    clear: both;
    visibility: hidden;
}
.clearfix {
    *zoom: 1;
}

给父元素添加双伪元素清除浮动
.clearfix:before,.clearfix:after {
    content: "";
    display: table;
}
.clearfix:after {
    clear:both;
}
.clearfix {
    *zoom: 1;
}
```

```text
3.定位=定位模式+边偏移
position属性来设置定位模式
属性值： static | relative | absolute | fixed
        静态定位 | 相对定位 | 绝对定位 | 固定定位
边偏移就是定位的盒子移动到最终位置
属性： top | bottom | left | right
示例：top: 80px        顶端偏移量，定义元素相对于其父元素上边线的距离
```
## 静态定位static（了解）
```text
静态定位是元素默认定位方式，无定位的意思
选择器 {position: static;}
```
## 相对定位relative（重要）
```text
移动位置的时候参照点是自己原来的位置
不脱标，继续保留原来的位置
选择器 {position： relative;
        top: 80px;
        left: 80px;
      }
```
## 绝对定位absolute（重要）
```text
绝对定位是元素在移动位置的时候，是相对于它祖先元素来说的
如果没有祖先元素或者祖先元素没有定位，则以浏览器为准定位
如果祖先元素有定位（相对、绝对、固定定位），则以最近一级的有定位祖先元素为参考点移动位置
脱标，不再占有原来的位置
选择器 {position: absolute;
        top: 80px;
        left: 80px;
        }
```
## 固定定位fixed（重要）
```text
固定定位是元素固定于浏览器可视区的位置。主要场景：可以在浏览器页面滚动时元素的位置不会改变
以浏览器的可视窗口为参照点移动元素
固定定位不占有原先的位置
选择器 {position: fixed;}

固定在版新的右侧
1.让固定定位的盒子left:50%.走到浏览器可视区的一半位置
2.让固定定位的盒子margin-left:版心宽度的一半距离。多走版心宽度的一半位置
就可以让固定定位的盒子贴着版心右侧对齐
```
## 粘性定位sticky（了解）
```text
粘性定位可以被认为是相对定位和固定定位的混合
1.以浏览器的可视窗口为参照点移动元素（固定定位特点）
2.粘性定位占有原先的位置（相对定位特点）
3.必须添加 top | right | bottom | left 其中的一个才有效
选择器 {position: sticky; top: 10px;}
```
## 定位叠放次序 z-index
```text
使用定位布局时，可能会出现盒子重叠的情况，此时，可以使用z-index来控制合资的前后次序（z轴）
选择器 {z-index: 1;}
数值可以是正整数、负整数或0，默认是auto，数值越大，盒子越靠上
如果属性值相同，则后来者居上
只有定位的盒子才有z-index属性
```
## 定位的拓展
```text
1.绝对定位的盒子居中
加了绝对定位的盒子不能通过margin: 0 auto 水平居中，但是可以通过以下计算方法实现水平和垂直居中
（1）left: 50%;   让盒子的左侧移动到父级元素的水平中心位置
（2）margin-left: -100px;     让盒子向左移动自身宽度的一半

2.定位特殊特性
绝对定位和固定定位也和浮动类似
（1）行内元素添加绝对或者固定定位，可以直接设置高度和宽度
（2）块级元素添加绝对或者固定定位，如果不给宽度或者高度，默认大小是内容的大小

3.脱标的盒子不会触发外边距塌陷
浮动元素、绝对定位（固定定位）元素都不会触发外边距合并的问题

4.绝对定位（固定定位）会完全压住盒子
浮动元素只会压住它下面标准流的盒子，但是不会压住下面标准流盒子里面的文字（图片）
绝对定位或固定定位会压住下面标准流所有的内容
浮动产生的目的最初就是为了做文字环绕结果的，文字会围绕浮动元素
```


# PS切图（必备技能）

```text
jpg图像格式：产品类的图片经常用jpg格式
gif图像格式：经常用于一些图片小动画效果
png图像格式：想要切成背景透明的图片，就用png格式
PSD图像格式：Photoshop专用格式，前端人员可以直接从上面复制文字，测量大小
```
## 图层切图
```text
右击图层----快速导出为PNG
图层菜单  shift相加----ctrl+E  合并图层
右击----快速导出为PNG
```
## 切片切图
```text
利用切片工具手动划出
文件菜单----导出----存储为web设备所用格式----选择我们要的图片格式----存储
```
## PS插件切图
```text
Cutterman插件
```


# 元素的显示与隐藏

```text
本质：让一个元素在页面中隐藏或者显示出来
1. display  显示隐藏
display: none;       隐藏对象
display: block;     除了转换为块级元素之外，同时还有显示元素的意思
display隐藏元素后，不在占有原来的位置

2.visibility    显示隐藏
visibility: inherit | visible | hidden | collapse
inherit：继承上一个父对象的可见性
visible：对象可视
hidden：对象隐藏
collapse：主要用来隐藏表格的行或列，隐藏的行或列能够被其他内容使用
visibility隐藏元素后，元素继续占有原来的位置

3.overflow  溢出显示隐藏
overflow: visible | auto | hidden | scroll
visible：不剪切内容也不添加滚动条
hidden：不显示超出对象尺寸的内容
scroll：不管内容超出否，总显示滚动条
auto：超出自动显示滚动条，不超出不显示滚动条
如果是有定位的盒子，慎用 overflow: hidden 因为它会隐藏多余的部分
```


# 精灵图

```text
精灵技术目的：为了有效减少服务器接收和发送请求的次数，提高页面的加载速度
```


# 字体图标

```text
P255
```


# CSS三角

```text
div {
    width: 0;
    height: 0;
    line-height: 0;
    font-size: 0;
    border: 50px solid transparent;
    border-top-color: pink;
}
```


# CSS用户界面样式

```text
1.鼠标样式cursor
li {cursor: pointer;}
属性值     default | pointer | move | text | not-allowed
            默认   |   小手  |  移动 |  文本 |  禁止
2.轮廓线outline
给表单添加 outline: 0; 或者 outline: none; 样式之后，就可以去掉默认的蓝色边框
```


# vertical-align属性的应用

```text
CSS的vertical-align属性使用场景:经常用于设置图片或者表单(行内块元素)和文字垂直对齐
vertical-align: baseline | top | middle | bottom

解决图片底侧空白缝隙
1.使用 vertical-align: top | middle | bottom 都可（提倡使用）
2.把图片转换为块级元素 display: block;
```


# 溢出文字省略号显示

```text
1.单行文本溢出显示省略号
/*（1）先强制一行内显示文本*/
white-space: nowrap;    (默认 normal 自动换行)
/*（2）超出的部分隐藏*/
overflow: hidden;
/*（3）文字用省略号替代超出的部分*/
text-overflow: ellipsis;
```


# 常见布局技巧

```text
1.margin负值的运用
盒子设置浮动后，让每个盒子 margin 往左侧移动 -1px 正好压住相邻盒子边框
鼠标经过某个盒子的时候，提高当前盒子的层级即可     如果没有定位，则 position:relative;  如果有定位，则加 z-index

2.文字围绕浮动元素
float 文字围绕

3.行内块的巧妙运用

4.css三角强化

```

# CSS语法规范

```
CSS规则由两个重要的部分构成：选择器以及一条或多条声明
```

# CSS字体属性

```
font-family 属性定义文本的字体属性
font-size 属性定义文本的字体大小
font-weight 设置文本的字体粗细		nomal(400) | bold(700) | bolder | lighter | number
font-style 设置文本的风格		nomal | italic(斜体)
```

```html
<!-- 
文字复合属性  不能更改属性顺序，属性之间以空格隔开 
必须保留 font-size 和 font-family 属性
-->
body {
	font: font-style font-weight font-size/font-height font-family;
}
body {
	italic 700 12px 'Microsoft yahei'
}
```

# CSS文本属性

```
color 属性定义文本的颜色
text-align 设置元素内文本内容的水平对齐方式
text-decoration 规定添加到文本的修饰  none | underline | overline | line-through(删除线)
text-indent 属性用来指定文本的第一行的缩进		10px/2em
line-height 属性设置行间距
```

# CSS引入方式

```
内部样式表		<style></style>
行内样式表		<div style="color: red; font-size: 12px;">xxx</div>
外部样式表		<link rel="stylesheet" href="css文件路径">
		rel定义当前文档与被链接文档之间的关系， stylesheet表示被链接的文档是一个样式表文件
		href定义所链接外部样式表文件的url，可以是相对路径，也可以是绝对路径
```

# CSS复合选择器

## 后代选择器（较多）

```
元素1 元素2 {样式声明}
.nav li a {样式声明}	类选择器里li中a标签元素
```

## 子选择器（较少）

```
元素1>元素2 {样式声明}
div>a {样式声明}	选择 div 里面所有最近一级 a 标签元素
```

## 并集选择器（较多）

```
元素1，元素2 {样式声明}
div,
p,
.pig .peiqi {样式声明}		选择 div，p 和类选择器  .pig 中 .peiqi 类里面的内容
```

## 链接伪类选择器（较多）

```html
a:link		选择所有未被访问的链接
a:visited	选择所有已被访问的链接
a:hover		选择鼠标指针位于其上的链接
a:active	选择活动链接（鼠标按下未弹起的链接）
实际开发中按照 LVHA 的顺序进行书写
<!-- 鼠标经过的时候，链接由原来的灰色变成了红色 -->
a {
	color: gray;
}
a:hover {
	color: red;		
}
```

## focus伪类选择器

```
用于选取获得焦点的表单元素
input: focus {
	bacjground-color: yellow;
}
```

# CSS元素显示模式

## 块元素

```
<h1>~<h6>|<p>|<div>|<ul>|<ol>|<li>
特点：
 1.独占一行
 2.高度、宽度、外边距以及内边距都可以控制
 3.宽度默认是容器的100%
 4.是一个容器盒子，里面可以放行内元素或者块元素（文字类标签里面不能再放块级元素）
```

## 行内元素

```
<a>|<strong>|<b>|<em>|<i>|<del>|<s>|<ins>|<u>|<span>
特点：
 1.相邻行内元素在一行上，一行可以显示多个
 2.高、宽直接设置是无效的
 3.默认宽度就是它本身内容的宽度
 4.行内元素只能容纳文本或其他行内元素
 <a>里面可以放块级元素，但是给<a>转换一下块级模块最安全
```

## 行内块元素

```
<img>|<input>|<td>
特点：
 1.和相邻行内元素在一行上，但它们之间会有空白缝隙，一行可以显示多个
 2.默认宽度是它本身内容的宽度
 3.高度、行高、外边距以及内边距都可以控制
```

## 元素显示模块转换

```
转换为块元素：display: block;
转换为行内元素：display: inline;
转换为行内块元素：display: inline-block;
```

# CSS的背景

## 背景颜色

```
background-color: transparent(透明)|color(颜色值)
```

## 背景图片

```
background-image: none|url
```

## 背景平铺

```
background-repeat: repeat(平铺)|no-repeat(不平铺)|repeat-x(横向平铺)|repeat-y（纵向平铺）
```

## 背景图片位置（重要）

```
background-position: x y;
参数值：
 length 百分数|由浮点数字和单位标识符组成的长度值（x y有顺序）
 position  top|center|bottom|left|center|right 方位名词（x y无顺序）
```

## 背景图像固定（背景附着）

```
background-attachment: scroll(随对象内容滚动)|fixed(固定)
```

## 背景复合写法(没有特定书写顺序)

```
background: 背景颜色  背景图片地址  背景平铺  背景图像滚动  背景图片位置
background: transparent url("../static/img_src/猫猫.jpg") no-repeat fixed top;
```

## 背景色半透明

```
background: rgba(0, 0, 0, 0.3);
```

## CSS三大特性

```
层叠性  继承性  优先级
```

### 选择器权重

```
继承<元素选择器<类选择器<id选择器<行内样式style=""<!important
继承的权重： 0, 0, 0, 0
元素选择器： 0, 0, 0, 1
类选择器是： 0, 0, 1, 0
id选择器是： 0, 1, 0, 0
行内样式style: 1, 0, 0, 0
!important: 无穷大
```

### 盒子模型

```
Box Model组成：
border边框
content内容（盒子里面的内容）
padding内边距（内容与盒子边框的距离）
margin外边距（盒子与盒子之间的距离）
```

#### 边框border

```
border: border-width(边框的粗细，单位px)|border-style(边框样式)|border-color|
border-style: dotted(点线)|dashed(虚线)|solid(实线)
none(无边框)|hidden(隐藏边框)|double(双线)
```

#### 边框复合写法

```
border: 1px solid red;(没有顺序)
```

#### 边框分开写法

```
border-top: 1px solid red;
```

#### 单元格边框

```
border-collapse: collapse;(相邻边框合并在一起)
```

#### 内外边距

```
padding: 5px(四边)	padding: 5px 10px(上下，左右)	padding: 5px 10px 20px(上，左右，下)
padding: 5px 10px 15px 20px(上，右，下，左)
margin 写法与 padding 一致
```

### 圆角边框（重点）

```
border-radius: length;(数值或百分比都可以)
后跟两个 length，则设置的是两个对角
后跟四个 length，则设置的是顺时针四个角
分开写：border-top-left-radius\border-top-right-radius\border-bottom-right-radius\border-bottom-left-radius
```

### 盒子阴影（重点）

```
box-shadow: h-shadow v-shadow blur spread color inset;
盒子阴影属性：水平阴影位置|垂直阴影位置|模糊距离|阴影尺寸|阴影颜色|外部阴影（outset）或内部阴影（inset）
```

### 文字阴影（了解）

```
text-shadow: h-shadow v-shadow blur color;
文字阴影属性：水平阴影位置|垂直阴影位置|模糊距离|阴影颜色
```

### CSS浮动（难点）

#### 1.标准流（普通流/文档流）

```
(1)块级元素会独占一行，从上向下顺序排列
(2)行内元素会按照顺序，从左到右顺序排列，碰到父元素边缘则自动换行
```

#### 2.浮动

```
选择器{float: 属性值;}
属性值: none | left | right
```

##### 浮动特性（重难点）

```
1.浮动元素会脱离标准（脱标），浮动的盒子不再保留原先的位置
2.如果多个盒子都设置了浮动，浮动的元素会一行内显示并且元素顶部对齐，盒子之间没有空隙
3.浮动的元素会具有行内块元素的特性，如果盒子没有设置宽度，默认宽度和父级一样，但添加浮动后，它的宽度根据内容多少来决定
4.浮动和标准流的父盒子搭配
5.一个元素浮动了，理论上其余的兄弟元素也要浮动
```

##### 清除浮动

##### clear闭合浮动

```
选择器{clear: 属性值;}
属性值： left | right | both(最常用)
```

##### 给父级添加overflow属性

```
将其属性值设置为 hidden | auto | scroll
```

##### 给父元素添加after属性

```html
.clearfix:after {
    content: "";
    display: block;
    height: 0;
    clear: both;
    visibility: hidden;
}
.clearfix {
    *zoom: 1;
}
```

##### 给父元素添加双伪元素清除浮动

```html
.clearfix:before,.clearfix:after {
    content: "";
    display: table;
}
.clearfix:after {
    clear:both;
}
.clearfix {
    *zoom: 1;
}
```

#### 3.定位=定位模式+边偏移

```
position: static | relative | absolute | fixed
		   静态定位 | 相对定位 | 绝对定位 | 固定定位
边偏移属性: top | bottom | left | right
示例： 
选择器 {position: relative;
		top: 80px;
		left: 20px;
		}
```

##### 相对定位relative

```
移动位置的时候参照点是自己原来的位置，不脱标，继续保留原来的位置
```

##### 绝对定位absolute

```
绝对定位是元素在移动位置的时候，相对于它祖先元素来说的
如果没有祖先元素或者祖先元素没有定位，则以浏览器为准；如果祖先元素有定位（相对、绝对、固定定位），则以最近一级的有定位祖先元素为参考点移动位置，且脱标，不再占有原来的位置
```

##### 固定定位fixed

```
固定定位是元素固定于浏览器可视区的位置（主要场景：可以在浏览器页面滚动时元素的位置不会改变），且固定定位不占有原先的位置
固定在版型的右侧：
1.让固定定位的盒子 left: 50% 走到浏览器可视区的一半位置
2.让固定定位的盒子 margin-left: 版型宽度的一半距离 
```

##### 定位叠放次序z-index

```
选择器 {z-index: 1;}
数值可以是正整数、负整数或0，默认是auto，数值越大，盒子越靠上；如果属性值相同，则后来者居上；只有定位的盒子才有 z-index 属性
```

##### 定位的拓展

###### 绝对定位的盒子居中

```
加了绝对定位的盒子不能通过 margin: 0 auto 水平居中，但是可以通过以下计算方法实现水平和垂直居中
（1）left: 50%;	让盒子的左侧移动到父级元素的水平中心位置
（2）margin-left: -100px;	   让盒子向左移动自身宽度的一半
```

###### 定位特殊特性

```
绝对定位和固定定位也和浮动类似
（1）行内元素添加绝对或者固定定位，可以直接设置高度和宽度
（2）块级元素添加绝对或者固定定位，如果不给宽度或者高度，默认大小是内容的大小
```

###### 脱标的盒子不会触发外边距塌陷

```
浮动元素、绝对定位（固定定位）元素都不会触发外边距合并的问题
```

###### 绝对定位（固定定位）会完全压住盒子

```
浮动元素只会压住它下面标准流的盒子，但是不会压住下面标准流盒子里面的文字（图片）
绝对定位或固定定位会压住下面标准流所有的内容
浮动产生的目的最初就是为了做文字环绕结果的，文字会围绕浮动元素
```

### 元素的显示与隐藏

#### display  显示隐藏

```
display: none;		隐藏对象
display: block;		除了转换为块级元素之外，同时还有显示元素的意思
diaplay隐藏元素后，不再占有原来的位置
```

#### visibility  显示隐藏

```
visibility: inherit | visible | hidden | collapse
inhreit: 继承上一个父对象的可见性
visible: 对象可视
hidden: 对象隐藏
collapse: 主要用来隐藏表格的行或列，隐藏的行或列能够被其它内容使用
visibility 隐藏元素后，元素继续占有原来的位置
```

#### overflow  溢出显示隐藏

```
overflow: visible | auto | hidden | scroll
visible: 不剪切内容也不添加滚动条
hidden: 不显示超出对象尺寸的内容
scroll: 不管内容是否超出，总显示滚动条
auto: 超出自动显示滚动条，不超出不显示滚动条
如果是有定位的盒子，慎用 overflow: hidden 因为它会隐藏多余的部分
```

### CSS三角

```html
<style>
        .box1 {
            width: 0;
            height: 0;
            border-top: 50px solid pink;
            border-right: 50px solid red;
            border-bottom: 50px solid blue;
            border-left: 50px solid green;
        }
        .box2 {
            width: 0;
            height: 0;
            border: 50px solid transparent;
            border-top-color: pink;
            margin: 0 auto;
        }
        .jingdong {
            position: relative;
            width: 120px;
            height: 249px;
            background-color: skyblue;
        }
        .jingdong span {
            position: absolute;
            right: 15px;
            top: -10px;
            width: 0;
            height: 0;
            /*为了照顾兼容性*/
            line-height: 0;
            font-size: 0;
            border: 5px solid transparent;
            border-bottom-color: skyblue;
        }
</style>
```

### 用户界面样式

##### 鼠标样式cursor

```
li{cursor: pointer;}
属性值	default | pointer | move | text | not-allowed
		默认   |  小手   | 移动 |  文本  | 禁止
```

##### 轮廓线outline

```
给表单添加 outline: 0; 或者 outline: none; 样式之后，就可以去掉默认的蓝色边框
```

### vertical-align 属性的应用

```
经常用于设置图片或者表单（行内块元素）和文字垂直对齐
vertical-align: baseline | top | middle | bottom
解决图片底测空白缝隙：
1.使用 vertical-align: top | middle | bottom 都可（提倡使用）
2.把图片转换为块级元素 display: block;
```

### 溢出文字省略号显示

```html
<style>
        div {
            width: 100px;
            height: 40px;
            background-color: pink;
            margin: 100px auto;
            /*white-space: normal; 的意思是 文字自动换行*/
            /*white-space: normal;*/
            /*1.white-space: nowrap; 的意思是 文字强制一行显示*/
            white-space: nowrap;
            /*2.溢出的部分隐藏起来*/
            overflow: hidden;
            /*3.文字用省略号替代超出的部分  ellipsis 省略号*/
            text-overflow: ellipsis;
        }
</style>
```


## 去掉li前面的 项目符号（小圆点）

```html
语法：
list-style: none;
```

## 背景组合写法

```text
background: rgba(0,0,0,.4) url(../static/img_src/猫猫.jpg) no-repeat center;
```
## CSS样式外部引入

```text
样式单独写到CSS文件中，之后把CSS文件使用link标签引入到HTML页面使用
<link rel="stylesheet" href="css文件路径">
```


## 清除浮动的方法

```text
1.额外标签法 也称 隔墙法
（1）清除浮动的本质是清除浮动元素脱离标准流造成的影响
（2）清除浮动的策略是闭合浮动，只让浮动在父盒子内部影响，不影响父盒子外面的其他盒子
（3）额外标签法也叫隔墙法，就是在最后一个浮动的子元素后面添加一个额外标签，添加浮动样式
实际工作中可能会遇到，但不常用

2.父级添加overflow属性
给父级添加overflow属性，将其属性值设置为 hidden | auto | scroll
overflow: hidden;

3.父级添加after伪元素
.clearfix:after {
    content: "";
    display: block;
    height: 0;
    clear: both;
    visibility: hidden;
}
.clearfix {
    *zoom: 1;
}

4.父级添加双伪元素
.clearfix:before,.clearfix:after {
    content: "";
    display: table;
}
.clearfix:after {
    clear:both;
}
.clearfix {
    *zoom: 1;
}
```


## 案例准备工作
```text
1.创建study目录文件夹（用于存放这个页面的相关内容）
2.软件打开这个目录文件夹
3.study目录内新建images文件夹，用于保存图片
4.新建首页文件index.html（网站首页统一规定为index.html）
5.新建style.css样式文件（外连接样式表）
6.将样式引入到HTML页面文件中
7.样式表写入清除内外边距的样式，来检测样式表是否引入成功
```
### CSS属性书写顺序（重点）
```text
1.布局定位属性： display | position | float | clear |visibility | overflow
2.自身属性： width | height | margin | padding | border | background
3.文本属性： color | font | text-decoration | text-align | vertical-align | white-space | break-word
4.其他属性（CSS3）： content | cursor | border-radius | box-shadow | text-shadow | background:linear-gradient
```


## 定位
```text
口诀：子绝父相     子级是绝对定位的话，父级要用相对定位
（1）子级绝对定位，不会占有位置，可以放到父盒子里面的任何一个地方，不会影响其它的兄弟盒子
（2）父盒子需要加定位限制子盒子在父盒子内显示
因为父级需要占有位置，因此是相对定位，子盒子不需要占有位置，则是绝对定位
如果父元素不需要占位置，子绝父绝也可以
```


# PS切图（必备技能）

```text
jpg图像格式：产品类的图片经常用jpg格式
gif图像格式：经常用于一些图片小动画效果
png图像格式：想要切成背景透明的图片，就用png格式
PSD图像格式：Photoshop专用格式，前端人员可以直接从上面复制文字，测量大小
```
## 图层切图
```text
右击图层----快速导出为PNG
图层菜单  shift相加----ctrl+E  合并图层
右击----快速导出为PNG
```
## 切片切图
```text
利用切片工具手动划出
文件菜单----导出----存储为web设备所用格式----选择我们要的图片格式----存储
```
## PS插件切图
```text
Cutterman插件
```