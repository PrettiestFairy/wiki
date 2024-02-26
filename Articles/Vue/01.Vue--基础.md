# Vue

## 一、Vue基础

**Vue的特点**

组件化模式、声明式编码、使用虚拟DOM+优秀的Diff算法

###  1. 初识Vue

1. 想让Vue工作，就必须创建一个Vue实例，且要传入一个配置对象
2. root容器里的代码依然符合html规范，只不过混入了一些特殊的Vue语法
3. root容器里的代码被称为【Vue模板】
4. Vue实例和容器是一一对应的
5. 真是开发中只有一个Vue实例，并且会配合着组件一起使用
6. {{xxx}}中的xxx要写js表达式，且xxx可以自动读取到data中的所有属性
7. 一旦data中的数据发生改变，那么模板中用到该数据的地方也会自动更新

### 2. Vue模板语法

1. 插值语法

```html
<h3>你好，{{xxx}}</h3>
```

2. 指令语法

功能：用于解析标签（标签属性、标签体内容、绑定事件...）

```html
<a v-bind:href="school.url">点我找{{school.name}}</a>
	<!--v-bind: 可以简写为 ：-->
```

### 3. 数据绑定v-bind/v-model

1. 单向数据绑定(v-bind:)	数据只能从data流向页面

```html
<input type="text" v-bind:value="name">
<!-- 可以简写为 <input type="text" :value="name"> -->
```

2. 双向数据绑定（v-model:）	数据不仅能从data流向页面，还可以从页面流向data

双向数据绑定一般应用在表单类元素（输入类元素）上（如：input、select等）

```html
<input type="text" v-model:value="name">
    <!--v-model:value 可以简写为 v-model-->
```

### 4. el与data的两种写法

1. el的两种写法

```js
 new Vue({
        el: '#root',
        data:{
            name: '输入v1按tab'
        }
    })		//ctrl+alt+s进入设置，查找Live Template进行增改删查
```

```js
const v = new Vue({
        data: {
            name: '先声明再绑定'
        }
    })		//用 $mount 指明容器
    console.log(v)
    v.$mount('#root')
```

2. data的两种写法

data的第一种写法：对象式

```js
new Vue({
        el: '#root',
        // data的第一种写法：对象式
        data: {
            name: 'data对象式写法'
        }
    })
```

data的第二种写法：函数式（组件中data必须写成函数式）

```js
new Vue({
        el: '#root',
        data: function () {
            return{
                name:'data函数式写法'
            }
        }
    })
```

### 5. MVVM模型

M（Model 模型）：对应data中的数据

V（View 视图）：模板

VM（ViewModel 视图模型）：Vue实例对象

1. data中所有的属性，最后都出现在了vm身上

2. vm身上所有的属性 及 Vue原型上所有的属性，在Vue模板中都可以直接使用

### 6. 数据代理

数据代理：通过一个对象代理对另一个对象中属性的操作（读/写）

##### 6.1 Vue中的数据代理

![Vue数据代理图示](./static/images/Vue%E6%95%B0%E6%8D%AE%E4%BB%A3%E7%90%86%E5%9B%BE%E7%A4%BA.png)

### 7. 事件处理methods

#### 7.1 事件的基本使用

1. 使用 `c-on:xxx`  或 `@xxx`  绑定事件，其中` xxx`  是事件名
2. 事件的回调需要配置在 `methods`  对象中，最终会在 vm 中
3. `methods`  中配置的函数，不要使用箭头函数！否则 this 就不是 vm 了
4. `methods`  中配置的函数，都是被Vue所管理的函数，this 的指向是 vm 或者组件实例对象
5. `@click = "demo"`  和 `@click = "demo($event)"`  效果一致，但后者可以传参

#### 7.2 事件修饰符(可以用 . 连写)

1. `prevent` : 阻止默认事件（常用）
2. `stop` : 阻止事件冒泡（常用）
3. `once` : 事件只触发一次(常用)
4. `capture` : 使用事件的捕获模式
5. `self` : 只有 `event.target`  是当前操作的元素时才触发事件
6. `passive` : 事件的默认行为立即执行，无需等待事件回调执行完毕

#### 7.3 键盘事件

- Vue中常用的案件别名

```html
回车 => enter       删除 => delete        退出 => esc       空格 => space   换行 => tab		  退格 => backspace
上 => up            下 => down            左 => left        右 => right
```

- Vue未提供别名的按键,可以使用按键原始的key值去绑定,但注意要转为 caps-lock (短横线命名)

- 系统修饰键(用法特殊): `ctrl \ alt \ shift \ meta` （修饰键可以用 . 连写）

  - 配合 keyup 使用：按下修饰键的同时，再按下其他键，随后释放其他键，事件才被触发

  - 配合 keydown 使用：正常触发事件

- 也可以使用 `keyCode`  去指定具体的按键（不推荐）

- `Vue.config.keyCode.自定义键名 = 键码` ，可以去定制按键别名（不推荐）

### 8. 计算属性computed

```html
1.定义：要用的属性不存在，要通过已有的属性计算得来
2.原理：底层借助了 Object.defineproperty 方法提供的 getter 和 setter
3.get函数什么时候执行？
    （1）初次读取时会执行一次
    （2）当依赖的数据发生变化时会被再次调用
4.优势：与 methods 实现相比，内部有缓存机制（复用），效率更高，调试方便
5.备注
    （1）计算属性最终会出现在vm上，直接读取使用即可
    （2）如果计算属性要被修改，那必须写 set 函数去响应修改，且set 中要引起计算时依赖的数据发生变化修改
```

```js
//完整写法
    fullName: {
        get() {
             return this.firstName + '' + this.lastName
        },
        set(value) {
            console.log('set', value)
            const arr = value.split('')
            this.firstName = arr[0]
            this.lastName = arr[1]
        },
//简写（确定了只读不改）
    fullName() {        //fullName: function(){}
        console.log('get被调用了')
        return this.firstName + this.lastName
    }
```



### 9. 监视属性watch

```html
1.当被监视的属性变化时，回调函数自动调用，进行相关操作
2.监视的属性必须存在，才能进行监视
3.监视的两种写法：
    （1）new Vue 时传入 watch 配置
    （2）通过 vm.$watch 监视
```

```html
深度监视：
    1.Vue中的 watch 默认不监测对象内部值的改变（一层）
    2.配置 deep: true 可以检测对象内部值改变（多层）
备注：
    1.Vue自身可以监测对象内部值的改变，但Vue提供的watch默认不可以
    2.使用watch时根据数据的具体结构，决定是否采用深度监视
```

```js
 //正常写法
    isHot:{
        // immediate: true,    //初始化时让handler调用一下，默认false
        // deep: true,     //深度监视
        handler(newValue,oldValue){
            console.log('isHot被修改了',newValue,oldValue)
        }
    },
//简写
    isHot(newValue,oldValue){
        console.log('isHot被修改了',newValue,oldValue)
    }
```

```js
//正常写法
    vm.$watch('isHot', {
        immediate: true,    //初始化时让handler调用一下，默认false
        deep: true,     //深度监视
        handler(newValue, oldValue) {
            console.log('isHot被修改了', newValue, oldValue)
        }
    })
    //简写
    vm.$watch('isHot',function (newValue, oldValue){
        console.log('isHot被修改了', newValue, oldValue)
    })
```

### 10. watch和computed的区别

```html
computed和watch之间的区别：
    1.computed能完成的功能，watch都可以完成
    2.watch能完成的功能，computed不一定能完成，例如：watch可以进行异步操作
两个重要的小原则：
    1.所被Vue管理的函数，最好写成普通函数，这样this的指向才是vm或组件实例对象
    2.所有不被Vue所管理的函数（定时器的回调函数、ajax的回调函数、Promise的回调函数等），最好写成箭头函数，这样this的指向才是vm或组件实例对象
```

### 11. 绑定class与style样式

```
绑定样式：
    1.class样式
        写法：class="xxx"  xxx可以是字符串、对象、数组
            字符串写法适用于：类名不确定，要动态获取
            对象写法适用于：要绑定多个样式，个数不确定，名字也不确定
            数组写法适用于：要绑定多个样式，个数确定，名字也确定，但要动态决定用不用
    2.style样式
        :style="{fontSize: xxx}"其中xxx是动态值
        :style="[a,b]"其中a、b是样式对象
```

### 12. 条件渲染

```html
1.v-if
    写法：
    （1）v-if="表达式"
    （2）v-else-if="表达式”
    （3）v-else="表达式"
    适用于：切换频率较低的场景
    特点：不展示的DOM元素直接被移除
    注意：v-if 可以和 v-else-if、v-else 一起使用，但要求结构不能被"打断"
    	template 母版，只能配合v-if使用   不会影响结构,页面渲染时会脱掉
2.v-show
    写法：v-show="表达式"
    适用于：切换频率较高的场景
    特点：不展示的DOM元素为被移除，仅仅是使用样式隐藏掉
3.备注：使用 v-if 时，元素可能无法获取到，而使用 v-show 一定可以获取到
```

### 13. 列表渲染

#### 13.1 基本列表

```html
v-for指令：
    1.用于展示列表数据
    2.语法：v-for="(item, index) in xxx" :key="yyy"
    3.可遍历：数组、对象、字符串（用的很少）、指定次数（用的很少）
```

#### 13.2 key的原理

![遍历列表时key的作用](./static/images/key.png)

#### 13.3 面试题：react、vue中的key有什么作用？（key的内部原理）

```html
面试题：react、vue中的key有什么作用？（key的内部原理）
    1.虚拟DOM中key的作用：
        key是虚拟DOM对象的标识，当状态中的数据发生变化时，Vue会根据【新数据】生成【新的虚拟DOM】，
        随后Vue进行【新虚拟DOM】与【旧虚拟DOM】的差异比较，比较规则如下：
    2.比较规则：
        （1）旧虚拟DOM中找到了与新虚拟DOM相同的key：
            a.若虚拟DOM中内容没变，直接使用之前的真实DOM
            b.若虚拟DOM中内容变了，则生成新的真实DOM，随后替换掉页面中之前的真实DOM
        （2）旧虚拟DOM中未找到与新虚拟DOM相同的key
            创建新的真实DOM，随后渲染到页面
    3.用index作为key可能会引发的问题：
        （1）若对数据进行：逆序添加、逆序删除等破坏顺序操作：
            会产生没有必要的真实DOM更新 ==> 界面效果没有问题，但效率低
        （2）如果结构中还包含输入类的DOM：
            会产生错误的DOM更新 ==> 界面有问题
    4.开发中如何选择key？：
        （1）最好使用每条数据的唯一标识作为key，比如id、手机号、身份证、学号等唯一值
        （2）如果不存在对数据的逆序添加、逆序删除等破坏顺序操作，仅用于渲染列表用于展示，
        使用index作为key是没有问题的
```

#### 13.4 列表过滤

1. 用 watch 实现

```js
new Vue({
        el: "#root",
        data: {
            keyWord:'',
            persons: [
                {id:'001', name:'马冬梅', age: 18, sex:'女'},
                {id:'002', name:'周冬雨', age: 22, sex:'女'},
                {id:'003', name:'周董', age: 38, sex:'男'},
                {id:'004', name:'温家董', age: 25, sex:'男'},
            ],
            filPersons: []
        },
        watch: {
            keyWord: {
                immediate: true,    //先调一次handler
                handler(newValue) {
                    this.filPersons = this.persons.filter((p)=>{      //filter过滤
                        //函数体
                        return p.name.indexOf(newValue) !== -1;    //indexOf() 方法可返回某个指定的字符串值在字符串中首次出现的位置
                    })
                }
            }
        }
        })
```

2. 用 computed 实现

```js
new Vue({
        el: "#root",
        data: {     //配置属性
            keyWord: '',
            persons: [
                {id: '001', name: '马冬梅', age: 18, sex: '女'},
                {id: '002', name: '周冬雨', age: 22, sex: '女'},
                {id: '003', name: '周董', age: 38, sex: '男'},
                {id: '004', name: '温家董', age: 25, sex: '男'},
            ]
        },
        computed: {     //配置计算属性
            filPersons() {
                return this.persons.filter((p)=>{
                    return p.name.indexOf(this.keyWord) !== -1
                })
            }
        },
    })
```

#### 13.5 列表排序

```html
new Vue({
        el: "#root",
        data: {     //配置属性
            keyWord: '',
            sortType: 0,    //0原顺序  1降序  2升序
            persons: [
                {id: '001', name: '马冬梅', age: 18, sex: '女'},
                {id: '002', name: '周冬雨', age: 22, sex: '女'},
                {id: '003', name: '周董', age: 38, sex: '男'},
                {id: '004', name: '温家董', age: 25, sex: '男'},
            ]
        },
        computed: {     //配置计算属性   计算属性靠返回值决定东西的
            filPersons() {
                const arr = this.persons.filter((p) => {
                    return p.name.indexOf(this.keyWord) !== -1
                })
                //判断一下是否需要排序
                if (this.sortType) {        //1和2为真
                    arr.sort((p1, p2) => {
                        return this.sortType === 1 ? p2.age - p1.age : p1.age - p2.age
                    })
                }
                return arr
            }
        },
    })
    
    //小复习
    /*let arr = [1,2,6,4,3,5]
    arr.sort((a,b)=>{
        return a-b  //a-b升序  b-a降序
    })
```

### 14. Vue 监测数据

#### 14.1 Vue监视数据的原理

```html
Vue监视数据的原理：
1.Vue 会监视 data 中所有层次的数据
2.如何检测对象中的数据？
    通过 setter 实现监视，且要在 new Vue 时就传入要检测的数据
    （1）对象中后追加的属性，Vue 默认不做响应式处理
    （2）如需给后添加的属性做响应式，使用如下 API ：
        Vue.set(taregt, propertyNmae/index, value) 或
        vm.$set(target, propertyName/index, value)
3.如何检测数组中的数据？
    通过包裹数组更新元素的方法实现，本质就是做了两件事：
    （1）调用原生对应的方法对数组进行更新
    （2）重新解析模板，进而更新页面
4.在 Vue 修改数组中的某个元素一定要用如下方法：
    （1）使用这些 API:push()、pop()、shift()、unshift()、splice()、sort()、reverse()
    （2）Vue.set() 或 vm.$set()
 特别注意：Vue.set() 和 vm.$set() 不能给 vm 或 vm的根数据对象添 加属性
```

#### 14.2 Vue监视数据实例

```html
<body>
<div id="root">
    <h1>学生信息</h1>
    
    <button @click="student.age++">年龄+1岁</button> <br>
    <button @click="addSex">添加性别属性，默认值：男</button> <br>
    <button @click="student.sex = '未知'">修改性别</button> <br>
    <button @click="addFriend">在列表首位添加一个朋友</button> <br>
    <button @click="updateFirstFriendName">修改第一个朋友的名字为：张三</button> <br>
    <button @click="addHobby">添加一个爱好</button> <br>
    <button @click="updateHobby">修改第一个爱好为：运动</button> <br>
    <button @click="removeIt">过滤掉爱好中的打豆豆</button> <br>
    
    <h3>姓名：{{student.name}}</h3>
    <h3>年龄：{{student.age}}</h3>
<!--    v-if 没有性别值时隐藏-->
    <h3 v-if="student.sex">性别：{{student.sex}}</h3>
    <h3>爱好：</h3>
    <ul>
        <li v-for="(h,index) in student.hobby" :key="h.id">
            {{h}}
        </li>
    </ul>
    <h3>朋友们：</h3>
    <ul>
        <li v-for="(f,index) in student.friends" :key="f.id">
            {{f.name}}--{{f.age}}
        </li>
    </ul>
</div>
</body>
<script>
    new Vue({
        el: '#root',
        data: {
            student: {
                name: 'Tom',
                age: 18,
                hobby: ['吃饭', '睡觉', '打豆豆'],
                friends: [
                    {name: 'Jquery', age: 35},
                    {name: 'Tony', age: 33}   
                ]
            }
        },
        methods: {
            addSex() {
                // Vue.set(在谁身上加,'加什么','默认值')
                // Vue.set(this.student,'sex','男')
                this.$set(this.student, 'sex', '男')
            },
            addFriend() {
                this.student.friends.unshift({name:'Jack', age: 27})
            },
            updateFirstFriendName() {
                this.student.friends[0].name = '张三'
            },
            addHobby() {
                this.student.hobby.push('学习')
            },
            updateHobby() {
                // splice(0,1,'bb')  从索引号为0开始，删除一个，添加值为bb
                // this.student.hobby.splice(0,1,'运动')
                // Vue.set(在谁身上改，第几个值，'改为什么')
                // Vue.set(this.student.hobby, 0, '运动')
                this.$set(this.student.hobby, 0, '运动')
            },
            removeIt() {
                // 所有不是由Vue控制的回调，都尽可能写成 箭头函数。箭头函数没有this会向上找，找到vm
                this.student.hobby = this.student.hobby.filter((h)=>{
                    //函数体
                    return h !== '打豆豆'
                })
            }
        }
    })
</script>
```

### 15. 收集表单数据

```html
收集表单数据:
若:<input type="text"/>,则 v-model 收集的是 value 值,用户输入的就是 value 值
若:<input type="radio"/>,则 v-model 收集的就是 value 值,且要给标签配置 value 值
若:<input type="checkbox"/>
    1.没有配置 input 的 value 属性,那么收集的就是 checked (勾选 or 未勾选,是布尔值)
    2.配置 input 的 value 属性:
        (1)v-model 的初始值是非数组,那么收集的就是 checked (勾选 or 未勾选,是布尔值)
        (2)v-model 的初始值是数组,那么收集的就是 value 组成的数组
备注:v-model 的三个修饰符
    lazy: 失去焦点再收集数据
    number: 输入字符串转为有效的数字
    trim: 输入首位空格过滤
```

#### 15.1 收集表单数据实例

```html
<body>
<div id="root">
<!--    pervent 阻止默认行为-->
    <form action="" @submit.pervent="demo">
        <label for="demo">账号：</label>
<!--        trim 不收集首字之前的空格-->
        <input type="text" id="demo" v-model.trim="userInfo.account">
        <br><br>
        <label for="password">密码：</label>
        <input type="text" id="password" v-model="userInfo.password">
        <br><br>
        <label for="age">年龄：</label>
        <input type="number" id="age" v-model.number="userInfo.age">
        <br><br>
        性别：
        男 <input type="radio" name="sex" value="male" v-model="userInfo.sex">
        女 <input type="radio" name="sex" value="female" v-model="userInfo.sex">
        <br><br>
        爱好：
        吃饭 <input type="checkbox" v-model="userInfo.hobby" value="eat">
        睡觉 <input type="checkbox" v-model="userInfo.hobby" value="sleep">
        打豆豆 <input type="checkbox" v-model="userInfo.hobby" value="game">
        <br><br>
        所属校区
        <select v-model="userInfo.city">
            <option value="">请选择校区</option>
            <option value="beijing">北京校区</option>
            <option value="shanghai">上海校区</option>
            <option value="shenzhen">深圳校区</option>
            <option value="chengdu">成都校区</option>
        </select>
        <br><br>
        其它信息
<!--        lazy 失去焦点的一瞬间再收集数据，不是实时收集-->
        <textarea cols="20" rows="6" v-model.lazy="userInfo.other"></textarea>
        <br><br>
        <input type="checkbox" v-model="userInfo.agree">阅读并接受 <a href="http://baidu.com">《用户协议》</a>
        <button>提交</button>
    </form>
</div>
</body>
<script>
    new Vue({
        el: '#root',
        data: {
            userInfo: {
                account: '',
                password: '',
                age: '',
                sex: 'female',      //双向绑定，默认女
                hobby: [],
                city: '',
                other: '',
                agree: ''
            }
        },
        methods: {
            demo() {
                // JSON.stringify(value,[replacer[,space]])  将某个对象转换为JSON字符串形式
                console.log(JSON.stringify(this.userInfo))
            }
        }
    })
</script>
```

### 16. 过滤器

```
过滤器：
定义：对要显示的数据进行特定格式化后再显示（适用于一些简单逻辑的处理）
语法：
    1.注册过滤器：Vue.filter(name, callback) 或 new Vue{filters:{}}
    2.使用过滤器：{{xxx | 过滤器名}} 或 v-bind:属性 = "xxx | 过滤器名"
备注：
    1.过滤器也可以接收额外参数、多个过滤器也可以串联
    2.并没有改变原本的数据，是产生新的对应的数据
```

#### 16.1 过滤器实例

```html
<body>
<div id="root">
    <h2>显示格式化后的时间</h2>
    <!--    计算属性实现-->
    <h3>现在的时间是:{{fmTime}}</h3>
    <!--    methods实现-->
    <h3>现在的时间是:{{getFmTime()}}</h3>
    <!--    过滤器实现-->
    <h3>现在的时间是:{{time | timeFormater}}</h3>
<!--    过滤器实现（传参）-->
    <h3>只要年月日:{{time | timeFormater('YYYY-MM-DD') | mySlice}}</h3>
    <h3 v-bind:x="msg | mySlice">八七</h3>
    
</div>

<div id="root2">
    <h2>{{msg}}</h2>
</div>
</body>
<script>
    new Vue({
        el: '#root',
        data: {
            time: 1666361125686,        //时间戳
            msg: '你好啊八零'
        },
        computed: {
            fmTime() {
                return dayjs(this.time).format('YYYY年MM月DD日 HH:mm:ss')
                /*  .startOf('month')
                  .add(1, 'day')
                  .set('year', 2018)
                  .format('YYYY-MM-DD HH:mm:ss')*/
            }
        },
        methods: {
            getFmTime() {
                return dayjs(this.time).format('YYYY年MM月DD日 HH:mm:ss')
            },
        },
        //局部过滤器
        filters: {
            // 如果str 有值，就用括号内的值，没有值就用默认的 str
            timeFormater: function (value,str='YYYY年MM月DD日 HH:mm:ss') {
                // console.log('@', value)
                return dayjs(value).format(str)
            },
            /*//mySlice 在全局配置时则不需要再局部配置
            mySlice(value) {
                // 截取 0到4 的值
                return value.slice(0,4)
            }*/
        //局部过滤器
        }
    })

    new Vue({
        el: '#root2',
        data: {
                msg: 'lei hao a'
        },
    })
</script>
```

### 17. 内置指令

#### 17.1 v-text指令

```
内置指令复习：
    v-bind:     单向绑定解析表达式，可简写为 :xxx
    x-model:    双向绑定数据
    v-for:      遍历数组/对象/字符串
    v-on:       绑定事件监听，可简写为 @
    v-if:       条件渲染（动态控制节点是否存在）
    v-else:     条件渲染（动态控制节点是否存在）
    v-show:     条件渲染（动态控制节点是否展示）
v-text 指令：
    1.作用：向其所在的节点中渲染文本内容
    2.与插值语法的区别：v-text 会替换掉节点中的内容，插值语法{{xx}}则不会
```

```html
<body>
<div id="root">
<!--    写法1，更常用-->
    <div>你好，{{name}}</div>
<!--    写法2-->
    <div v-text="name"></div>
<!--    str 内有标签，v-text不认-->
    <div v-text="str"></div>
</div>
</body>
<script>
    new Vue({
        el: '#root',
        data: {
            name: 'Tony',
            str: '<h3>leihaoa</h3>'
        },
    })
</script>
```

