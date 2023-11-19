# DAY01

-  Vue 快速上手 

Vue 概念 / 创建实例 / 插值表达式 / 响应式特性 / 开发者工具 

-  Vue 指令 

v-html / v-show / v-if / v-else / v-on / v-bind / v-for / v-model 

-  综合案例 - 小黑记事本 

列表渲染 / 删除功能 / 添加功能 / 底部统计 / 清空

## 一、为什么要学习Vue

## 1 - VUE概念

- Vue 是一个用于 构建用户界面 的 渐进式 框架 （Vue2官网：<https://v2.cn.vuejs.org/>）

  - 建用户界面：基于数据渲染出用户看到的页面

  - 渐进式：循环渐进 （只要先学主要）

    - Vue 的两种使用方式： 

      ① Vue 核心包开发 

      场景：局部 模块改造 

      ② Vue 核心包 & Vue 插件 工程化开发 

      场景：整站 开发 

  - 框架： 一套完整的项目解决方案
    - 优点：大大提升开发效率 (70%↑) 
    - 缺点：需要理解记忆规则 → 官网
    - 举个栗子：

- 如果把一个完整的项目比喻为一个装修好的房子，那么框架就是一个毛坯房。


- 我们只需要在“毛坯房”的基础上，增加功能代码即可。


- 提到框架，不得不提一下库。


- 库，类似工具箱，是一堆方法的集合，比如 axios、lodash、echarts等

- 框架，是一套完整的解决方案，实现了大部分功能，我们只需要按照一定的规则去编码即可。

- 框架的特点：有一套必须让开发者遵守的**规则**或者**约束**


