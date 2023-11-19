- Vue 是一个渐进式的JS的框架

  - 以前有接触过JS库：jQuery， Zepto，axios （JS 的库会一属性 和 方法 来帮助我们完成的一些功能目的就是简化代码
    - 比如：你封装好一个函数供其他人去用那你的代码就是JS库

  - 框架 ：是一个条条框框， 是一个约束， 如果你要使用前端框架必须要遵守框架提供的规则， 所以我们要学习vue的使用规则 一级 内部的使用方式
  - Vue将网页开发中最常用的DOM操作内置到了框架之中， 所以我们在操作中可以更加专注==数据逻辑的处理==  而不是需要繁琐的DOM操作
  - 可以将数据和页面的呈现将分离有效的降低了代码的耦合度
  - VUE支持组件化开发

- 安装有两种方式
  - 用scpirt 标签引用 （下载CDN）
  - Vue CLI 
- 引入vue 之后开始创建vue实例使用

# 1.响应式数据

- 前端的工作就是把数据呈现到页面显示，以前普通的数据， 每次修改数据都要手动去改
- 响应式数据值得是：在Vue内部对主句操作， 它会自动的更新搞试图中

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20231108134356615.png" alt="image-20231108134356615" style="zoom:50%;" />

- 响应式的使用方式：
  - el 属性： Vue 实例的配置属性 （ 用于设置Vue 的生效位置）
  - 声明响应式数据：data 用于声明响应式数据 （ 数据写在 return 里面 - 固定写法）
- 好处： 以后数据修改， 试图也会自动修改
- Vue 内部的数据可以通过Vue实例来访问 （检测响应式数据直接在这里修改）
- data 中的数据会被添加到实例上
  - 访问数据：实例.属性名
  - 修改数据：实例.属性名 = 新值


<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20231108135120139.png" alt="image-20231108135120139" style="zoom: 25%;" />

# 2.插值表达式： （渲染语法）

- 是一个固定的语法结构在内部可以直接以名称的方式来访问响应式数据

- 利用表达式进行插值，渲染到页面中

-  插值表达式可以做一些简单的计算操作： 数学计算， 逻辑计算

~~~javascript
money + 100
money - 100
money * 10
money / 10 
price >= 100 ? '真贵':'还行'
obj.name
arr[0]
fn()
obj.fn()
~~~

- 语法：插值表达式语法：`=={{ 表达式 }}==`

~~~javascript
<h3>{{title}}<h3>
// toUpperCase() 方法用于把字符串转换为大写//
<p>{{nickName.toUpperCase()}}</p> 

<p>{{age >= 18 ? '成年':'未成年'}}</p>

<p>{{obj.name}}</p>

<p>{{fn()}}</p>
~~~



- 注意：
  - 使用的数据必须要存在 （data）
  - 因为是一种文本给用户看， 所以不能在标签属性中使用{{}}

# 3.methods 属性 （调用时使用括号）

- 用于封装逻辑代码 ( 复杂的逻辑不能直接写在插值表达式)
  - 例如：在method 里面写函数然后用插值表达式调用
  - 可以再内部通过 this 的方式来使用外边的响应式数据（data的数据）

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20231108135947019.png" alt="image-20231108135947019" style="zoom:25%;" />

# 3.计算属性 computed （调用时不要使用括号）

- 计算属性：*基于现有的数据，计算出来的新属性。 依赖的数据变化，自动重新计算 （比如算总计产品， 里面有任何产品数量变化，结果自动改变）
- 书写是一个函数， 可以直接复制 methods方法
- 具有缓存性： 第一次计算的时候会把计算结果去在内部做一个缓存， 如果第二次发现 响应式如果没变 意味着整体结果也没变  - 不会再计算了 - 直接用

![image-20231108140730132](/Users/guoguo/Library/Application Support/typora-user-images/image-20231108140730132.png)

~~~javascript
 <p>礼物总数：{{ totalCount }} 个</p>

<script>
    const app = new Vue({
      el: '#app',
      data: {
        // 现有的数据
        list: [
          { id: 1, name: '篮球', num: 1 },
          { id: 2, name: '玩具', num: 2 },
          { id: 3, name: '铅笔', num: 5 },
        ]
      },
      computed: {
        totalCount () {
          
          // 需求：对 this.list 数组里面的 num 进行求和 → reduce
          let total = this.list.reduce((sum, item) => sum + item.num, 0)
          return total
        }
      }
    })
  </script>

//2.算总和用 reduce()

this.list.reduce((sum, item) => sum + item.num, 0) 

//sum = 每次计算总和

//item.num = 当前的值
~~~

### 计算属性完整写法

- **既然计算属性也是属性，能访问，应该也能修改了？**

1. 计算属性默认的简写，只能读取访问，不能 "修改"
2. 如果要 "修改"  → 需要写计算属性的完整写法

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20231109131652795.png" alt="image-20231109131652795" style="zoom:80%;" />

![image-20231109132050247](/Users/guoguo/Library/Application Support/typora-user-images/image-20231109132050247.png)

~~~javascript
<body>

  <div id="app">
    姓：<input type="text" v-model="firstName"> +
    名：<input type="text" v-model="lastName"> =
    <span>{{ fullName }}</span><br><br>
    
    <button @click="changeName">改名卡</button>
  </div>
  <script>
    const app = new Vue({
      el: '#app',
      data: {
        firstName: '刘',
        lastName: '备',
      },
      methods: {
        changeName () {
          this.fullName = '黄忠' 
        }
      },
      computed: {
        fullName: {
          get () {
            return this.firstName + this.lastName
          },
          set (value) {
            this.firstName = value.slice(0, 1)
            this.lastName = value.slice(1)
          }
        }
      }
    })
  </script>
</body>

总结：
2. get 直接 return 返回值作为求值的结果 （求data 的 值）
3. set 修改 data 的值

set (value) - 形参 = （this.fullName = '黄忠'）- 实参

set 获取实参后 （value），然后修改 firstName 跟 lastName 的值

4. slice(0, 1)  分出来 获取 [0,1) --> 没有获取1
~~~



# 4.侦听器 watch

- 监听某个数据的变化， 然后再某个数据变化的时候并不仅仅更新页面， 还能做点别的。 这样以前做不了， 以前只要数据更新页面就更新
- 必须要是响应式数据
- 应用场景： 数据变化做个输出，数据变化进行方法的使用， 调用一些库发请求

![image-20231108141325591](/Users/guoguo/Library/Application Support/typora-user-images/image-20231108141325591.png)