#### 17.2 v-html指令

> cookie工作原理（不允许跨浏览器）

![cookie工作原理](./static/images/cookie%E5%B7%A5%E4%BD%9C%E5%8E%9F%E7%90%86.png)

```
v-html指令：
1.作用：向指定节点中渲染包含 html 结构的内容
2.与插值语法的区别：
    （1）v-html 会替换掉节点中所有的内容 ，{{xx}}则不会
    （2）v-html 可以识别 html 结构
3.严重注意： v-html 有安全性问题
    （1）在网站上动态渲染任意 HTML 时非常危险的，容易导致 XSS 攻击
    （2）一定要在可信的内容上使用 v-html，不要用在客户提交的内容上
```

```html
<body>
<div id="root">
    <div>你好，{{name}}</div>
    <div v-html="str"></div>
    <div v-html="str2"></div>
</div>
</body>
<script>
    new Vue({
        el: '#root',
        data: {
            name: 'Tony',
            str: '<h3>leihaoa</h3>',
            str2: '<a href=javascript:location.href="http://www.baidu.com"+document.cookie>点击跳转得cookie</a>'
            
        },
    })
</script>
```

#### 17.3 v-clock指令

```
v-cloak指令（没有值）：
    1.本质是一个特殊属性，Vue 实例创建完毕并接管容器后，会删掉 v-cloak 属性
    2.使用css配合 v-cloak 可以解决网速慢时页面展示出 {{xxx}} 的问题
```

```html
 <style>
        [v-cloak] {
            display: none;
        }
    </style>
</head>
<body>
<div id="root">
    <h2 v-cloak>{{name}}</h2>
</div>
</body>
<script>
    new Vue({
        el: '#root',
        data: {
            name: 'tony',
        },
    })
</script>
```

#### 17.4 v-once指令

```
v-once指令（没有值）：
1.v-once 所在节点在初次动态渲染后，就视为静态内容了
2.以后数据的改变不会引起 v-once 所在结构的更新，可以用于优化性能
```

```html
<body>
<div id="root">
    <h2 v-once>初始的n值是：{{n}}</h2>
    <h2>当前的n值是：{{n}}</h2>
    <button @click="n++">点我n+1</button>
</div>
</body>
<script>
    new Vue({
        el: '#root',
        data: {
            n: 1,
        },
    })
</script>
```

#### 17.5 v-pre指令

```
v-pre指令（没有值）：
1.跳过其所在节点的编译过程
2.可以用它跳过：没有使用指令语法、没有使用插值语法的节点，会加快编译
```

```html
<body>
<div id="root">
    <h2 v-pre>v-pre指令</h2>
    <h2>当前的n值是：{{n}}</h2>
    <button @click="n++">点我+1</button>
</div>
</body>
<script>
    new Vue({
        el: '#root',
        data: {
            n: 1,
        },
    })
</script>
```

### 18. 自定义指令

```
一、定义语法：
	（1）局部指令：
        new Vue({
            directives:{指令名：配置对象}
        })
        或
        new Vue({
            directives(){}
        })
	（2）全局指令：
		Vue.directive(指令名，配置对象)
		或
		Vue.directive(指令名，回调函数)
二、配置对象中常用的3个回调：
	（1）bind: 指令与元素成功绑定时调用
	（2）inserted: 指令所在元素被插入页面时调用
	（3）update: 指令所在模板结构被重新解析时调用
三、备注：
	1.指令定义时不加 v- ，但使用时要加 v-
	2.指令名如果是多个单词，要使用 kebab-case 命名方式，不要用 cameClase 命名
```



> 需求1：定义1个 v-big 指令，和 v-text 功能类似，但会把绑定的数值放大8倍

```js
<body>
<div id="root">
    <h2>当前的n值是：{{n}}</h2>
    <h2>放大8倍后的n值是：<span v-big="n"></span></h2>
    <button @click="n++">点我+1</button>
</div>
</body>
<script>
    new Vue({
        el: '#root',
        data: {
            n: 1,
        },
        // 指令
        directives: {
            // 两种写法，先写函数式，再写对象式
            //big函数何时会被调用？ 1.指令与元素成功绑定时（一上来）  2.指令所在的模板被重新解析时
            // 'big-number': function (element,binding) {          // 完整写法
            big: (element,binding) {			// 相当于只写了 bind 和 update
                element.innerText = binding.value * 8
            }
        }
    })
</script>
```

> 需求2：定义1个 v-fbind 指令，和 v-bind 功能类似，但可以让其所绑定的 input 元素默认获取焦点(autofocus)

```js
<body>
<div id="root">
    <button @click="n++">点我+1</button>
    <hr>
    <input type="text" v-fbind:value="n">
</div>
</body>
<script>
    new Vue({
        el: '#root',
        data: {
            n: 1,
        },
        // 指令
        directives: {
            // 两种写法，先写函数式，再写对象式
            //big函数何时会被调用？ 1.指令与元素成功绑定时（一上来）  2.指令所在的模板被重新解析时
            fbind: {
                // 指令与元素成功绑定时（一上来）
                bind(element,binding) {
                    element.value = binding.value
                },
                // 指令所在元素被插入页面时
                inserted(element,binding) {
                    element.focus()
                },
                // 指令所在的模板被重新解析时
                update(element,binding) {
                    element.value = binding.value
                    // element.focus()
                }
    }
        }
    })
</script>
```

> 有很多特殊功能的指令可以用全局指令（供所有编码人员使用）

```js
<script>
// 定义全局指令(对象式）
    Vue.directive('fbind', {
        // 指令与元素成功绑定时（一上来）
        bind(element,binding) {
            element.value = binding.value
        },
        // 指令所在元素被插入页面时
        inserted(element,binding) {
            element.focus()
        },
        // 指令所在的模板被重新解析时
        update(element,binding) {
            element.value = binding.value
            // element.focus()
        }
    })
    //定义全局指令（函数式）
    Vue.directive('big',function (element,binding) { 
        element.innerText = binding.value * 8
    })
</script>
```

### 19. 生命周期

![生命周期](./static/images/%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F.png)

```
生命周期：
1.又名：生命周期回调函数、生命周期函数、生命周期钩子
2.是什么：Vue在关键时刻帮我们第哦啊用的一些特殊名称的函数
3.生命周期函数的名字不可更改，但函数的具体内容是程序员根据需求编写的
4.生命周期函数中的this指向的是 vm 或 组件实例对象
```

```js
<body>
<div id="root">
    <h2 v-if="a">你说啥</h2>
    <!--    动态绑定style中，{opacity: opacity}简写{opacity}-->
    <h2 :style="{opacity}">欢迎来到Vue生命周期</h2>
</div>
</body>
<script>
    const vm = new Vue({
        el: '#root',
        data: {
            opacity: 1,
            a: false
        },
        methods: {
            
        },
        // Vue 完成模板的解析并把初始的真实DOM元素放入页面后（挂载完毕）调用 mounted
        mounted(){
            setInterval(() => {
                this.opacity -= 0.01
                if (this.opacity <= 0)  this.opacity = 1
            },16)
        }
    })
</script>
```

#### 19.1 生命周期总结

```
常用生命周期钩子：
    1.mounted: 发送 ajax 请求、启动定时器、绑定自定义事件、订阅消息等【初始化操作】
    2.beforeDestory: 清楚定时器、解绑自定义事件、取消订阅消息等【收尾工作】
关于销毁 Vue 实例：
    1.销毁后借助 Vue 开发者工具看不到任何信息
    2.销毁后自定义事件会失效，但原生 DOM 事件依然有效
    3.一般不会在 beforeDestory 中操作数据，因为即便操作数据，也不会再触发更新流程了
```

```js
<body>
<div id="root">
    <!--    动态绑定style中，{opacity: opacity}简写{opacity}-->
    <h2 :style="{opacity}">欢迎来到Vue生命周期</h2>
    <button @click="stop">点我停止变换</button>
    <button @click="opacity = 1">透明度回到1</button>
</div>
</body>
<script>
    const vm = new Vue({
        el: '#root',
        data: {
            opacity: 1,
        },
        methods: {
            stop(){
                // clearInterval(this.timer)
                this.$destroy()
            }
        },
        // Vue 完成模板的解析并把初始的真实DOM元素放入页面后（挂载完毕）调用 mounted
        mounted(){
            this.timer = setInterval(() => {
                this.opacity -= 0.01
                if (this.opacity <= 0)  this.opacity = 1
            },16)
        },
        beforeDestroy() {
            clearInterval(this.timer)
        }
    })
</script>
```

## 二、Vue组件化编程

**非单文件组件**

一个文件(a.html)中包含有n个组件

### 1. 基本使用

```
Vue中使用组建的三大步骤：
    一、定义组件（创建组件）
    二、注册组件
    三、使用组件（写组件标签）
一、如何定义一个组件？
    使用 Vue.extend(options) 创建，其中 options 和 new Vue(options) 时传入的那个 options 几乎一样，但也有点区别
    区别如下：
        1.el不要写 ———— 最终所有的组件都要经过一个 vm 的管理，由 vm 中的 el决定服务于哪个容器
        2.data必须写成函数 ———— 避免组件被复用时，数据存在引用关系
    备注：使用 template 可以配置组件结构
二、如何注册组件？
    1.局部注册：靠 new Vue 的时候传入 components 选项
    2.全局注册：靠 Vue.component('组件名',组件)
三、编写组件标签：
    <school></school>
```

```js
<body>
<div id="root">
    <hello></hello>
    <hr>
    <h2>{{msg}}</h2>
    <hr>
<!--    第三步：编写组件标签-->
    <xuexiao></xuexiao>
    <hr>
<!--    第三步：编写组件标签-->
    <xuesheng></xuesheng>
</div>
<div id="root2">
    <hello></hello>
</div>
</body>
<script>
    // 第一步：创建 school 组件
    const school = Vue.extend({
        template:`
            <div>
                <h2>学校名称：{{schoolName}}</h2>
                <h2>学校地址：{{address}}</h2>
                <button @click="showName">点我提示学校名</button>
            </div>
        `,
        // 组件定义时不要写el配置项，因为最终所有的组件都要被一个vm管理，由vm决定服务于哪个容器
        data() {        // data写成函数式
            return {
                schoolName: '半导体学院',
                address: '平川半导体街道'
            }
        },
        methods: {
            showName(){
                alert(this.schoolName)
            }
        }
    })
    // 第一步：创建 student 组件
    const student = Vue.extend({
        template:`
          <div>
              <h2>学生姓名：{{studentName}}</h2>
              <h2>学生年龄：{{age}}</h2>
          </div>
        `,
        data() {
            return {
                studentName: '张三',
                age: 18
            }

        }
    })
    
    
    // 第一步创建hello组件（全局注册）
    const hello = Vue.extend({
        template:`
            <div>
                <h2>你好啊{{name}}</h2>
            </div>
        `,
        data(){
            return{
                name: 'Tom'
            }
        }
    })
    //第二步全局注册组件
    Vue.component('hello',hello)


    // 创建vm
    new Vue({
        el: '#root',
        data:{
            msg: 'leihaoa'
        },
        // 第二步：注册组件（局部注册）
        components: {
            xuexiao: school,        //同名简写是可以只写一个 school
            xuesheng: student
        }
    })

    new Vue({
        el: '#root2',
    })
</script>
```

#### 1.1 几个注意点

```
1.关于组件名：
    一个单词组成：
        第一种写法（首字母小写）: school
        第二种写法（首字母大写）: School
    多个单词组成：
        第一种写法（kebab-case命名）: my-school
        第二种写法（CamelCase命名）: MySchool （需要Vue脚手架支持）
    备注：
        （1）组件名尽可能回避HTML中已有的元素名称，例如：h2、H2都不行
        （2）可以使用 name 配置项指定组件在开发者工具中呈现的名字
2.关于组件标签：
    第一种写法：<school></school>
    第二种写法：<school/>
    备注：不使用脚手架时，<school/>会导致后续组件不能渲染
3.一个简写方式：
    const school = Vue.extend(options) 可简写为： const school = options
```

#### 1.2 组建的嵌套

```js
<body>
<div id="root">
    
</div>
</body>
<script>
    // 定义student组件(school的子组件得在school之前定义）
    const student = Vue.extend({
        name: 'student',
        template:`
            <div>
                <h2>学生姓名：{{name}}</h2>
                <h2>学生性别：{{sex}}</h2>
            </div>
        `,
        data(){
            return{
                name: '张三',
                sex: '男'
            }
        }
    })
    
    // 定义school组件
    const school = Vue.extend({
        name: 'school',
        template:`
            <div>
                <h2>学校内容：{{name}}</h2>
                <h2>学校地址：{{address}}</h2>
                <student></student>
           </div>
        `,
        data(){
            return {
                name: '魔导学院',
                address: '家里蹲'
            }
        },
        // 注册组件（局部）
        components: {
            student
        }
    })
    
    // 定义hello组件
    const hello = Vue.extend({
        name: 'hello',
        template:`
                <h2>{{msg}}</h2>
        `,
        data(){
            return{
                msg:'你好啊'
            }
        }
    })
    
    // 定义app组件
    const app = Vue.extend({
        template:`
          <div>
            <hello></hello>
            <school></school>
          </div>
        `,
        components: {
            school,
            hello
        }
    })

    new Vue({
        el: '#root',
        template:'<app></app>',     // 盒子里不写时可以在这里写
        // 注册组件（局部）
        components: {
            app
        }
    })
</script>
```

#### 1.3 VueComponent

```
关于VueComponent：
    1.school组件本质是一个名为 VueComponent 的构造函数，且不是程序员定义的，是 Vue.extend 生成的
    2.我们只需要写 <school/> 或 <school></school> ， Vue解析时会帮我们创建school组件的实例对象
        即Vue 帮我们执行的： new VueComponent(options)
    3.特别注意：每次调用 Vue.extend ，返回的都是一个全新的 VueComponent
    4.关于this指向：
        （1）组件配置中：
            data函数、methods中的函数、watch中的函数、computed中的函数 它们的this均是【VueComponent实例对象】
        （2）new Vue()配置中：
            data函数、methods中的函数、watch中的函数、computed中的函数 它们的this均是【Vue实例对象】
    5.VueComponent 的实例对象，以后简称 vc （也可称之为：组件实例对象）
        Vue的实例对象，以后简称 vm
```

#### 1.4 一个重要的内置关系

```
1.一个重要的内置关系：VueComponent.prototype.__proto__ === Vue.prototype
2.为什么要有这个关系：让组件实例对象（vc）可以访问到 Vue 原型上的属性、方法
```

> 实例的原型对象永远指向自己缔造者的原型对象

![Vue与VueComponent的关系](./static/images/Vue%E4%B8%8EVueComponent%E7%9A%84%E5%85%B3%E7%B3%BB.png)

### 2. 单文件组件

一个文件中(a.vue)只包含有1个组件

#### 2.1 ES6模块化导入导出

> 使用import关键字导入其它模块成员
>
> 使用export关键字导出自己模块的成员

1. 默认导入和导出 `export default`

```js
export default{ }
```

```js
import xx from 'xx(path)'
```

2. 按需导入和导出

```js
export let a = 1
export function eat(){
    console.log('xxx')
}
export {
	c as cc		// 重命名
}
export default { }
```

```js
import {xx,xxx} from 'path'
```

```js
import defaultObj {xx,xxx} form 'path'
```

```js
import * as newObj from 'path'
```

3. 直接导入并执行代码模块

```js
import 'path'
```

#### 2.2  .Vue格式

```vue
<template>
  <!--组件的结构-->
  <div class="demo">
    <h2>学校名称：{{ schoolName }}</h2>
    <h2>学校地址：{{ address }}</h2>
    <button @click="showX">点我提示学校名</button>
  </div>
</template>

<script>
// 组件交互相关的代码（数据、方法等等）
export default {
  name: 'School',   // 一般跟文件名保持一致
  data() {
    return {
      schoolName: '魔导学院',
      address: '家里蹲'
    }
  },
  methods: {
    showX() {
      console.log(this.schoolName)
    },
  },
}
/*export default Vue.extend({   // 默认暴露完整写法
  name: 'school',
  data() {
    return {
      schoolName: '魔导学院',
      address: '家里蹲'
    }
  },
  methods: {
    showX() {
      console.log(this.schoolName)
    }
  }
})*/
// export {school}   // 统一暴露
// export default school   // 默认暴露第二种写法
</script>

<!--<style scoped>  样式私有化，不易修改，以适合用在公共组件样式-->
<style>
/*组件的样式*/
.demo {
  background-color: pink;
}
</style>
```

## 三、使用Vue脚手架

### 1. 安装Vue Cli

![安装VueCli](./static/images/%E5%AE%89%E8%A3%85VueCli.png)`vue -V` 可检查安装版本

`Ctrl+C` 停止批处理操作（Y/N/Ctrl+C）

### 2. 脚手架文件结构

![脚手架结构图](./static/images/%E8%84%9A%E6%89%8B%E6%9E%B6%E7%BB%93%E6%9E%84%E5%9B%BE.png)

#### 2.1 相关命令

在终端执行`npm run serve`命令 运行`main.js`文件，要注意项目路径，否则会报错

![npm run serve](./static/images/npm%20run%20serve.png)

#### 2.2 main.js拆解分析

```js
/* 该文件是整个项目的入口文件 */
// 引入Vue
import Vue from 'vue'
// 引入App组件，它是所有组件的父组件（vm之下，其余组件之上）
import App from './App.vue'
// 关闭Vue的生产提示
Vue.config.productionTip = false

// 创建Vue实例对象---vm
new Vue({
  // render函数，简写。   完成功能：将App组件放入容器中
  render: h => h(App),
  // .$mount('#app') 容器，可以用 el:'#app'，同样的作用
}).$mount('#app')

```

#### 2.3 index.html拆解分析

```html
<!DOCTYPE html>
<html lang="">
  <head>
    <meta charset="utf-8">
<!--针对IE浏览器的一个特殊配置，含义是让IE浏览器以最高的渲染级别渲染页面-->
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
<!--开启移动端的理想视口-->
    <meta name="viewport" content="width=device-width,initial-scale=1.0">
<!--配置页签图标    <%= BASE_URL %> 指的是 public所在的路径-->
    <link rel="icon" href="<%= BASE_URL %>favicon.ico">
<!--配置网页标题   <%= htmlWebpackPlugin.options.title %> 指向 package.json中的name-->
    <title><%= htmlWebpackPlugin.options.title %></title>
  </head>
  <body>
<!--当浏览器不支持js的时候，noscript中的元素就会被渲染-->
    <noscript>
      <strong>We're sorry but <%= htmlWebpackPlugin.options.title %> doesn't work properly without JavaScript enabled. Please enable it to continue.</strong>
    </noscript>
<!--容器-->
    <div id="app"></div>
    <!-- built files will be auto injected -->
  </body>
</html>
```

#### 2.4 render函数

```js
render: h => h('App')
```

完整写法

```js
render: function (createElement) {
     return createElement(App);
}
```

#### 2.5 关于不同版本的Vue

1. vue.js与vue.runtime.xxx.js的区别：
     （1）vue.js是完整版的Vue，包含：核心功能+模板解析器
     （2）vue.runtime.xxx.js时运行版本的Vue，只包含：核心功能；没有模板解析器
2. 因为vue.runtime.xxx.js没有模板解析器，所以不能使用template配置项，需要使用
     render函数接收到的creatElement函数去指定具体内容

#### 2.6 vue.config.js配置文件

1. 使用vue inspect > output.js可以查看到Vue脚手架的默认配置

2. 使用vue.config.js可以对脚手架进行个性定制，详情见：<https://cli.vuejs.org/zh>

> 红色框尽量不要改

![脚手架默认配置](./static/images/%E8%84%9A%E6%89%8B%E6%9E%B6%E9%BB%98%E8%AE%A4%E9%85%8D%E7%BD%AE.png)

Vue Cli官网中配置参考`vue.config.js`中的`pages`文档中内容，复制粘贴修改(修改默认配置，比如上图中红色框部分，不过一般不建议修改），修改后需要重新输入npm run serve运行

> 配置文件修改了就需要重新启动

Vue Cli官网中配置参考`vue.config.js`中的`lintOnSave`文档中内容，与`pages`平级，关闭语法检查（此时在`main.js`中定义一个没有用的东西也不会在控制台报错了）

```js
lintOnSave: false		// 关闭语法检查
```

### 3. ref属性

1. 被用来给元素或子组件注册引用信息（id的替代者）
2. 应用在html标签上获取的是真实DOM元素，应用在组件标签上是组件实例对象（vc)
3. 使用方式：

- 打标识：<h1 ref="xxx">. . . . . . </h1> 或 <MySchool ref="xxx"></MySchool>
- 获取：this.$refs.xxx

```vue
<template>
<div>
  <h1 v-text="msg" ref="title"></h1>
  <button ref="btn" @click="showDOM">点我输出上方DOM元素</button>
  <MySchool ref="sch"></MySchool>
  <MySchool/>
</div>
</template>

<script>
import MySchool from "@/components/MySchool";
export default {
  name: "App",
  components: {MySchool},
  data() {
    return {
      msg: '欢迎来到脚手架'
    }
  },
  methods: {
    showDOM() {
      // console.log(this)     // 输出组件实例对象,得到各种参数
      console.log(this.$refs.title)   // 真实DOM元素
      console.log(this.$refs.btn)   // 真实DOM元素
      console.log(this.$refs.sch)   // MySchool组件的实例对象（vc)
    }
  }
}
</script>
```

### 4. props配置项

功能：让组件接收外部传递过来的数据

- 传递数据：

  <Demo name="xxx"/>

- 接受数据：

1. 第一种方式（只接收）：

```js
props: ['name']
```

2. 第二种方式（限制类型）：

```js
props: {
    name: String
}
```

3. 第三种方式（限制类型、限制必要性、指定默认值）：

```js
props: {
    name:{
        type: String,		// 类型
        required: true,		// 必要性（与默认值二选一）
        default: '张三'		// 默认值（与必要性二选一）
    }
}
```

- 备注：props是只读的，Vue底层会监测你对props的修改，如果进行了修改，就会发出警告，若业务需求确实需要修改，那么需要复制props的内容到data中一份，然后去修改data中的数据


```js
// 组件中代码
<template>
  <div class="student">
    <h3>{{msg}}</h3>
    <h2>学生姓名：{{name}}</h2>
    <h2>学生年龄：{{myAge+1}}</h2>
    <h2>学生性别：{{sex}}</h2>
    <button @click="updateAge">尝试修改收到的年龄</button>
  </div>
</template>

<script>
export default {
  name: "MyStudent",
  data() {
    return {
      msg: '我是一名学生',
      myAge: this.age
    }
  },
  methods: {
    updateAge(){
      this.myAge++
    }
  },
  // 简单接收
  props:['name','sex','age']
}
  </script>
```

```js
// App.vue中template添加值
<template>
<div>
  <MyStudent name="李四" :age="18" sex="女"/>
  <hr>
  <MySchool></MySchool>
</div>
</template>
```

### 5. mixin混入

功能：可以把多个组件共同的配置提取成一个混入对象

使用方法：

1. 第一步定义混合（src中新建一个js文件 mixin.js），例如：