- 咱们学框架就是学习的这些规则 [官网](https://v2.cn.vuejs.org/)


## 2 - 创建Vue实例

- 我们已经知道了Vue框架可以 基于数据帮助我们渲染出用户界面，那应该怎么做呢？


- 比如就上面这个数据，基于提供好的msg 怎么渲染后右侧可展示的数据呢？

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230918065559370.png" alt="image-20230918065559370" style="zoom:50%;" />

- **核心步骤（4步）：**

1. 准备容器 （准备div）
2. 引包（官网） — 开发版本（建议用这个）/生产版本（https://v2.cn.vuejs.org/v2/guide/installation.html#%E7%9B%B4%E6%8E%A5%E7%94%A8-lt-script-gt-%E5%BC%95%E5%85%A5  引用链接，
3. 创建Vue实例  new Vue()
4. 指定配置项，渲染数据 （用逗号隔开）
   1. el:指定挂载点 (处理那个标签)
   2. data提供数据

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230918065635332.png" alt="image-20230918065635332" style="zoom: 33%;" />**总结：创建Vue实例需要执行哪4步**

## 3 - 插值表达式 {{}}

- 插值表达式是一种Vue的模板语法


- 我们可以用插值表达式渲染出Vue提供的数据


### 1.作用：

- 利用表达式进行插值，渲染到页面中

- 表达式：是可以被求值的代码，JS引擎会讲其计算出一个结果


- 以下的情况都是表达式：


```js
money + 100
money - 100
money * 10
money / 10 
price >= 100 ? '真贵':'还行'
obj.name
arr[0]
fn()
obj.fn()
```

### 2. 语法

- 插值表达式语法：`{{ 表达式 }}`


```js
<h3>{{title}}<h3>

<p>{{nickName.toUpperCase()}}</p>

<p>{{age >= 18 ? '成年':'未成年'}}</p>

<p>{{obj.name}}</p>

<p>{{fn()}}</p>
```

### 3.错误用法

```js
1.在插值表达式中使用的数据 必须在data中进行了提供
<p>{{hobby}}</p>  //如果在data中不存在 则会报错

2.支持的是表达式，而非语句，比如：if   for ...
<p>{{if}}</p>

3.不能在标签属性中使用 {{  }} 插值 (插值表达式只能标签中间使用)
<p title="{{username}}">我是P标签</p>
```

~~~html
<body>
  <!-- 
  插值表达式：Vue的一种模板语法
  作用：利用 表达式 进行插值渲染
  语法：{{ 表达式 }}

  注意点：
    1. 使用的数据要存在
    2. 支持的是表达式，不是语句  if  for
    3. 不能在标签属性中使用 {{ }}
 -->
  <div id="app">
    <!-- 要求到结果 -->
    <p>{{ nickname }}</p>
    <!-- toUpperCase()： 全部大写 -->
    <p>{{ nickname.toUpperCase() }}</p>
    <p>{{ nickname + '你好' }}</p>
     <!-- if 函数 -->
    <p>{{ age >= 18 ? '成年' : '未成年' }}</p>
    <!-- 获取对象的值 -->
    <p>{{ friend.name }}</p>
    <p>{{ friend.desc }}</p>

    <!-- ----------------------- -->
    <!-- <p>{{ hobby }}</p> -->
    <!-- <p>{{ if }}</p> -->
    <!-- <p title="{{ nickname }}">我是p标签</p> -->
  </div>

  <script src="https://cdn.jsdelivr.net/npm/vue@2.7.14/dist/vue.js"></script>
  <script>

    const app = new Vue({
      el: '#app',
      data: {
        nickname: 'tony',
        age: 18,
        friend: {
          name: 'jepson',
          desc: '热爱学习 Vue'
        }
      }
    })


  </script>

</body>

~~~

### * 总结

1.插值表达式的作用是什么

2.语法是什么

3.插值表达式的注意事项

## 4 - 响应式特性

### 1. 什么是响应式？

- 简单理解就是==数据变，视图对应变==


### 2.响应式演示

- data中的数据, 最终会被添加到实例上


① 访问数据： "实例.属性名"

② 修改数据： "实例.属性名"= "值"

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230918073017981.png" alt="image-20230918073017981" style="zoom: 25%;" />

## 二、Vue开发者工具安装

1. 通过谷歌应用商店安装（国外网站）
2. 极简插件下载（推荐） <https://chrome.zzzmh.cn/index>

- 安装步骤


<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230918073409601.png" alt="image-20230918073409601" style="zoom:50%;" />

- 安装之后可以F12后看到多一个Vue的调试面板


<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230918073436222.png" alt="image-20230918073436222" style="zoom:50%;" />

## 三、Vue中的常用指令

- **概念：**指令（Directives）是 Vue 提供的带有 **`v-前缀`** 的 特殊 标签**属性**， 可以解析标签 (写在属性标签里面)
- 作用：设置元素的 innerHTML
- 语法：`v-html = "表达式 "`

~~~html
<body>
  <div id="app">
    <!-- 可以解析标签 -->
    <div v-html="msg"></div>
  </div>
  
  <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
  <script>

    const app = new Vue({
      el: '#app',
      data: {
        msg: `
          <h3>学前端~来黑马！</h3>
        `
      }
    })

  </script>
</body>
~~~

为啥要学：提高程序员操作 DOM 的效率。

- vue 中的指令按照不同的用途可以分为如下 6 大类：

  -  内容渲染指令（v-html、v-text）

  -  条件渲染指令（v-show、v-if、v-else、v-else-if）

  -  事件绑定指令（v-on）

  -  属性绑定指令 （v-bind）

  -  双向绑定指令（v-model）

  -  列表渲染指令（v-for）


指令是 vue 开发中最基础、最常用、最简单的知识点

## 四、内容渲染指令（v-html、v-text）

内容渲染指令用来辅助开发者渲染 DOM 元素的文本内容。常用的内容渲染指令有如下2 个：

- v-text（类似innerText）


- - 使用语法：`<p v-text="uname">hello</p>`，意思是将 uame 值渲染到 p 标签中
  - 类似 innerText，使用该语法，会覆盖 p 标签原有内容


- v-html（类似 innerHTML）


- - 使用语法：`<p v-html="intro">hello</p>`，意思是将 intro 值渲染到 p 标签中
  - 类似 innerHTML，使用该语法，会覆盖 p 标签原有内容
  - 类似 innerHTML，使用该语法，能够将HTML标签的样式呈现出来。

代码演示：

```js
 
  <div id="app">
    <h2>个人信息</h2>
	// 既然指令是vue提供的特殊的html属性，所以咱们写的时候就当成属性来用即可
    <p v-text="uname">姓名：</p> 
    <p v-html="intro">简介：</p>
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
```



## 五、条件渲染指令(v-show、v-if、v-else、v-else-if）

- 条件判断指令，用来辅助开发者按需控制 DOM 的显示与隐藏。条件渲染指令有如下两个，分别是：


### 1. v-show

1. 作用：  控制元素显示隐藏
2. 语法：  v-show = "表达式"   表达式值为 true 显示， false 隐藏
3. 原理：  切换 display:none 控制显示隐藏
4. 场景：频繁切换显示隐藏的场景

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230918094537597.png" alt="image-20230918094537597" style="zoom:50%;" />

### 2. v-if

1. 作用：  控制元素显示隐藏（条件渲染）
2. 语法：  v-if= "表达式"          表达式值 true显示， false 隐藏
3. 原理：  基于条件判断，是否创建 或 移除元素节点
4. 场景：  要么显示，要么隐藏，不频繁切换的场景 （  v-show ： 会一直显示标签）

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230918094700322.png" alt="image-20230918094700322" style="zoom:50%;" />

示例代码：

```html
 <body>

  <!-- 
    v-show底层原理：切换 css 的 display: none 来控制显示隐藏
    v-if  底层原理：根据 判断条件 控制元素的 创建 和 移除（条件渲染）
  -->

  <div id="app">
    <div v-show="flag" class="box">我是v-show控制的盒子</div>
    <div v-if="flag" class="box">我是v-if控制的盒子</div>
  </div>
  
  <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
  <script>
    const app = new Vue({
      el: '#app',
      //值写在data 数据里面
      data: {
        flag: false
      }
    })
  </script>

</body>
```

### 3. v-else 和 v-else-if

1. 作用：辅助v-if进行判断渲染
2. 语法：v-else       v-else-if="表达式" (条件)
3. ==需要紧接着v-if使用==

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230918095340301.png" alt="image-20230918095340301" style="zoom:50%;" />

- v-else：只有两个条件，两个标签但是只按条件显示一个

~~~html
<p v-if="gender === 1">性别：♂ 男</p>
<p v-else>性别：♀ 女</p>
~~~

- 示例代码：


```js
<body>
  
  <div id="app">
     <!-- if else 两个条件选一个 -->
    <p v-if="gender === 1">性别：♂ 男</p>
    <p v-else>性别：♀ 女</p>
    <hr>
      <!-- 多条件，显示一个 要用if 开始，v-else-if 中间， v-else 结束 -->
    <p v-if="score >= 90">成绩评定A：奖励电脑一台</p>
    <p v-else-if="score >= 70">成绩评定B：奖励周末郊游</p>
    <p v-else-if="score >= 60">成绩评定C：奖励零食礼包</p>
    <p v-else>成绩评定D：惩罚一周不能玩手机</p>
  </div>
  
  <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
  <script>

    const app = new Vue({
      el: '#app',
      data: {
        gender: 2,
        score: 95
      }
    })
  </script>

</body>
```

## 久、事件绑定指令 （v-on）

使用Vue时，如需为DOM注册事件，及其的简单，语法如下：

- <button v-on:`事件名="内联语句"`>按钮</button>
- <button v-on:事件名="处理函数">按钮</button>
- <button v-on:事件名="处理函数(实参)">按钮</button>
- `v-on:` 简写为 **@**

### 1.内联语句

- 语法：v-on:事件名  简写 @事件名

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230918201436293.png" alt="image-20230918201436293" style="zoom: 25%;" />

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230918201504209.png" alt="image-20230918201504209" style="zoom:25%;" />

#### *练习

```html
<body>
  <div id="app">
    <!-- v-on: 简写为 @ -->
    <button @click="count--">-</button>
    <span>{{ count }}</span>
    <!-- <button v-on:click="count + 2 ">+</button> 
      每次点击加2
    -->
    <button v-on:click="count++">+</button>
  </div>
  <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
  <script>
    const app = new Vue({
      el: '#app',
      data: {
        count: 100
      }
    })
  </script>
</body>
```

### 2. 事件处理函数

注意：在new Vue在写 methods带函数： **Phương pháp, cách thức**

- 语法：`v-on:事件名 = "methods中的函数名"`

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230918100328745.png" alt="image-20230918100328745" style="zoom: 33%;" />

- 事件处理函数应该写到一个跟data同级的配置项（methods）中
- ==methods中的函数内部的this都指向Vue实例==

```html
<body>
  <div id="app">
    <button @click="fn">切换显示隐藏</button>
    <h1 v-show="isShow">黑马程序员</h1>
  </div>
  <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
  <script>
    const app4 = new Vue({
      //app是全局变量
      el: '#app',
      data: {
        isShow: true
      },
      methods: {
        fn () {
          // 让提供的所有methods中的函数，this都指向当前实例 （当前的 new 的Vue）
          //app.isShow 可以取到 isShow的值
           console.log('执行了fn', app.isShow)
          // console.log(app3 === this)
          //取反值 原来true 改成false
          //click 如果当时是true， 点击后会取false
          this.isShow = !this.isShow
        }
      }
    })
  </script>
</body>
```

### 3. Vue 指令 v-on 调用传参

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230918102541801.png" alt="image-20230918102541801" style="zoom:50%;" />

####  *练习：显示总计

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230918102621795.png" alt="image-20230918102621795" style="zoom:50%;" />

```html
 <body>

  <div id="app">
    <div class="box">
      <h3>小黑自动售货机</h3>
      <!-- 实参   -->
      <button @click="buy(5)">可乐5元</button>
      <button @click="buy(10)">咖啡10元</button>
      <button @click="buy(8)">牛奶8元</button>
    </div>
    <p>银行卡余额：{{ money }}元</p>
  </div>

  <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
  <script>
    const app = new Vue({
      el: '#app',
      data: {
        money: 100
      },
      methods: {
        //形参 price
        buy (price) {
          this.money -= price
        }
      }
    })
  </script>
</body>
```



## 十、属性绑定指令（v-bind）

1. **作用：**动态设置html的==标签属性== 比如：src、url、title

   （上面有说不能直接卸载标签的属性里面）

2. **语法**：**v-bind: **`属性名=“表达式”`

3. **v-bind:**可以简写成 =>   **:**

比如: 有一个图片，它的 `src` 属性值，是一个图片地址。这个地址在数据 data 中存储。

则可以这样设置属性值：

- `<img v-bind:src="url" />`
- `<img :src="url" />`   （v-bind可以省略）

```html
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
```

### *练习：波仔的学习之旅

- 需求：默认展示数组中的第一张图片，点击上一页下一页来回切换数组中的图片

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230919100227670.png" alt="image-20230919100227670" style="zoom:50%;" />


- 实现思路：


1.数组存储图片路径 ['url1','url2','url3'，...]

2.可以准备个下标index 去数组中取图片地址。

3.通过v-bind给src绑定当前的图片地址

4.点击上一页下一页只需要修改下标的值即可

5.当展示第一张的时候，上一页按钮应该隐藏。展示最后一张的时候，下一页按钮应该隐藏 (隐藏按钮)

```html
<body>
  <!-- 
    1. 绑定指令
     + 点击上一页， 下一页，隐藏 显示 show， v-bind 获取数组下标
     2. 动态下标 （ 获取数组下标）
     3. 点击按钮 --- 选数组下标 上一页 index++， 下一页 index--
     4. 用 v-bind 通过下标获取图片
     5. 限制下标 <0 隐藏
       index > 0 ： 显示
       index < list.length - 1 ： 显示


   -->
  <div id="app">
    <button v-show="index>0" @click="index--">上一页</button>
    <div>
      <img :src="list[index]" alt="">
    </div>
    <button v-show="index < list.length - 1" @click="index++">下一页</button>
  </div>
  <script src="./vue.js"></script>
  <script>
    const app = new Vue({
      el: '#app',
      data: {
        //可以先给值从0开始，刷新页面会从0开始
        index: 0,
        list: [
          './imgs/11-00.gif',
          './imgs/11-01.gif',
          './imgs/11-02.gif',
          './imgs/11-03.gif',
          './imgs/11-04.png',
          './imgs/11-05.png',
        ],

      }
    })
  </script>
</body>

```

## 十一、列表渲染指令（v - for）

- 基于数据循环， 多次渲染整个元素 → ==数组==、对象、数字...


- v-for 指令需要使用 `(item, index) in 数组名` 形式的特殊语法，其中：

  - item 是数组中的每一项

  - index 是每一项的索引，不需要可以省略

  - arr 是被遍历的数组


~~~html
<body>

  <div id="app">
    <h3>小黑水果店</h3>
    <ul>
      <!-- 数组有多少项就渲染多少次 -->
      <!-- item 是数组的每一项 -- 要渲染写item -->
      <!-- index 是数组的下标 -->
      <li v-for="(item, index) in list">
        {{ item }} - {{ index }}
      </li>
    </ul>

    <ul>
      <!-- index 跟 小括号可以省略 -->
      <li v-for="item in list">
        {{ item }}
      </li>
    </ul>
  </div>

  <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
  <script>
    const app = new Vue({
      el: '#app',
      data: {
        list: ['西瓜', '苹果', '鸭梨', '榴莲']
      }
    })
  </script>
</body>
~~~



- 此语法也可以遍历**对象和数字**


```js
//遍历对象
<div v-for="(value, key, index) in object">{{value}}</div>
value:对象中的值
key:对象中的键
index:遍历索引从0开始

//遍历数字
<p v-for="item in 10">{{item}}</p>
item从1 开始
```

### * filter() 方法

- filter() 方法创建一个新的数组，新数组中的元素是通过检查指定数组中符合条件的所有元素。

  **注意：** filter() 不会对空数组进行检测。

  **注意：** filter() 不会改变原始数组

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230919104155028.png" alt="image-20230919104155028" style="zoom: 25%;" />

### *练习：小案例-小黑的书架

需求：

1.根据左侧数据渲染出右侧列表（v-for）

2.点击删除按钮时，应该把当前行从列表中删除（获取当前行的id，利用filter进行过滤）

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230918140517768.png" alt="image-20230918140517768" style="zoom:50%;" />

准备代码：

```js
<div id="app">
    <h3>小黑的书架</h3>
    <ul>
      <li>
        <span>《红楼梦》</span>
        <span>曹雪芹</span>
        <button>删除</button>
      </li>
    </ul>
  </div>
  <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
  <script>
    const app = new Vue({
      el: '#app',
      data: {
        booksList: [
          { id: 1, name: '《红楼梦》', author: '曹雪芹' },
          { id: 2, name: '《西游记》', author: '吴承恩' },
          { id: 3, name: '《水浒传》', author: '施耐庵' },
          { id: 4, name: '《三国演义》', author: '罗贯中' }
        ]
      }
    })
  </script>
```



## 十二、v-for中的key

- **语法：** key="唯一值"

- **作用：**给列表项添加的**唯一标识**。便于Vue进行列表项的**正确排序复用**。

- **为什么加key：**Vue 的默认行为会尝试原地修改元素（**就地复用**）

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230918142701258.png" alt="image-20230918142701258" style="zoom: 33%;" />

- 实例代码：


```js
<ul>
  <li v-for="(item, index) in booksList" :key="item.id">
    <span>{{ item.name }}</span>
    <span>{{ item.author }}</span>
    <button @click="del(item.id)">删除</button>
  </li>
</ul>
```

注意：

1.  key 的值只能是字符串 或 数字类型
2. key 的值必须具有唯一性
3. 推荐使用  id 作为 key（唯一），不推荐使用 index 作为 key（会变化，不对应）

## 十三、双向绑定指令 （v - model)

- 给 表单元素 使用, 双向数据绑定 → 可以快速 获取 或 设置 表单元素内容
  - 数据变化 → 视图自动更新
  - 视图变化 → 数据自动更新
- 所谓双向绑定就是：

1. 数据改变后，呈现的页面结果会更新
2. 页面结果更新后，数据也会随之而变

- **作用：** 给**表单元素**（input、radio、select）使用，双向绑定数据，可以快速 ==**获取** 或 **设置**== 表单元素内容

- **语法：**`v-model="变量"`

### * 练习

**需求：**使用双向绑定实现以下需求

1. 点击登录按钮获取表单中的内容
2. 点击重置按钮清空表单中的内容

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230918191048469.png" alt="image-20230918191048469" style="zoom:50%;" />

```html
<body>

  <div id="app">
    <!-- 
      v-model 可以让数据和视图，形成双向数据绑定
      (1) 数据变化，视图自动更新
      (2) 视图变化，数据自动更新
      可以快速[获取]或[设置]表单元素的内容
     -->
     <!-- 获取data的username， password -->
    账户：<input type="text" v-model="username"> <br><br>
    密码：<input type="password" v-model="password"> <br><br>
    <button @click="login">登录</button>
    <button @click="reset">重置</button>
  </div>
  <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
  <script>
    const app = new Vue({
      el: '#app',
      data: {
        username: '',
        password: ''
      },
      //点击登录获取 username password
      methods: {
        login() {
          console.log(this.username, this.password)
        },
        //点击重置 -- 为空
        reset() {
          this.username = ''
          this.password = ''
        }
      }
    })
  </script>
</body>
```

## 十七、综合案例-小黑记事本

**功能需求：**

1. 列表渲染

2. 删除功能 （叉号）

3. 添加功能

4. 底部统计 和 清空

   <img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230919111727399.png" alt="image-20230919111727399" style="zoom:50%;" />
   
   ### 1.列表渲染和删除

~~~html
<body>

<!-- 主体区域 -->
<section id="app">
  <!-- 输入框 -->
  <header class="header">
    <h1>小黑记事本</h1>
    <input  placeholder="请输入任务" class="new-todo" />
    <button class="add">添加任务</button>
  </header>
  <!-- 列表区域 -->
  <section class="main">
    <ul class="todo-list">
      <!-- 2.遍历数组 for 要加key 要按照数组id的选 是唯一值-->
      <li class="todo" v-for="(item, index) in list" :key="item.id">
        <div class="view">
          <!-- 3. 需要建议用 index 删除后序号也跟着变-->
          <span class="index">{{ index + 1 }}.</span> <label>{{ item.name }}</label>
          <!-- 4.用id来删 -->
          <button @click="del(item.id)" class="destroy"></button>
        </div>
      </li>
    </ul>
  </section>
  <!-- 统计和清空 -->
  <footer class="footer">
    <!-- 统计 -->
    <span class="todo-count">合 计:<strong> 2 </strong></span>
    <!-- 清空 -->
    <button class="clear-completed">
      清空任务
    </button>
  </footer>
</section>

<!-- 底部 -->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
  const app = new Vue({
    el: '#app',
    data: {
      //1.准备数组
      list: [
        { id: 1, name: '跑步一公里' },
        { id: 3, name: '游泳100米' },
      ]
    },
    /* 5. 创建函数 */
    methods: {
      del (id) {
        // console.log(id) => filter 保留所有不等于该 id 的项
        this.list = this.list.filter(item => item.id !== id)
      }
    }
  })

</script>
</body>
~~~

### 2.添加 （输入后点添加任务添加到列表下面， 并且清空输入框）

1. 通过 v-model 绑定 输入框 → 实时获取表单元素的内容

2. 点击按钮，进行新增，往数组最前面加 unshift
   - bug：没有输入内容也可以添加： 写if

~~~html
<body>

<!-- 主体区域 -->
<section id="app">
  <!-- 输入框 -->
  <header class="header">
    <h1>小黑记事本</h1>
    <!-- 1. 添加model指令获取输入内容 -->
    <input v-model="todoName"  placeholder="请输入任务" class="new-todo" />
    <!-- 2.点击按钮绑定事件 -->
    <button @click="add" class="add">添加任务</button>
  </header>
  <!-- 列表区域 -->
  <section class="main">
    <ul class="todo-list">
      <li class="todo" v-for="(item, index) in list" :key="item.id">
        <div class="view">
          <span class="index">{{ index + 1 }}.</span> <label>{{ item.name }}</label>
          <button @click="del(item.id)" class="destroy"></button>
        </div>
      </li>
    </ul>
  </section>
  <!-- 统计和清空 -->
  <footer class="footer">
    <!-- 统计 -->
    <span class="todo-count">合 计:<strong> 2 </strong></span>
    <!-- 清空 -->
    <button class="clear-completed">
      清空任务
    </button>
  </footer>
</section>

<!-- 底部 -->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
  // 添加功能
  // 1. 通过 v-model 绑定 输入框 → 实时获取表单元素的内容
  // 2. 点击按钮，进行新增，往数组最前面加 unshift
  const app = new Vue({
    el: '#app',
    data: {
      //创建新数组存储获取的内容
      todoName: '',
      list: [
        { id: 1, name: '跑步一公里' },
        { id: 3, name: '游泳100米' },
      ]
    },
    methods: {
      del (id) {
        // console.log(id) => filter 保留所有不等于该 id 的项
        this.list = this.list.filter(item => item.id !== id)
      },
      // 3. 添加函数
      add () {
        //6.修复没有输入内容也添加， trim  用于删除字符串的头尾空白符
        //return 结束函数 后面不会再执行
        if (this.todoName.trim() === '') {
          alert('请输入任务名称')
          return
        }
        //4.点击，unshift  用于向当前数组的开头位置插入对象
        this.list.unshift({
          //+new Date() 是唯一 可以用于id
          id: +new Date(),
          name: this.todoName
        })
        //5.清空输入框
        this.todoName = ''
      }
    }
  })

</script>
</body>
~~~



### 3.底部的合计 跟清除任务

~~~html
 <!-- 1.合计就是数组的长度 -->
    <span class="todo-count">合 计:<strong> {{ list.length }} </strong></span>
  <!-- 2. 清空， 下面写clear 函数把list变成空数组 -->
    <button @click="clear" class="clear-completed">
      清空任务
    </button>
clear () {
  this.list = []


~~~



- 小优化：没有任务了 合计 跟清空 隐藏


~~~html
<!-- 统计和清空 → 如果没有任务了，底部隐藏掉 → v-show -->
  <footer class="footer" v-show="list.length > 0">
~~~



***功能总结：** 

① 列表渲染： 

v-for key 的设置 {{ }} 插值表达式 

② 删除功能 

v-on 调用传参 filter 过滤 覆盖修改原数组 

③ 添加功能 

v-model 绑定 unshift 修改原数组添加 

④ 底部统计 和 清空 

数组.length累计长度 

覆盖数组清空列表 

v-show 控制隐藏

## 