- 完整写法 → 添加额外配置项

  (1) deep: true 对复杂类型深度监视 （表示会监视所有对象的属性）

  (2) immediate: true 一进页面就执行一次

  ~~~javascript
  <div class="query">
          <span>翻译成的语言：</span>
          <select v-model="obj.lang">
            <option value="italy">意大利</option>
            <option value="english">英语</option>
            <option value="german">德语</option>
          </select>
  </div>
  <script>
  const app = new Vue({
          el: '#app',
          data: {
            obj: {
              words: '小黑',
              lang: 'italy'
            },
            result: '', // 翻译结果
          },
          watch: {
            obj: {
              deep: true, // 深度监视
              immediate: true, // 立刻执行，一进入页面handler就立刻执行一次
              handler (newValue) {
                clearTimeout(this.timer)
                this.timer = setTimeout(async () => {
                  const res = await axios({
                    url: 'https://applet-base-api-t.itheima.net/api/translate',
                    params: newValue
                  })
                  this.result = res.data.data
                  console.log(res.data.data)
                }, 300)
              }
            }
  
  
  
  ~~~

  

# 5.指令 （试图显示）

## 5.1 v-text 跟 v-html

- 都让内容显示出来
- v-html 可以解析标签 （如果里面有标签值解析显示标签里面的内容）

~~~javascript

<div id="app">
    <h2>个人信息</h2>
	// 既然指令是vue提供的特殊的html属性，所以咱们写的时候就当成属性来用即可
    <p v-text="uname">姓名：</p>  // 张三
    <p v-html="intro">简介：</p>   // 非常优秀的boy
  </div> 

<script>
        const app = new Vue({
            el:'#app',
            data:{
                uname:'张三', //标签属性
                intro:'<h2>这是一个<strong>非常优秀</strong>的boy<h2>'
            }
        })
</script>
~~~



## 5.2 条件渲染指令(v-show、v-if、v-else、v-else-if）

## 5.3 事件绑定指令 （v-on）

## 5.4 属性绑定指令（v-bind）

- **作用：**动态设置html的标签属性  比如：src、url、title
  - 动态属性：可以使用响应式数据

~~~javascript
<body>
  <div id="app">
    <!-- v-bind:src   =>   :src -->
    <img v-bind:src="imgUrl" v-bind:title="msg" alt="">
    <img :src="imgUrl" :title="msg" alt="">
  </div>
  <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
  <script>
    const app = new Vue({
      el: '#app',
      data: {
        imgUrl: './imgs/10-02.png',
        msg: 'hello 波仔'
      }
    })

  </script>
</body>
~~~

### 6.4.1 v-bind操作class

- 语法

~~~html
<div> :class = "对象/数组">这是一个div</div>
~~~

#### 对象语法

~~~html
<div class="box" :class="{ 类名1: 布尔值, 类名2: 布尔值 }"></div> 
~~~



- 当class动态绑定的是**对象**时
  - **键就是类名:  color : true (要加上这个类名， false 没有这个类名）
  - **值就是布尔值**，如果值是**true**，就有这个类，否则没有这个类
- 使用场景： tab栏切换效果 （ 一个类名 切换 ： true  false ）

#### 数组语法

~~~html
<div class="box" :class="[ 类名1, 类名2, 类名3 ]"></div>
~~~

- 当class动态绑定的是**数组**时
  -  数组中所有的类，都会添加到盒子上
- 使用场景：批量添加或者 删除

| <img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20231109113602606.png" alt="image-20231109113602606" style="zoom:25%;" /> |
| ------------------------------------------------------------ |

~~~javascript
需求：.pink 盒子变粉色， .big 盒子变大
<body>
  <div id="app">
    //对象添加 或者 溢出类名
    <div class="box" :class="{ pink: true, big: true }">黑马程序员</div>
    //数组 添加全部里面的类名
    <div class="box" :class="['pink', 'big']">黑马程序员</div>
  </div>
  <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
  <script>
    const app = new Vue({
      el: '#app',
      data: {

      }
    })
  </script>
</body>
~~~

### 6.4.1 v-bind操作style

- 添加动态行内样式
  - 例如：每次点击都把盒子放大 20px width = big + 'px'
    - 准备 data = 需要动态的 （big ：0）
    - button 每次点击 修改 data的值 big 修改 ==》 width自动修改

- 语法

~~~javascript
<div class="box" :style="{ CSS属性名1: CSS属性值, CSS属性名2: CSS属性值 }"></div>

<div class="box" :style="{ width: '400px', height: '400px', backgroundColor: 'green' }"></div>
~~~

- 代码演示：进度条
  - 需求： 点击对应按钮 进度条对应现实
    - 动态样式

![image-20231109115242818](/Users/guoguo/Library/Application Support/typora-user-images/image-20231109115242818.png)

~~~javascript
<body>
  <div id="app">
    <!-- 外层盒子底色 （黑色） -->
    <div class="progress">
      <!-- 内层盒子 - 进度（蓝色） -->
      <div class="inner" :style="{ width: percent + '%' }">
        <!-- % 显示  -->
        <span>{{ percent }}%</span>
      </div>
    </div>
    <button @click="percent = 25">设置25%</button>
    <button @click="percent = 50">设置50%</button>
    <button @click="percent = 75">设置75%</button>
    <button @click="percent = 100">设置100%</button>
  </div>
  <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
  <script>
    const app = new Vue({
      el: '#app',
      //添加变量
      data: {
        percent: 30
      }
    })
  </script>
</body>
~~~



## 5.5 渲染指令：  v-for

- **作用：** 基于数据循环， 多次渲染整个元素 → 数组、对象、数字...

- **遍历数组语法：**

  - v-for = "(item, index) in 数组" 

    - item 每一项， index 下标 

    -  省略 index: v-for = "item in 数组

### v-for 中的 key

- **语法：**key属性 = "唯一标识" 
- **作用：**给列表项添加的唯一标识。便于Vue进行列表项的正确排序复用 （li 里面的每一个） 
- 加 key  跟 不加 key 的区别
  - 没有加key删除的第一个li的样式没有被删除
  - v-for 的默认行为会尝试 原地修改元素 （就地复用） ：就是没有给 li 起名字， vue 不知道要删哪个li 所以把最后一个删， 文字起上去 所以第一个li样式还保留

![image-20231109105052611](/Users/guoguo/Library/Application Support/typora-user-images/image-20231109105052611.png)

![image-20231109104940528](/Users/guoguo/Library/Application Support/typora-user-images/image-20231109104940528.png)

- 注意：
  - key 的值只能是 字符串 或 数字类型 
  - key 的值必须具有 唯一性 
  - 推荐使用 id 作为 key（唯一），不推荐使用 index 作为 key（会变化，不对应），因为下标会变

![image-20231109105454008](/Users/guoguo/Library/Application Support/typora-user-images/image-20231109105454008.png)

~~~javascript
<ul>
  <li v-for="(item, index) in booksList" :key="item.id">
    <span>{{ item.name }}</span>
    <span>{{ item.author }}</span>
    <button @click="del(item.id)">删除</button>
  </li>
</ul>
~~~

## 5.6 双向绑定指令 （v - model)

- 表单指令 ： v - mode 专门给表单使用实现双向数据绑定

  -  单向绑定： 数据更改视图自动更改

  -  双向绑定： 试图进行修改数据， 数据修改视图

- 可以快速 获取 或 设置 表单元素内容

- 属性指令
  - 平时一个属性要写固定的值， 不能插入响应式的属性进行动态渲染
  - ==class 跟 style 属性绑定==

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20231108142653800.png" alt="image-20231108142653800" style="zoom:50%;" />



### v-model在其他表单元素的使用

- 每个表单需要获取的值不一样， 但是 v-model 自动选取  **正确的方法** 来更新元素

~~~javascript
输入框  input:text   ——> value
文本域  textarea	 ——> value
复选框  input:checkbox  ——> checked
单选框  input:radio   ——> checked
下拉菜单 select    ——> value
~~~





# 6- 修饰符

- 用来实现与指令相关的常用操作
- 目标：
  -  用来简化代码 和 Dom操作 ： 事件里面的事件冒泡， 事件捕获的拿下情况 如果不需要出发 用修饰符来阻止这些标签的默认行为， 除了dom操作还可以做内容处理操作
  
- 语法：
  - 通过 "." 指明一些指令 后缀，不同 后缀 封装了不同的处理操作


## 6.1 按键修饰符

- @keyup.enter  —>当点击enter键的时候才触发 （@keyup 键盘弹起， 只监听 enter键）
  - 需求：
    - 输入内容后店家添加按钮 就触发添加下面数据 
    -  需求更改： 输入后按回车内容添加到下面

~~~javascript
 //需求： 键盘回车打印输入框输入内容
<input @keyup.enter="fn" v-model="username" type="text">
   
  <script>
    const app = new Vue({
      el: '#app',
      data: {
        username: ''
      },
      methods: {
        fn (e) {
          console.log('键盘回车的时候触发', this.username)
        }
~~~

## 6.2 v-model修饰符

- v-model.trim  —>去除首位空格
- v-model.number —>转数

~~~javascript
<!-- v-model.trim  —>去除首位空格，v-model.number —>转数字   -->
    姓名：<input v-model.trim="username" type="text"><br>
    年纪：<input v-model.number="age" type="text"><br>
~~~



## 6.3 事件修饰符

- @事件名.stop = '子的方法' —> 阻止冒泡 （点击子元素如果父元素也绑定事件， 触发在父元素）
- 以前 js 的办法 ： 给子元素  e.stopPropagation()

~~~vue
<div @click="fatherFn" class="father">
   <!-- 写在子元素 -->
      <div @click.stop="sonFn" class="son">儿子</div>
    </div>
 <script>
    const app = new Vue({
      el: '#app',
      data: {
        username: '',
        age: '',
      },
      methods: {
        fatherFn () {
          alert('老父亲被点击了')
        },
        sonFn (e) {
          // e.stopPropagation() 以前办法
          alert('儿子被点击了')
        }
      }
    })
  </script>
~~~

- @事件名.prevent  —>阻止默认行为 (prevent :**ngăn cản**)

~~~vue
<a @click.prevent href="http://www.baidu.com">阻止默认行为</a>
~~~



# 7 - 生命周期

## Vue生命周期

- 一个Vue实例从 创建 到 销毁 的整个过程。

- 生命周期四个阶段：① 创建 ② 挂载 ③ 更新 ④ 销毁
  - 销毁： 关网页 或者用 app.$destroy ： 以前紫的DOM值还在，所有事件监听跟Vue有关系没有

![image-20231109132905750](/Users/guoguo/Library/Application Support/typora-user-images/image-20231109132905750.png)

## 钩子函数

- Vue生命周期过程中，会自动运行一些函数，被称为【生命周期钩子】→ 让开发者可以在【特定阶段】运行自己的代码
- created () ：子昂意识数据已经准备好， 可以初始化发送请求
- mounted () ： 可以开始进行DOM操作

![image-20231109133027796](/Users/guoguo/Library/Application Support/typora-user-images/image-20231109133027796.png)

~~~javascript
<div id="app">
    <h3>{{ title }}</h3>
    <div>
      <button @click="count--">-</button>
      <span>{{ count }}</span>
      <button @click="count++">+</button>
    </div>
  </div>
  <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
  <script>
    const app = new Vue({
      el: '#app',
      data: {
        count: 100,
        title: '计数器'
      },
      // 1. 创建阶段（准备数据）
      beforeCreate () {
        console.log('beforeCreate 响应式数据准备好之前', this.count) //undefined
      },
      created () {
        console.log('created 响应式数据准备好之后', this.count)
        // this.数据名 = 请求回来的数据
        // 可以开始发送初始化渲染的请求了
      },

      // 2. 挂载阶段（渲染模板）
      beforeMount () {
        console.log('beforeMount 模板渲染之前', document.querySelector('h3').innerHTML) 
      }, //模版语法 {{title}}
      mounted () {
        console.log('mounted 模板渲染之后', document.querySelector('h3').innerHTML)
        // 可以开始操作dom了
      },

      // 3. 更新阶段(修改数据 → 更新视图)
      beforeUpdate () {
        console.log('beforeUpdate 数据修改了，视图还没更新', document.querySelector('span').innerHTML) //拿到更新之前，试图没更新
      },
      updated () {
        console.log('updated 数据修改了，视图已经更新', document.querySelector('span').innerHTML) //拿到更新之后，视图已更新
      },

      // 4. 卸载阶段
      beforeDestroy () {
        console.log('beforeDestroy, 卸载前')
        console.log('清除掉一些Vue以外的资源占用，定时器，延时器...')
      },
      destroyed () {
        console.log('destroyed，卸载后')
      }
    })
  </script>
</body>
</html>
~~~

### created 应用

- created 数据准备好了，可以开始发送初始化渲染请求

~~~javascript
<script>
    // 接口地址：http://hmajax.itheima.net/api/news
    // 请求方式：get
    const app = new Vue({
      el: '#app',
      data: {
        list: []
      },
      async created () {
        // 1. 发送请求获取数据
        //简写 不用写url
        const res = await 
        axios.get('http://hmajax.itheima.net/api/news')
        // 2. 更新存到 list 中，用于页面渲染 v-for
        this.list = res.data.data
      }
    })
  </script>
~~~

### mounted 应用

需求：引进页面获取焦点

- 操作DOM所以要在mounted写

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20231109134012763.png" alt="image-20231109134012763" style="zoom:50%;" />

~~~javascript

<script>
  const app = new Vue({
    el: '#app',
    data: {
      words: ''
    },
     //核心思路：
     1. Vue 要等输入框渲染出来 mounted 钩子
     2. 让input框获取焦点 inp.focus()
    mounted () {
      //获取输入框 inp = 输入框id
      //获取的没有model - 被解析了
      document.querySelector('#inp').focus()
    }
  })
</script>
~~~



# 8 - 工程化开发入门

## 8.1 开发Vue的两种方式

- 核心包传统开发模式：基于html / css / js 文件，直接引入核心包，开发 Vue。
- **工程化开发模式：基于构建工具（例如：webpack）的环境中开发Vue。**
- 工程化开发模式优点：提高编码效率，比如使用JS新语法、Less/Sass、Typescript等通过webpack都可以编译成浏览器识别的ES3/ES5/CSS等

![image-20231109135435901](/Users/guoguo/Library/Application Support/typora-user-images/image-20231109135435901.png)

## 8.2 脚手架Vue CLI

- Vue CLI 是Vue官方提供的一个**全局命令工具**
- 可以帮助我们**快速创建**一个开发Vue项目的**标准化基础架子** （就是一个目录帮忙生成一系列文件 然后继承了webpack配置）
- 好处：
  - 开箱即用，零配置
  - 内置babel等工具
  - 标准化的webpack配置
- 使用步骤：
  - 全局安装（只需安装一次即可） yarn global add @vue/cli 或者 npm i @vue/cli -g
  - 查看vue/cli版本： vue --version
  - 创建项目架子：**vue create project-name**(项目名不能使用中文)
  - 启动项目：**yarn serve** 或者 **npm run serve**(命令不固定，找package.json)
  - serve 有可能会修改 `yarn div`

## 8.3 脚手架目录文件介绍  

![image-20231109140025549](/Users/guoguo/Library/Application Support/typora-user-images/image-20231109140025549.png)

> 6.创建Vue
>
> - 安装脚手架工具 Vue CLi
>
>   - 安装 node.js （建议安装12以上的版本）， 检测 node 和 npm 的版本
>
>   <img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20231108144528625.png" alt="image-20231108144528625" style="zoom:50%;" />
>
>   - 安装 node 同时会安装 npm 工具 是node 包的管理器， 像一个应用商店， 如果需要安装某个应用， 只要知道应用的名字
>
>     - 例如： 需要安装 Vue Cli 通过 npm install 命令来安装
>
>     ![image-20231108144909442](/Users/guoguo/Library/Application Support/typora-user-images/image-20231108144909442.png)
>
> - 通过 Vue Cli 创建 Vue 项目：
>
>   - 通过 vue creat 项目名称 
>
> - 通过 vue ui 进行创建
>
> 7.目录结构
>
> - 注意：通过 cd 命令 进入到 当前项目
>
> - package.json
>
>   - sever : 静态资源服务器
>   - bulid  进行代码打包 npm run bulid ， 创建了dist 文件 npm i sever  - g 快速启动静态资源服务器， 然后 sever dist 运行 
>
> - public 保存不参与变异的资源
>
>   - favicon.ico
>
>   - index.html
>
>     ~~~html
>     <body>
>         <!-- 兼容：给不支持js的浏览器一个提示 -->
>         <noscript>
>           <strong>We're sorry but <%= htmlWebpackPlugin.options.title %> doesn't work properly without JavaScript enabled. Please enable it to continue.</strong>
>         </noscript>
>         
>         <!-- Vue所管理的容器：将来创建结构动态渲染这个容器 -->
>         <div id="app">
>           <!-- 工程化开发模式中：这里不再直接编写模板语法，通过 App.vue 提供结构渲染 -->
>         </div>
>         
>         <!-- built files will be auto injected -->
>       </body>
>     ~~~
>
>     
>
> - src 需要变异的资源：
>
>   - assets:静态资源目录，存放图片，字体等
>   - components 自定义的组件
>     - app.vue 根组件: 项目运行看到的内容就在这里编写
>     - main.js ： 入口文件， 打包或者运行第一个执行的文件
>
> - main.js Vue 应用的一个入口文件
>
> ~~~javascript
> // 文件核心作用：导入App.vue，基于App.vue创建结构渲染index.html
> // 1. 导入 Vue 核心包
> import Vue from 'vue'
> 
> // 2. 导入 App.vue 根组件
> import App from './App.vue'
> 
> // 提示：当前处于什么环境 (生产环境 / 开发环境)
> Vue.config.productionTip = false
> //控制台 true有提示在哪个环境
> 
> // 3. Vue实例化，提供render方法 → 基于App.vue创建结构渲染index.html
> new Vue({
>   // el: '#app', 作用：和$mount('选择器')作用一致，用于指定Vue所管理容器
>   //简写写法
>    render: h => h(App),
>   //完整写法 createElement = h （形参）
>   render: (createElement) => {
>     // 通过App创建元素结构，这些结构用来以后渲染index
>     return createElement(App)
>     //以前直接在index 写模版， 现在写在app 
>   }
> }).$mount('#app')
> 
> ~~~
>
> 
>
> - vue.config  有这些相关配置的定义

 ![image-20231109142439168](/Users/guoguo/Library/Application Support/typora-user-images/image-20231109142439168.png)

## 8.4 组件化开发

- 组件化开发：一个页面可以拆分成一个个组件，每个组件有着自己独立的结构、样式、行为

![image-20231109142708336](/Users/guoguo/Library/Application Support/typora-user-images/image-20231109142708336.png)

- 普通组件：每个小组件
- 根组件： 包括全部小组件的页面

![image-20231109142943919](/Users/guoguo/Library/Application Support/typora-user-images/image-20231109142943919.png)

​	

### 组件的三个部分 

- 用语法高亮插件 （vetur）

- template：结构 （有且只能一个根元素 div）

- script:   js逻辑 

- style： 样式 (可支持less，需要装包)

  - 让组件支持less： 可以写嵌套盒子， 子元素直接写在父元素里面

    - （1） style标签，lang="less" 开启less功能  （

      （2） 装包: yarn add less less-loader -D 或者npm i less less-loader -D

![image-20231109143136214](/Users/guoguo/Library/Application Support/typora-user-images/image-20231109143136214.png)

- default 导出当前的配置项

~~~javascript
<script>
// 导出的是当前组件的配置项
// 里面可以提供 data(特殊) methods computed watch 生命周期八大钩子
export default {
  created () {
    console.log('我是created')
  },
  methods: {
    fn () {
      alert('你好')
    }
  }
}
</script>
~~~



## 8.5 局部注册

- 只能在注册的组件内使用 

  1.创建.vue文件（三个组成部分）

  <img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20231109144503857.png" alt="image-20231109144503857" style="zoom:50%;" />

  2.在使用的组件内先导入再注册，最后使用

  - 组件名跟组件对象取同一个名字 (组件名规范 —> 大驼峰命名法， 如 HmHeader)

  ![image-20231109144706535](/Users/guoguo/Library/Application Support/typora-user-images/image-20231109144706535.png)

  - 当成html标签使用即可  `<组件名></组件名>`

## 8.6 全局注册

-  全局注册：所有组件内都能使用, 不要进行导入

  ① 创建 .vue 文件 (三个组成部分) 

  <img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20231109145013865.png" alt="image-20231109145013865" style="zoom:50%;" />

  ② ==main.js== 中进行全局注册

![image-20231109145102340](/Users/guoguo/Library/Application Support/typora-user-images/image-20231109145102340.png)

- 当成html标签使用即可  `<组件名></组件名>`



# 9 - scoped样式冲突

## 9.1 组件的三大组成部分 - 注意点说明

![image-20231109145712758](/Users/guoguo/Library/Application Support/typora-user-images/image-20231109145712758.png)

## 9.2 组件的样式冲突 scoped

- **默认情况**：写在组件中的样式会 全局生效 → 因此很容易造成多个组件之间的样式冲突问题

  - 例如： 给一个组件的div 写样式， 其他组件的div也会被影响
  - 全局样式: 默认组件中的样式会作用到全局

  2. 局部样式: 可以给组件加上 scoped 属性, 可以让样式只作用于当前组件

~~~vue
<template>
  <div class="base-one">BaseOne</div>
</template>

<script>
export default {};
// 添加scoped属性
</script> 
<style scoped>
/* 
  1.style中的样式 默认是作用到全局的
  2.加上scoped可以让样式变成局部样式

  组件都应该有独立的样式，推荐加scoped（原理）
  -----------------------------------------------------
  scoped原理：
  1.给当前组件模板的所有元素，都会添加上一个自定义属性
  data-v-hash值
  data-v-5f6a9d56  用于区分开不通的组件
  2.css选择器后面，被自动处理，添加上了属性选择器
  div[data-v-5f6a9d56]
*/
div {
  border: 3px solid blue;
  margin: 30px;
}
</style>
~~~

### scoped原理

1. 当前组件内标签都被添加 data-v-hash值 的属性
2. css选择器都被添加 [data-v-hash值] 的属性选择器

最终效果: 必须是当前组件的元素, 才会有这个自定义属性, 才会被这个样式作用到

![image-20231109150214825](/Users/guoguo/Library/Application Support/typora-user-images/image-20231109150214825.png)

## 9.3 data必须是一个函数

- 一个组件的 data 选项必须是一个函数。→ 保证每个组件实例，维护独立的一份数据对象。
  - 每次创建新的组件实例，都会新执行一次 data 函数，得到一个新对象

![image-20231109150407093](/Users/guoguo/Library/Application Support/typora-user-images/image-20231109150407093.png)

- 用一个组件使用三次， 每次都创建一个新的对象互不影响
  - BaseCount 组件

~~~html
<template>
  <div class="base-count">
    <button @click="count--">-</button>
    <span>{{ count }}</span>
    <button @click="count++">+</button>
  </div>
</template>

<script>
export default {
  // data() {
  //   console.log('函数执行了')
  //   return {
  //     count: 100,
  //   }
  // },
  // 写成对象 （报错 -- 必须是函数）
  data: function () {
    return {
      count: 100,
    };
  },
};
</script>

<style>
.base-count {
  margin: 20px;
}
</style>
~~~



- 使用 BaseCount 组件

![image-20231109150712644](/Users/guoguo/Library/Application Support/typora-user-images/image-20231109150712644.png)











# 10 组件通

## 10.1 组件通信语法

- 组件通信，就是指**组件与组件**之间的**数据传递**
  - 组件的数据是独立的，无法直接访问其他组件的数据。
  - 想使用其他组件的数据，就需要组件通信

## 10.2 父传子

- 父组件通过 props 将数据传递给子组件
  - 就是给组件标签身上新增一个的一个的自定义属性

![image-20231109151128220](/Users/guoguo/Library/Application Support/typora-user-images/image-20231109151128220.png)

## 10.3  子传父

- 子组件利用 $emit 通知父组件，进行修改更新
  - 不能直接修改父组件串过来的数据（ 舍得数据谁要改）
    - 子组件准备一个按钮
    - 要通过 $emit  来触发事件
    - 监听子元素串过来的数据 （ 为了收到你传过来的消息）
    - methods 提供对应处理函数拿到数据， 进行修改

![image-20231109151359676](/Users/guoguo/Library/Application Support/typora-user-images/image-20231109151359676.png)

## 10.4  prop 讲解

- Prop 定义：组件上 注册的一些 自定义属性
  - 向子组件传递数据
  - 可以 传递 任意数量 的prop
  - 可以 传递 任意类型 的prop
- 如果不要校验可以直接写成数组里面的属性
- 如果需要校验要写成对象， 然后写要校验的属性

~~~javascript
*父组件

<template>
  <div class="app">
    <!-- 1. 创建组件，动态传 要加冒号， 后面赋值 -->
    <UserInfo
      :username="username"
      :age="age"
      :isSingle="isSingle"
      :car="car"
      :hobby="hobby"
    ></UserInfo>
  </div>
</template>

<script>
// 还是需要路由导入
import UserInfo from './components/UserInfo.vue'
export default {
  data() {
    return {
      username: '小帅',
      age: 28,
      isSingle: true,
      car: {
        brand: '宝马',
      },
      hobby: ['篮球', '足球', '羽毛球'],
    }
  },
  components: {
    UserInfo,
  },
}
</script>


* 子组件：

<template>
<!-- 3.渲染到页面 -->
  <div class="userinfo">
    <h3>我是个人信息组件</h3>
    <div>姓名：{{username}}</div>
    <div>年龄：{{age}}</div>
    <div>是否单身：{{isSingle}}</div>
    <!-- 数对象里面的属性 car.brand -->
    <div>座驾：{{car.brand}}</div>
    <!-- join('、') 用逗号隔开每个值 -->
    <div>兴趣爱好：{{hobby.join('、')}}</div>
  </div>
</template>

<script>
// 2. 注册自定义属性 props
export default {
//写成数组
  props:['username','age','isSingle','car','hobby']
}
~~~



### props 校验

- 为组件的 prop 指定验证要求，不符合要求，控制台就会有错误提示 → 帮助开发者，快速发现错误

- **语法：**

  ① 类型校验

  ② 非空校验

  ③ 默认值

  ④ 自定义校验

![image-20231109152130852](/Users/guoguo/Library/Application Support/typora-user-images/image-20231109152130852.png)

~~~javascript
*父组件
<div class="app">
    <BaseProgress :w="width"></BaseProgress>

*子组件
<script>
export default {
// props写转换成对象， 要求： 对类型给要求 Number
  props: {
    w: Number, //string,array,object...
  },
}
</script>
~~~



### props 完整写法

~~~javascript
<script>
export default {
  // 完整写法（类型、默认值、非空、自定义校验）
  props: {
    w: {
      type: Number,
      //required: true,
      default: 0,
      //validator(val) 里面的形参val 可以拿到 props接受的值， 然后给值写条件
      validator(val) {
        // console.log(val)
        if (val >= 100 || val <= 0) {
          //为了报错时显示报错内容
          console.error('传入的范围必须是0-100之间')
          return false
          //传过去的数据显示在范围内 -- 正常
        } else {
          return true
        }
      },
    },
  },
}
</script>
~~~

### props&data、单向数据流

- **共同点：都可以给组件提供数据**

- 区别：

  -  data 的数据是自己的 → 随便改

  - prop 的数据是外部的 → 不能直接改，要遵循 单向数据流

- 单向数据流：父级 prop 的数据更新，会向下流动，影响子组件。这个数据流动是单向的

![image-20231109153205163](/Users/guoguo/Library/Application Support/typora-user-images/image-20231109153205163.png)

## 10.5  非父子通信 (拓展) - event bus 事件总线

![image-20231109153435455](/Users/guoguo/Library/Application Support/typora-user-images/image-20231109153435455.png)

### 非父子通信 (拓展) - provide & inject

![image-20231109153512283](/Users/guoguo/Library/Application Support/typora-user-images/image-20231109153512283.png)





# 11 v-model原理  

- v-model本质上是一个语法糖。例如应用在输入框上，就是 value属性 和 input事件 的合写

- **作用：**提供数据的双向绑定

  ① 数据变，视图跟着变 :value 

  ② 视图变，数据跟着变 @input （监听当前输入框的输入， 一旦用户输入内容就触发当前事件）

  - $event.target.value ： 拿输入框的值，获取事件的形参，并且更新到msg当中
  - 对input， click 形参就是 e.target.valu额 （但是模版当中要用$event）

![image-20231109155147157](/Users/guoguo/Library/Application Support/typora-user-images/image-20231109155147157.png)

## 11.1 表单组件封装 

- 表单类组件 封装 → 实现 子组件 和 父组件数据 的双向绑定

  ① 父传子：数据 应该是父组件 props 传递 过来的，拆解 v-model 绑定数据

  - 不能直接写 v-model 因为子组件不能直接修改数据

  ② 子传父：监听输入，子传父传值给父组件修改

  - 子组件修改后要传给父组件

  ![image-20231109160430695](/Users/guoguo/Library/Application Support/typora-user-images/image-20231109160430695.png)

![image-20231109162528734](/Users/guoguo/Library/Application Support/typora-user-images/image-20231109162528734.png)

## 11.2 父组件 v-model 简化代码

2. 父组件 v-model 简化代码，实现 子组件 和 父组件数据 双向绑定
   - 子组件不能用v-model 但是可以给父组件绑定 v-model

① 子组件中：props 通过 value 接收，事件触发 input 

② 父组件中：v-model 给组件直接绑数据 ( :value + @input )

![image-20231109163201867](/Users/guoguo/Library/Application Support/typora-user-images/image-20231109163201867.png)

# 12 - sync修饰符

- **作用：**可以实现 子组件 与 父组件数据 的 双向绑定，简化代码

- **特点：**prop属性名，可以自定义，用 v-model 固定为 value 

- **场景：**封装弹框类的基础组件， visible属性 true显示 false隐藏

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20231109163450579.png" alt="image-20231109163450579" style="zoom:33%;" />

- **本质：**就是 :属性名 和 @update:属性名 合写 

![image-20231109163731633](/Users/guoguo/Library/Application Support/typora-user-images/image-20231109163731633.png)

![image-20231109163952971](/Users/guoguo/Library/Application Support/typora-user-images/image-20231109163952971.png) 

# 13 - ref 和 $refs 

- **作用：**利用 ref 和 $refs 可以用于 获取 dom 元素, 或 组件实例
- 特点：**查找范围 → 当前组件内 (更精确稳定) 

## 13.1 **获取 dom**

![image-20231109164235794](/Users/guoguo/Library/Application Support/typora-user-images/image-20231109164235794.png)

- 如果用 querySelector 查找范围 → 整个页面 （只要同名的盒子会应用）
  1. 目标标签 – 添加 ref 属性
  2. 恰当时机 要等到渲染完成写在 mounted 里面, 通过 this.$refs.xxx, 获取目标标签

![image-20231109164424776](/Users/guoguo/Library/Application Support/typora-user-images/image-20231109164424776.png)

![image-20231109164711287](/Users/guoguo/Library/Application Support/typora-user-images/image-20231109164711287.png)

## 13.2 获取组件

![image-20231109164907740](/Users/guoguo/Library/Application Support/typora-user-images/image-20231109164907740.png)

![image-20231109165146272](/Users/guoguo/Library/Application Support/typora-user-images/image-20231109165146272.png)

![image-20231109165619294](/Users/guoguo/Library/Application Support/typora-user-images/image-20231109165619294.png)

# 14 - $nextTick

- 需求：点击编辑，1 隐藏，然后显示 2  并且 自动获取焦点
  - 点击编辑，显示编辑框 （v-if）
  - 让编辑框，立刻获取焦点

![image-20231109171913523](/Users/guoguo/Library/Application Support/typora-user-images/image-20231109171913523.png)

![image-20231109172134777](/Users/guoguo/Library/Application Support/typora-user-images/image-20231109172134777.png)

问题：输入框显示之后立马获取焦点是不能成功 （因为vue 是异步更新，还没渲染完页面）

- $nextTick：等 DOM 更新后, 才会触发执行此方法里的函数体
  - ** 语法:** this.$nextTick(函数体) 

![image-20231109172740141](/Users/guoguo/Library/Application Support/typora-user-images/image-20231109172740141.png)

~~~javascript
<template>
  <div class="app">
    <div v-if="isShowEdit">
       <!-- 3.ref="inp"获取 -->
      <input type="text" v-model="editValue" ref="inp" />
      <button>确认</button>
    </div>
    <div v-else>
      <span>{{ title }}</span>
       <!-- 1.绑定点击事件 -->
      <button @click="editFn">编辑</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      title: '大标题',
      isShowEdit: false,
      editValue: '',
    }
  },
  methods: {
    editFn() {
        // 2.显示输入框 （异步dom更新）
        this.isShowEdit = true  
        // 4. 获取焦点 （ 获取不了）， 解决添加 this.$nextTick
      //等DOM更新完立刻执行代码
      this.$nextTick((){
        this.$refs.inp.focus()
         })       
    }  },
}
</script> 
~~~



# 15 - 自定义指令

- 自定义指令：自己定义的指令, 可以封装一些 dom 操作， 扩展额外功能
- 为什么要使用自定义指令： 当页面加载是，让元素获取焦点 （如果需要其他页面也一样就封装成一个指令然后复用）

![image-20231109204439220](/Users/guoguo/Library/Application Support/typora-user-images/image-20231109204439220.png)

- 语法：
  - 全局注册 - 语法
    - "inserted"：是一个钩子，指的是所绑定的元素什么时候添加到页面就自动调用（跟mounted 相同等DOM渲染完）
    - el ： 形参可以拿到指令所绑定的元素 （例如： 绑定输入框的指令， 可以拿到输入框的值） 
    - el.focus() = this.$refs.inp.focus()

![image-20231109204733445](/Users/guoguo/Library/Application Support/typora-user-images/image-20231109204733445.png)

- 局部注册 – 语法
  - 去掉Vue

![image-20231109205340630](/Users/guoguo/Library/Application Support/typora-user-images/image-20231109205340630.png)

- 页面使用：

![image-20231109205449296](/Users/guoguo/Library/Application Support/typora-user-images/image-20231109205449296.png)

- 全局注册

~~~javascript
import Vue from 'vue'
import App from './App.vue'

Vue.config.productionTip = false
// 第二个方法： 注册自定义值指令
// // 1. 全局注册指令
 Vue.directive('focus', {
//   // inserted 会在 指令所在的元素，被插入到页面中时触发
   inserted (el) {
      el 就是指令所绑定的元素
      console.log(el); // <input" type="text" />
// 2.3 获取焦点
     el.focus()
   }
 })
 -》可以已多次使用， 需要再哪个页面复用 直接 v-focus

new Vue({
  render: h => h(App),
}).$mount('#app')
~~~



- 局部注册

~~~javascript
<template>
  <div>
    <h1>自定义指令</h1>
    <!-- 1. 添加 ref="inp 方便找到-->
    <!-- 2.2 使用 v-focus 指令名-->
    <input v-focus ref="inp" type="text" />
  </div>
</template>

<script>
export default {
  // 2. 可以用这个方法
   mounted () {
     //找到输入框然后获取焦点
     this.$refs.inp.focus()
   }

  // 3. 局部注册指令 - 只能在这个组件用
  directives: {
    // 指令名：指令的配置项
    focus: {
      inserted(el) {
        el.focus();
      },
    },
  },
};
</script>
~~~



###  自定义指令 - 指令的值

- 指令还可以传值
- 需求：实现一个 color 指令 - 传入不同的颜色, 给标签设置文字颜色 (一进页面两个同一个指令 传两个不同颜色的值)
- 语法：
  - 在绑定指令时，可以通过“等号”的形式为指令 绑定 具体的参数值 
  - 通过 binding.value 可以拿到指令值，指令值修改会 触发 update 函数
  - inserted ： dom 加载完毕出发意思就是一进页面出发，update 指令的值修改的时候触发

![image-20231109211952104](/Users/guoguo/Library/Application Support/typora-user-images/image-20231109211952104.png)

 ~~~javascript
 <template>
   <div>
     <!-- 2.使用写上指令， 两个传不同的值 -->
     <h1 v-color="color1">指令的值1测试</h1>
     <h1 v-color="color2">指令的值2测试</h1>
   </div>
 </template>
 
 <script>
 export default {
   data() {
     return {
       // 3. 给实参
       color1: "red",
       color2: "orange",
     };
   },
   // 1. 局部指令
   directives: {
     color: {
       // 1. inserted 提供的是元素被添加到页面中时的逻辑 （已进入页面就触发）
       inserted(el, binding) {
         console.log(el, binding.value); // <h1 style="color: red">指令的值1测试</h1> "red"
         // binding.value 就是指令的值， 不能直接给值
         el.style.color = binding.value;
       },
       // 除了初始化（一打开页面）还有数据变化的时候也要变颜色
       // 2. update 指令的值修改的时候触发，提供值变化后，dom更新的逻辑
       update(el, binding) {
         console.log("指令的值修改了");
         el.style.color = binding.value;
       },
     },
   },
 };
 </script>
 
 <style>
 </style>
 ~~~

### ？自定义指令 - v-loading 指令封装 

-  场景：实际开发过程中，发送请求需要时间，在请求的数据未回来时，页面会处于空白状态 => 用户体验不好

# 16 - 插槽

## 16.1 插槽 - 默认插槽

- 使用场景： 结构一样， 内容部分不希望写死， 能使用的时候可以自定义

![image-20231109213834510](/Users/guoguo/Library/Application Support/typora-user-images/image-20231109213834510.png)

- 作用：让组件内部的一些 结构 支持 自定义 
- 插槽基本语法： 
  1. 组件内需要定制的结构部分，改用<slot></slot>占位 
  2. 使用组件时, <MyDialog></MyDialog>标签内部, 传入结构替换slot

![image-20231109214127040](/Users/guoguo/Library/Application Support/typora-user-images/image-20231109214127040.png)

![image-20231109214510603](/Users/guoguo/Library/Application Support/typora-user-images/image-20231109214510603.png)

## 16.2 插槽 - 后备内容（没有写内容给默认值 ）

- 通过插槽完成了内容的定制，传什么显示什么, 但是如果不传，则是空白 =》 能否给插槽设置 默认显示内容 呢？
- 插槽后备内容：封装组件时，可以为预留的 `<slot>` 插槽提供后备内容（默认内容）
- 语法: 在 <slot> 标签内，放置内容, 作为默认显示内容

![image-20231109214934050](/Users/guoguo/Library/Application Support/typora-user-images/image-20231109214934050.png)

## 16.3 插槽 - 具名插槽 （多个部分需要修改）

- 默认插槽只能一个的定制位置 =》 给多个插槽起名字

![image-20231109215132624](/Users/guoguo/Library/Application Support/typora-user-images/image-20231109215132624.png)

- 需求：一个组件内有多处结构，需要外部传入标签，进行定制 

- 具名插槽语法:

  - 子组件：使用name属性起名字
  - 父组件 ： 用 ==template标签== 配合 ==v-slot:名字== 来分发对应标签 
  - 如果是一个button ， 要写在父组件然后用temple抱起来

  ![image-20231109215616500](/Users/guoguo/Library/Application Support/typora-user-images/image-20231109215616500.png)

  - v-slot:插槽名 可以简化成 #插槽名

![image-20231109215854738](/Users/guoguo/Library/Application Support/typora-user-images/image-20231109215854738.png)

## 插槽 - 作用域插槽 （插槽传值）

- 作用域 ： 是插槽的一个传参语法

- 插槽分类： 默认插槽 和具名插槽

- 作用域插槽: 定义 slot 插槽的同时, 是可以传值的。给 插槽 上可以 绑定数据，将来 使用组件时可以用

- 场景：封装表格组件 

  1. 父传子，动态渲染表格内容 

  2. 利用默认插槽，定制操作列 

  3. 删除或查看都需要用到 当前项的 id，属于 组件内部的数据 通过 作用域插槽 传值绑定，进而使用
     - 需求： 
       - 点击删除按钮回去对应id 然后 写方法删除
       - 点击查看按钮 获取对应 row 数据 然后显示出来对应信息

  <img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20231109224701044.png" alt="image-20231109224701044" style="zoom:50%;" />

- ![image-20231109224317608](/Users/guoguo/Library/Application Support/typora-user-images/image-20231109224317608.png)

# 17 - VueRouter 路由入门

- Vue中路由：路径 和 组件 的 映射 关系

![image-20231109224825838](/Users/guoguo/Library/Application Support/typora-user-images/image-20231109224825838.png)

- VueRouter 的 介绍 

  目标：认识插件 VueRouter，掌握 VueRouter 的基本使用步骤 

  作用：修改地址栏路径时，切换显示匹配的组件 

  说明：Vue 官方的一个路由插件，是一个第三方包 

  官网：https://v3.router.vuejs.org/zh/

## 17.1 VueRouter 的 使用 (5 + 2)

- 单页应用程序: SPA - Single Page Application
  - 单页面应用(SPA): 所有功能在 一个html页面 上实现

![image-20231110062151759](/Users/guoguo/Library/Application Support/typora-user-images/image-20231110062151759.png)

- 多页面

![image-20231110062238221](/Users/guoguo/Library/Application Support/typora-user-images/image-20231110062238221.png)

- 5个基础步骤 (固定) 

![image-20231110063239755](/Users/guoguo/Library/Application Support/typora-user-images/image-20231110063239755.png)

- 2 个核心步骤：

  - ① 创建需要的组件 (views目录)，配置路由规则

  - ② 配置导航，配置路由出口(路径匹配的组件显示的位 

    置)

![image-20231110064433253](/Users/guoguo/Library/Application Support/typora-user-images/image-20231110064433253.png)

- src/views文件夹 

  - **页面组件** - 页面展示 - 配合路由用

-  src/components文件夹

  - **复用组件** - 展示数据 - 常用于复用

  ![image-20231110064901190](/Users/guoguo/Library/Application Support/typora-user-images/image-20231110064901190.png)

  

## 17.2 组件存放目录问题 

- 注意：.vue文件 本质无区别	
- 路由相关的组件，为什么放在 views 目录呢？ 组件分类
- 组件分类： .vue文件分2类； 页面组件 & 复用组件

# 18 - VueRouter 路由进阶

## 18.1 路由的封装抽离

- 全部路由相关的 放在 src/router/index.js

![image-20231110070937470](/Users/guoguo/Library/Application Support/typora-user-images/image-20231110070937470.png)

## 18.2 router-link （点击文案 - 跟a标签相似）

需求：实现导航高亮效果

- vue-router 提供了一个全局组件 router-link (取代 a 标签)

  ① 能跳转，配置 to 属性指定路径(必须) 。本质还是 a 标签 ，to 无需 # 

  ② 能高亮，默认就会提供高亮类名，可以直接设置高亮样式 

![image-20231110071543445](/Users/guoguo/Library/Application Support/typora-user-images/image-20231110071543445.png)

### router-link  2各个类名

- 说明：我们发现 router-link 自动给当前导航添加了 两个高亮类名

![image-20231110071741100](/Users/guoguo/Library/Application Support/typora-user-images/image-20231110071741100.png)

① router-link-active 模糊匹配 (用的多) 

to="/my" 可以匹配 /my /my/a /my/b ....

- 使用场景  只要 discover

![image-20231110072111948](/Users/guoguo/Library/Application Support/typora-user-images/image-20231110072111948.png)

② router-link-exact-active 精确匹配 

to="/my" 仅可以匹配 /my

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20231110072002802.png" alt="image-20231110072002802" style="zoom:33%;" />

### router-link - 定义类名

- 说明：router-link 的 两个高亮类名 太长了，我们希望能定制怎么办？ 

![image-20231110072509504](/Users/guoguo/Library/Application Support/typora-user-images/image-20231110072509504.png)

### router-link - 跳转传参

1. 查询参数传参 
2. 动态路由传参

#### 查询参数传参 

- 需求

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20231110100429860.png" alt="image-20231110100429860" style="zoom: 50%;" />

① 语法格式如下 

lto="/path?参数名=值" 

② 对应页面组件接收传递过来的值 

$route.query.参数名

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20231110093559520.png" alt="image-20231110093559520" style="zoom:50%;" />



#### 动态路由传参 （路由写动态）

需求：



① 配置动态路由

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20231110100501672.png" alt="image-20231110100501672" style="zoom:50%;" />

② 配置导航链接 

l to="/path/参数值" 

③ 对应页面组件接收传递过来的值 

l $route.params.参数名

#### 两种传参方式的区别

1. 查询参数传参 (比较适合传多个参数，每个组要穿的值都要有参数名) 

① 跳转：to="/path?参数名=值&参数名2=值" 

② 获取：$route.query.参数名 

2. 动态路由传参 (优雅简洁，传单个参数比较方便， 用共同的传参名， 所以只能传一个参数) 

① 配置动态路由：path: "/path/参数名" 

② 跳转：to="/path/参数值" 

③ 获取：$route.params.参数名

![image-20231110101206790](/Users/guoguo/Library/Application Support/typora-user-images/image-20231110101206790.png)

#### 动态路由参数可选符

问题： 点击搜索 显示空白（是因为都是search 路由但是没有传值）

原因：/search/:words 表示，必须要传参数

解决：配了路由 path: "/search/:words"

![image-20231110100939838](/Users/guoguo/Library/Application Support/typora-user-images/image-20231110100939838.png)

![image-20231110100846771](/Users/guoguo/Library/Application Support/typora-user-images/image-20231110100846771.png)

## 18.3 Vue路由 - 重定向 

#### 需求：一打开首页， 自动跳转到home

**问题：**网页打开， url 默认是 / 路径，未匹配到组件时，会出现空白

**说明：**重定向 → 匹配path后, 强制跳转path路径

**语法：** { path: 匹配路径, redirect: 重定向到的路径 }

![image-20231110101511605](/Users/guoguo/Library/Application Support/typora-user-images/image-20231110101511605.png)

- 代码演示

![image-20231110101548256](/Users/guoguo/Library/Application Support/typora-user-images/image-20231110101548256.png)

## 18.4  Vue路由 - 404 

**作用：**当路径找不到匹配时，给个提示页面 

**位置：**配在路由最后 

**语法：**path: "*" (任意路径) – 前面不匹配就命中最后这个

![image-20231110101633221](/Users/guoguo/Library/Application Support/typora-user-images/image-20231110101633221.png)

## 18.5 Vue路由 - 模式设置

问题: 路由的路径看起来不自然, 有#，能否切成真正路径形式? 

-  hash路由(默认) 例如: http://localhost:8080/#/home  （有#）

-  history路由(常用) 例如: http://localhost:8080/home (以后上线需要服务器端支持)

![image-20231110102004565](/Users/guoguo/Library/Application Support/typora-user-images/image-20231110102004565.png)

## 18.6 编程式导航

### 编程式导航 - 基本跳转

问题：点击按钮跳转如何实现？ 

编程式导航：用JS代码来进行跳转 

两种语法: 

① path 路径跳转 

② name 命名路由跳转



#### path 路径跳转 (简易方便)

- $router 就是路由实例对象

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20231110171741261.png" alt="image-20231110171741261" style="zoom:50%;" />

#### name 命名路由跳转 (适合 path 路径长的场景)

- 先去路由规则加上name
- 然后用name 去跳转 （适合路径长的时候直接用name）

![image-20231110172239494](/Users/guoguo/Library/Application Support/typora-user-images/image-20231110172239494.png)

![image-20231110172640919](/Users/guoguo/Library/Application Support/typora-user-images/image-20231110172640919.png)

### 编程式导航 - 路由传参

问题：点击搜索按钮，跳转需要传参如何实现？ 

两种传参方式：查询参数（？） + 动态路由传参  （：动态）

两种跳转方式，对于两种传参方式都支持： 

① path 路径跳转传参 

② name 命名路由跳转传参

- 可以用2种传参方式
  - 查询参数（？） -  获取值：$route.query.参数名
  - 动态路由传参  （：动态） - 获取值：$route.params.参数名

![image-20231110202313616](/Users/guoguo/Library/Application Support/typora-user-images/image-20231110202313616.png)



![image-20231110200909241](/Users/guoguo/Library/Application Support/typora-user-images/image-20231110200909241.png)

### 组件缓存 keep-alive （缓存组件）

问题：从面经 点到 详情页，又点返回，数据重新加载了 → 希望回到原来的位置 

![image-20231110202522289](/Users/guoguo/Library/Application Support/typora-user-images/image-20231110202522289.png)

原因：路由跳转后，组件被销毁了，返回回来组件又被重建了，所以数据重新被加载了 

需求： 把原来已经下载的数据缓存下来， 需要用的时候直接显示， 不要销毁重新下载

解决：利用 keep-alive 将组件缓存下来

#### **1. keep-alive是什么** 

keep-alive 是 Vue 的内置组件，当它包裹动态组件时，会缓存不活动的组件实例，而不是销毁它们。 

keep-alive 是一个抽象组件：它自身不会渲染成一个 DOM 元素，也不会出现在父组件链中。 

#### **2. keep-alive的优点** 

在组件切换过程中 把切换出去的组件保留在内存中，防止重复渲染DOM， 

减少加载时间及性能消耗，提高用户体验性。

问题： 如果直接用 keep-alive 包裹 router- view 会保存所有页面切换到额组件， 所以希望可以指定哪些被缓存， 用keep-alive 的属性

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20231110203632370.png" alt="image-20231110203632370" style="zoom:50%;" />

#### **3.** **keep-alive的****三个属性** 

① include ： 组件名数组，只有匹配的组件会被缓存 

② exclude ： 组件名数组，任何匹配的组件都不会被缓存 

③ max ： 最多可以缓存多少组件实例

~~~javascript
// 组件名如果没有配name才会使用文件名
export default {
  name: "LayoutPage",
~~~



<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20231110203731889.png" alt="image-20231110203731889" style="zoom:50%;" />

#### **4. keep-alive的使用会触发两个生命周期函数** （了解）

- 组件被缓存的时候created mounted destroyed 不会执行， 所以提供两个， 这样用户离开还是进入页面都被监听到

  - activated 当组件被激活（使用）的时候触发，组件被看到时 → 进入这个页面的时候触发 

  - deactivated 当组件不被使用的时候触发 → 离开这个页面的时候触发，组件看不见

- 所以其提供了actived 和 deactived钩子，帮我们实现业务需求。

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20231110204440479.png" alt="image-20231110204440479" style="zoom:50%;" />

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20231112101044230.png" alt="image-20231112101044230" style="zoom:50%;" />

![image-20231112101912411](/Users/guoguo/Library/Application Support/typora-user-images/image-20231112101912411.png)



#### 5.检讨路由 children

#### 6.导航守卫

- 在路由切换的过程中给 它设置对应的导航守卫， 对这样的行为做限制
  - 全局前置守卫（index.js) beforeEach
    - to切换到的路由
    - from 是要离开的路由
    - next 是否允许进入目标路由 

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20231112092959958.png" alt="image-20231112092959958" style="zoom:50%;" />

- 路由 beforeEnter

 ![image-20231112093154438](/Users/guoguo/Library/Application Support/typora-user-images/image-20231112093154438.png)

- 组件：beforeRouterEnter

![image-20231112093314556](/Users/guoguo/Library/Application Support/typora-user-images/image-20231112093314556.png)

- 点击按钮离开当前页面： beforeRouterLeave
  - this.isConfi 为true 直接离开，false 就给弹窗确认是否离开 然后离开

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20231112093624481.png" alt="image-20231112093624481" style="zoom:50%;" />

7.eventBus 事件总线 （非父子关系）

- 进行信息的发布于订阅（创建一个都能访问到的事件总线）
  - this.$eventBus 都可以访问到因为通过原型找到
  - A组件监听eventBus实现， B出发

![image-20231112102246444](/Users/guoguo/Library/Application Support/typora-user-images/image-20231112102246444.png)

8. `$children ` `$parent`

- children ： 父组件中， 返回一个组件集合的数组， 如果秦楚组件的顺序可以直接用下标 （如果变了会有bug）
- parent ：子组件， `this.$parent` 指向父组件让父组件去改 this.$parent. 属性 = 200  `this.$parent. fn()`

 19 - Vuex



















==11.组件插槽==

- 例子： 在 app

12. Vue Router

- Vue 路由管理： ulr 修改 点击跳转其他页面

- router 文件

  - mode： 'history'  没有写就默认 hash

  ![image-20231108154505763](/Users/guoguo/Library/Application Support/typora-user-images/image-20231108154505763.png)

- hash 路由会加上 # 就一单页应用： 兼容性好
- history ： 看的更好看， 兼容性差

![image-20231108154834671](/Users/guoguo/Library/Application Support/typora-user-images/image-20231108154834671.png)

- router：
  - path： 路径
  - name：代替路由
  - components：当前路由切换以后要显示那个组件 （需要import 导入这个组件）
- Vue router的两个组件：
  - router - link： 除了使用to 写路径， 还可以用name
  - router  - view
- 动态路由：
- 嵌套路由：
- 编程导航处理方式：做哪些主动跳转登录以后才能看视频， 过几秒会自动跳转到登录页面
- 在跳转过程中进行数据专递
- route（ 看路由相关的数据： 地址， 参数） 跟router （路由操作的工具， 一些动作） 的区别
- router. beforeEach

13.VueX

- 统一的数据存储方式