```js
export const mixin = {
    methods: {
        showName(){
            alert(this.name)
        }
    },
    mounted() {		// 钩子不受覆盖影响
        console.log('你点啥')
    }
}

export const mixin2 = {
    data() {
        return {
            x: 1,	// 可以被组件中同属性数值覆盖
            y: 2
        }

    }
}
```

2. 第二部使用混入，例如：

   （1）全局混入（main.js）：Vue,mixin(xxx)

```js
// 全局引入混合
import {mixin,mixin2} from "@/mixin";
// 在创建vm之前，全局混合使用
Vue.mixin(mixin)
Vue.mixin(mixin2)
```

​		（2）局部混入（组件）：mixins: ['xxx']

```js
// 引入一个mixin混合
import {mixin, mixin2} from "@/mixin";
export default {
      mixins: [mixin,mixin2]
}
```

### 6. 插件

功能：用于增强Vue

本质：包含install方法的一个对象，install的第一个参数是Vue，第二个以后的参数是插件使用者传递的数据。

- （在src文件夹中新建一个js文件）定义插件：

```js
对象.install = function (Vue, options) {
    // 1.添加全局过滤器
    Vue.filter(...)
    
    // 2.添加全局指令
    Vue.directive(...)
    
    // 3.配置全局混入（合）
    Vue.mixin(...)
              
    // 4.添加实例方法
    Vue.prototype.$myMethod = function () {...}
    Vue.prototype.$myProperty = xxx
}
```

```js
export default {
    install(Vue,x,y,z) {
        console.log(x,y,z)		//1 2 3
        // 全局过滤器
        Vue.filter('mySlice',function (value) {
            return value.slice(0,4)
        })		// 过滤器使用在template中     <h2>学校名称：{{name | mySlice}}</h2>

        // 定义全局指令
        Vue.directive('fbind',{
            // 指令与元素成功绑定时（一上来）
            bind(element,binding) {
                element.value = binding.value
        },
            // 指令所在元素被插入页面时
            inserted(element) {
                element.focus()
            },
            // 指令所在的模板被重新解析时
            update(element,binding) {
                element.value = binding.value
            }
        })

        // 定义混入
        Vue.mixin({
            data() {
                return{
                    x:1,
                    y:2
                }
            }
        })

        // 给Vue原型上添加一个方法（vm和vc就都能用了）
        Vue.prototype.hello = ()=> {
            alert('这是啥，这没懂啊')
        }
    }
}
```

- （在main.js中）使用插件：

```js
Vue.use( )
```

```js
// 引入插件
import plugins from "@/plugins";
// 应用（使用）插件
Vue.use(plugins,1,2,3)		//plugins为定义的插件名
```

### 7. scoped样式

>  终端命令`npm view webpack versions` 查看webpack版本

>  终端命令`npm view less-loader versions` 查看less-loader版本

> 终端命令`npm install less-loader`@7 安装7版本中最新的less-loader版本

作用：让样式在局部生效，防止冲突

```js
<style lang="less" scoped>   // 不写lang默认是css
.demo {
  background-color: pink;
  .qwe {
    font-size: 50px;
  }
}
</style>
```

### 8. Todo-list案例

#### 8.1 组件化编码流程（通用）

1. 实现静态组件：抽取组件，使用组件实现静态页面效果

   拆分静态组件：组件要按照功能点拆分，命名不要与html元素冲突

2. 展示动态数据：

   实现动态组件：考虑好数据的存放位置，数据时一个组件在用，还是一些组件在用：

   （1）一个组件在用：放在组件自身即可

   （2）一些组件在用：放在它们共同的父组件上（状态提升）

   2.1. 数据的类型、名称是什么？

   - `[{ id: ' ', name: ' ', ... }, {  }, {  }]`
     - 插入id时可以使用库：`uuid`   或者库：`nanoid`	（终端安装时` npm i nanoid`)

   2.2. 数据保存在哪个组件？

   - 谁用给谁

3. 交互——从绑定事件监听开始

#### 8.2 props适用于：

1. 父组件 ==> 子组件 通信
2. 子组件 ==> 父组件 通信 （要求父先给子一个函数）

#### 8.3  使用v-model时注意

v-model 绑定的值不能是 props 传过来的值，因为 props 是不可以修改的

#### 8.4 props传值注意

props传过来的若是对象类型的值，修改对象中的属性时Vue不会报错，但不推荐这样做

#### 8.5 遍历数组

- forEach( ) 遍历数组，将遍历到的元素传递给回调函数，便利的数组不能是空的，要有值。

```js
[ ].forEach(function(value,index,array){
　　//code something
});
```

- reduce( )逐个遍历数组元素，每一步都将当前元素的值与上一步的计算结果总和相加，直到没有更多的元素被相加。

```js
arr.reduce(callback,[initialValue])
```

> ruduce 为数组中的每一个元素依次执行回调函数，不包括数组中被删除或从未被赋值的元素，接受四个参数：初始值（或者上一个回调函数的返回值），当前元素值，当前索引，调用 reduce 的数组。

```
callback (执行数组中每个值的函数，包含四个参数)
	1.previousValue (上一次调用回调返回的值，或者是提供的初始值（initialValue）)
	2.currentValue (数组中当前被处理的元素)
	3.index (当前元素在数组中的索引)
	4.array (调用 reduce 的数组)
initialValue (作为第一次调用 callback 的第一个参数)
```

#### 8.6 Todo-list案例代码

- 组件OneHeader.vue

```vue
<template>
<div class="todo-header">
  <input type="text" placeholder="请输入你的任务名称，按回车键确认" @keyup.enter="add">
</div>
</template>

<script>
import {nanoid} from 'nanoid'

export default {
  name: "OneHeader",
  props:['addTodo'],
  /* 若input框中存在 v-model="title",则可以配置一下data，再加 methods
  data() {
    return{
      title:''
    }
  },
  methods:{
    add() {
      if(!this.title) return    // 输入框为空时，不存内容
      // 将用户的输入包装成一个todo对象
      const todoObj = {id:nanoid(), title:this.title, done:false}
      // console.log(todoObj)
      this.addTodo(todoObj)
      this.title = ''   // 清空输入框
    }
  }*/

  methods:{
    add(e) {
      // 校验数据
      if(!e.target.value.trim()) return alert('输入不能为空') // 输入框为空时，不存内容   trim() 去掉空格
      // 将用户的输入包装成一个todo对象
      const todoObj = {id:nanoid(), title:e.target.value, done:false}
      // console.log(todoObj)
      // 通知App组件去添加一个todo对象
      this.addTodo(todoObj)
      // 清空输入
      e.target.value = ''   // 清空输入框
    }
  }

}
</script>

<style scoped>
/*header*/
.todo-header input {
  width: 560px;
  height: 28px;
  font-size: 14px;
  border: 1px solid #ccc;
  border-radius: 4px;
  padding: 4px 7px;
}

.todo-header input:hover {
  outline: none;
  border-color: rgba(82,168,236,0.8);
  box-shadow: inset 0 1px 1px rgba(0,0,0,0.072),0 0 8px rgba(82,168,236,0.8);
}
</style>
```

- 组件OneItem.vue

```vue
<template>
  <li>
    <label for="">
<!--      @change="handleCheck(todo.id)或者@click都可，一般用change -->
      <input type="checkbox" :checked="todo.done" @change="handleCheck(todo.id)">
      <!-- 如下代码也能实现功能，但是不太推荐，因为有点违反原则，因为修改了props -->
      <!-- <input type="checkbox" v-model="todo.done"/> -->
      <span>{{todo.title}}</span>
      <button class="btn btn-danger" @click="handleDelete(todo.id)">删除</button>
    </label>
  </li>
</template>

<script>
export default {
  name: "OneItem",
  // 声明接收todo对象
  props:['todo','checkTodo','deleteTodo'],
  methods: {
    // 勾选or取消勾选
    handleCheck(id) {
      // 通知App组件将对应的todo对象的done值取反
      this.checkTodo(id)
    },
    handleDelete(id) {
      // confirm()  根据用户的交互确定布尔值为真还是为假，弹窗有确定和取消，alert弹窗只有确定
      if (confirm('确定删除吗')) {
        // 通知App删除对应id的todo项
        this.deleteTodo(id)
        console.log(id)
      }
    }
  },
}
</script>

<style scoped>
/*item*/
li {
  list-style: none;
  height: 36px;
  line-height: 36px;
  padding: 0 5px;
  border-bottom: 1px solid #ddd;
}

.li label {
  float: left;
  cursor: pointer;
}

li label li input {
  vertical-align: middle;
  margin-right: 6px;
  position: relative;
  top: -1px;
}

li button {
  float: right;
  display: none;
  margin-top: 3px;
}

li:before {
  /*content属性用于动态生成内容，initial将属性设置为其默认值*/
  content: initial;
}

li:last-child {
  border-bottom: none;
}

li:hover {
  background-color: #ddd;
}
li:hover button{
  display: block;
}
</style>
```

- 组件OneList.vue

```vue
<template>
<ul class="todo-main">
  <OneItem
      v-for="todoObj in todos"
      :key="todoObj.id"
      :todo="todoObj"
      :checkTodo="checkTodo"
      :deleteTodo="deleteTodo"
  ></OneItem>
</ul>
</template>

<script>
import OneItem from "@/components/OneItem";

export default {
  name: "OneList",
  components:{OneItem},
  props:['todos','checkTodo','deleteTodo']
}
</script>

<style scoped>
/*main*/
.todo-main {
  margin-left: 0;
  border: 1px solid #ddd;
  border-radius: 2px;
  padding: 0;
}

.todo-empty {
  height: 40px;
  line-height: 40px;
  border: 1px solid #ddd;
  border-radius: 2px;
  padding-left: 5px;
  margin-top: 10px;
}
</style>
```

- 组件OneFooter.vue

```vue
<template>
<!--  如果total是0，就不展示-->
<div class="todo-footer" v-show="total">
  <label for="">
<!--    <input type="checkbox" :checked="isAll" @change="checkAll">-->
<!--    isAll是自己计算出来的，不是props值，可以绑定修改-->
    <input type="checkbox" v-model="isAll">
  </label>
  <span>
    <span>已完成{{doneTotal}}</span> / 全部{{total}}
  </span>
  <button class="btn btn-danger" @click="clearAll">清除已完成任务</button>
</div>
</template>

<script>
export default {
  name: "OneFooter",
  props:['todos','checkAllTodo','clearAllTodo'],
  computed:{
    total() {
      return this.todos.length
    },
    doneTotal(){
      /* forEach() 遍历数组 [ ].forEach(function(value,index,array){code something});
      let i = 0
      this.todos.forEach((todo)=>{
        if (todo.done) i++
      })
      return i*/

      // reduce(function(previousValue, currentValue, currentIndex, array) { /* ... */ }, initialValue)
      /*const x = this.todos.reduce((pre,current)=>{
        return pre + (current.done ? 1 : 0)
      },0)
      console.log('###',x)*/
      // 上式简写
      return this.todos.reduce((pre,todo)=>pre + (todo.done ? 1 : 0),0)
    },
    /*isAll(){    // 简写
      return this.doneTotal === this.total && this.total > 0
    }*/
    isAll:{   //计算属性完整版写法   相当于 （上下）isAll + checkAll
      get(){
        return this.doneTotal === this.total && this.total > 0
      },
      set(value){
        this.checkAllTodo(value)
      }
    }
  },
  methods:{
    /*checkAll(e){
      // e.target  借助DOM，拿到一个属性值 checked
      this.checkAllTodo(e.target.checked)
    },*/
    clearAll(){
      if (confirm('确定删除吗？')){
        this.clearAllTodo()
      }
    }
  }
}
</script>

<style scoped>
/*footer*/
.todo-footer {
  height: 40px;
  line-height: 40px;
  padding-left: 6px;
  margin-top: 5px;
}

.todo-footer label {
  display: inline-block;
  margin-right: 20px;
  cursor: pointer;
}

.todo-footer label input {
  position: relative;
  top: -1px;
  vertical-align: middle;
  margin-right: 5px;
}

.todo-footer button {
  float: right;
  margin-top: 5px;
}
</style>
```

- App.vue

```vue
<template>
<div class="root">
  <div class="todo-container">
    <div class="todo-main">
      <OneHeader :addTodo="addTodo"/>
      <OneList :todos="todos" :checkTodo="checkTodo" :deleteTodo="deleteTodo"/>
      <OneFooter :todos="todos" :checkAllTodo="checkAllTodo" :clearAllTodo="clearAllTodo"/>
    </div>
  </div>
</div>
</template>

<script>
import OneHeader from "@/components/OneHeader";
import OneList from "@/components/OneList";
import OneFooter from "@/components/OneFooter";

export default {
  name: "App",
  components:{
    OneHeader,
    OneList,
    OneFooter
  },
  data() {
    return{
      // 由于todos是MyHeader组件和MyFooter组件都在使用，所以放在App中（状态提升）
      /*todos: [
        /!*{id:'001',title:'吃饭',done:true},
        {id:'002',title:'睡觉',done:false},
        {id:'003',title:'看电视',done:true},*!/
      ]*/
      // JSON.parse()方法用于将一个JSON字符串转换为对象
        // 为 [] 是为了避免TodoList中没有东西时，本地存储记录todos
      todos:JSON.parse(localStorage.getItem('todos')) || []
    }
  },
  methods: {
    // 添加一个todo
    addTodo(toboObj) {
      // console.log('我是App组件，我收到了数据：',x)
      this.todos.unshift(toboObj)
    },
    // 勾选or取消勾选一个todo
    checkTodo(id){
      this.todos.forEach((todo)=>{    // forEach 遍历
        if (todo.id === id) todo.done = !todo.done
          })
    },
    // 删除一个todo
    deleteTodo(id){
      this.todos = this.todos.filter((todo) =>{
        // 函数体，返回过滤的条件
        return todo.id !== id
      })
    },
    // 全选or取消全选
    checkAllTodo(done){
      this.todos.forEach((todo)=>{
        todo.done = done
      })
    },
    // 清除所有已经完成的todo
    clearAllTodo(){
      this.todos = this.todos.filter((todo)=>{
        return !todo.done
      })
    }
  },
  watch:{
    todos:{
      deep:true,
      handler(value) {
        // JSON.stringify()方法用于将JS值转换为JSON字符串
        localStorage.setItem('todos',JSON.stringify(value))
      }
    }
  }
}
</script>

<style>
/*base*/
body {
  background-color: #fff;
}

.btn {
  display: inline-block;
  padding: 4px 12px;
  margin-bottom: 0;
  font-size: 14px;
  line-height: 20px;
  text-align: center;
  vertical-align: middle;
  cursor: pointer;
  box-shadow: inset 0 1px 0 rgba(255,255,255,0.2);
  border-radius: 4px;
}

.btn-danger {
  color: white;
  background-color: #da4f49;
  border: 1px solid #bd362f;
}

.btn-danger:hover {
  color: white;
  background-color: #bd362f;
}

.btn:focus {
  outline: none;
}

.todo-container {
  width: 600px;
  margin: 0 auto;
}
.todo-container .todo-wrap {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
}
</style>
```

#### 8.7 webStorage

1. 存储内容大小一般支持5MB左右（不同浏览器可能还不一样）

2. 浏览器端通过 Window.sessionStorage 和 Window.localStorage 属性来实现本地存储机制

3. 相关API：

   - `xxxxStorage.setItem('key', 'value');`

     该方法接受一个键和值作为参数，会把键值对添加到存储中，如果键名存在，则更新其对应的值

   - `xxxxStorage.getItem('person');`

     该方法接受一个键名作为参数，返回键名对应的值

   - `xxxxStorage.removeItem('key');`

     该方法接受一个键名作为参数，并把该键名从存储中删除

   - `xxxxStorage.clear()`

     该方法会清空存储中的所有数据

4. 备注：

   - SessionStorage存储的内容会随着浏览器窗口关闭而消失
   - LocalStorage存储的内容，需要手动清楚才会消失（清除缓存或借助API清除）
   - `xxxxStorage.getItem(xxx);`如果xxx对应的value获取不到，那么getItem的返回值是null
   - `JSON.parse(null)`的结果依然是null

##### 8.7.1  JSON.parse()和JSON.stringify()

- JSON.parse()方法用于将一个 JSON 字符串转换为对象

```js
JSON.parse(text[,reviver])
// text:一个有效的JSON字符串
// reviver:可选，一个转换结果的函数，将为对象的每个成员调用此函数
```

- JSON.stringify()方法用于将JavaScript值转换为JSON字符串

```js
JSON.stringify(value[,replace[,space]])
// value:要转换的JavaScript值（通常为对象或数组）
// replacer:可选，用于转换结果的函数或数组
// space:可选，文本添加缩进、空格和换行符，如果 space 是一个数字，则返回值文本在每个级别缩进指定数目的空格，如果 space 大于 10，则文本缩进 10 个空格。space 也可以使用非数字，如：\t
```

### 9. Vue中的自定义事件

#### 9.1 子传父

通过父组件给子组件传递函数类型的props实现：子给父传递数据

- 先定义一个函数

```js
methods:{
    getSchoolName(name){
      console.log('App收到了学校名：',name)
    }
  }
```

- 再把函数给模板中的元素

```js
<!--    通过父组件给子组件传递函数类型的props实现：子给父传递数据-->
<MySchool :getSchoolName="getSchoolName"/>
```

- 元素子组件再声明接收函数

```js
 props:['getSchoolName']
```

- 在子组件中调用函数
  - 例如：先创建一个button，设置点击事件@click="sendSchoolName"(to App)，配置methods通过点击事件调用getSchoolName

```js
 methods:{
    sendSchoolName(){
      // this.getSchoolName调用函数，传值this.name
      this.getSchoolName(this.name)
    }
  }
```

#### 9.2 自定义事件绑定

##### 9.2.1 第一种写法：使用@或v-on

- 在App.vue中给组件实例对象vc绑定事件   `v-on:事件名="函数名"`

```js
<!--    通过父组件给子组件绑定一个自定义事件实现：子给父传递数据（第一种写法，使用@或v-on）-->
<MyStudent v-on:atxx="getStudentName"/>
```

- 没有getStudentName函数，在methods中配置

```js
methods:{
    getStudentName(){
      console.log('getStudentName被调用了！')
    }
}
```

- 触发事件$emit（给谁绑定就找谁触发）
  - 例如：先创建一个button，设置点击事件@click="sendStudentName"(to App)，配置methods通过点击事件触发MyStudent组件实例身上的atxx事件

```js
methods:{
    sendStudentName(){
      // $emit() 触发MyStudent组件实例身上的atxx事件
      this.$emit('atxx',this.name)
    }
  }
```

- 回到App.vue给getStudentName函数加上形参name

```js
methods:{
    getStudentName(name){
      console.log('App收到了学生名：',name)
    }
}
```

##### 9.2.2 子传父和自定义事件$emit的用法

- 父组件可以使用props把数据传给子组件（props一层一层的传）
- 子组件可以使用$emit，让父组件监听到自定义事件（$emit子组件直接触发）

`vm.$emit(event, arg)` // 触发当前实例上的事件

`vm.$on(event, fn)`  // 监听event事件后运行fn

##### 9.2.3 第二种写法：使用ref

- 在App.vue中给组件实例对象vc绑定事件

```js
<MyStudent ref="student"/>
```

- 在methods中配置getStudentName函数

```js
// name,...params    不管传多少,第一个参数作为name形参接收，剩余的整理到数组params
    getStudentName(name,...params){
      console.log('App收到了学生名：',name,params)
    }
```

- 触发事件$emit（给谁绑定就找谁触发）
  - 例如：先创建一个button，设置点击事件@click="sendStudentName"(to App)，配置methods通过点击事件触发MyStudent组件实例身上的atxx事件

```js
methods:{
    sendStudentName(){
      // $emit() 触发MyStudent组件实例身上的atxx事件
      this.$emit('atxx',this.name)
    }
  }
```

- 配置钩子mounted

```js
mounted() {   // App挂在完毕
    // $on 的作用就是把atxx事件绑定到student的vc上，所以MyStudent组件才能$emit调用
    this.$refs.student.$on('atxx',this.getStudentName)
    // $once只触发一次
    // this.$refs.student.$once('atxx',this.getStudentName)
    /*// 延迟3秒
    setTimeout(()=>{
      this.$refs.student.$on('atxx',this.getStudentName)
    },3000)*/
  }
```

#### 9.3 自定义事件解绑

##### 9.3.1 解绑一个事件

- 创一个`button`，设置点击事件`unbind`

```js
methods:{
    unbind(){
        this.$off('unbind');		// $off解绑
    }
}
```

##### 9.3.2 解绑多个事件

- 借助$off解绑多个自定义事件

```
methods:{
    unbind(){
        this.$off(['unbind','demo']);		// $off解绑，后跟事件名
    }
}
```

- 借助$off解绑所有自定义事件

```js
methods:{
    unbind(){
        this.$off();		// $off解绑，后跟事件名
    }
}
```

- 设置点击事件`death`，借助`$destroy`销毁当前组件的实例vc

```js
death(){
    this.$destroy()		// 销毁了当前组件的实例，销毁后该组件所有实例的自定义事件都不奏效
}
```

##### 9.3.3 this指向

- 箭头函数没有自己的this，箭头函数在哪儿定义，this永远指向父级函数this，且this指向不可改变，即作为属性添加到另一个对象上也是如此
- 普通函数作为方法添加到另一个对象上，this指向由调用者决定。作用域与this指向是分开的

##### 9.3.4 修饰符native

```js
<!--    修饰符native，把click给组件的template-->
    <MyStudent ref="student" @click.native="show"/>
// 点击MyStudent组件，触发show事件
```

#### 9.4 自定义事件总结

1. 一种组件间通信的方式，适用于：子组件 ===> 父组件

2. 使用场景：A是父组件，B是子组件，B想给A传数据，那么就要在A中给B绑定自定义事件（事件的回调在A中），B触发事件

3. 绑定自定义事件：

   （1）第一种方式，在父组件中：`<Demo @atxx="test"/>` 或`<Demo v-on:atxx="test"/>` 

   （2）第二种方式，在父组件中：

   ```js
   <Demo ref="demo"/>
       ......
   mounted(){
       this.$refs.xxx.$on('atxx',this.test)
   }
   ```

   （3）若想让自定义事件只能触发一次，可以使用`once` 修饰符，或`$once` 方法

4. 触发自定义事件：`this.$emit('atxx', 数据)` 

5. 解绑自定义事件：`this.$off('atxx')` 

6. 组件上也可以绑定原生DOM事件，需要`native` 修饰符

7. 注意：通过`this.$refs.xxx.$on('atxx', 回调)` 绑定自定义事件时，回调要么配置在methods中，要么用箭头函数，否则this指向会出问题

### 10. 全局事件总线(GlobalEventBus)

1. 一种组件间通信的方式，适用于任意组件间通信
2. main.js中安装全局事件总线

```js
new Vue({
    ......
    beforeCreate(){
    // 生命周期钩子中的this本身就是Vue实例vm
    	Vue.prototype.$bus = this	// 安装全局事件总线，$bus就是当前应用的vm
	},
    ......
})
```

3. 使用事件总线

   （1）接收数据：A组件想接收数据，则在A组件中给$bus绑定自定义事件，事件的回调留在A组件自身

