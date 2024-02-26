# 初识JavaScript

```text
1.行内式JS
<input type="button" value="点我试试" onclick="alert('Hello')"/>
2.内嵌JS
<script>
    alert('Hello');
</script>
3.外部JS文件
<script src="my.js"></script>
```

```text
JS输入输出语句
alert(msg)      浏览器弹出警示框
console.log(msg)    浏览器控制台打印输出信息
prompt(info)    浏览器弹出输入框，用户可以输入
```


# 变量

```text
1.声明变量
var age;//声明一个名称为age的变量
2.变量的初始化
var age = 18;//声明变量同时赋值为18
3.声明变量的特殊情况
4.变量的命名规范
```


# 数据类型

```text
JS 的变量数据类型是只有程序在运行过程中，根据等号右边的值来确定的
JS 拥有动态类型，同时也意味着相同的变量可用作不同的类型
JS把数据类型分为两类
（1）简单数据类型（Number | Boolean | String | Undefined | Null)
八进制   var num1 = 010;       //数字前面加0
十六进制  var num2 = 0x9;       //数字前面加0x

字符串类型string     推荐使用单引号     嵌套时  内双外单  或  外双内单
字符串转义字符  都是用\开头  但是这些转义字符要写到引号里面
字符串长度  length
var str = 'my name is Lean'
console.log(str.length);  //显示15
字符串拼接   字符串 + 任何类型 = 拼接之后的字符串       数值相加，字符相连
console.log('我永远' + 18);  //我永远18
            var age = 18;
            console.log('我永远' + age + '岁');  //我永远18岁

布尔型boolean      true 和 false

如果一个变量未赋值  就是undefined 未定义数据类型
null  空值

（2）复杂数据类型
```

## 获取变量数据类型

```text
1.获取检测变量的数据类型
typeof 可用来获取检测变量的数据类型
var num = 10;
console.log(typoef num);  //number
2.字面量
数字字面量：8,9,10
字符串字面量：'段憨憨是猪','晒伤了‘
布尔字面量：true,false
```

## 数据类型转换

```text
使用表单、prompt获取过来的数据默认是字符串类型的，此时就不能直接简单的进行加法运算，而需要转换变量的数据类型，就是把一种数据类型的变量转换成另外一种数据类型。
1.转换为字符串     加号拼接字符串    num + ''
2.转换为数字型（重点）        
parseInt(string)函数      将string类型转成整数数值型    parseInt('78')
parseFloat(string)函数    将string类型转成浮点数数值型   parseFloat('78.21')
Number()强制转换函数        将string类型转换为数值型      Number('12')
js隐式转换(- * /)          利用算术运算隐式转换为数值型    '12' - 0
3.转换为布尔型
Boolean()函数             其它类型转成布尔值             Boolean('true');
代表空、否定的值会被转换为false
```


# JavaScript运算符


```text
1.算术运算符（+  -  *  /  %）
2.递增和递减运算符（++  --）
    （1）前置递增运算符      ++num   num = num + 1       先加1后反值
    （2）后置递增运算符      num++   num = num + 1       先反值后加1
3.比较运算符（>  <  >=  <=  ==  !=  ===  !==）
    判等号默认转型为数字类型        全等要求值和数据类型都一致
4.逻辑运算符（&&  ||  !）
短路运算（逻辑中断）console.log(123 && 456);  //456           如果第一个表达式的值为真，则返回表达式2
                console.log(0 && 456);  //0                如果第一个表达式的值为假，则返回表达式1
                console.log(123 || 456);  //123             如果第一个表达式为真，则返回表达式1
                console.log(0 || 456);  //456               如果第一个表达式为假，则返回表达式2
5.赋值运算符(=  +=  -=  *=  /=  %=)
    num = num + 2;  //num += 2
6.运算符优先级
```


# 流程控制

```text
1.顺序流程控制        按照代码的先后顺序，依次执行
2.分支流程控制        if else语句       if else if语句
三元表达式       条件表达式 ? 表达式1 : 表达式2;
如果条件表达式结果为真 则 返回表达式1的值 如果条件表达式结果为假 则返回表达式2的值
var num = 10;
var result = num > 5 ? '是的' : '不是的';
console.log(result);
    switch语句也是多分支语句
    switch(表达式) {
        case value1:
            执行语句1;
            break;
        case value2:
            执行语句2;
            break;
        ...
        default:
            执行最后的语句;
    }
3.循环流程控制        for循环       while循环        do...while循环
        for(初始化变量; 条件表达式; 操作表达式) {
            //循环体
        }
                for (外层的初始化变量;外层的条件表达式;外层的操作表达式) {
                    for (里层的初始化变量;里层的条件表达式;里层的操作表达式) {
                    // 执行语句;
                    }
                }
    while (条件表达式) {
    //  循环体代码;
    }
                do {
                    //  循环体
                } while (条件表达式)
4.continue  break
continue关键字跳出本次循环，继续下一次循环
break关键字跳出整个循环，结束循环
```