```js
methods:{
    demo(data){......}
}
.....
mounted(){
    // $on\$off\$emit 都在Vue的原型对象上
    this.$bus.$on('xxx',this.demo)
    /* 不使用demo时
    this.$bus.$on('hello',(data)=>{
      console.log('MySchool组件收到了数据',data)
    })*/
},
beforeDestroy() {
    this.$bus.$off('hello')
}
```

   （2）提供数据：`this.$bus.$emit('xxx', 数据)` 

```js
// 设置点击事件sendStudentName
methods:{
    sendStudentName() {
      this.$bus.$emit('hello',this.name)
    }
  }
```

4. 最好在`beforeDestroy`钩子中，用`$off`去解绑当前组件所用到的事件

### 11. 消息订阅与发布(subpub)

1. 一种组件间通信的方式，适用于任意组件间通信

2. 使用步骤：

   1. 安装pubsub：`npm i pubsub-js` 
   2. 引入：`import pubsub from 'pubsub-js'` 
   3. 接收数据：A组件想接收数据，则在A组件中订阅消息，订阅的回调留在A组件自身

   ```js
   methods:{
       demo(data){......}
   }
   ......
   mounted(){
       this.pId = pubsub.subscribe('xxx',this.demo)	// 订阅消息
       /* 不使用demo时
       this.pubId = pubsub.subscribe('hello',(msgName,data) => {
       console.log('有人发布了hello消息，hello消息的回调执行了',msgName,data)
       }) */
   },
   beforeDestroy(){
       pubsub.unsubscribe(this.pId)
   }
   ```

   4. 提供数据：`pubsub.publish('xxx',数据)` 

   ```js
   methods:{
       sendStudentName() {
         pubsub.publish('hello',666)
       }
     }
   ```

   5. 最好在beforeDestroy钩子中，用`pubsub.unsubscribe(pId)` 去<span style='color:red'>取消订阅</span>。

### 12. $nextTick

1. 语法：`this.$nextTick(回调函数)` 
2. 作用：在下一次 DOM 更新结束后执行其指定的回调
3. 什么时候用：当改变数据后，要基于更新后的新DOM进行某些操作，要在nextTick所指定的回调函数中执行

```js
methods:{
    handleEdit(todo){
      // 借助 hasOwnProperty 这个API，看自己身上有没有某个属性
      if(todo.hasOwnProperty.call('isEdit')){
        todo.isEdit = true
      }else {
        console.log('todo身上没有isEdit')
        this.$set(todo,'isEdit',true)   //添加'isEdit'就是一个响应式的
      }
      /*setTimeout(()=>{
        this.$refs.inputTitle.focus()
      },200)*/
      //$nextTick 所指定的回调函数会在DOM节点更新后再执行
      this.$nextTick(function (){
        this.$refs.inputTitle.focus()
      })
    }
}
```

### 13. 过渡与动画

- 作用：在插入、更新或移除DOM元素时，在合适的时候给元素添加样式类名
- 图示

![Vue封装的过渡与动画](./static/images/Vue%E5%B0%81%E8%A3%85%E7%9A%84%E8%BF%87%E6%B8%A1%E4%B8%8E%E5%8A%A8%E7%94%BB.png)

- 写法

  1. 准备好样式：

     - 元素进入的样式：

     1. v-enter：进入的起点
     2. v-enter-active：进入的过程
     3. v-enter-to：进入的终点

     - 元素离开的样式：

     1. v-leave：离开的起点
     2. v-leave-active：离开的过程
     3. v-leave-to：离开的终点

  2. 使用`<transition>` 包裹要过渡的元素，并配置name属性：
  
  ```vue
  <transition name="hello" appear>
      <h2 v-show="isShow">你好啊！</h2>
    </transition>
  ```
  
  3. 要让页面一开始就显示动画，需要添加`appear` 
  4. 备注：若有多个元素需要过渡，则需要使用：`<transition-group>` ，且每个元素都要指定 key 值

#### 13.1 单个元素过渡代码

```vue
<template>
<div>
  <button @click="isShow = !isShow">显示/隐藏</button>
<!--  transition 过渡,style用v-enter/leave-active
      若transition有了name，则style应该用'name'-enter/leave-active
      apper属性，即使有效果，也可以写成  :apper="true"  -->
  <transition name="hello" appear>
    <h2 v-show="isShow">你好啊！</h2>
  </transition>
</div>
</template>

<script>
export default {
  name: "MyTest",
  data(){
    return{
      isShow:true
    }
  },

}
</script>

<style scoped>
h2{
  background-color: orange;
}

/* transition没有name时，可直接用 v-enter-active */
.hello-enter-active{	
  animation: atxx 1s linear;
}

.hello-leave-active{
  animation: atxx 1s linear reverse;
}

/*动画*/
@keyframes atxx{
  from{
    transform: translateX(-100%);
  }
  to{
    transform: translateX(0px);
  }
}
</style>
```

#### 13.2 多个元素过渡代码

```vue
<template>
  <div>
    <button @click="isShow = !isShow">显示/隐藏</button>
    <!--  transition 过渡,style用v-enter/leave-active
          若transition有了name，则style应该用'name'-enter/leave-active
          apper属性，即使有效果，也可以写成  :apper="true"  -->
    <transition-group name="hello" appear>
      <h2 v-show="isShow" key="1">你好啊！</h2>
      <h2 v-show="isShow" key="2">小猪佩奇</h2>
    </transition-group>
  </div>
</template>

<script>
export default {
  name: "MyTest2",
  data(){
    return{
      isShow:true
    }
  },

}
</script>

<style scoped>
h2{
  background-color: orange;
}

/*进入的起点，离开的终点*/
.hello-enter,.hello-leave-to{
transform: translateX(-100%);
}
.hello-enter-active,.hello-leave-active{
  transition: 1s;
}
/*进入的终点，离开的起点*/
.hello-enter-to,.hello-leave{
transform: translateX(0);
}

</style>
```

#### 13.3 第三方库 Animate.css

- 名称：Animate.css
- 安装：npm i animate.css
- 引入：import 'animate.css'

```vue
<template>
  <div>
    <button @click="isShow = !isShow">显示/隐藏</button>
    <!--  transition 过渡,style用v-enter/leave-active
          若transition有了name，则style应该用'name'-enter/leave-active
          apper属性，即使有效果，也可以写成  :apper="true"  -->
    <transition-group
        name="animate__animated animate__bounce"
        enter-active-class="animate__swing"
        leave-active-class="animate__backOutUp"
        appear>
      <h2 v-show="isShow" key="1">你好啊！</h2>
      <h2 v-show="isShow" key="2">小猪佩奇</h2>
    </transition-group>
  </div>
</template>

<script>
import 'animate.css'

export default {
  name: "MyTest2",
  data(){
    return{
      isShow:true
    }
  },

}
</script>

<style scoped>
h2{
  background-color: orange;
}
</style>
```

#### 13.4 TodoList案例代码

- 方案一：添加在`OneItem` 组件内

```vue
<template>
  <transition name="todo" appear>
    <li>
      <label for="">
        <!--      @change="handleCheck(todo.id)或者@click都可，一般用change -->
        <input type="checkbox" :checked="todo.done" @change="handleCheck(todo.id)">
        <!-- 如下代码也能实现功能，但是不太推荐，因为有点违反原则，因为修改了props -->
        <!-- <input type="checkbox" v-model="todo.done"/> -->
        <span v-show="!todo.isEdit">{{todo.title}}</span>
        <!--      失去焦点事件@blur-->
        <input
            type="text"
            v-show="todo.isEdit"
            :value="todo.title"
            @blur="handleBlur(todo,$event)"
            ref="inputTitle"
        >
      </label>
      <button class="btn btn-danger" @click="handleDelete(todo.id)">删除</button>
      <button v-show="!todo.isEdit" class="btn btn-edit" @click="handleEdit(todo)">编辑</button>
    </li>
  </transition>
</template>

<script>
import pubsub from "pubsub-js";

export default {
  name: "OneItem",
  // 声明接收todo对象
  props:['todo'],
  methods: {
    // 勾选or取消勾选
    handleCheck(id) {
      // 通知App组件将对应的todo对象的done值取反
      // this.checkTodo(id)
      this.$bus.$emit('checkTodo',id)
    },
    handleDelete(id) {
      // confirm()  根据用户的交互确定布尔值为真还是为假，弹窗有确定和取消，alert弹窗只有确定
      if (confirm('确定删除吗')) {
        // 通知App删除对应id的todo项
        // this.deleteTodo(id)
        // console.log(id)
        // this.$bus.$emit('deleteTodo',id)
        pubsub.publish('deleteTodo',id)
      }
    },
    // 编辑
    handleEdit(todo){
      // 借助 hasOwnProperty 这个API，看自己身上有没有某个属性
      if(todo.hasOwnProperty.call('isEdit')){
        todo.isEdit = true
      }else {
        console.log('todo身上没有isEdit')
        this.$set(todo,'isEdit',true)   //添加'isEdit'就是一个响应式的
      }
      /*setTimeout(()=>{
        this.$refs.inputTitle.focus()
      },200)*/
      //$nextTick 所指定的回调函数会在DOM节点更新后再执行
      this.$nextTick(function (){
        this.$refs.inputTitle.focus()
      })
    },
    // 失去焦点回调（真正执行修改逻辑）
    handleBlur(todo,e){
      todo.isEdit = false
      if(!e.target.value.trim()) return alert('输入不能为空！')
      this.$bus.$emit('updateTodo',todo.id,e.target.value)
    }
  },
}
</script>

<style scoped>
/*item*/
li {
  list-style: none;
  height: 36px;
  line-height: 36px;
  padding: 0 5px;
  border-bottom: 1px solid #ddd;
}

.li label {
  float: left;
  cursor: pointer;
}

li label li input {
  vertical-align: middle;
  margin-right: 6px;
  position: relative;
  top: -1px;
}

li button {
  float: right;
  display: none;
  margin-top: 3px;
}

li:before {
  /*content属性用于动态生成内容，initial将属性设置为其默认值*/
  content: initial;
}

li:last-child {
  border-bottom: none;
}

li:hover {
  background-color: #ddd;
}
li:hover button{
  display: block;
}

.todo-enter-active {
  animation: atxx 1s linear;
}
.todo-leave-active{
  animation: atxx 1s linear reverse;
}
@keyframes atxx {
  from{
    transform: translateX(100%);
  }
  to{
    transform: translateX(0px);
  }
}
</style>
```

- 方案二：添加在`OneList`组件内

```vue
<template>
<ul class="todo-main">
  <transition-group name="todo" appear>
    <OneItem
        v-for="todoObj in todos"
        :key="todoObj.id"
        :todo="todoObj"
    ></OneItem>
  </transition-group>
</ul>
</template>

<script>
import OneItem from "@/components/OneItem";

export default {
  name: "OneList",
  components:{OneItem},
  props:['todos']
}
</script>

<style scoped>
/*main*/
.todo-main {
  margin-left: 0;
  border: 1px solid #ddd;
  border-radius: 2px;
  padding: 0;
}

.todo-empty {
  height: 40px;
  line-height: 40px;
  border: 1px solid #ddd;
  border-radius: 2px;
  padding-left: 5px;
  margin-top: 10px;
}

.todo-enter-active {
  animation: atxx 1s linear;
}
.todo-leave-active{
  animation: atxx 1s linear reverse;
}
@keyframes atxx {
  from{
    transform: translateX(100%);
  }
  to{
    transform: translateX(0px);
  }
}
</style>
```

## 四、Vue 中的 Ajax

### 1. 配置代理

配置参考文档https://cli.vuejs.org/zh/config/中查找`devServer.proxy` 

脚手架配置代理首先得下载`axios` 库，命令`npm install axios` 

`vue.config.js` 是一个可选的配置文件（和 `package.json` 同级），如果项目的根目录中存在这个文件，那么它会被`@vue/cli-service` 自动加载。

#### 1.1 方式一

在`vue.config.js` 中添加如下配置

```js
module.exports = {
  // 开启代理服务器，端口号写服务端端口号
  devServer: {
    proxy: 'http://127.0.0.1:5000'
  },
}
```

- 优点：配置简单，请求资源时直接发送给前端（8080）即可
- 缺点：不能配置多个代理，不能灵活的控制请求是否走代理
- 工作方式：若按照上述配置代理，当请求了前端不存在的资源时，那么该请求会转发给服务器（优先匹配前端资源，也就是`public` 文件夹中的资源）

`APP.vue` 中代码

```vue
<template>
  <div>
  <button @click="getStudents">获取学生信息</button>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: "App",
  methods:{
    getStudents(){
      axios.get('http://localhost:8080/students').then(
          response => {
            console.log('请求成功了',response.data)
          },
          error => {
            console.log('请求失败',error.message)
          }
      )
    }
  },
}
</script>
```

#### 1.2 方式二

在`vue.config.js` 中添加如下配置

```js
module.exports = {
// 开启代理服务器（方式二）
  devServer: {
    proxy: {
      '/api1': {// 匹配所有以 '/api1'开头的请求路径
        target: 'http://localhost:5000',// 代理目标的基础路径
        changeOrigin: true,
        pathRewrite: {'^/api1': ''}//代理服务器将请求地址转给真实服务器时会将 /api1 去掉
      },
      '/api2': {// 匹配所有以 '/api2'开头的请求路径
        target: 'http://localhost:5001',// 代理目标的基础路径
        changeOrigin: true,
        pathRewrite: {'^/api2': ''}
      }
    }
  }
/*changeOrigin设置为true时，服务器收到的请求头中的host为：localhost:5000
changeOrigin设置为false时，服务器收到的请求头中的host为：localhost:8080
changeOrigin默认值为true*/
}
```

- 优点：可以配置多个代理，且可以灵活的控制请求是否走代理
- 缺点：配置略微繁琐，请求资源时必须加前缀

#### 1.3 代码展示

![配置代理目录文件](./static/images/%E9%85%8D%E7%BD%AE%E4%BB%A3%E7%90%86%E7%9B%AE%E5%BD%95%E6%96%87%E4%BB%B6.png)

`APP.vue` 中代码

```vue
<template>
  <div>
  <button @click="getStudents">获取学生信息</button>
  <button @click="getCars">获取汽车信息</button>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  name: "App",
  methods:{
    /*getStudents(){    // 不走代理
      axios.get('http://localhost:5000/students').then(*/
      getStudents(){    // 走方式一代理
      axios.get('http://localhost:8080/api1/students').then(
          response => {
            console.log('请求成功了',response.data)
          },
          error => {
            console.log('请求失败',error.message)
          }
      )
    },
    getCars(){    // 走方式二代理
      axios.get('http://localhost:8080/api2/cars').then(
          response => {
            console.log('请求成功了',response.data)
          },
          error => {
            console.log('请求失败',error.message)
          }
      )
    }
  },
}
</script>
```

`vue.config.js` 中代码

```js
const { defineConfig } = require('@vue/cli-service')
module.exports = defineConfig({
  transpileDependencies: true
})

module.exports = {
  pages: {
    index: {
      // 入口
      entry: 'src/main.js',
    },
  },

  lintOnSave:false ,  // 关闭语法检查

  // 开启代理服务器，端口号写服务端端口号(方式一）
  /*devServer: {
    proxy: 'http://localhost:5000'
  },*/

  // 开启代理服务器（方式二）
  devServer: {
    proxy: {
      '/api1': {// 匹配所有以 '/api1'开头的请求路径
        target: 'http://localhost:5000',// 代理目标的基础路径
        ws: true,   // 用于支持websocket
        changeOrigin: true,
        pathRewrite: {'^/api1': ''}//代理服务器将请求地址转给真实服务器时会将 /api1 去掉
        // 匹配所有以 api1 为开头的路径
      },
      '/api2': {// 匹配所有以 '/api2'开头的请求路径
        target: 'http://localhost:5001',// 代理目标的基础路径
        changeOrigin: true,
        pathRewrite: {'^/api2': ''}
      }
    }
  }
/*changeOrigin设置为true时，代理服务器撒谎掩盖，服务器收到的请求头中的host为：localhost:5000
changeOrigin设置为false时，代理服务器实话实说，服务器收到的请求头中的host为：localhost:8080
changeOrigin默认值为true*/
}
```

`server1.js` 中代码

```js
// 1. 引入 express
const express = require('express')
const {request, response} = require("express");

// 2. 创建应用对象
const app = express();

// 有人请求服务器时
app.use((request,response,next)=>{
    console.log('有人请求服务器1了');
    console.log('请求的资源是：',request.url);
    console.log('请求来自于',request.get('Host'));
    next()
})

// 3. 创建路由规则
// request 是对请求报文的封装
// response 是对响应报文的封装
// axios 服务
app.all('/students', (request, response)=> {
    // 设置响应头  设置允许跨域
    response.setHeader('Access-Control-Allow-Origin', '*')
    response.setHeader('Access-Control-Allow-Headers', '*')
    const students = [
        {name: '小猪佩奇', age: 18},
        {name: 'tom', age: 23},
        {name: 'tony', age: 12}
    ]
    response.send(JSON.stringify(students));
})

// 4. 监听端口启动服务
app.listen(5000, ()=>{
    console.log('服务已经启动， 5000 端口监听中')
})
```

`server2.js` 中代码

```js
// 1. 引入 express
const express = require('express')
const {request, response} = require("express");

// 2. 创建应用对象
const app = express();

// 有人请求服务器时
app.use((request,response,next)=>{
    console.log('有人请求服务器1了');
    console.log('请求的资源是：',request.url);
    console.log('请求来自于',request.get('Host'));
    next()
})

// 3. 创建路由规则
// request 是对请求报文的封装
// response 是对响应报文的封装
// axios 服务
app.all('/cars', (request, response)=> {
    // 设置响应头  设置允许跨域
    response.setHeader('Access-Control-Allow-Origin', '*')
    response.setHeader('Access-Control-Allow-Headers', '*')
    const cars = [
        {name: '什么车', price: 18},
        {name: '车2', price: 23},
        {name: '车3', price: 12}
    ]
    response.send(JSON.stringify(cars));
})

// 4. 监听端口启动服务
app.listen(5001, ()=>{
    console.log('服务已经启动， 5001 端口监听中')
})
```

### 2. Github 用户搜索案例

**样式引入方式一：** 

- 在`components` 文件夹中创建`assets/css` 目录，导入`.css` 文件，在`APP.vue` 组件中引入 CSS 样式

```vue
import './assets/css/bootstrap.css'
```

**样式引入方式二：** 

- `public` 文件夹中创建`css` 目录，导入`.css` 文件，在`index.html` 文件中引入 CSS 样式

```html
<!--引入第三方样式-->
    <link rel="stylesheet" href="<%= BASE_URL %>css/bootstrap.css">
```

**GitHub 免费提供的接口地址**  https://api.github.com/search/users?q=xxx

`index.html` 中代码

```html
<!DOCTYPE html>
<html lang="">
  <head>
    <meta charset="utf-8">
<!--针对IE浏览器的一个特殊配置，含义是让IE浏览器以最高的渲染级别渲染页面-->
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
<!--开启移动端的理想视口-->
    <meta name="viewport" content="width=device-width,initial-scale=1.0">
<!--配置页签图标    <%= BASE_URL %> 指的是 public所在的路径-->
    <link rel="icon" href="<%= BASE_URL %>favicon.ico">

<!--引入第三方样式-->
    <link rel="stylesheet" href="<%= BASE_URL %>css/bootstrap.css">

<!--配置网页标题   <%= htmlWebpackPlugin.options.title %> 指向 package.json中的name-->
    <title><%= htmlWebpackPlugin.options.title %></title>
  </head>
  <body>
<!--当浏览器不支持js的时候，noscript中的元素就会被渲染-->
    <noscript>
      <strong>We're sorry but <%= htmlWebpackPlugin.options.title %> doesn't work properly without JavaScript enabled. Please enable it to continue.</strong>
    </noscript>
<!--容器-->
    <div id="app"></div>
    <!-- built files will be auto injected -->
  </body>
</html>
```

`OneSearch.vue` 中代码

```vue
<template>
  <section class="jumbotron">
    <h3 class="jumbotron-heading">Search Github Users</h3>
    <div>
      <input type="text" placeholder="enter the name you search" v-model="keyWord"/>&nbsp;
      <button @click="searchUsers">Search</button>
    </div>
  </section>
</template>

<script>
import axios from "axios";

export default {
  name: "OneSearch",
  data(){
    return{
      keyWord:''
    }
  },
  /*methods:{
    searchUsers(){
      // 请求前更新List的数据
      this.$bus.$emit(`updateListData`,false,true,'',[])
      // `...`  模板字符串
      axios.get(`https://api.github.com/search/users?q=${this.keyWord}`).then(
        response => {
          console.log('请求成功了')
          // this.$emit('自定义事件名',要传送的数据)
          // this.$bus.$emit('updateListData',response.data.items)
          // 把数据给 List 可以用 全局事件总线bus 或 消息订阅与发布pubsub
          this.$bus.$emit(`updateListData`,false,false,'',response.data.items)

        },
          error => {
            console.log('请求失败了',error.message)
          }
      )
    }
  }*/
  methods:{
    searchUsers(){
      // 请求前更新List的数据
      this.$bus.$emit(`updateListData`, {isFirst:false, isLoading:true, errMsg:'', users:[]})
      // `...`  模板字符串
      axios.get(`https://api.github.com/search/users?q=${this.keyWord}`).then(
          response => {
            console.log('请求成功了')
            // this.$emit('自定义事件名',要传送的数据)
            // this.$bus.$emit('updateListData',response.data.items)
            // 把数据给 List 可以用 全局事件总线bus 或 消息订阅与发布pubsub
      // 请求成功后更新List的数据
            this.$bus.$emit(`updateListData`, {isLoading:false, errMsg:'',users:response.data.items})


          },
          error => {
      // 请求失败后更新List的数据
            this.$bus.$emit(`updateListData`, {isLoading:false, errMsg:error.message, users:[]})

          }
      )
    }
  }

}
</script>
```

`OneList.vue` 中的代码

```vue
<template>
  <div class="row">
<!--展示用户列表-->
<!--v-for="user in users" 遍历生成，只要涉及v-for遍历，:key就必须写上-->
    <div v-show="info.users.length" class="card" v-for="user in info.users" :key="user.login">
<!--html_url  点击跳转的地址-->
      <a :href="user.html_url" target="_blank">
<!--avatar_url  用户头像地址-->
        <img :src="user.avatar_url" style='width: 100px'/>
      </a>
<!--login  用户的登录名-->
      <p class="card-text">{{user.login}}</p>
    </div>
<!--展示欢迎词-->
    <h1 v-show="info.isFirst">欢迎使用！</h1>
<!--展示加载中-->
    <h1 v-show="info.isLoading">加载中...</h1>
<!--展示错误信息-->
    <h1 v-show="info.errMsg">{{info.errMsg}}</h1>
  </div>
</template>

<script>
export default {
  name: "OneList",
  data() {
    return {
      info:{
        isFirst: true,
        isLoading: false,
        errMsg: '',
        users: [],
      }

    }
  },
  /*mounted() {   // 挂载前接收数据
    // VM.$on('事件名',callback)   callback回调$emit要传送的数据
    this.$bus.$on('updateListData',(isFirst,isLoading,errMsg,users)=>{
      // console.log('我是List组件，收到数据：',users)
      this.isFirst = isFirst
      this.isLoading = isLoading
      this.errMsg = errMsg
      // 收到的数据 users 存到自身 this.users
      this.users = users
    })
  }
}*/

  mounted() {
    this.$bus.$on('updateListData', (dataObj) => {
      this.info = {...this.info,...dataObj}
    })
  }
}
</script>

<style scoped>
.album {
  min-height: 50rem; /* Can be removed; just added for demo purposes */
  padding-top: 3rem;
  padding-bottom: 3rem;
  background-color: #f7f7f7;
}

.card {
  float: left;
  width: 33.333%;
  padding: .75rem;
  margin-bottom: 2rem;
  border: 1px solid #efefef;
  text-align: center;
}

.card > img {
  margin-bottom: .75rem;
  border-radius: 100px;
}

.card-text {
  font-size: 85%;
}
</style>
```

`APP.vue` 中代码

```vue
<template>
  <div class="container">
    <OneSearch/>
    <OneList/>
  </div>
</template>

<script>
import OneSearch from "@/components/OneSearch";
import OneList from "@/components/OneList";

export default {
  name: "App",
  components: {OneSearch, OneList},
}
</script>

<style>
    
</style>
```

### 3. vue-resource 库

`vue` 项目常用的两个`Ajax` 库

1. `axios` ：通用的 Ajax 请求库，官方推荐，效率高
2. `vue-resource` ：vue 插件库，vue 1.x 使用广泛，官方现已不维护

下载`vue-resource` 库`npm i vue-resource` 

**使用**：跟`axios` 使用极其类似，差别于在`main.js` 中引入并使用插件

```js
// 引入插件
import vueResource from 'vue-resource'
// 使用插件
Vue.use(vueResource)
```

在`OneSearch.vue` 中不再引入，直接使用`this.$http.get()` 和`axios.get()` 类似

```js
      this.$http.get(`https://api.github.com/search/users?q=${this.keyWord}`).then(
		response => {},
          error => {})
```

### 4. slot 插槽

作用：让父组件可以向子组件指定位置插入`html` 结构，也是一种组件间通信的方式，适用于 **父组件==>子组件** 

#### 4.1 默认插槽

`Category.vue` 子组件代码

```vue
<template>
<div class="Category">
  <h3>{{title}}分类</h3>
<!--定义一个插槽（挖个坑，等着组件的使用者进行填充-->
  <slot>我是一个默认值，当使用者没有传递具体结构时，我会出现</slot>
</div>
</template>

<script>
export default {
  name: "Category",
  props:['title']
}
</script>

<style scoped>
.Category{
  background-color: skyblue;
  width: 200px;
  height: 300px;
}
h3 {
  text-align: center;
  background-color: orange;
}

img {
  width: 100%;
}
video{
  width: 100%;
}
</style>
```

`APP.vue` 父组件代码

```vue
<template>
  <div class="container">
    <Category title="美食">
      <img src="https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fpic1.win4000.com%2Fwallpaper%2Ff%2F57a42b9002e19.jpg&refer=http%3A%2F%2Fpic1.win4000.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=auto?sec=1672727500&t=bd14d062321b1de9b25f543e3670cee2" alt="">
    </Category>

    <Category title="游戏">
      <ul>
        <li v-for="(g,index) in games" :key="index">{{g}}</li>
      </ul>
    </Category>

    <Category title="电影">
      <video controls src="http://clips.vorwaerts-gmbh.de/big_buck_bunny.mp4"></video>
    </Category>
  </div>
</template>

<script>
import Category from "@/components/Category";

export default {
  name: "App",
  components: {Category},
  data(){
    return{
      foods:['火锅','烧烤','蛋糕','奶茶'],
      games:['红色预警','穿越火线','超级玛丽','QQ飞车'],
      films:['拆弹专家','萌宠成长记','小猪佩奇','猪猪传奇']
    }
  },
}
</script>

<style>
.container{
  display: flex;
  /*justify-content 主轴（横轴）方向上的对齐方式 ， space-around 每个元素周围分配相同的空间*/
  justify-content: space-around;
}
</style>
```

#### 4.2 具名插槽

父组件指明放入子组件的哪个插槽`slot="footer"` ，如果是`template` 可以写成`v-slot: footer` 

`Category.vue` 子组件代码

```vue
<template>
<div class="Category">
  <h3>{{title}}分类</h3>
<!--定义一个插槽（挖个坑，等着组件的使用者进行填充-->
  <slot name="center">我是一个默认值，当使用者没有传递具体结构时，我会出现1</slot>
  <slot name="footer">我是一个默认值，当使用者没有传递具体结构时，我会出现2</slot>
</div>
</template>

<script>
export default {
  name: "Category",
  props:['title']
}
</script>

<style scoped>
.Category{
  background-color: skyblue;
  width: 200px;
  height: 300px;
}
h3 {
  text-align: center;
  background-color: orange;
}

img {
  width: 100%;
}
video{
  width: 100%;
}
</style>
```

`APP.vue` 父组件代码

```vue
<template>
  <div class="container">
    <Category title="美食">
      <img slot="center" src="https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fpic1.win4000.com%2Fwallpaper%2Ff%2F57a42b9002e19.jpg&refer=http%3A%2F%2Fpic1.win4000.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=auto?sec=1672727500&t=bd14d062321b1de9b25f543e3670cee2" alt="">
      <a slot="footer" href="http://baidu.com">更多美食</a>
    </Category>

    <Category title="游戏">
      <ul slot="center">
        <li v-for="(g,index) in games" :key="index">{{g}}</li>
      </ul>
      <div class="foot" slot="footer">
        <a href="http://baidu.com">单机游戏</a>
        <a href="http://baidu.com">网络游戏</a>
      </div>
    </Category>

    <Category title="电影">
      <video slot="center" controls src="http://clips.vorwaerts-gmbh.de/big_buck_bunny.mp4"></video>
      <template v-slot:footer>
        <div class="foot">
          <a href="http://baidu.com">经典</a>
          <a href="http://baidu.com">热门</a>
          <a href="http://baidu.com">推荐</a>
        </div>
        <h4 slot="footer">欢迎前来观影</h4>
      </template>
    </Category>
  </div>
</template>

<script>
import Category from "@/components/Category";

export default {
  name: "App",
  components: {Category},
  data(){
    return{
      foods:['火锅','烧烤','蛋糕','奶茶'],
      games:['红色预警','穿越火线','超级玛丽','QQ飞车'],
      films:['拆弹专家','萌宠成长记','小猪佩奇','猪猪传奇']
    }
  },
}
</script>

<style>
.container, .foot{
  display: flex;
  /*justify-content 主轴（横轴）方向上的对齐方式 ， space-around 每个元素周围分配相同的空间*/
  justify-content: space-around;
}
h4{
  text-align: center;
}

</style>
```

#### 4.3 作用域插槽

理解：数据在组件的自身（子组件），但根据数据生成的结构需要组件的使用者（父组件）来决定。（`games`数据在`Category`（子）组件中，但使用数据所遍历出来的结构由`App`（父）组件决定）

APP父组件中`scope="shoushuju"` 、`scope={games}` 、`slot-scope={games}` 都用于接收子组件 Category 中 `:games="games"` 的数据

**vue3中 scope 已被 v-slot="" 替代** 

`Category.vue` 子组件代码

```vue
<template>
<div class="Category">
  <h3>{{title}}分类</h3>
  <slot :games="games">一个插槽</slot>

</div>
</template>

<script>
export default {
  name: "Category",
  props:['title'],
  data(){
    return{
      games:['红色预警','穿越火线','超级玛丽','QQ飞车'],
    }
  },
}
</script>

<style scoped>
.Category{
  background-color: skyblue;
  width: 200px;
  height: 300px;
}
h3 {
  text-align: center;
  background-color: orange;
}

img {
  width: 100%;
}
video{
  width: 100%;
}
</style>
```

`APP.vue` 父组件代码

```vue
<template>
  <div class="container">

    <Category title="游戏">
      <template scope="shoushuju">
        <ul>
          <li v-for="(g,index) in shoushuju.games" :key="index">{{g}}</li>
        </ul>
      </template>
    </Category>

    <Category title="游戏">
      <template scope="{games}">
        <ol>
          <li style="color: red" v-for="(g,index) in games" :key="index">{{g}}</li>
        </ol>
      </template>
    </Category>

    <Category title="游戏">
      <template slot-scope="{games}">
        <h4 v-for="(g,index) in games" :key="index">{{g}}</h4>
      </template>
    </Category>

  </div>
</template>

<script>
import Category from "@/components/Category";

export default {
  name: "App",
  components: {Category},

}
</script>

<style>
.container, .foot{
  display: flex;
  /*justify-content 主轴（横轴）方向上的对齐方式 ， space-around 每个元素周围分配相同的空间*/
  justify-content: space-around;
}
h4{
  text-align: center;
}
</style>
```

## 五、Vuex

### 1. 理解 vuex

#### 1.1 vuex 是什么

概念：专门在 Vue 中实现集中式状态（数据）管理的一个 Vue 插件，对 vue 应用中多个组件的共享状态进行集中式的管理（读/写），也是一种组件间通信的方式，且适用于任意组件间通信

GitHub 地址：https://github.com/vuejs/vuex

![vuex1](./static/images/vuex1.png)

![vuex2](./static/images/vuex2.png)

#### 1.2 什么时候使用 vuex

1. 多个组件依赖于同一状态
2. 来自不同组件的行为需要变更同一状态

#### 1.3 vuex 工作原理图

![vuex工作原理](./static/images/vuex%E5%B7%A5%E4%BD%9C%E5%8E%9F%E7%90%86.png)

#### 1.4 求和案例纯 vue 

`Count.vue` 子组件代码

```vue
<template>
  <div class="counter">
    <h2>当前求和为：{{sum}}</h2>
    <select v-model.number="number">
      <option value="1">1</option>
      <option value="2">2</option>
      <option value="3">3</option>
    </select>
    <button @click="increment">+</button>
    <button @click="decrement">-</button>
    <button @click="incrementOdd">当前为奇数则加</button>
    <button @click="incrementWait">延迟加法</button>
  </div>
</template>

<script>
export default {
  name: "Count",
  data(){
    return{
      number:1,   // 用户选择的数字
      sum:0,    // 当前的和
    }
  },
  methods:{
    increment(){
      this.sum += this.number
    },
    decrement() {
      this.sum -= this.number
    },
    incrementOdd(){
      if(this.sum % 2){
        this.sum += this.number
      }
    },
    incrementWait(){
      setTimeout(()=>{
        // 函数体
        this.sum += this.number
      },1000)
    },

  },

}
</script>

<style scoped>
button {
  margin-left: 8px;

}
</style>
```

`APP.vue` 父组件代码

```vue
<template>
  <div class="container">
    <Count></Count>
  </div>
</template>

<script>
import Count from "@/components/Count";

export default {
  name: "App",
  components: {Count},

}
</script>
```

`main.js` 代码

```js
// 引入Vue
import Vue from "vue";
// 引入App
import App from "./App";
// 引入插件
import vueResource from 'vue-resource'

// 关闭Vue的生产提示
Vue.config.productionTip = false
// 使用插件
Vue.use(vueResource)

// 创建vm
new Vue({
    el: '#app',
    render: h => h(App),
    lintOnSave: false,	// 关闭语法检查
    beforeCreate() {
        Vue.prototype.$bus = this       // 安装全局事件总线
    }
})
```

### 2. 搭建 vuex 环境及使用

1. `npm i vuex@3 ` vue2中要安装vuex的3版本，直接`npm i vuex` 安装的是最新版本（第4版本），第4版本需要在vue3中运行，在vue中运行会报错
2. 在`main.js` 文件中引入`import Vuex from 'vuex'` 并使用插件`Vue.use(Vuex)`
3. 创建`store`
4. vc ==> store，在`main.js` 中创建vm时传入`store` 配置项

#### 2.1 vuex 的基本使用

1. 初始化数据`state` ，配置`actions` 、`mutations` 、操作文件`store` 
2. 组件中读取`vuex` 中的数据·`$store.state.数据` 
3. 组件中修改`vuex` 中的数据`$store.dispatch('action中的方法名', 数据)` 或`$store.commit('mutation中的方法名', 数据)`  

> 若没有网络请求或其它业务逻辑，组件中也可越过`actions` ，即不写`dispatch` ，直接编写`commit`

- 在`src` 文件夹中创建`store` 目录，并在`store` 目录中创建`index.js` 文件

```js
// 该文件用于创建Vuex中最为核心的store

import Vue from "vue";
// 引入Vuex
import Vuex from "vuex";
// 使用Vuex插件
Vue.use(Vuex)       // use(Vuex)后，创建vm时就可以传入store配置项了

// 准备 actions——用于响应组件中的动作
const actions = {}
// 准备 mutations——用于操作数据（state）
const mutations = {}
// 准备 state——用于存储数据
const state = {}

// 创建并导出 store
export default new Vuex.Store({
    actions,     // actions:actions,
    mutations,      // mutations:mutations,
    state,      // state:state,
})
/*// 创建 store
const store = new Vuex.Store({
    actions,     // actions:actions,
    mutations,      // mutations:mutations,
    state,      // state:state,
})

// 导出 store
export default store*/
```

- vc ==> store，在`main.js` 中创建vm时传入`store` 配置项

```js
// 引入Vue
import Vue from "vue";
// 引入App
import App from "./App";
// 引入插件
import vueResource from 'vue-resource'
// 引入store
import store from "@/store";

// 关闭Vue的生产提示
Vue.config.productionTip = false
// 使用插件
Vue.use(vueResource)


// 创建vm
new Vue({
    el: '#app',
    render: h => h(App),
    store,
    lintOnSave: false,	// 关闭语法检查
    beforeCreate() {
        Vue.prototype.$bus = this       // 安装全局事件总线
    }
})
```

#### 2.2 求和案例 vuex

`Count.vue` 子组件代码

```vue
<template>
  <div class="counter">
    <h2>当前求和为：{{$store.state.sum}}</h2>
    <select v-model.number="number">
      <option value="1">1</option>
      <option value="2">2</option>
      <option value="3">3</option>
    </select>
    <button @click="increment">+</button>
    <button @click="decrement">-</button>
    <button @click="incrementOdd">当前为奇数则加</button>
    <button @click="incrementWait">延迟加法</button>
  </div>
</template>

<script>
export default {
  name: "Count",
  data(){
    return{
      number:1,   // 用户选择的数字
    }
  },
  methods:{
    increment(){
      this.$store.commit('JIA',this.number)
    },  //没有业务逻辑，直接跳过actions，用commit跟mutations对接
    decrement() {
      this.$store.commit('JIAN',this.number)
    },  //没有业务逻辑，直接跳过actions，用commit跟mutations对接
    incrementOdd(){
      /*if(this.$store.state.sum % 2){
        this.$store.dispatch('jia',this.number)
      }*/
      this.$store.dispatch('jiaOdd',this.number)
    },
    incrementWait(){
      /*setTimeout(()=>{
        // 函数体
        this.$store.dispatch('jia',this.number)
      },1000)*/
      this.$store.dispatch('jiaWait',this.number)

    },
  mounted(){
    console.log()
  }
  },

}
</script>

<style scoped>
button {
  margin-left: 8px;

}
</style>
```

`APP.vue` 父组件代码

```vue
<template>
  <div class="container">
    <Count></Count>
  </div>
</template>

<script>
import Count from "@/components/Count";

export default {
  name: "App",
  components: {Count},

}
</script>

<style>

</style>
```

`index.js` 代码

```js
// 该文件用于创建Vuex中最为核心的store

import Vue from "vue";
// 引入Vuex
import Vuex from "vuex";
// 使用Vuex插件
Vue.use(Vuex)       // use(Vuex)后，创建vm时就可以传入store配置项了

// 准备 actions——用于响应组件中的动作
const actions = {
    /*jia:function (context,value){       // context 上下文  actions里的jia
        // console.log('actions中的jia被调用了',context,value)
        context.commit('JIA',value)     // mutations里的JIA
    },
    jian(context,value){       // context 上下文  actions里的jia
        // console.log('actions中的jian被调用了',context,value)
        context.commit('JIAN',value)     // mutations里的JIA
    },*/
    jiaOdd:function (context,value){       // context 上下文  actions里的jia
        // console.log('actions中的jiaOdd被调用了',context,value)
        if(context.state.sum % 2){
            context.commit('JIA',value)     // mutations里的JIA
        }
    },
    jiaWait:function (context,value){       // context 上下文  actions里的jia
           // console.log('actions中的jiaWait被调用了',context,value)
        setTimeout(()=>{
            context.commit('JIA',value)     // mutations里的JIA
        },1000)
    },
}
// 准备 mutations——用于操作数据（state）
const mutations = {     // mutations能帮助直接操作sum
    JIA(state,value){
        // console.log('mutations中的JIA被调用了',state,value)
        state.sum += value
    },
    JIAN(state,value){
        // console.log('mutations中的JIAN被调用了',state,value)
        state.sum -= value
    }
}
// 准备 state——用于存储数据
const state = {
    sum:0       // 当前的和
}

// 创建并导出 store
export default new Vuex.Store({
    actions,     // actions:actions,
    mutations,      // mutations:mutations,
    state,      // state:state,
})
/*// 创建 store
const store = new Vuex.Store({
    actions,     // actions:actions,
    mutations,      // mutations:mutations,
    state,      // state:state,
})

// 导出 store
export default store*/
```

`main.js` 代码

```js
// 引入Vue
import Vue from "vue";
// 引入App
import App from "./App";
// 引入插件
import vueResource from 'vue-resource'
// 引入store
import store from "@/store";

// 关闭Vue的生产提示
Vue.config.productionTip = false
// 使用插件
Vue.use(vueResource)


// 创建vm
new Vue({
    el: '#app',
    render: h => h(App),
    store,
    lintOnSave: false,	// 关闭语法检查
    beforeCreate() {
        Vue.prototype.$bus = this       // 安装全局事件总线
    }
})
```

### 3. getters 的使用

1. 概念：当 state 中的数据需要经过加工后再使用时，可以使用 getters 加工
2. 在`store.js` 中追加`getters` 配置

```js
......
// 准备 getters——用于将 state 中的数据进行加工
const getters = {
    bigSum(state){      // 收到参数 state
        return state.sum * 10
    }
}

// 创建并导出 store
export default new Vuex.Store({
    actions,     // actions:actions,
    mutations,      // mutations:mutations,
    state,      // state:state,
    getters,
})
......
```

3. 组件中读取数据：`$store.getters.bigSum` 

`Count.vue` 子组件代码

```vue
<template>
  <div class="counter">
    <h2>当前求和为：{{$store.state.sum}}</h2>
    <h2>当前求和放大10倍为：{{$store.getters.bigSum}}</h2>
    <select v-model.number="number">
      <option value="1">1</option>
      <option value="2">2</option>
      <option value="3">3</option>
    </select>
    <button @click="increment">+</button>
    <button @click="decrement">-</button>
    <button @click="incrementOdd">当前为奇数则加</button>
    <button @click="incrementWait">延迟加法</button>
  </div>
</template>

<script>
export default {
  name: "Count",
  data(){
    return{
      number:1,   // 用户选择的数字
    }
  },
  methods:{
    increment(){
      this.$store.commit('JIA',this.number)
    },  //没有业务逻辑，直接跳过actions，用commit跟mutations对接
    decrement() {
      this.$store.commit('JIAN',this.number)
    },  //没有业务逻辑，直接跳过actions，用commit跟mutations对接
    incrementOdd(){
      /*if(this.$store.state.sum % 2){
        this.$store.dispatch('jia',this.number)
      }*/
      this.$store.dispatch('jiaOdd',this.number)
    },
    incrementWait(){
      /*setTimeout(()=>{
        // 函数体
        this.$store.dispatch('jia',this.number)
      },1000)*/
      this.$store.dispatch('jiaWait',this.number)

    },
  mounted(){
    console.log('Count',this.$store)
  }
  },

}
</script>

<style scoped>
button {
  margin-left: 8px;

}
</style>
```

`src/store/index.js` 中代码

```js
// 该文件用于创建Vuex中最为核心的store

import Vue from "vue";
// 引入Vuex
import Vuex from "vuex";
// 使用Vuex插件
Vue.use(Vuex)       // use(Vuex)后，创建vm时就可以传入store配置项了

// 准备 actions——用于响应组件中的动作
const actions = {
    /*jia:function (context,value){       // context 上下文  actions里的jia
        // console.log('actions中的jia被调用了',context,value)
        context.commit('JIA',value)     // mutations里的JIA
    },
    jian(context,value){       // context 上下文  actions里的jia
        // console.log('actions中的jian被调用了',context,value)
        context.commit('JIAN',value)     // mutations里的JIA
    },*/
    jiaOdd:function (context,value){       // context 上下文  actions里的jia
        // console.log('actions中的jiaOdd被调用了',context,value)
        if(context.state.sum % 2){
            context.commit('JIA',value)     // mutations里的JIA
        }
    },
    jiaWait:function (context,value){       // context 上下文  actions里的jia
           // console.log('actions中的jiaWait被调用了',context,value)
        setTimeout(()=>{
            context.commit('JIA',value)     // mutations里的JIA
        },1000)
    },
}
// 准备 mutations——用于操作数据（state）
const mutations = {     // mutations能帮助直接操作sum
    JIA(state,value){
        // console.log('mutations中的JIA被调用了',state,value)
        state.sum += value
    },
    JIAN(state,value){
        // console.log('mutations中的JIAN被调用了',state,value)
        state.sum -= value
    }
}
// 准备 state——用于存储数据
const state = {
    sum:0       // 当前的和
}
// 准备 getters——用于将 state 中的数据进行加工
const getters = {
    bigSum(state){      // 收到参数 state
        return state.sum * 10
    }
}

// 创建并导出 store
export default new Vuex.Store({
    actions,     // actions:actions,
    mutations,      // mutations:mutations,
    state,      // state:state,
    getters,
})
/*// 创建 store
const store = new Vuex.Store({
    actions,     // actions:actions,
    mutations,      // mutations:mutations,
    state,      // state:state,
})

// 导出 store
export default store*/
```

### 4. 四个 map 方法的使用

先导入

```js
import {mapState, mapGetters, mapActions, mapMutations} from 'vuex'
```

1. **mapState方法**：用于帮助映射`state` 中的数据为**计算属性**

```js
computed: {
    // ES6语法 扩展运算符(...)用于取出参数对象所有可遍历属性然后拷贝到当前对象,结尾(,)不能少
    //借助mapState生成计算属性：sum、school、subject（对象写法）
     ...mapState({sum:'sum',school:'school',subject:'subject'}),
         
    //借助mapState生成计算属性：sum、school、subject（数组写法）
    ...mapState(['sum','school','subject']),
},
```

2. **mapGetters方法**：用于帮助映射`getters` 中的数据为计算属性

```js
computed: {
    //借助mapGetters生成计算属性：bigSum（对象写法）
    ...mapGetters({bigSum:'bigSum'}),

    //借助mapGetters生成计算属性：bigSum（数组写法）
    ...mapGetters(['bigSum'])
},
```

`src/components/Count.vue` 

```vue
<template>
  <div class="counter">
    <h2>当前求和为：{{sum}}</h2>
    <h2>当前求和放大10倍为：{{bigSum}}</h2>
    <h3>我在{{school}}学习{{subject}}</h3>
    <select v-model.number="number">
      <option value="1">1</option>
      <option value="2">2</option>
      <option value="3">3</option>
    </select>
    <button @click="increment">+</button>
    <button @click="decrement">-</button>
    <button @click="incrementOdd">当前为奇数则加</button>
    <button @click="incrementWait">延迟加法</button>
  </div>