# 数组

```text
1.利用new创建数组
var 数组名 = new Array();
var arr =  new Array();  //  创建一个新的空数组
2.利用数组字面量创建数组
//  1.使用数组字面量方式创建空的数组
var 数组名 = [];
//  2.使用数组字面量方式创建带初始值的数组
var 数组名 = ['小白','小黑','大黄','瑞奇'];
3.遍历数组：把数组的元素从头到尾访问一次     用循环
4.数组的长度
数组名.length
5.新增数组元素
修改length长度
修改索引号
```


# JS函数

```text
函数就是封装了一段可以被重复执行调用的代码块      目的就是可以让大量代码重复使用
1.声明函数
            function 函数名() {            （1）function 声明函数的关键字  全部小写
                //  函数体                 （2）函数是做某件事情，函数名一般是动词
            }                             （3）函数不调用自己不执行
2.调用函数
            //  函数名();                  （1）调用函数的时候不要忘记加小括号
3.形参和实参
    function 函数名(形参1,形参2...) {          //  形式上的参数
        //  函数体
    }
    函数名(实参1,实参2...);                    //  实际的参数
4.return 语句         不仅可以退出循环，还能够返回return语句中的值，同时还可以结束当前函数体内的代码
            function 函数名() {
                return 需要返回的结果;
            }
            函数名();
5.arguments的使用
arguments对象存储了传递的所有实参
6.函数的相互调用       27-函数综合案例
7.函数的2中声明方式
（1）利用函数关键字自定义函数（命明函数）
        function fn() {
        
        }
        fn();
（2）函数表达式（匿名函数）
                var 变量名 = function() {
                
                };
                变量名();
```


# JS作用域

```text
作用域：代码名字（变量）在某个范围内起作用和效果，目的是为了提高程序的可靠性，减少命名冲突
1.全局作用域：整个script标签    或者是一个单独的JS文件
2.局部作用域：在函数内部就是局部作用域  这个代码的名字只在函数内部起作用
```


# 预解析

```text
案例
 f1();
    console.log(c);
    console.log(b);
    console.log(a);
    
    function f1() {
        var a = b = c = 9;
        console.log(c);
        console.log(b);
        console.log(a);
    }
    
                //  相当于
               /* function f1() {
                    var a;
                    a = b = c = 9;
                    // var a = b = c = 9;
                    //相当于 var a = 9; b = 9; c = 9;  b 和 c 直接赋值 没有var声明，相当于全局变量
                    console.log(c);     //  9
                    console.log(b);     //  9
                    console.log(a);     //  9
                }
                f1();
                console.log(c);     //  9
                console.log(b);     //  9
                console.log(a);     // 报错*/
```


# JS对象

```text
1.利用 字面量 创建对象            var obj = {};
2.利用 new Object 创建对象            var obj = new Object();
3.利用 构造函数 创建对象
构造函数就是把对象里面一些相同的属性和方法抽象出来封装到函数里面
            function  构造函数名() {
                    this.属性 = 值;
                    this.方法 = function () {}
                }
                new 构造函数名();
 4.new 关键字执行过程
 //1.new 构造函数可以在内存中创建一个空的对象
 //2.this 就会指向刚才创建的空对象
 //3.执行构造函数里面的代码 给这个空对象添加属性和方法
 //4.返回这个对象
 5.遍历对象属性
 for...in语句 用于对数组或者对象的属性进行循环操作
         for(变量 in 对象) {}
```


# 内置对象

```text
1.Math对象        console.log(Math.max(1,22,42)); // 42
Math.PI          //圆周率
Math.floor()        //向下取整
Math.ceil()         //向上取整
Math.round()        //四舍五入  就近取整
Math.abs()          //绝对值
Math.max() / Math.min()         //求最大值和最小值
随机数方法       Math.random()       返回一个随机的小数       0 =< x < 1
2.日期对象
        var now = new Date();
        console.log(now);
getFullYear()       //获取当年
getMonth()          //获取当月（0-11）
getDate()           //获取当天日期
getDay()            //获取星期几（周日0 到周六6）
getHours()          //获取当前小时
getMinutes()        //获取当前分钟
getSeconds()        //获取当前秒
获得Date总的毫秒数     通过 valueOf() getTime()
3.数组对象
    创建数组对象  var arr = [];    或   var arr = new Array();
    检测是否为数组     arr instanceof Array        或       Array.isArray(参数值)
                添加删除数组元素的方法
                push(参数1...)        末尾添加一个或多个元素，注意修改原数组     并返回新的长度
                pop()            删除数组最后一个元素，把数组长度减1 无参数、修改原数组       返回它删除的元素的值
                unshift(参数1...)     向数组的开头添加一个或更多元素，注意修改原数组     并返回新的长度
                shift()         删除数组的第一个元素，数组长度减1 无参数、修改原数组     并返回第一个元素的值
    翻转数组    reverse()       arr.reverse();
    冒泡排序    sort()          arr.sort(function(a,b) {
                                    return a - b;
                                    });
              返回数组索引号   indexOf()           lastIndexOf()
                        数组转换为字符串    toString()      把数组转换成字符串，逗号分隔每一项
                                         join('分隔符')    方法用于把数组中的所有元素转换为一个字符串
concat()    连接两个或多个数组 不影响原数组        返回一个新的数组
slice()     数组截取slice(begin,end)            返回被截取项目的新数组
splice()    数组删除splice(第几个开始，要删除个数)     返回被删除项目的新数组，这个会影响原数组
4.字符串对象
根据字符返回位置    indexOf('要查找的字符',开始的位置)         lastindexOf('要查找的字符',开始的位置)
        （重点）根据位置返回字符    charAt(index)           返回指定位置的字符（index索引号）     str.charAt(0)
                                charCodeAt(index)       获取指定位置字符的ASCII码             str.charCodeAt(0)
                                str[index]              获取指定位置处字符                   和charAt()等效
字符串操作方法（重点）     concat(str1,str2,str3...)   concat()方法用于连接两个或多个字符串 等效于+，+更常用
                        substr(start,length)        从start位置开始（索引号），length取的个数
                        slice(start,end)            从start位置开始，截取到end位置，end取不到
                        substring(start,end)        与slice基本相同，但是不接受负值
```

# 简单数据类型和复杂数据类型

```text
简单数据类型 是存放在栈里面 里面直接开辟一个空间存放的是值
复杂数据类型 首先在栈里面存放地址 十六进制表示 然后这个地址指向堆里面的数据
```

### 字符串转义符

```text
\n      换行符
\\      斜杠\
\'      '单引号
\"      "双引号
\t      tab缩进
\b      空格
```


### 三元表达式

```text
三元表达式       条件表达式 ? 表达式1 : 表达式2;
如果条件表达式结果为真 则 返回表达式1的值 如果条件表达式结果为假 则返回表达式2的值
var num = 10;
var result = num > 5 ? '是的' : '不是的';
console.log(result);
```


### switch表达式

```text
switch语句也是多分支语句
    switch(表达式) {
        case value1:
            执行语句1;
            break;
        case value2:
            执行语句2;
            break;
        ...
        default:
            执行最后的语句;
    }
```


### for循环

```text
        for(初始化变量; 条件表达式; 操作表达式) {
            //循环体
        }
1.初始化变量  就是用var 声明的一个普通变量  通常用于作为计数器使用
2.条件表达式  就是用来决定每一次循环是否继续执行  就是终止的条件
3.操作表达式  是每次循环最后执行的代码  经常用于计数器变量的更新（递增或递减）
```


## 断点调试

```text
F12————源代码————单击设置断点————刷新————F11下一步————再次单击取消断点————再次刷新
watch查看参数
```


##  JS函数

```text
函数就是封装了一段可以被重复执行调用的代码块      目的就是可以让大量代码重复使用
1.声明函数
            function 函数名() {            （1）function 声明函数的关键字  全部小写
                //  函数体                 （2）函数是做某件事情，函数名一般是动词
            }                             （3）函数不调用自己不执行
2.调用函数
            //  函数名();                  （1）调用函数的时候不要忘记加小括号
            3.形参和实参
    function 函数名(形参1,形参2...) {          //  形式上的参数
        //  函数体
    }
    函数名(实参1,实参2...);                    //  实际的参数
4.return 语句         不仅可以退出循环，还能够返回return语句中的值，同时还可以结束当前函数体内的代码
            function 函数名() {
                return 需要返回的结果;
            }
            函数名();
5.arguments的使用
arguments对象存储了传递的所有实参
6.函数的相互调用       27-函数综合案例
7.函数的2中声明方式
（1）利用函数关键字自定义函数（命明函数）
        function fn() {
        
        }
        fn();
（2）函数表达式（匿名函数）
                var 变量名 = function() {
                
                };
                变量名();
```