</template>

<script>
import {mapState,mapGetters} from 'vuex'

export default {
  name: "Count",
  data(){
    return{
      number:1,   // 用户选择的数字
    }
  },
  computed:{
    // 靠手写计算属性
    /*he(){
      return this.$store.state.sum
    },
    xuexiao(){
      return this.$store.state.school
    },
    xueke(){
      return this.$store.state.subject
    },*/
    // 对象{...{对象}},    扩展运算符(...)用于取出参数对象所有可遍历属性然后拷贝到当前对象,(,)不能少

    // 借助mapState生成计算属性，从state中读取数据（对象写法）
    // ...mapState({he:'sum',xuexiao:'school',xueke:'subject'}),
    // 借助mapState生成计算属性，从state中读取数据（数组写法）
    ...mapState(['sum','school','subject']),

    /*bigSum(){
      return this.$store.getters.bigSum
    },*/

    // 借助mapGetters生成计算属性，从getters中读取数据（对象写法）
    // ...mapGetters({bigSum:"bigSum"})
    // 借助mapGetters生成计算属性，从getters中读取数据（数组写法）
    ...mapGetters(["bigSum"])
  },
  methods:{
    increment(){
      this.$store.commit('JIA',this.number)
    },  //没有业务逻辑，直接跳过actions，用commit跟mutations对接
    decrement() {
      this.$store.commit('JIAN',this.number)
    },  //没有业务逻辑，直接跳过actions，用commit跟mutations对接
    incrementOdd(){
      /*if(this.$store.state.sum % 2){
        this.$store.dispatch('jia',this.number)
      }*/
      this.$store.dispatch('jiaOdd',this.number)
    },
    incrementWait(){
      /*setTimeout(()=>{
        // 函数体
        this.$store.dispatch('jia',this.number)
      },1000)*/
      this.$store.dispatch('jiaWait',this.number)

    },
  mounted(){
    console.log('Count',this.$store)
  }
  },
 /* mounted() {
    const x = mapState({he:'sum',xuexiao:'school',xueke:'subject'})
    console.log(x)
  }*/

}
</script>

<style scoped>
button {
  margin-left: 8px;

}
</style>
```

`src/store/index.js` 

```js
// 该文件用于创建Vuex中最为核心的store

import Vue from "vue";
// 引入Vuex
import Vuex from "vuex";
// 使用Vuex插件
Vue.use(Vuex)       // use(Vuex)后，创建vm时就可以传入store配置项了

// 准备 actions——用于响应组件中的动作
const actions = {
    /*jia:function (context,value){       // context 上下文  actions里的jia
        // console.log('actions中的jia被调用了',context,value)
        context.commit('JIA',value)     // mutations里的JIA
    },
    jian(context,value){       // context 上下文  actions里的jia
        // console.log('actions中的jian被调用了',context,value)
        context.commit('JIAN',value)     // mutations里的JIA
    },*/
    jiaOdd:function (context,value){       // context 上下文  actions里的jia
        // console.log('actions中的jiaOdd被调用了',context,value)
        if(context.state.sum % 2){
            context.commit('JIA',value)     // mutations里的JIA
        }
    },
    jiaWait:function (context,value){       // context 上下文  actions里的jia
           // console.log('actions中的jiaWait被调用了',context,value)
        setTimeout(()=>{
            context.commit('JIA',value)     // mutations里的JIA
        },1000)
    },
}
// 准备 mutations——用于操作数据（state）
const mutations = {     // mutations能帮助直接操作sum
    JIA(state,value){
        // console.log('mutations中的JIA被调用了',state,value)
        state.sum += value
    },
    JIAN(state,value){
        // console.log('mutations中的JIAN被调用了',state,value)
        state.sum -= value
    }
}
// 准备 state——用于存储数据
const state = {
    sum:0,    // 当前的和
    school:'前端学院',
    subject:'web前端'
}
// 准备 getters——用于将 state 中的数据进行加工
const getters = {
    bigSum(state){      // 收到参数 state
        return state.sum * 10
    }
}

// 创建并导出 store
export default new Vuex.Store({
    actions,     // actions:actions,
    mutations,      // mutations:mutations,
    state,      // state:state,
    getters,
})
/*// 创建 store
const store = new Vuex.Store({
    actions,     // actions:actions,
    mutations,      // mutations:mutations,
    state,      // state:state,
})

// 导出 store
export default store*/
```

3. **mapActions方法**：用于帮助生成与`actions` 对话的方法，即包含`$store.dispatch(xxx)` 的函数

```js
methods:{
    //靠mapActions生成：incrementOdd、incrementWait（对象形式）
    ...mapActions({incrementOdd:'jiaOdd',incrementWait:'jiaWait'})

    //靠mapActions生成：incrementOdd、incrementWait（数组形式）
    ...mapActions(['jiaOdd','jiaWait'])		// 事件名叫 jiaOdd  jiaWait
}
```

4. **mapMutations方法**：用于帮助生成与`mutations` 对话的方法，即包含`$store.commit(xxx)` 的函数

```js
methods:{
    //靠mapActions生成：increment、decrement（对象形式）
    ...mapMutations({increment:'JIA',decrement:'JIAN'}),
    
    //靠mapMutations生成：JIA、JIAN（对象形式）
    ...mapMutations(['JIA','JIAN']),	// 事件名叫 JIA  JIAN
}
```

`src/components/Count.vue` 

```vue
<template>
  <div class="counter">
    <h2>当前求和为：{{sum}}</h2>
    <h2>当前求和放大10倍为：{{bigSum}}</h2>
    <h3>我在{{school}}学习{{subject}}</h3>
    <select v-model.number="number">
      <option value="1">1</option>
      <option value="2">2</option>
      <option value="3">3</option>
    </select>
    <button @click="increment(number)">+</button>
    <button @click="decrement(number)">-</button>
    <button @click="incrementOdd(number)">当前为奇数则加</button>
    <button @click="incrementWait(number)">延迟加法</button>
  </div>
</template>

<script>
import {mapState,mapGetters,mapMutations,mapActions} from 'vuex'

export default {
  name: "Count",
  data(){
    return{
      number:1,   // 用户选择的数字
    }
  },
  computed:{
    // 对象{...{对象}},    扩展运算符(...)用于取出参数对象所有可遍历属性然后拷贝到当前对象,(,)不能少
    // 借助mapState生成计算属性，从state中读取数据（对象写法）
    // ...mapState({he:'sum',xuexiao:'school',xueke:'subject'}),
    // 借助mapState生成计算属性，从state中读取数据（数组写法）
    ...mapState(['sum','school','subject']),

    // 借助mapGetters生成计算属性，从getters中读取数据（对象写法）
    // ...mapGetters({bigSum:"bigSum"})
    // 借助mapGetters生成计算属性，从getters中读取数据（数组写法）
    ...mapGetters(["bigSum"])
  },
  methods:{
    // 纯手写方法
    /*increment(){
      this.$store.commit('JIA',this.number)
    },  //没有业务逻辑，直接跳过actions，用commit跟mutations对接
    decrement() {
      this.$store.commit('JIAN',this.number)
    },  //没有业务逻辑，直接跳过actions，用commit跟mutations对接*/

    // 借助mapMutations 生成对应的方法，方法中会调用 commit 去联系 mutations（对象写法）
    ...mapMutations({increment:'JIA',decrement:'JIAN'}),
    // 借助mapMutations 生成对应的方法，方法中会调用 commit 去联系 mutations（数组写法）
    // ...mapMutations(["JIA","JIAN"]),  // 事件名也叫JIA \ JIAN

    // 纯手写方法
    /*incrementOdd(){
      this.$store.dispatch('jiaOdd',this.number)
    },
    incrementWait(){
      this.$store.dispatch('jiaWait',this.number)
    },*/

    // 借助maoActions 生成对应的方法，方法中会调用 dispatch 去联系 actions（对象写法）
    ...mapActions({incrementOdd:'jiaOdd',incrementWait:'jiaWait'}),
    // 借助maoActions 生成对应的方法，方法中会调用 dispatch 去联系 actions（数组写法）
    // ...mapActions(['jiaOdd','jiaWait']),


  mounted(){
    console.log('Count',this.$store)
  }
  },

}
</script>

<style scoped>
button {
  margin-left: 8px;

}
</style>
```

`src/store/index.js` 

```js
// 该文件用于创建Vuex中最为核心的store

import Vue from "vue";
// 引入Vuex
import Vuex from "vuex";
// 使用Vuex插件
Vue.use(Vuex)       // use(Vuex)后，创建vm时就可以传入store配置项了

// 准备 actions——用于响应组件中的动作
const actions = {
    /*jia:function (context,value){       // context 上下文  actions里的jia
        // console.log('actions中的jia被调用了',context,value)
        context.commit('JIA',value)     // mutations里的JIA
    },
    jian(context,value){       // context 上下文  actions里的jia
        // console.log('actions中的jian被调用了',context,value)
        context.commit('JIAN',value)     // mutations里的JIA
    },*/
    jiaOdd:function (context,value){       // context 上下文  actions里的jia
        // console.log('actions中的jiaOdd被调用了',context,value)
        if(context.state.sum % 2){
            context.commit('JIA',value)     // mutations里的JIA
        }
    },
    jiaWait:function (context,value){       // context 上下文  actions里的jia
           // console.log('actions中的jiaWait被调用了',context,value)
        setTimeout(()=>{
            context.commit('JIA',value)     // mutations里的JIA
        },1000)
    },
}
// 准备 mutations——用于操作数据（state）
const mutations = {     // mutations能帮助直接操作sum
    JIA(state,value){
        // console.log('mutations中的JIA被调用了',state,value)
        state.sum += value
    },
    JIAN(state,value){
        // console.log('mutations中的JIAN被调用了',state,value)
        state.sum -= value
    }
}
// 准备 state——用于存储数据
const state = {
    sum:0,    // 当前的和
    school:'前端学院',
    subject:'web前端'
}
// 准备 getters——用于将 state 中的数据进行加工
const getters = {
    bigSum(state){      // 收到参数 state
        return state.sum * 10
    }
}

// 创建并导出 store
export default new Vuex.Store({
    actions,     // actions:actions,
    mutations,      // mutations:mutations,
    state,      // state:state,
    getters,
})
/*// 创建 store
const store = new Vuex.Store({
    actions,     // actions:actions,
    mutations,      // mutations:mutations,
    state,      // state:state,
})

// 导出 store
export default store*/

```

### 5. 多组件数据共享

`src/components/Count.vue` 

```vue
<template>
  <div class="counter">
    <h2>当前求和为：{{sum}}</h2>
    <h2>当前求和放大10倍为：{{bigSum}}</h2>
    <h3>我在{{school}}学习{{subject}}</h3>
    <select v-model.number="number">
      <option value="1">1</option>
      <option value="2">2</option>
      <option value="3">3</option>
    </select>
    <button @click="increment(number)">+</button>
    <button @click="decrement(number)">-</button>
    <button @click="incrementOdd(number)">当前为奇数则加</button>
    <button @click="incrementWait(number)">延迟加法</button>
    <h3 style="color: pink">Person组件的总人数是：{{personList.length}}</h3>
  </div>
</template>

<script>
import {mapState,mapGetters,mapMutations,mapActions} from 'vuex'

export default {
  name: "Count",
  data(){
    return{
      number:1,   // 用户选择的数字
    }
  },
  computed:{
    // 借助mapState生成计算属性，从state中读取数据（数组写法）
    ...mapState(['sum','school','subject','personList']), // 读取的是index.js中的state数据
    // 借助mapGetters生成计算属性，从getters中读取数据（数组写法）
    ...mapGetters(["bigSum"])
  },
  methods:{
    // 借助mapMutations 生成对应的方法，方法中会调用 commit 去联系 mutations（对象写法）
    ...mapMutations({increment:'JIA',decrement:'JIAN'}),
    // 借助maoActions 生成对应的方法，方法中会调用 dispatch 去联系 actions（对象写法）
    ...mapActions({incrementOdd:'jiaOdd',incrementWait:'jiaWait'}),


  mounted(){
    console.log('Count',this.$store)
  }
  },

}
</script>

<style scoped>
button {
  margin-left: 8px;

}
</style>
```

`src/components/Person.vue` 

```vue
<template>
<div>
  <h3 style="color: blue">Count组件的求和为：{{sum}}</h3>
  <h1>人员列表</h1>
  <input type="text" placeholder="请输入名字" v-model="name">
  <button @click="add">添加</button>
  <ul>
    <li v-for="p in personList" :key="p.id">{{p.name}}</li>
  </ul>
</div>
</template>

<script>
import {mapState} from "vuex";
import {nanoid} from "nanoid";

export default {
  name: "Person",
  data(){
    return{
      name:''
    }
  },
  computed:{
    personList(){
      return this.$store.state.personList
    },
    sum(){
      return this.$store.state.sum
    }
    /*...mapState(['personList','sum']),*/
  },
  methods:{
    add(){
      const personObj = {id:nanoid(), name:this.name}
      this.$store.commit('ADD_PERSON',personObj)
      this.name = ''
    }
  }
}
</script>

<style scoped>

</style>
```

`src/store/index.js` 

```js
// 该文件用于创建Vuex中最为核心的store

import Vue from "vue";
// 引入Vuex
import Vuex from "vuex";
// 使用Vuex插件
Vue.use(Vuex)       // use(Vuex)后，创建vm时就可以传入store配置项了

// 准备 actions——用于响应组件中的动作
const actions = {
    /*jia:function (context,value){       // context 上下文  actions里的jia
        // console.log('actions中的jia被调用了',context,value)
        context.commit('JIA',value)     // mutations里的JIA
    },
    jian(context,value){       // context 上下文  actions里的jia
        // console.log('actions中的jian被调用了',context,value)
        context.commit('JIAN',value)     // mutations里的JIA
    },*/
    jiaOdd:function (context,value){       // context 上下文  actions里的jia
        // console.log('actions中的jiaOdd被调用了',context,value)
        if(context.state.sum % 2){
            context.commit('JIA',value)     // mutations里的JIA
        }
    },
    jiaWait:function (context,value){       // context 上下文  actions里的jia
           // console.log('actions中的jiaWait被调用了',context,value)
        setTimeout(()=>{
            context.commit('JIA',value)     // mutations里的JIA
        },1000)
    },
}
// 准备 mutations——用于操作数据（state）
const mutations = {     // mutations能帮助直接操作sum
    JIA(state,value){
        // console.log('mutations中的JIA被调用了',state,value)
        state.sum += value
    },
    JIAN(state,value){
        // console.log('mutations中的JIAN被调用了',state,value)
        state.sum -= value
    },
    ADD_PERSON(state,value){        // value也就是personObj
        console.log('mutations中的ADD_PERSON被调用了',state,value)
        state.personList.unshift(value)
    }

}
// 准备 state——用于存储数据
const state = {
    sum:0,    // 当前的和
    school:'前端学院',
    subject:'web前端',
    personList:[
        {id:'001',name:'张三'}
    ],
}
// 准备 getters——用于将 state 中的数据进行加工
const getters = {
    bigSum(state){      // 收到参数 state
        return state.sum * 10
    }
}

// 创建并导出 store
export default new Vuex.Store({
    actions,     // actions:actions,
    mutations,      // mutations:mutations,
    state,      // state:state,
    getters,
})
/*// 创建 store
const store = new Vuex.Store({
    actions,     // actions:actions,
    mutations,      // mutations:mutations,
    state,      // state:state,
})

// 导出 store
export default store*/
```

`src/App.vue` 

```vue
<template>
  <div class="container">
    <Count></Count>
    <hr>
    <Person></Person>
  </div>
</template>

<script>
import Count from "@/components/Count";
import Person from "@/components/Person";

export default {
  name: "App",
  components: {Count,Person},

}
</script>
```

### 6. vuex 模块化+ namespace

1. 目的：让代码更好维护，让多种数据分类更加明确
2. 修改`store.js` 

为了解决不同模块命名冲突的问题，将不同模块的<font color='red'>`namespaced: true` </font>,之后在不同页面中引入`getters` 、`actions` 、`mutations` 时，需要加上所属的模块名

```js
const countAbout = {
  namespaced:true,//开启命名空间
  state:{x:1},
  mutations: { ... },
  actions: { ... },
  getters: {
    bigSum(state){
       return state.sum * 10
    }
  }
            
const personAbout = {
  namespaced:true,//开启命名空间
  state:{ ... },
  mutations: { ... },
  actions: { ... },
  getters: { ... }
}
            
// 创建并导出 store
export default new Vuex.Store({
    modules:{
        countAbout,
        personAbout:personAbou
    }
})
```

3. 开启命名空间后，组件中读取`state` 数据

```js
// 方式一：自己读取
this.$store.state.personAbout.list
// 方式二：借助 mapState 读取
...mapState('countAbout',['sum','school','subject']),
```

4. 开启命名空间后，组件中读取`getters` 数据

```js
// 方式一：自己直接读取
this.$store.getters['personAbout/firstPersonName']
// 方式二：借助 mapGetters 读取
...mapGetters('countAbout',['bigSun']),
```

5. 开启命名空间后，组件中调用`dispatch` 

```js
// 方式一：自己直接 dispatch
this.$store.dispatch('personAbout/addPersonWang',personObj)
// 方式二：借助 mapActions
...mapActions('countAbout',{incrementOdd:'jiaOdd',incrementWait:'jiaWait'})
```

6. 开启命名空间后，组件中调用`commit` 

```js
// 方式一:自己直接 commit
this.$store.commit('personAbout/ADD_PERSON',personObj)
// 方式二：借助 mapMutations
...mapMutations('countAbout',{increment:'JIA',decrement:'JIAN'})
```

`src/store/index.js` 

```js
// 该文件用于创建Vuex中最为核心的store
import Vue from "vue";
// 引入Vuex
import Vuex from "vuex";

import countOptions from "@/store/count";
import personOptions from './person'

// 使用Vuex插件
Vue.use(Vuex)       // use(Vuex)后，创建vm时就可以传入store配置项了

// 创建并导出 store
export default new Vuex.Store({
    modules:{
        countAbout:countOptions,
        personAbout:personOptions
    }
})
```

`src/store/count.js` 

```js
// 求和相关的配置
const countOptions = {
    namespaced:true,
    actions:{
        jiaOdd:function (context,value){       // context 上下文  actions里的jia
                                               // console.log('actions中的jiaOdd被调用了',context,value)
            if(context.state.sum % 2){
                context.commit('JIA',value)     // mutations里的JIA
            }
        },
        jiaWait:function (context,value){       // context 上下文  actions里的jia
            // console.log('actions中的jiaWait被调用了',context,value)
            setTimeout(()=>{
                context.commit('JIA',value)     // mutations里的JIA
            },1000)
        },
    },
    mutations:{
        JIA(state,value){
            // console.log('mutations中的JIA被调用了',state,value)
            state.sum += value
        },
        JIAN(state,value){
            // console.log('mutations中的JIAN被调用了',state,value)
            state.sum -= value
        },
    },
    state:{
        sum:0,    // 当前的和
        school:'前端学院',
        subject:'web前端',
    },
    getters:{
        bigSum(state){      // 收到参数 state
            return state.sum * 10
        }
    },
}

export default countOptions
```

`src/store/person.js` 

```js
// 人员管理相关的配置
import axios from "axios";
import {nanoid} from "nanoid";

export default {
    namespaced:true,
    actions:{
        addPersonWang(context,value){
            if (value.name.indexOf('王') === 0) {
                context.commit('ADD_PERSON',value)
            } else {
                alert('请输入王姓！')
            }
        },
        // 访问 API
        addPersonServer(context){
            axios.get('https://api.uixsj.cn/hitokoto/get?type=social').then(
                response => {
                    context.commit('ADD_PERSON',{id:nanoid(),name:response.data})
                },
                error => {
                    alert(error.message)
                }
            )
        }
    },
    mutations:{
        ADD_PERSON(state,value){        // value也就是personObj
            console.log('mutations中的ADD_PERSON被调用了',state,value)
            state.personList.unshift(value)
        }
    },
    state:{
        personList:[
            {id:'001',name:'张三'}
        ],
    },
    getters:{
        firstPersonName(state){
            return state.personList[0].name
        }
    },
}
```

`src/components/Count.vue` 

```vue
<template>
  <div class="counter">
    <h2>当前求和为：{{sum}}</h2>
    <h2>当前求和放大10倍为：{{bigSum}}</h2>
    <h3>我在{{school}}学习{{subject}}</h3>
    <select v-model.number="number">
      <option value="1">1</option>
      <option value="2">2</option>
      <option value="3">3</option>
    </select>
    <button @click="increment(number)">+</button>
    <button @click="decrement(number)">-</button>
    <button @click="incrementOdd(number)">当前为奇数则加</button>
    <button @click="incrementWait(number)">延迟加法</button>
    <h3 style="color: pink">Person组件的总人数是：{{personList.length}}</h3>
  </div>
</template>

<script>
import {mapState,mapGetters,mapMutations,mapActions} from 'vuex'

export default {
  name: "Count",
  data(){
    return{
      number:1,   // 用户选择的数字
    }
  },
  computed:{
    // 借助mapState生成计算属性，从state中读取数据（数组写法）
    ...mapState('countAbout',['sum','school','subject']),
    ...mapState('personAbout',['personList']),
    // 借助mapGetters生成计算属性，从getters中读取数据（数组写法）
    ...mapGetters('countAbout',["bigSum"])
  },
  methods:{
    // 借助mapMutations 生成对应的方法，方法中会调用 commit 去联系 mutations（对象写法）
    ...mapMutations('countAbout',{increment:'JIA',decrement:'JIAN'}),
    // 借助maoActions 生成对应的方法，方法中会调用 dispatch 去联系 actions（对象写法）
    ...mapActions('countAbout',{incrementOdd:'jiaOdd',incrementWait:'jiaWait'}),
  },

  mounted(){
    console.log('Count',this.$store)
  }
}
</script>

<style scoped>
button {
  margin-left: 8px;
}
</style>
```

`src/components/Person.vue` 

```vue
<template>
<div>
  <h3 style="color: blue">Count组件的求和为：{{sum}}</h3>
  <h1>人员列表</h1>
  <h3>列表中第一个人的名字是：{{firstPersonName}}</h3>
  <input type="text" placeholder="请输入名字" v-model="name">
  <button @click="add">添加</button>
  <button @click="addWang">添加一个姓王的人</button>
  <button @click="addPersonServer">添加一段话，随机</button>
  <ul>
    <li v-for="p in personList" :key="p.id">{{p.name}}</li>
  </ul>
</div>
</template>

<script>
import {mapState} from "vuex";
import {nanoid} from "nanoid";

export default {
  name: "Person",
  data(){
    return{
      name:''
    }
  },
  computed:{
    personList(){
      return this.$store.state.personAbout.personList
    },
    sum(){
      return this.$store.state.countAbout.sum
    },
    /*...mapState('personAbout',['personList','sum']),*/
    firstPersonName() {
      return this.$store.getters["personAbout/firstPersonName"]
    }
  },
  methods:{
    add(){
      const personObj = {id:nanoid(), name:this.name}
      this.$store.commit('personAbout/ADD_PERSON',personObj)    // 联系 mutation
      this.name = ''
    },
    addWang(){
      const personObj = {id:nanoid(), name:this.name}
      this.$store.dispatch('personAbout/addPersonWang',personObj)  // 有业务逻辑，联系 action
      this.name=''
    },
    addPersonServer(){
      this.$store.dispatch('personAbout/addPersonServer')
    }
  }
}
</script>
```

`src/App.vue` 

```vue
<template>
  <div class="container">
    <Count></Count>
    <hr>
    <Person></Person>
  </div>
</template>

<script>
import Count from "@/components/Count";
import Person from "@/components/Person";

export default {
  name: "App",
  components: {Count,Person},

}
</script>
```

`src/main.js` 

```js
// 引入Vue
import Vue from "vue";
// 引入App
import App from "./App";
// 引入插件
import vueResource from 'vue-resource'
// 引入store
import store from "@/store";

// 关闭Vue的生产提示
Vue.config.productionTip = false
// 使用插件
Vue.use(vueResource)

// 创建vm
new Vue({
    el: '#app',
    render: h => h(App),
    store,
    lintOnSave: false,	// 关闭语法检查
    beforeCreate() {
        Vue.prototype.$bus = this       // 安装全局事件总线
    }
})
```

## 六、Vue Router 路由

### 1. 相关理解

1. **vue-router**：`vue` 的一个插件库，专门用来实现`SPA` 应用

2. **对 SPA 应用的理解**：

   - 单页`Web`应用（single page wed application, SPA)
   - 整个应用**只有一个完整的页面** 
   - 点击页面中的导航链接**不会刷新**页面，只会做页面的**局部更新**
   - 数据需要通过`ajax` 请求获取

3. **路由的理解**：

   - 什么是路由？

     a. 一个路由就是一组映射关系（key-value)

     b. `key`为**路径**，`value`可能是`function`或`component` 

   - 路由分类

     a. **后端路由**：`value`是`function`,用于处理客户端提交的请求。服务器接收到一个请求时，根据请求路径找到匹配的函数来处理请求

     b.**前端路由**： `value`是`component`，用于展示页面内容。当浏览器的路径改变时，对应的组件就会显示

### 2. 路由基本使用

#### 2.1 基本路由

1. 安装`vue-router`,命令`npm i vue-router` (在vue2中安装`npm i vue0router@3`版本)
2. 应用插件`Vue.use(VueRouter)` 
3. 编写`router`配置项，路径`src/router/index.js` 

```js
// 该文件专门用于创建整个应用的路由器
import VueRouter from "vue-router";
// 引入组件
import About from "@/components/About";
import Home from "@/components/Home";

// 创建一个路由器
const router = new VueRouter({
    routes:[
        {
            path:'/about',
            component:About
        },
        {
            path:'/home',
            component:Home
        },
    ]
})

// 暴露路由器
export default router
```

4. 实现切换（`active-class` 可配置高亮样式）

```vue
<!-- Vue中借助router-link标签实现路由的切换 -->
<router-link class="list-group-item" active-class="active" to="/about">About</router-link>
```

5. 指定展示位置

```vue
<!-- 指定组件的呈现位置 -->
<router-view></router-view>
```

`src/router/index.js` 该文件专门用于创建整个应用的路由器

```js
// 该文件专门用于创建整个应用的路由器
import VueRouter from "vue-router";
// 引入组件
import About from "@/components/About";
import Home from "@/components/Home";

// 创建一个路由器
const router = new VueRouter({
    routes:[
        {
            path:'/about',
            component:About
        },
        {
            path:'/home',
            component:Home
        },
    ]
})

// 暴露路由器
export default router
```

`src/main.js` 

```js
// 引入Vue
import Vue from "vue";
// 引入App
import App from "./App";
// 引入路由VueRouter
import VueRouter from "vue-router";
// 引入路由器（一个应用有一个路由器就够了）
import router from "@/router";

// 关闭Vue的生产提示
Vue.config.productionTip = false
// 应用插件
Vue.use(VueRouter)

// 创建vm
new Vue({
    el: '#app',
    render: h => h(App),
    router,     // 配置项
    lintOnSave: false,	// 关闭语法检查
})
```

`src/App.vue` 

```vue
<template>
  <div>
    <div class="row">
      <div class="col-xs-offset-2 col-xs-8">
        <div class="page-header"><h2>Vue Router Demo</h2></div>
      </div>
    </div>
    <div class="row">
      <div class="col-xs-2 col-xs-offset-2">
        <div class="list-group">
<!--          原始html中我们使用a标签实现页面跳转-->
<!--          <a class="list-group-item active" href="./about.html">About</a>-->
<!--          <a class="list-group-item" href="./home.html">Home</a>-->

<!--         Vue中借助router-link标签实现路由的切换 -->
          <router-link class="list-group-item" active-class="active" to="/about">About</router-link>
          <router-link class="list-group-item" active-class="active" to="/home">Home</router-link>
        </div>
      </div>
      <div class="col-xs-6">
        <div class="panel">
          <div class="panel-body">
<!--            指定组件的呈现位置-->
            <router-view></router-view>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
}
</script>
```

`src/components/Home.vue` 

```vue
<template>
  <h2>我是Home的内容</h2>