### 内置对象

```text
1.Math对象        
 //  利用对象封装自己的数学对象  里面有 PI 最大值 和 最小值
    var myMath = {
        PI: 3.1415925,
        max: function () {
            var max = arguments[0];
            for (var i = 1; i < arguments; i++) {
                if (arguments[i] > max) {
                    max = arguments[i];
                }
            }
            return max;
        },
        min: function () {
            var min = arguments[0];
            for (var i = 1; i < arguments; i++) {
                if (arguments[i] < min) {
                    min = arguments[i];
                }
            }
            return min;
        }
    }
    console.log(myMath.PI);
    console.log(myMath.max(1,5,39));
    console.log(myMath.min(-10,2,28));

// 随机数方法      random()    返回一个随机的小数   0 =< x <1
   console.log(Math.random());
// 若想要得到两个数之间的随机整数 并且 包含这两个整数
// Math.floor(Math.random() * (max - min + 1)) + min;
2.日期对象
        var now = new Date();
        console.log(now);
getFullYear()       //获取当年
getMonth()          //获取当月（0-11）
getDate()           //获取当天日期
getDay()            //获取星期几（周日0 到周六6）
getHours()          //获取当前小时
getMinutes()        //获取当前分钟
getSeconds()        //获取当前秒
        //获得Date总的毫秒数  不是当前时间的毫秒数，而是距离1970年1月1日的
        //1.通过 valueOf() getTime()
        var date = new Date();
        console.log(date.valueOf());
        console.log(date.getTime());        //得到距离1970年1月1日的总毫秒数
        //2.简单的写法（最常用的写法）
        var date3 = +new Date(); // +new Date()  返回的就是总的毫秒数
        console.log(date3);
        //3.H5 新增的 获得总的毫秒数
        console.log(Date.now());
3.数组对象
    创建数组对象  var arr = [];    或   var arr = new Array();
    检测是否为数组     arr instanceof Array        或       Array.isArray(参数值)
                /添加删除数组元素的方法
                //1.push() 在数组的末尾 添加一个或者多个数组元素
                var arr = [1,2,3];
                arr.push(4,'bala');     //返回值是新数组的长度
                console.log(arr);
                //2.unshift() 在数组的开头 添加一个或者多个数组
                arr.unshift('red',5);       //返回值是新数组的长度
                console.log(arr);
                //3.pop() 可以删除数组的最后一个元素
                arr.pop();      //不跟参数      返回值是删除的那个元素
                console.log(arr);
                //4.shift() 可以删除数组第一个元素
                arr.shift();        //不跟参数      返回值是删除的那个元素
                console.log(arr);
    /数组排序
    //1.翻转数组
    var arr1 = ['red','pink','green'];
    arr1.reverse();
    console.log(arr1);
    //2.数组排序（冒泡排序）
    var arr2 = [12,6,4,25,7,1];
    arr2.sort(function (a,b) {
        return a - b;  //升序的顺序排列        b - a 则为降序的顺序排列
    });
    console.log(arr2);
            //返回数组元素索引号方法   indexOf()    lastIndexOf()
            //它只返回第一个满足条件的索引号     如果在数组中找不到元素则返回 -1
            var arr3 = ['red','pink','blue','green','purple'];
            console.log(arr3.indexOf('green'));
            console.log(arr3.lastIndexOf('green'));
4.字符串对象
根据字符返回位置    indexOf('要查找的字符',开始的位置)         lastindexOf('要查找的字符',开始的位置)
        （重点）根据位置返回字符    charAt(index)           返回指定位置的字符（index索引号）     str.charAt(0)
                                charCodeAt(index)       获取指定位置字符的ASCII码             str.charCodeAt(0)
                                str[index]              获取指定位置处字符                   和charAt()等效
字符串操作方法（重点）     concat(str1,str2,str3...)   concat()方法用于连接两个或多个字符串 等效于+，+更常用
                        slice(start,end)            从start位置开始，截取到end位置，end取不到
                        substring(start,end)        与slice基本相同，但是不接受负值
```