</template>

<script>
export default {
  name: "Home"
}
</script>
```

`src/components/About.vue` 

```vue
<template>
  <h2>我是About的内容</h2>
</template>

<script>
export default {
  name: "About"
}
</script>
```

`public/css/index.html` 

```html
<!DOCTYPE html>
<html lang="">
  <head>
    <meta charset="utf-8">
<!--针对IE浏览器的一个特殊配置，含义是让IE浏览器以最高的渲染级别渲染页面-->
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
<!--开启移动端的理想视口-->
    <meta name="viewport" content="width=device-width,initial-scale=1.0">
<!--配置页签图标    <%= BASE_URL %> 指的是 public所在的路径-->
    <link rel="icon" href="<%= BASE_URL %>favicon.ico">

<!--引入第三方样式-->
<link rel="stylesheet" href="<%= BASE_URL %>css/bootstrap.css">

<!--配置网页标题   <%= htmlWebpackPlugin.options.title %> 指向 package.json中的name-->
    <title><%= htmlWebpackPlugin.options.title %></title>
  </head>
  <body>
<!--当浏览器不支持js的时候，noscript中的元素就会被渲染-->
    <noscript>
      <strong>We're sorry but <%= htmlWebpackPlugin.options.title %> doesn't work properly without JavaScript enabled. Please enable it to continue.</strong>
    </noscript>
<!--容器-->
    <div id="app"></div>
    <!-- built files will be auto injected -->
  </body>
</html>
```

#### 2.2 几个注意点

1. 路由组件通常存放在**`pages`**文件夹，一般组件通常存放在**`components`**文件

   比如上一节的案例就可以修改为`src/pages/Home.vue`、`src/pages/About.vue`、`src/router/index.js`、`src/components/Banner.vue`、`src/App.vue` 

2. 通过切换，“隐藏”了的路由组件，**默认是被销毁的，需要的时候再去挂载**

3. 每个组件都有自己的<font color="red">**`$route`**</font>属性，里面存储着自己的路由信息

4. 整个应用只有一个**router**，可以通过组件的<font color="red">**`$router`**</font>属性获取到

### 3. 嵌套(多级)路由

1. 配置路由规则，使用**children**配置项`src/router/index.js`

```js
routes:[
        // 一级路由
        {
            path:'/about',
            component:About
        },
        {
            path:'/home',
            component:Home,
            children:[
                {
                    path:'news',    // 不需要再加斜杠
                    component:News,
                },
                {
                    path:'message',    // 不需要再加斜杠
                    component:Message,
                },
            ]
        },
    ]
```

2. 跳转（要写完整路径）

```js
<router-link class="list-group-item" active-class="active" to="/home/news">News</router-link>

```

3. 指定展示位置

```js
<router-view/>
```

`src/components/Banner.vue` 

```vue
<template>
  <div class="col-xs-offset-2 col-xs-8">
    <div class="page-header"><h2>Vue Router Demo</h2></div>
    </div>
</template>

<script>
export default {
  name: "Banner"
}
</script>
```

`src/pages/About.vue` 

```vue
<template>
  <h2>我是About的内容</h2>
</template>

<script>
export default {
  name: "About",
}
</script>
```

`src/pages/Home.vue` 

```vue
<template>
  <div>
    <h2>Home组件内容</h2>
    <div>
      <ul class="nav nav-tabs">
        <li>
          <router-link class="list-group-item" active-class="active" to="/home/news">News</router-link>
        </li>
        <li>
          <router-link class="list-group-item " active-class="active" to="/home/message">Message</router-link>
        </li>
      </ul>
      <router-view/>
    </div>
  </div>
</template>

<script>
export default {
  name: "Home",
}
</script>
```

`src/pages/Message.vue` 

```vue
<template>
  <div>
    <ul>
      <li>
        <a href="/message1">message001</a>&nbsp;&nbsp;
      </li>
      <li>
        <a href="/message2">message002</a>&nbsp;&nbsp;
      </li>
      <li>
        <a href="/message/3">message003</a>&nbsp;&nbsp;
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "Message"
}
</script>
```

`src/pages/News.vue` 

```vue
<template>
    <ul>
      <li>news001</li>
      <li>news002</li>
      <li>news003</li>
    </ul>
</template>

<script>
export default {
  name: "News"
}
</script>
```

`src/router/index.js` 

```js
// 该文件专门用于创建整个应用的路由器
import VueRouter from "vue-router";
// 引入组件
import About from "@/pages/About";
import Home from "@/pages/Home";
import News from "@/pages/News";
import Message from "@/pages/Message";

// 创建一个路由器
const router = new VueRouter({
    routes:[
        // 一级路由
        {
            path:'/about',
            component:About
        },
        {
            path:'/home',
            component:Home,
            children:[
                {
                    path:'news',    // 不需要再加斜杠
                    component:News,
                },
                {
                    path:'message',    // 不需要再加斜杠
                    component:Message,
                },
            ]
        },
    ]
})

// 暴露路由器
export default router
```

`src/App.vue`

```vue
<template>
  <div>
    <div class="row">
      <Banner/>
    </div>
    <div class="row">
      <div class="col-xs-2 col-xs-offset-2">
        <div class="list-group">
<!--         Vue中借助router-link标签实现路由的切换 -->
          <router-link class="list-group-item" active-class="active" to="/about">About</router-link>
          <router-link class="list-group-item" active-class="active" to="/home">Home</router-link>
        </div>
      </div>
      <div class="col-xs-6">
        <div class="panel">
          <div class="panel-body">
<!--            指定组件的呈现位置-->
            <router-view></router-view>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Banner from "@/components/Banner";

export default {
  name: "App",
  components:{Banner}
}
</script>
```

### 4. 路由传参

#### 4.1 路由的 query 参数

1. 传递参数

```js
<!-- 跳转路由并携带query参数， to的字符串写法-->
<router-link :to="`/home/message/detail?id=${m.id}&title=${m.title}`">{{ m.title }}</router-link>&nbsp;&nbsp;

<!-- 跳转路由并携带query参数， to的对象写法（建议写法）-->
<router-link :to="{
  path:'/home/message/detail',
  query:{
  id:m.id,
  title:m.title
  }
}">
{{m.title}}
</router-link>
```

2. 接收参数

```js
<ul>
  <li>消息编号：{{ $route.query.id }}</li>
  <li>消息标题：{{ $route.query.title }}</li>
</ul>
```

`src/router/index.js` 

```js
// 该文件专门用于创建整个应用的路由器
import VueRouter from "vue-router";
// 引入组件
import About from "@/pages/About";
import Home from "@/pages/Home";
import News from "@/pages/News";
import Message from "@/pages/Message";
import Detail from "@/pages/Detail";

// 创建一个路由器
const router = new VueRouter({
    routes:[
        // 一级路由
        {
            path:'/about',
            component:About
        },
        {
            path:'/home',
            component:Home,
            children:[
                {
                    path:'news',    // 不需要再加斜杠
                    component:News,
                },
                {
                    path:'message',    // 不需要再加斜杠
                    component:Message,
                    children:[
                        {
                            path:'detail',    // 不需要再加斜杠
                            component:Detail,
                        }
                    ]
                },
            ]
        },
    ]
})

// 暴露路由器
export default router
```

`src/pages/Message.vue` 

```vue
<template>
  <div>
    <ul>
      <li v-for="m in messageList" :key="m.id">
<!--        跳转路由并携带query参数， to的字符串写法-->
        <router-link :to="`/home/message/detail?id=${m.id}&title=${m.title}`">{{ m.title }}</router-link>&nbsp;&nbsp;

<!--        跳转路由并携带query参数， to的对象写法（建议写法）-->
        <router-link :to="{
          path:'/home/message/detail',
          query:{
            id:m.id,
            title:m.title
          }
        }">
          {{m.title}}
        </router-link>

      </li>
    </ul>
    <hr>
    <router-view/>
  </div>
</template>

<script>
export default {
  name: "Message",
  data(){
    return{
    messageList:[
      {id:'001',title:'消息001'},
      {id:'002',title:'消息002'},
      {id:'003',title:'消息003'},
    ]
    }
  }
}
</script>
```

`src/pages/Detail.vue` 

```vue
<template>
<ul>
  <li>消息编号：{{ $route.query.id }}</li>
  <li>消息标题：{{ $route.query.title }}</li>
</ul>
</template>

<script>
export default {
  name: "Detail",
  mounted() {
    console.log(this.$route)
  }
}
</script>
```

#### 4.2 命名路由

1. 作用：可以简化路由的跳转
2. 如何使用

- 给路由命名：

```js
{
            path:'/home',
            component:Home,
            children:[
                {
                    path:'news',    // 不需要再加斜杠
                    component:News,
                },
                {
                    path:'message',    // 不需要再加斜杠
                    component:Message,
                    children:[
                        {
                            name:'xiangqing',       // 给路由命名
                            path:'detail',    // 不需要再加斜杠
                            component:Detail,
                        }
                    ]
                },
            ]
        },
```

- 简化跳转

```js
<!--简化前，需要写完整的路径 -->
<router-link to="/demo/test/welcome">跳转</router-link>

<!--简化后，直接通过名字跳转 -->
<router-link :to="{name:'hello'}">跳转</router-link>

<!--简化写法配合传递参数 -->
<router-link 
	:to="{
		name:'hello',
		query:{
		   id:666,
            title:'你好'
		}
	}"
>跳转</router-link>
```

`src/pages/Message.vue` 

```vue
<template>
  <div>
    <ul>
      <li v-for="m in messageList" :key="m.id">
<!--        跳转路由并携带query参数， to的字符串写法-->
<!--        <router-link :to="`/home/message/detail?id=${m.id}&title=${m.title}`">{{ m.title }}</router-link>&nbsp;&nbsp;-->

<!--        跳转路由并携带query参数， to的对象写法（建议写法）-->
        <router-link :to="{
          // path:'/home/message/detail',
          name:'xiangqing',
          query:{
            id:m.id,
            title:m.title
          }
        }">
          {{m.title}}
        </router-link>

      </li>
    </ul>
    <hr>
    <router-view/>
  </div>
</template>

<script>
export default {
  name: "Message",
  data(){
    return{
    messageList:[
      {id:'001',title:'消息001'},
      {id:'002',title:'消息002'},
      {id:'003',title:'消息003'},
    ]
    }
  }
}
</script>
```

`src/router/index.js` 

```js
// 该文件专门用于创建整个应用的路由器
import VueRouter from "vue-router";
// 引入组件
import About from "@/pages/About";
import Home from "@/pages/Home";
import News from "@/pages/News";
import Message from "@/pages/Message";
import Detail from "@/pages/Detail";

// 创建一个路由器
const router = new VueRouter({
    routes:[
        // 一级路由
        {
            name:'guanyu',
            path:'/about',
            component:About
        },
        {
            path:'/home',
            component:Home,
            children:[
                {
                    path:'news',    // 不需要再加斜杠
                    component:News,
                },
                {
                    path:'message',    // 不需要再加斜杠
                    component:Message,
                    children:[
                        {
                            name:'xiangqing',       // 给路由命名
                            path:'detail',    // 不需要再加斜杠
                            component:Detail,
                        }
                    ]
                },
            ]
        },
    ]
})

// 暴露路由器
export default router
```

`src/App.vue` 

```vue
<template>
  <div>
    <div class="row">
      <Banner/>
    </div>
    <div class="row">
      <div class="col-xs-2 col-xs-offset-2">
        <div class="list-group">
<!--          原始html中我们使用a标签实现页面跳转-->
<!--          <a class="list-group-item active" href="./about.html">About</a>-->
<!--          <a class="list-group-item" href="./home.html">Home</a>-->

<!--         Vue中借助router-link标签实现路由的切换 -->
          <router-link class="list-group-item" active-class="active" :to="{name:'guanyu'}">About</router-link>
          <router-link class="list-group-item" active-class="active" to="/home">Home</router-link>
        </div>
      </div>
      <div class="col-xs-6">
        <div class="panel">
          <div class="panel-body">
<!--            指定组件的呈现位置-->
            <router-view></router-view>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Banner from "@/components/Banner";

export default {
  name: "App",
  components:{Banner}
}
</script>
```

#### 4.3 路由的 params 参数

1. 配置路由，声明接收 **params**参数

```js
{
            path:'/home',
            component:Home,
            children:[
                {
                    path:'news',    // 不需要再加斜杠
                    component:News,
                },
                {
                    path:'message',    // 不需要再加斜杠
                    component:Message,
                    children:[
                        {
                            name:'xiangqing',       // 给路由命名
                            path:'detail/:id/:title',
                            component:Detail,
                        }
                    ]
                },
            ]
        },
```

2. 传递参数

```js
<!-- 跳转并携带params参数，to的字符串写法 -->
<router-link :to="/home/message/detail/666/你好">跳转</router-link>
				
<!-- 跳转并携带params参数，to的对象写法 -->
<router-link 
	:to="{
		name:'xiangqing',
		params:{
		   id:666,
            title:'你好'
		}
	}"
>跳转</router-link>
```

> 特别注意：路由携带**params**参数时，若使用**to**的对象写法，则不能使用**path**配置项，必须使用**name**配置项！

3. 接收参数

```js
<ul>
  <li>消息编号：{{ $route.params.id }}</li>
  <li>消息标题：{{ $route.params.title }}</li>
</ul>
```

`src/pages/Message.vue` 

```vue
<template>
  <div>
    <ul>
      <li v-for="m in messageList" :key="m.id">
<!--        跳转路由并携带params参数， to的字符串写法-->
<!--        <router-link :to="`/home/message/detail/${m.id}/${m.title}`">{{ m.title }}</router-link>&nbsp;&nbsp;-->

<!--        跳转路由并携带params参数， to的对象写法（建议写法）-->
        <router-link :to="{
          // path:'/home/message/detail',   // params 不能用path
          name:'xiangqing',
          params:{
            id:m.id,
            title:m.title
          }
        }">
          {{m.title}}
        </router-link>

      </li>
    </ul>
    <hr>
    <router-view/>
  </div>
</template>

<script>
export default {
  name: "Message",
  data(){
    return{
    messageList:[
      {id:'001',title:'消息001'},
      {id:'002',title:'消息002'},
      {id:'003',title:'消息003'},
    ]
    }
  }
}
</script>
```

`src/pages/Detail.vue` 

```vue
<template>
<ul>
  <li>消息编号：{{ $route.params.id }}</li>
  <li>消息标题：{{ $route.params.title }}</li>
</ul>
</template>

<script>
export default {
  name: "Detail",
  mounted() {
    // console.log(this.$route)
  }
}
</script>
```

`src/router/index.js` 

```js
// 该文件专门用于创建整个应用的路由器
import VueRouter from "vue-router";
// 引入组件
import About from "@/pages/About";
import Home from "@/pages/Home";
import News from "@/pages/News";
import Message from "@/pages/Message";
import Detail from "@/pages/Detail";

// 创建一个路由器
const router = new VueRouter({
    routes:[
        // 一级路由
        {
            name:'guanyu',
            path:'/about',
            component:About
        },
        {
            path:'/home',
            component:Home,
            children:[
                {
                    path:'news',    // 不需要再加斜杠
                    component:News,
                },
                {
                    path:'message',    // 不需要再加斜杠
                    component:Message,
                    children:[
                        {
                            name:'xiangqing',       // 给路由命名
                            path:'detail/:id/:title',
                            component:Detail,
                        }
                    ]
                },
            ]
        },
    ]
})

// 暴露路由器
export default router
```

#### 4.4 路由的 props 配置

作用：让路由组件更方便的收到参数

```js
{
	name:'xiangqing',
	path:'detail/:id',
	component:Detail,

	//第一种写法：props值为对象，该对象中所有的key-value的组合最终都会通过props传给Detail组件
	// props:{a:900}

	//第二种写法：props值为布尔值，布尔值为true，则把路由收到的所有params参数通过props传给Detail组件
	// props:true
	
	//第三种写法：props值为函数，该函数返回的对象中每一组key-value都会通过props传给Detail组件
	props($route) {
		return {
		  id: $route.query.id,
		  title:$route.query.title,
		  a: 1,
		  b: 'hello'
		}
	}
}
```

`src/router/index.js` 

```js
// 该文件专门用于创建整个应用的路由器
import VueRouter from "vue-router";
// 引入组件
import About from "@/pages/About";
import Home from "@/pages/Home";
import News from "@/pages/News";
import Message from "@/pages/Message";
import Detail from "@/pages/Detail";

// 创建一个路由器
const router = new VueRouter({
    routes:[
        // 一级路由
        {
            name:'guanyu',
            path:'/about',
            component:About
        },
        {
            path:'/home',
            component:Home,
            children:[
                {
                    path:'news',    // 不需要再加斜杠
                    component:News,
                },
                {
                    path:'message',    // 不需要再加斜杠
                    component:Message,
                    children:[
                        {
                            name:'xiangqing',       // 给路由命名
                            path:'detail',      // 接收query参数，不用占位
                            component:Detail,
                            // props 的第一种写法，值为对象,该对象中的所有key-value都会以props形式传给Detail组件
                            // props:{a:1,b:'hello'}

                            // props 的第二种写法，值为布尔值，若布尔值为真，就会把该路由组件收到的所有params参数，以props形式传给Detail组件
                            // props:true

                            // props 的第三种写法，值为函数
                            props($route){        // 常用写法
                                return{
                                    id:$route.query.id,
                                    title:$route.query.title
                                }
                            }
                            /*props({query}){
                                return{id:query.id,title:query.title}
                            }*/
                            /*props({query:{id,title}}){        // 不推荐写法
                                return{id,title}
                            }*/
                        }
                    ]
                },
            ]
        },
    ]
})

// 暴露路由器
export default router
```

`src/pages/Message.vue` 

```vue
<template>
  <div>
    <ul>
      <li v-for="m in messageList" :key="m.id">
<!--        跳转路由并携带params参数， to的字符串写法-->
<!--        <router-link :to="`/home/message/detail/${m.id}/${m.title}`">{{ m.title }}</router-link>&nbsp;&nbsp;-->

<!--        跳转路由并携带params参数， to的对象写法（建议写法）-->
        <router-link :to="{
          // path:'/home/message/detail',   // params 不能用path
          name:'xiangqing',
         query:{
            id:m.id,
            title:m.title
          }
        }">
          {{m.title}}
        </router-link>

      </li>
    </ul>
    <hr>
    <router-view/>
  </div>
</template>

<script>
export default {
  name: "Message",
  data(){
    return{
    messageList:[
      {id:'001',title:'消息001'},
      {id:'002',title:'消息002'},
      {id:'003',title:'消息003'},
    ]
    }
  }
}
</script>
```

`src/pages/Detail.vue` 

```vue
<template>
<ul>
  <li>消息编号：{{ id }}</li>
  <li>消息标题：{{ title }}</li>
<!--  <li>a:{{a}}</li>
  <li>b:{{b}}</li>    // 第一种props展示
  -->
</ul>
</template>

<script>
export default {
  name: "Detail",
  // props:['a','b'],  // 第一种props接收
  props:['id','title'],  // 第二三种props接收

  mounted() {
    // console.log(this.$route)
  }
}
</script>
```

#### 4.5 router-link 的 replace 属性

1. 作用：控制路由跳转时操作浏览器历史纪录的模式
2. 浏览器的历史记录有两种写入方式：分别为**`push`**和**`replace`**，**`push`**是追加历史记录，**`replace`**是替换当前记录。路由跳转时候默认为**`push`**
3. 如何开启**`replace`**模式：**`<router-link replace ...... >News</router-link>`**

```js
<router-link :replace='true' class="list-group-item" active-class="active" :to="{name:'guanyu'}">About</router-link>
<router-link replace class="list-group-item" active-class="active" to="/home">Home</router-link>
```

### 5. 编程式路由导航

1. 作用：不借助**`<router-link>`**实现路由跳转，让路由跳转更加灵活
2. 具体编码：

```js
//$router的两个API
this.$router.push({
	name:'xiangqing',
		params:{
			id:xxx,
			title:xxx
		}
})

this.$router.replace({
	name:'xiangqing',
		params:{
			id:xxx,
			title:xxx
		}
})
this.$router.forward() //前进
this.$router.back() //后退
this.$router.go() //可前进也可后退
```

`src/pages/Message.vue` 

```vue
<template>
  <div>
    <ul>
      <li v-for="m in messageList" :key="m.id">
<!--        跳转路由并携带params参数， to的字符串写法-->
<!--        <router-link :to="`/home/message/detail/${m.id}/${m.title}`">{{ m.title }}</router-link>&nbsp;&nbsp;-->

<!--        跳转路由并携带params参数， to的对象写法（建议写法）-->
        <router-link :to="{
          // path:'/home/message/detail',   // params 不能用path
          name:'xiangqing',
          query:{
            id:m.id,
            title:m.title
          }
        }">
          {{m.title}}
        </router-link>
        <button @click="pushShow(m)">push查看</button>
        <button @click="replaceShow(m)">replace查看</button>
      </li>
    </ul>
    <hr>
    <router-view/>
  </div>
</template>

<script>
export default {
  name: "Message",
  data(){
    return{
    messageList:[
      {id:'001',title:'消息001'},
      {id:'002',title:'消息002'},
      {id:'003',title:'消息003'},
    ]
    }
  },
  methods:{
    pushShow(m){
      // console.log(this.$router)
      this.$router.push({
        name:'xiangqing',
        query:{
          id:m.id,
          title:m.title
        }
      })
    },
    replaceShow(m){
    this.$router.replace({
      name:'xiangqing',
      query: {
        id:m.id,
        title:m.title
      }
    })
    }
  }
}
</script>
```

`src/components/Banner.vue` 

```vue
<template>
  <div class="col-xs-offset-2 col-xs-8">
    <div class="page-header"><h2>Vue Router Demo</h2>
    <button @click="back">后退</button>
    <button @click="forward">前进</button>
    <button @click="test">测试一下go</button>
    </div>
    </div>
</template>

<script>
export default {
  name: "Banner",
  methods: {
    back() {
      this.$router.back()
      // console.log(this.$router)
    },
    forward(){
      this.$router.forward()
    },
    test(){
      this.$router.go(-2)   // -2后退两步， 2前进两步
    }
  }
}
</script>
```

### 6. 缓存路由组件

1. 作用：让不展示的路由组件保持挂载，不被销毁
2. 具体编码：

```js
<!--      include后跟组件名-->
// 缓存一个路由组件
<keep-alive include="News">
  <router-view/>
</keep-alive>
```

```js
// 缓存多个路由组件
<keep-alive :include="['News','Message']">
  <router-view/>
</keep-alive>
```

`src/pages/News.vue` 

```vue
<template>
    <ul>
      <li>news001 <input type="text"></li>
      <li>news002 <input type="text"></li>
      <li>news003 <input type="text"></li>
    </ul>
</template>

<script>
export default {
  name: "News",
  /*beforeDestroy() {
    console.log('News组件即将被销毁了')
  }*/
}
</script>
```

`src/pages/Home.vue` 

```vue
<template>
  <div>
    <h2>Home组件内容</h2>
    <div>
      <ul class="nav nav-tabs">
        <li>
          <router-link class="list-group-item" active-class="active" to="/home/news">News</router-link>
        </li>
        <li>
          <router-link class="list-group-item " active-class="active" to="/home/message">Message</router-link>
        </li>
      </ul>
<!--      include后跟组件名-->
<!--      <keep-alive include="News">
        <router-view/>
      </keep-alive>-->
      <keep-alive :include="['News','Message']">
        <router-view/>
      </keep-alive>
    </div>
  </div>
</template>

<script>
export default {
  name: "Home",
 /* beforeDestroy() {
    console.log('Home组件即将被销毁了')
  },
  mounted() {
    console.log('Home组件挂载完毕了')
  }*/
}
</script>
```

### 7. 两个新的生命周期钩子

**`activated`**和**`deactivated`**是路由组件所独有的两个钩子，用于捕获路由组件的激活状态

- `activated`路由组件被激活时触发
- `deactivated`路由组件失活时触发

> 这两个生命周期钩子需要配合前面的缓存路由组件使用（没有缓存路由组件不起效果）

> 除了**activated**和**deactivated**，还有一个生命周期钩子**$nextTick**也不在生命周期图中

`src/pages/News.vue` 

```vue
<template>
    <ul>
      <li :style="{opacity}">欢迎学习Vue</li>
      <li>news001 <input type="text"></li>
      <li>news002 <input type="text"></li>
      <li>news003 <input type="text"></li>
    </ul>
</template>

<script>
export default {
  name: "News",
  data(){
    return{
      opacity: 1
    }
  },
  /*beforeDestroy() {
    console.log('News组件即将被销毁了')
    clearInterval(this.timer)
  },*/
  /*mounted() {
    this.timer = setInterval(()=>{
      this.opacity -= 0.01
      if (this.opacity <= 0) this.opacity = 1
    },16)
  },*/
  activated() {     // 激活
    console.log('News组件被激活了')
    this.timer = setInterval(()=>{
      this.opacity -= 0.01
      if (this.opacity <= 0) this.opacity = 1
    },16)
  },
  deactivated() {   // 失活
    console.log('News组件失活了')
    clearInterval(this.timer)
  },

}
</script>
```

**setTimeout**和**setInterval**的区别：`setTimeout()`只执行函数一次，如果需要多次调用可以使用`setInterval()`，或者在函数体内再次调用`setTimeout()`；`setInterval()`会不停的调用函数，直到`clearInterval()`被调用或者窗口被关闭，由`setInterval()`返回的 ID 值可用作`clearInterval()`方法的参数。

### 8. 路由守卫

**作用**：对路由进行权限控制

**分类**：全局守卫、独享守卫、组件内守卫

#### 8.1 全局守卫

**`meta`**路由源信息

```js
//全局前置守卫：初始化时执行、每次路由切换前执行
router.beforeEach((to,from,next)=>{
	console.log('beforeEach',to,from)
	if(to.meta.isAuth){ //判断当前路由是否需要进行权限控制
		if(localStorage.getItem('school') === 'zhejiang'){ //权限控制的具体规则
			next() //放行
		}else{
			alert('暂无权限查看')
			// next({name:'guanyu'})
		}
	}else{
		next() //放行
	}
})

//全局后置守卫：初始化时执行、每次路由切换后执行
router.afterEach((to,from)=>{
	console.log('afterEach',to,from)
	if(to.meta.title){ 
		document.title = to.meta.title //修改网页的title
	}else{
		document.title = 'vue_test'
	}
})
```

`src/router/index.js` 

```js
// 该文件专门用于创建整个应用的路由器
import VueRouter from "vue-router";
// 引入组件
import About from "@/pages/About";
import Home from "@/pages/Home";
import News from "@/pages/News";
import Message from "@/pages/Message";
import Detail from "@/pages/Detail";

// 创建一个路由器
const router = new VueRouter({
    routes:[
        // 一级路由
        {
            name:'guanyu',
            path:'/about',
            component:About,
            meta:{title:'关于'},
        },
        {
            name:'zhuye',
            path:'/home',
            component:Home,
            meta:{title:'主页'},
            children:[
                {
                    name:'xinwen',
                    path:'news',    // 不需要再加斜杠
                    component:News,
                    meta:{isAuth:true,title:'新闻'}      // meta:{} 路由元信息，程序员自定义的信息

                },
                {
                    name:'xiaoxi',
                    path:'message',    // 不需要再加斜杠
                    component:Message,
                    meta:{isAuth:true,title:'消息'},      // meta:{} 路由元信息，程序员自定义的信息
                    children:[
                        {
                            name:'xiangqing',       // 给路由命名
                            path:'detail',      // 接收query参数，不用占位
                            component:Detail,
                            meta:{isAuth:true,title:'详情'},      // meta:{} 路由元信息，程序员自定义的信息
                            // props 的第一种写法，值为对象,该对象中的所有key-value都会以props形式传给Detail组件
                            // props:{a:1,b:'hello'}

                            // props 的第二种写法，值为布尔值，若布尔值为真，就会把该路由组件收到的所有params参数，以props形式传给Detail组件
                            // props:true

                            // props 的第三种写法，值为函数
                            props($route){        // 常用写法
                                return{
                                    id:$route.query.id,
                                    title:$route.query.title
                                }
                            }
                            /*props({query}){
                                return{id:query.id,title:query.title}
                            }*/
                            /*props({query:{id,title}}){        // 不推荐写法
                                return{id,title}
                            }*/
                        }
                    ]
                },
            ]
        },
    ]
})

// 暴露之前加路由守卫
// 全局前置路由守卫————初始化的时候被调用、每次路由切换之前被调用
// router.beforeEach((to,from,next)=>{
router.beforeEach((to,from,next)=>{
    console.log('前置路由守卫',to,from)
    // if(to.path === '/home/news' || to.path === '/home/message'){
    // if(to.name === 'xinwen' || to.name === 'xiaoxi'){
    if(to.meta.isAuth){     // 判断是否需要鉴权
        if (localStorage.getItem('school')==='atxx'){
            // document.title = to.meta.title || '路由系统'        // 每次next放行之前写（放在后置守卫更方便）
            next()
        }else {
            alert('学校不对，无权限查看')
        }
    }else {
        // document.title = to.meta.title || '路由系统'        // 每次next放行之前写（放在后置守卫更方便）
        next()
    }
})

// 全局后置路由守卫————初始化的时候被调用、每次路由切换之后被调用
router.afterEach((to,from,next)=>{
    console.log('后置路由守卫',to,from)
    document.title = to.meta.title || '路由系统'
})

// 暴露路由器
export default router
```

#### 8.2 独享守卫

在`routes`子路由内写守卫

```js
beforeEnter(to,from,next){
	console.log('beforeEnter',to,from)
	if(to.meta.isAuth){ //判断当前路由是否需要进行权限控制
		if(localStorage.getItem('school') === 'atguigu'){
			next()
		}else{
			alert('暂无权限查看')
			// next({name:'guanyu'})
		}
	}else{
		next()
	}
}
```

`src/router/index.js` 

```js
// 该文件专门用于创建整个应用的路由器
import VueRouter from "vue-router";
// 引入组件
import About from "@/pages/About";
import Home from "@/pages/Home";
import News from "@/pages/News";
import Message from "@/pages/Message";
import Detail from "@/pages/Detail";

// 创建一个路由器
const router = new VueRouter({
    routes:[
        // 一级路由
        {
            name:'guanyu',
            path:'/about',
            component:About,
            meta:{title:'关于'},
        },
        {
            name:'zhuye',
            path:'/home',
            component:Home,
            meta:{title:'主页'},
            children:[
                {
                    name:'xinwen',
                    path:'news',    // 不需要再加斜杠
                    component:News,
                    meta:{isAuth:true,title:'新闻'},    // meta:{} 路由元信息，程序员自定义的信息
                    beforeEnter:(to,from,next)=>{
                        console.log('独享路由守卫',to,from)
                        if (to.meta.isAuth){    // 判断是否需要鉴权
                            if (localStorage.getItem('school')==='atxx'){
                                next()
                            }else {
                                alert('学校名不对，无查看权限')
                            }
                        }else {
                            next()
                        }
                    }
                },
                {
                    name:'xiaoxi',
                    path:'message',    // 不需要再加斜杠
                    component:Message,
                    meta:{isAuth:true,title:'消息'},      // meta:{} 路由元信息，程序员自定义的信息
                    children:[
                        {
                            name:'xiangqing',       // 给路由命名
                            path:'detail',      // 接收query参数，不用占位
                            component:Detail,
                            meta:{isAuth:true,title:'详情'},      // meta:{} 路由元信息，程序员自定义的信息
                            // props 的第一种写法，值为对象,该对象中的所有key-value都会以props形式传给Detail组件
                            // props:{a:1,b:'hello'}

                            // props 的第二种写法，值为布尔值，若布尔值为真，就会把该路由组件收到的所有params参数，以props形式传给Detail组件
                            // props:true

                            // props 的第三种写法，值为函数
                            props($route){        // 常用写法
                                return{
                                    id:$route.query.id,
                                    title:$route.query.title
                                }
                            }
                            /*props({query}){
                                return{id:query.id,title:query.title}
                            }*/
                            /*props({query:{id,title}}){        // 不推荐写法
                                return{id,title}
                            }*/
                        }
                    ]
                },
            ]
        },
    ]
})

// 暴露之前加路由守卫
// 全局前置路由守卫————初始化的时候被调用、每次路由切换之前被调用
/*// router.beforeEach((to,from,next)=>{
router.beforeEach((to,from,next)=>{
    console.log('前置路由守卫',to,from)
    // if(to.path === '/home/news' || to.path === '/home/message'){
    // if(to.name === 'xinwen' || to.name === 'xiaoxi'){
    if(to.meta.isAuth){     // 判断是否需要鉴权
        if (localStorage.getItem('school')==='atxx'){
            // document.title = to.meta.title || '路由系统'        // 每次next放行之前写（放在后置守卫更方便）
            next()
        }else {
            alert('学校不对，无权限查看')
        }
    }else {
        // document.title = to.meta.title || '路由系统'        // 每次next放行之前写（放在后置守卫更方便）
        next()
    }
})*/

// 全局后置路由守卫————初始化的时候被调用、每次路由切换之后被调用
router.afterEach((to,from,next)=>{
    console.log('后置路由守卫',to,from)
    document.title = to.meta.title || '路由系统'
})

// 暴露路由器
export default router
```

#### 8.3 组件内守卫

在具体组件内写守卫

```js
//进入守卫：通过路由规则，进入该组件时被调用
beforeRouteEnter (to, from, next) {
},
//离开守卫：通过路由规则，离开该组件时被调用
beforeRouteLeave (to, from, next) {
}
```

`src/router/index.js` 

```js
// 该文件专门用于创建整个应用的路由器
import VueRouter from "vue-router";
// 引入组件
import About from "@/pages/About";
import Home from "@/pages/Home";
import News from "@/pages/News";
import Message from "@/pages/Message";
import Detail from "@/pages/Detail";

// 创建一个路由器
const router = new VueRouter({
    routes:[
        // 一级路由
        {
            name:'guanyu',
            path:'/about',
            component:About,
            meta:{isAuth:true,title:'关于'},
        },
        {
            name:'zhuye',
            path:'/home',
            component:Home,
            meta:{title:'主页'},
            children:[
                {
                    name:'xinwen',
                    path:'news',    // 不需要再加斜杠
                    component:News,
                    meta:{isAuth:true,title:'新闻'},    // meta:{} 路由元信息，程序员自定义的信息
                    /*beforeEnter:(to,from,next)=>{
                        console.log('独享路由守卫',to,from)
                        if (to.meta.isAuth){    // 判断是否需要鉴权
                            if (localStorage.getItem('school')==='atxx'){
                                next()
                            }else {
                                alert('学校名不对，无查看权限')
                            }
                        }else {
                            next()
                        }
                    }*/
                },
                {
                    name:'xiaoxi',
                    path:'message',    // 不需要再加斜杠
                    component:Message,
                    meta:{isAuth:true,title:'消息'},      // meta:{} 路由元信息，程序员自定义的信息
                    children:[
                        {
                            name:'xiangqing',       // 给路由命名
                            path:'detail',      // 接收query参数，不用占位
                            component:Detail,
                            meta:{isAuth:true,title:'详情'},      // meta:{} 路由元信息，程序员自定义的信息
                            // props 的第一种写法，值为对象,该对象中的所有key-value都会以props形式传给Detail组件
                            // props:{a:1,b:'hello'}

                            // props 的第二种写法，值为布尔值，若布尔值为真，就会把该路由组件收到的所有params参数，以props形式传给Detail组件
                            // props:true

                            // props 的第三种写法，值为函数
                            props($route){        // 常用写法
                                return{
                                    id:$route.query.id,
                                    title:$route.query.title
                                }
                            }
                            /*props({query}){
                                return{id:query.id,title:query.title}
                            }*/
                            /*props({query:{id,title}}){        // 不推荐写法
                                return{id,title}
                            }*/
                        }
                    ]
                },
            ]
        },
    ]
})

// 暴露之前加路由守卫
// 全局前置路由守卫————初始化的时候被调用、每次路由切换之前被调用
/*// router.beforeEach((to,from,next)=>{
router.beforeEach((to,from,next)=>{
    console.log('前置路由守卫',to,from)
    // if(to.path === '/home/news' || to.path === '/home/message'){
    // if(to.name === 'xinwen' || to.name === 'xiaoxi'){
    if(to.meta.isAuth){     // 判断是否需要鉴权
        if (localStorage.getItem('school')==='atxx'){
            // document.title = to.meta.title || '路由系统'        // 每次next放行之前写（放在后置守卫更方便）
            next()
        }else {
            alert('学校不对，无权限查看')
        }
    }else {
        // document.title = to.meta.title || '路由系统'        // 每次next放行之前写（放在后置守卫更方便）
        next()
    }
})*/

// 全局后置路由守卫————初始化的时候被调用、每次路由切换之后被调用
router.afterEach((to,from,next)=>{
    console.log('后置路由守卫',to,from)
    document.title = to.meta.title || '路由系统'
})

// 暴露路由器
export default router
```

`src/pages/About.vue` 

```vue
<template>
  <h2>我是About的内容</h2>
</template>

<script>
export default {
  name: "About",
  /*beforeDestroy() {
    console.log('About组件即将被销毁了')
  },
  mounted() {
    console.log('About组件挂载完毕了')
  }*/
  /*mounted() {
    console.log('%%%',this.$route)
  }*/

  // 通过路由规则，进入该组件时被调用
  beforeRouteEnter(to, from, next) {
    console.log('About--beforeRouteEnter', to, from)
    if (to.meta.isAuth) {    // 判断是否需要鉴权
      if (localStorage.getItem('school') === 'atxx') {
        next()
      } else {
        alert('学校名不对，无查看权限')
      }
    } else {
      next()
    }
  },

  // 通过路由规则，离开该组件时被调用
  beforeRouteLeave(to,from,next){
    console.log('About--beforeRouteLeave', to, from)
    next()
  },
}
</script>
```

### 9. 路由器的两种工作模式

1. 对于一个`url`来说，什么是`hash`值？

   **`#`及其后面的内容就是`hash`值**

2. `hash`值不会包含在`HTTP`请求中，即：`hash`值不会带给服务器

3. **`hash`模式**

   - 地址中永远带着 `#` 号，不美观
   - 若以后将地址通过第三方手机app分享，若app校验严格，则地址会被标记为不合法
   - 兼容性较好

4. **`history`模式**

   - 地址干净、美观
   - 兼容性和`hash`模式相比略差
   - 应用部署上线时需要后端人员支持，解决刷新页面服务端404的问题

```js
......
// 创建一个路由器
const router = new VueRouter({
    mode:'history',     // history模式 和 hash模式
    routes:[...]
})
    
// 暴露路由器
export default router
```

### 10. 快速创建测试demo

1. 项目打包后，桌面创建文件夹`demo` 
2. `Webstorm`或`VScode`打开文件夹，终端输入命令`npm init` 
3. `package name: (demo)`后输入包名，例如`atxx_test_server` ,后接`ENTER`
4. 输入命令`npm i express` 等待安装
5. 在项目目录处新建主文件`server.js` 

`server.js` 

```js
const express = require('express')

const app = express()
app.use(express.static(__dirname+'/static'))

app.get('/person',(req,res)=>{
    res.send({
        name:'tom',
        age:18
    })
})

app.listen(5005,(err)=>{
    if (!err) console.log('服务器启动成功了！')
})
```

6. 网页输入`localhost:5005/person` 测试微型服务器是否可用
7. 项目目录创建静态文件夹`static` ，将打包好的`dist`文件夹中的内容全部 CV 进`static`文件夹中

> 此时`history`模式网页刷新会报错404，需要后端配合解决，`hash`模式不会报错，但路径不好看

8. `history`模式页面打包后，在`demo`项目终端输入`npm i connect-history-api-fallback`进行安装，安装完成后在页面进行引入`const history = require('connect-history-api-fallback')` ,并使用`app.use(history())`

`server.js` 

```js
const express = require('express')
const history = require('connect-history-api-fallback')

const app = express()
app.use(history())
app.use(express.static(__dirname+'/static'))

app.get('/person',(req,res)=>{
    res.send({
        name:'tom',
        age:18
    })
})

app.listen(5005,(err)=>{
    if (!err) console.log('服务器启动成功了！')
})
```

## 七、Vue UI 组件库

### 1. 移动端常用 UI 组件库

1. **Vant**	https://youzan.github.io/vant
2. **Cube UI**     https://didi.github.io/cube-ui
3. **Mint UI**     http://mint-ui.github.io

### 2. PC 端常用 UI 组件库

1. **Element UI**     https://element.eleme.cn
2. **IView UI**     https://www.iviewui.com

### 3. Element-ui

1. 安装`element-ui`：`npm i element-ui -S` 
2. 按需引入，借助`babel-plugin-component`，安装命令`npm install babel-plugin-component -D`，修改`babel-config-js` 







