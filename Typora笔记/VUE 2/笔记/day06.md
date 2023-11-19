# DAY 06

## 一、声明式导航-导航链接

- 就是替换a链接标签 `<router-link to="path的值"`

![image-20230930132724561](/Users/guoguo/Library/Application Support/typora-user-images/image-20230930132724561.png)

### 1. 需求

- 实现导航高亮效果


<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230925122308921.png" alt="image-20230925122308921" style="zoom:50%;" />

- 如果使用a标签进行跳转的话，需要给当前跳转的导航加样式，同时要移除上一个a标签的样式，太麻烦！！！


### 2. 解决方案

- vue-router 提供了一个全局组件 router-link (取代 a 标签)

  - **能跳转**，配置 to 属性指定路径(**必须**) 。本质还是 a 标签 ，**to 无需 #**

  - **能高亮**，默认就会提供**高亮类名**，可以直接设置高亮样式


<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230925122424319.png" alt="image-20230925122424319" style="zoom: 25%;" />

- 语法： <router-link to="path的值">发现音乐</router-link>


```vue
  <div>
    <div class="footer_wrap">
      <router-link to="/find">发现音乐</router-link>
      <router-link to="/my">我的音乐</router-link>
      <router-link to="/friend">朋友</router-link>
    </div>
    <div class="top">
      <!-- 路由出口 → 匹配的组件所展示的位置 -->
      <router-view></router-view>
    </div>
  </div>
```

### 3. 通过router-link自带的两个样式进行高亮

- 使用router-link跳转后，我们发现。当前点击的链接默认加了两个class的值

 `router-link-exact-active`和`router-link-active`

- 我们可以给任意一个class属性添加高亮样式即可实现功能


~~~html
<style>
.footer_wrap a.router-link-active {
  background-color: purple;
</style>
~~~

### 4. 总结

![image-20230930133503760](/Users/guoguo/Library/Application Support/typora-user-images/image-20230930133503760.png)


## 二、声明式导航-两个类名

- 当我们使用<router-link></router-link>跳转时，自动给当前导航加了**两个类名**


<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230925124710492.png" alt="image-20230925124710492" style="zoom:50%;" />

### 1. router-link-active

![image-20230930133649908](/Users/guoguo/Library/Application Support/typora-user-images/image-20230930133649908.png)

- **模糊匹配（用的多）**：只要是以/my开头的路径 都可以和 to="/my"匹配到

to="/my"  可以匹配 /my    /my/a    /my/b    ....  

### 2. router-link-exact-active

- **精确匹配**
  - to="/my" 仅可以匹配  /my

### 3. 在地址栏中输入二级路由查看类名的添加

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230930134040558.png" alt="image-20230930134040558" style="zoom:50%;" />

### 4. 总结

- router-link 会自动给当前导航添加两个类名，有什么区别呢？

  - router-link-active 模糊匹配 (用的多) 
- router-link-exact-active 精确匹配


## 三、声明式导航-自定义类名（了解）

### 1.问题

![image-20230930134117312](/Users/guoguo/Library/Application Support/typora-user-images/image-20230930134117312.png)

- router-link的**两个高亮类名 太长了**，我们希望能定制怎么办？

### 2.解决方案

- 我们可以在创建路由对象时，额外配置两个配置项即可， `linkActiveClass`和`linkExactActiveClass`

- index.js （routes 配置项）

~~~javascript
// link自定义高亮类名
  linkActiveClass: 'active', // 配置模糊匹配的类名
  linkExactActiveClass: 'exact-active' // 配置精确匹配的类名
~~~

```js
const router = new VueRouter({
  routes: [...],
  linkActiveClass: "类名1",
  linkExactActiveClass: "类名2"
})
```

### 3.代码演示

```js
// 创建了一个路由对象
const router = new VueRouter({
  routes: [
    ...
  ], 
  linkActiveClass: 'active', // 配置模糊匹配的类名
  linkExactActiveClass: 'exact-active' // 配置精确匹配的类名
})
```

- 然后去找对应的类名样式修改类名 （app.vue) 

![image-20230930134425771](/Users/guoguo/Library/Application Support/typora-user-images/image-20230930134425771.png)

### 4.总结

- 如何自定义router-link的两个**高亮类名**

  - linkActiveClass 模糊匹配 类名自定义 

  - linkExactActiveClass 精确匹配 类名自定义

## 四、声明式导航-查询参数传参

### 1.目标

- 声明式导航： 就是router-link

- 例如：点击 「黑马程序员」跳转到对应的页面 并且 发送请求显示对应数据 （跳转 + 带上传值）

![image-20230930141259195](/Users/guoguo/Library/Application Support/typora-user-images/image-20230930141259195.png) 

- 在跳转路由时，进行传参 

- 比如：现在我们在搜索页点击了热门搜索链接，跳转到详情页，**需要把点击的内容带到详情页**，改怎么办呢？


### 2.跳转传参

我们可以通过两种方式，在跳转的时候把所需要的参数传到其他页面中

- 查询参数传参
- 动态路由传参

### 3.查询参数传参

![image-20230930141316605](/Users/guoguo/Library/Application Support/typora-user-images/image-20230930141316605.png)

- 如何传参？

  ` to="/path?参数名=值"`

- 如何接受参数

  固定用法：`$router.query.参数名`

### 4.代码演示

- 点击黑马程序员跳转到搜索页并且把参数传过去

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230926091257481.png" alt="image-20230926091257481" style="zoom:50%;" />

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230926094617579.png" alt="image-20230926094617579" style="zoom:50%;" />

- Home.vue （父组件）


```vue
<template>
  <div class="home">
    <div class="logo-box"></div>
    <div class="search-box">
      <input type="text">
      <button>搜索一下</button>
    </div>
    <div class="hot-link">
      <!-- 1.把a标签改成 router-link-->
      <!-- 2.传参数 "/path?参数名=值" -->
      热门搜索：
      <router-link to="/search?key=黑马程序员">黑马程序员</router-link>
      <router-link to="/search?key=前端培训">前端培训</router-link>
      <router-link to="/search?key=如何成为前端大牛">如何成为前端大牛</router-link>
    </div>
  </div>
</template>
```

- Search.vue （子组件）

> 1.对应需要传的标签获取数据
>
> 2.发请求 `this.$route.query.参数名` 获取路由参数


```vue
<template>
  <div class="search">
    <!--3.要显示搜索关键词，key后面的部分 -->
    <!-- 获取参数 ： $router.query.参数名 -->
    <p>搜索关键字: {{ $route.query.key }} </p>
    <p>搜索结果: </p>
    <ul>
      <li>.............</li>
      <li>.............</li>
      <li>.............</li>
      <li>.............</li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'MyFriend',
  created () {
    // 4. 发请求
    // 在created中，获取路由参数
    // this.$route.query.参数名 获取
    console.log(this.$route.query.key);
  }
}
</script>
```

## 五、声明式导航-动态路由传参

 ![image-20230930141332448](/Users/guoguo/Library/Application Support/typora-user-images/image-20230930141332448.png)

### 1.动态路由传参方式

- 配置动态路由

  > 动态路由后面的参数可以随便起名，但要有语义

  ```js
  //加冒号 :word
  const router = new VueRouter({
    routes: [
      ...,
      { 
        path: '/search/:words', 
        component: Search 
      }
    ]
  })
  ```

- 配置导航链接

  `to="/path/参数值"`

- 对应页面组件**接受参数**

  `$route.params.参数名`

  > params后面的参数名要和动态路由配置的参数保持一致

- index.js -配路由规则 （参数名，加上点号）

`path: '/search/:words' `

~~~javascript
import Home from '@/views/Home'
import Search from '@/views/Search'
import Vue from 'vue'
import VueRouter from 'vue-router'
Vue.use(VueRouter) // VueRouter插件初始化

// 创建了一个路由对象
const router = new VueRouter({
  routes: [
    { path: '/home', component: Home },
    // 1.改成动态路由， 用 words 所以获取的时候也要用words
    { path: '/search/:words', component: Search }
  ]
})

export default router
~~~

- home.vue

~~~html
<template>
  <div class="home">
    <div class="logo-box"></div>
    <div class="search-box">
      <input type="text">
      <button>搜索一下</button>
    </div>
    <div class="hot-link">
      热门搜索：
      <!-- 2.后面直接写需要传的值-->
      <router-link to="/search/黑马程序员">黑马程序员</router-link>
      <router-link to="/search/前端培训">前端培训</router-link>
      <router-link to="/search/如何成为前端大牛">如何成为前端大牛</router-link>
    </div>
  </div>
</template>
~~~

- search.vue

~~~html
<script>
export default {
  name: 'MyFriend',
  created () {
    //3.获取
    // 在created中，获取路由参数
    // this.$route.query.参数名 获取查询参数
    // this.$route.params.参数名 获取动态路由参数
    console.log(this.$route.params.words);
  }
}
</script>
~~~

### 2.查询参数传参 VS 动态路由传参

![image-20230930143355464](/Users/guoguo/Library/Application Support/typora-user-images/image-20230930143355464.png)

1.  查询参数传参  (比较适合传**多个参数**) 

   1. 跳转：to="/p ath?参数名=值&参数名2=值"
   2. 获取：$route.query.参数名

2. 动态路由传参 (**优雅简洁**，传单个参数比较方便)

   1. 配置动态路由：path: "/path/:参数名" 
   2. 跳转：to="/path/参数值"
   3. 获取：$route.params.参数名 

   注意：动态路由也可以传多个参数，但一般只传一个

### 3.总结

声明式导航跳转时, 有几种方式传值给路由页面？

- 查询参数传参（多个参数）

  - 跳转：to="/path?参数名=值" 

    接收：$route.query.参数名

- 动态路由传参（一个参数，优雅简洁）

  - 路由： /path/:参数名 

    跳转： to="/path/值" 

    接收：$route.params.参数名

## 六、动态路由 参数的可选符(了解)

- 动态路由参数如果是没有传值页面会显示空白 -->添加可选付 （?)

`path:"/search/:words"  `

![image-20230930143509464](/Users/guoguo/Library/Application Support/typora-user-images/image-20230930143509464.png)

### 1.问题

- 配了路由 path:"/search/:words"  为什么按下面步骤操作，会未匹配到组件，显示空白？

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230926094857835.png" alt="image-20230926094857835" style="zoom:50%;" />

### 2.原因

- /search/:words  表示，**必须要传参数**。如果不传参数，也希望匹配，可以加个可选符"？" （添加之后"？" 表示可传可不传）


```js
const router = new VueRouter({
  routes: [
 	...
    { path: '/search/:words?', component: Search }
  ]
})
```

## 七、Vue路由-重定向

- 需要一打开页面会自动跳转到一个页面（例如： home）
- 或者可以强制一个路径跳转到另外一个路径

![image-20230930144400750](/Users/guoguo/Library/Application Support/typora-user-images/image-20230930144400750.png)

### 1.问题

网页打开时， url 默认是 / 路径，未匹配到组件时，会出现空白

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230926102619727.png" alt="image-20230926102619727" style="zoom:50%;" />

### 2.解决方案

**重定向** → 匹配 / 后, 强制跳转 /home 路径

### 3.语法

-  path: 匹配路径  （path:'/' ：就是根路径）
- redirect: 重定向到的路径（强制跳转到另外一个页面） 

```js

{ path:'/' ,redirect:'/home' }
```

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230926102939079.png" alt="image-20230926102939079" style="zoom:50%;" />

### 4.代码演示

```
const router = new VueRouter({
  routes: [
    { path: '/', redirect: '/home'},
 	 ...
  ]
})
```

## 八、Vue路由-404

- 在找不到匹配 -- 页面给提示

![image-20230930144750619](/Users/guoguo/Library/Application Support/typora-user-images/image-20230930144750619.png)

### 1.作用

- 当路径找不到匹配时，给个提示页面


### 2.位置

- 404的路由，虽然配置在任何一个位置都可以，但一般都**配置在其他路由规则的最后面**

### 3.语法

- path: "*"   (任意路径) – 前面不匹配就命中最后这个
- 当发现你前面几个路径匹配不到，最后显示404
  - Views 新建NotFind 提示 （自己写样式， 文案）
  - index 导入使用



```js
import NotFind from '@/views/NotFind'

const router = new VueRouter({
  routes: [
    ...
    //component 是一个组件
    { path: '*', component: NotFind } //最后一个
  ]
})
```

### 4.代码示例

- 新建 NotFound.vue 组件， 然后再index导入使用


```vue
<template>
  <div>
    <h1>404 Not Found</h1>
  </div>
</template>

<script>
export default {

}
</script>

<style>

</style>
```

- router/index.js


```js
...
import NotFound from '@/views/NotFound'
...

// 创建了一个路由对象
const router = new VueRouter({
  routes: [
     ...
    { path: '*', component: NotFound }
  ]
})

export default router
```

## 九、Vue路由-模式设置

![image-20230930145236119](/Users/guoguo/Library/Application Support/typora-user-images/image-20230930145236119.png)

### 1.问题

路由的路径看起来不自然, 有#，能否切成真正路径形式?

- hash路由(默认)        例如:  http://localhost:8080/#/home
- history路由(常用)     例如: http://localhost:8080/home   (以后上线需要服务器端支持，开发环境webpack给规避掉了history模式的问题)

### 2.语法

- Index.js

```js
const router = new VueRouter({
    mode:'histroy', //默认是hash
    routes:[]
})
```

> 注意： 一旦采用了history模式，地址栏就没有#了，需要后台配置访问规则
>

## 十、编程式导航-两种路由跳转方式

- 点击按钮跳转页面

- 上面是导航点击跳转页面，现在是点击按钮跳转对应页面（例如： 点击注册 - 跳转到注册页面）-->编程式导航

![image-20230930145738001](/Users/guoguo/Library/Application Support/typora-user-images/image-20230930145738001.png)

### 1.问题

- 点击按钮跳转如何实现？


<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230926104044744.png" alt="image-20230926104044744" style="zoom:50%;" />

### 2.方案

- 编程式导航：用JS代码来进行跳转


### 3.语法

两种语法：

- path 路径跳转 （简易方便）
- name 命名路由跳转 (适合 path 路径长的场景)

### 4.path路径跳转语法

![image-20230930150101263](/Users/guoguo/Library/Application Support/typora-user-images/image-20230930150101263.png)

- 点击按钮 --  跳转页面 （给按钮绑定事件 -- 写方法（写对应路径）

~~~html
<script>
export default {
  name: 'FindMusic',
  methods: {
    goSearch () {
      //简写
      // (1) this.$router.push('路由路径')
      /this.$router.push('/search')
			
      //完整
      // (2) this.$router.push({     
      //         path: '路由路径' 
      //     })
       this.$router.push({
         path: '/search'
       })

      })
    }
  }
}
</script>
~~~



- 特点：简易方便


```js
//简单写法
this.$router.push('路由路径')

//完整写法
this.$router.push({
  path: '路由路径'
})
```

### 6.name命名路由跳转 

- 给路径起名字

![image-20230930150413836](/Users/guoguo/Library/Application Support/typora-user-images/image-20230930150413836.png)

- 特点：适合 path 路径长的场景 （给路径起名字）


- 语法：


- 路由规则，必须配置name配置项

  ```js
  { name: '路由名', path: '/path/xxx', component: XXX },
  ```

- 通过name来进行跳转

  ```js
  this.$router.push({
    name: '路由名'
  })
  ```



### 7.代码演示通过name命名路由跳转跟 path跳转方式

- home.vue

~~~html
<template>
  <div class="home">
    <div class="logo-box"></div>
    <div class="search-box">
      <input type="text">
      <!--绑定点击事件 -->
      <button @click="goSearch">搜索一下</button>
    </div>
    <div class="hot-link">
      热门搜索：
      <router-link to="/search/黑马程序员">黑马程序员</router-link>
      <router-link to="/search/前端培训">前端培训</router-link>
      <router-link to="/search/如何成为前端大牛">如何成为前端大牛</router-link>
    </div>
  </div>
</template>

<script>
export default {
  name: 'FindMusic',
  methods: {
    goSearch () {
      // 1. 通过路径的方式跳转
      // (1) this.$router.push('路由路径') [简写]
      // this.$router.push('/search')

      // (2) this.$router.push({     [完整写法 - 对象]
      //         path: '路由路径' 
      //     })
      // this.$router.push({
      //   path: '/search'
      // })

      // 2. 通过命名路由的方式跳转 
      // (需要给路由起名字)  在index  { name: 'search', path: '/search/:words?', component: Search }
      // 适合长路径 - 对象
      //    this.$router.push({
      //        name: '路由名'
      //    })
      this.$router.push({
        name: 'search'
      })
    }
  }
}
</script>
~~~

### 8.总结

![image-20230930150546501](/Users/guoguo/Library/Application Support/typora-user-images/image-20230930150546501.png)

## 十一、编程式导航-path路径跳转传参

- 点击按钮跳转页面 + 带上用户输入内容
- 上面学过两种传参： 查询传参 + 动态路由传参 （两个路径跳转都支持）

![image-20230930150715621](/Users/guoguo/Library/Application Support/typora-user-images/image-20230930150715621.png)

### 1.问题

- 点击搜索按钮，跳转需要把文本框中输入的内容传到下一个页面如何实现？

### 2.两种传参方式

1.查询参数 

2.动态路由传参

### 3.传参

两种跳转方式，对于两种传参方式都支持：

① path 路径跳转传参

② name 命名路由跳转传参

### 4.path路径跳转传参（query传参）

![image-20230930150910267](/Users/guoguo/Library/Application Support/typora-user-images/image-20230930150910267.png)

- 传参

```js
//简单写法
this.$router.push('/路径?参数名1=参数值1&参数2=参数值2')
//完整写法
this.$router.push({
  path: '/路径',
  query: {
    参数名1: '参数值1',
    参数名2: '参数值2'
  }
})
```

- 获取
  - 参数的方式依然是：$route.query.参数名


### 5.path路径跳转传参（动态路由传参）

![image-20230930151035314](/Users/guoguo/Library/Application Support/typora-user-images/image-20230930151035314.png)

- 传参

```javascript
//简单写法
this.$router.push('/路径/参数值')
//完整写法
this.$router.push({
  path: '/路径/参数值'
})
```

- 获取
  - 参数的方式依然是：$route.params.参数值

**注意：**path不能配合params使用

1.查询参数

- 简写语法

> 1.model 获取输入框内容， data获取数据
>
> 2.直接给按钮方法 传参数 （获取数据 - 传）
>
> ~~~javascript
> 
> this.$router.push(`/search?key=${this.inpValue}`)
> ~~~
>
> 3. 子组件： 找对应标签获取数据 -- 添加对应标签
>
> ~~~html
>  <p>搜索关键字: {{ $route.query.words }} </p>
> ~~~

~~~javascript
//1.传参 this.$router.push('路由路径?参数名=参数值')
 this.$router.push(`/search?key=${this.inpValue}`)
//2.参数值需要给data绑定
data () {
    return {
      inpValue: ''
    }
//3. 绑定到输入框
 <input v-model="inpValue" type="text">
//4. 获取内容 search.vue
 <p>搜索关键字: {{ $route.query.words }} </p>

~~~

- 完整语法（写成对象更适合传参）

  - 可以传多个值

  ~~~html
  query: { 
  参数名: 参数值
  参数名: 参数值
  }
  ~~~

  - 代码演示：

~~~javascript
     this.$router.push({
        path: 路由,
         query: {
           key: this.inpValue
           key2: this.inpValue
         }
~~~

2.动态路由参数

- 先配动态路由 ` :words `（index）
  - ${this.inpValue} = 值

~~~javascript
//完整写法
 this.$router.push({
         path: `/search/${this.inpValue}`
       })
//简写
this.$router.push(/search/${this.inpValue})
//在search解析
 <p>搜索关键字: {{ $route.params.words }} </p>
~~~

## 十二、编程式导航-name命名路由传参

- 点击按钮， 提哦啊转页面并且传用户输入的内容

![image-20230930152452525](/Users/guoguo/Library/Application Support/typora-user-images/image-20230930152452525.png)

### 1.name 命名路由跳转传参 (query传参)

```js
this.$router.push({
  name: '路由名字',
  query: {
    参数名1: '参数值1',
    参数名2: '参数值2'
  }
})
```

- 接受

~~~html
<p>搜索关键字: {{ $route.query.key }} </p>
~~~

### 2.name 命名路由跳转传参 (动态路由传参)

```js
this.$router.push({
  name: '路由名字',
  params: {
    参数名: '参数值',
  }
})
```

- 接受

~~~html
<p>搜索关键字: {{ $route.params.words }} </p>
~~~

- 代码演示


```js
this.$router.push({
  name: '路由名字',
  params: {
    参数名: '参数值',
  }
})
 this.$router.push({
        name: 'search',
        // query: {
        //   key: this.inpValue
        // }
        params: {
          //按照index路由：words
          words: this.inpValue
        }
      })
```

### 3.总结

- 编程式导航，如何跳转传参？怎么跳转 -怎么传参

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230926114409404.png" alt="image-20230926114409404" style="zoom:50%;" />

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230926114513178.png" alt="image-20230926114513178" style="zoom:50%;" />

## 十三、面经基础版-案例效果分析

### 1.面经效果演示

### 2.功能分析

- 通过演示效果发现，主要的功能页面有两个，一个是**列表页**，一个是**详情页**，并且在列表页点击时可以跳转到详情页
- 底部导航可以来回切换，并且切换时，只有上面的主题内容在动态渲染

![image-20230926114805840](/Users/guoguo/Library/Application Support/typora-user-images/image-20230926114805840.png)

### 3.实现思路分析：配置路由+功能实现

1.配置路由

- 首页和面经详情页，两个一级路由
- 首页内嵌套4个可切换的页面（嵌套二级路由）

2.实现功能

- 首页请求渲染
- **跳转传参** 到 详情页，详情页动态渲染
- 组件缓存，性能优化

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230926114911461.png" alt="image-20230926114911461" style="zoom:50%;" />

## 十四、面经基础版-一级路由配置

1.把文档中准备的素材拷贝到项目中

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230926115645317.png" alt="image-20230926115645317" style="zoom:25%;" />

2.针对router/index.js文件 进行一级路由配置

```js
//1.1 导入 渲染Layout
import Layout from '@/views/Layout.vue'
//2.1 面经详情渲染
import ArticleDetail from '@/views/ArticleDetail.vue'



const router = new VueRouter({
  //1.配路径
  routes: [
    {
      path: '/',
      component: Layout
    },
    {
      //2.面经详情路由
      path: '/detail',
      component: ArticleDetail
    }
  ]
})
```



## 十五、面经基础版-二级路由配置

- 二级路由也叫嵌套路由，当然也可以嵌套三级、四级...

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230926131636721.png" alt="image-20230926131636721" style="zoom:50%;" />

### 1.使用场景

- 当在页面中点击链接跳转，只是部分内容切换时，我们可以使用嵌套路由


### 2.语法

- 在一级路由下，配置children属性即可
- 配置二级路由的出口 

 1.在一级路由下，配置children属性

 **注意**:一级的路由path 需要加 `/`   二级路由的path不需要加 `/`

```js
const router = new VueRouter({
  routes: [
    {
      path: '/',
      component: Layout,
      children:[
        // 通过 children 配置项，可以配置嵌套子路由
      // 1. 在children配置项中，配规则
      // 2. 准备二级路由出口
        {path:'xxxx',component:xxxx.vue},
        {path:'xxxx',component:xxxx.vue},
      ]
    }
  ]
})
```

- 技巧：二级路由应该配置到哪个一级路由下呢？


**这些二级路由对应的组件渲染到哪个一级路由下，children就配置到哪个路由下边**

2.配置二级路由的出口 (在哪里显示)

` <router-view></router-view>`

**注意：** 配置了嵌套路由，一定配置对应的路由出口，否则不会渲染出对应的组件

- Layout.vue


```vue
<template>
  <div class="h5-wrapper">
    <div class="content">
      <!-- 二级路由出口, -->
      <router-view></router-view>
    </div>
  ....
  </div>
</template>
```

### 3.代码实现

- router/index.js


```js
...
import Article from '@/views/Article.vue'
import Collect from '@/views/Collect.vue'
import Like from '@/views/Like.vue'
import User from '@/views/User.vue'
...

const router = new VueRouter({
  routes: [
    {
      path: '/',
      component: Layout,
      redirect: '/article',
      children:[
        {
          path:'/article',
          component:Article
        },
        {
          path:'/collect',
          component:Collect
        },
        {
          path:'/like',
          component:Like
        },
        {
          path:'/user',
          component:User
        }
      ]
    },
    ....
  ]
})

```

Layout.vue

```vue
<template>
  <div class="h5-wrapper">
    <div class="content">
      <!-- 内容部分 -->
      <router-view></router-view>
    </div>
    <nav class="tabbar">
      <a href="#/article">面经</a>
      <a href="#/collect">收藏</a>
      <a href="#/like">喜欢</a>
      <a href="#/user">我的</a>
    </nav>
  </div>
</template>
```



## 十六、面经基础版-二级导航高亮

### 1.实现思路

- 将a标签替换成 <router-link></router-link>组件，配置to属性，不用加 #
- 结合高亮类名实现高亮效果 (推荐模糊匹配：router-link-active)

### 2.代码实现

Layout.vue

```vue
....
    <nav class="tabbar">
      <router-link to="/article">面经</router-link>
      <router-link to="/collect">收藏</router-link>
      <router-link to="/like">喜欢</router-link>
      <router-link to="/user">我的</router-link>
    </nav>

<style>
   a.router-link-active {
      color: orange;
    }
</style>
```

## 十七、面经基础版-首页请求渲染

### 1.步骤分析

1.安装axios 

2.看接口文档，确认请求方式，请求地址，请求参数

3.created中发送请求，获取数据，存储到data中

4.页面动态渲染

### 2.代码实现

1.安装axios

`yarn add axios `  `npm i axios`

重启服务器 

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230926133732923.png" alt="image-20230926133732923" style="zoom:50%;" />

2.接口文档

```vue
请求地址: https://mock.boxuegu.com/mock/3083/articles
请求方式: get
```

3.created中发送请求，获取数据，存储到data中

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230926134128887.png" alt="image-20230926134128887" style="zoom: 25%;" />

```vue
 <script>
   //2.提供数据
data() {
    return {
      //创建空数组
      articelList: [],
    }
  },
    //1.发送请求
  async created() {
    const {  data: { result: { rows } }} = await axios.get('https://mock.boxuegu.com/mock/3083/articles')
    //数据给articelList
    //this.articelList = res.data.result.rows
    this.articelList = rows
  },
 </script>
```

4.页面动态渲染

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230926134521068.png" alt="image-20230926134521068" style="zoom:50%;" />

```vue
<template>
  <div class="article-page">
    <!-- 1.用for渲染数据 -->
    <div
      v-for="item in articles"
      :key="item.id"
      @click="$router.push(`/detail/${item.id}`)"
      class="article-item">
      <!-- 2.配置对应的值 -->
      <div class="head">
        <img :src="item.creatorAvatar" alt="" />
        <div class="con">
          <p class="title">{{ item.stem }}</p>
          <p class="other">{{ item.creatorName }} | {{ item.createdAt }}</p>
        </div>
      </div>
      <div class="body">
        {{ item.content }}
      </div>
      <div class="foot">点赞 {{ item.likeCount }} | 浏览 {{ item.views }}</div>
    </div>
  </div>
</template>
```

## 十八、面经基础版-查询参数传参

### 1.说明

跳转详情页需要把当前点击的文章id传给详情页，获取数据

- 查询参数传参  this.$router.push('/detail?参数1=参数值&参数2=参数值') 
- 动态路由传参  先改造路由 在传参  this.$router.push('/detail/参数值')

### 2.查询参数传参实现

- Article.vue


```vue
<template>
<!-- 1.注册点击事件 @click="$router.push(`/detail）
2. 获取id，地址会带上id (`/detail?id=${item.id}`)
-->
  <div class="article-page">
    <div class="article-item" 
      v-for="item in articelList" :key="item.id" 
      @click="$router.push(`/detail?id=${item.id}`)">
     ...
    </div>
  </div>
</template>
```

- ArticleDetail.vue


```javascript
  //获取id
	created(){
    console.log(this.$route.query.id)
  }
```

## 十九、面经基础版-动态路由传参

### 1.实现步骤

- 改造路由
- 动态传参
- 在详情页获取参数

### 2.代码实现

- 改造路由


router/index.js

```js
...
//改成id
  {
      path: '/detail/:id',
      component: ArticleDetail
  }
```

- Article.vue


```vue
//加上 ${item.id}
<div class="article-item" 
     v-for="item in articelList" :key="item.id" 
     @click="$router.push(`/detail/${item.id}`)">
       ....
 </div>
```

- ArticleDetail.vue


```vue
  created(){
    console.log(this.$route.params.id)
  }
```

- Index.js 访问 / 重定向到 /article   (redirect)

~~~javascript
//加上冲顶向到redirect 面经显示 article
routes: [
    { 
      path: '/',
      component: Layout,
      redirect: '/article',
~~~



### 3.额外优化功能点-点击回退跳转到上一页

- ArticleDetail.vue


```vue
//1.点击事件 加上back() 返回 @click="$router.back()"
<template>
  <div class="article-detail-page">
    <nav class="nav"><span class="back" @click="$router.back()">&lt;</span> 面经详情</nav>
     ....
  </div>
</template>
```



## 二十、面经基础版-详情页渲染

### 1.实现步骤分析

- 导入axios
- 查看接口文档
- 在created中发送请求
- 页面动态渲染

### 2.代码实现

- 接口文档


```vue
 //:id 传的是id， 冒号动态传参
请求地址: https://mock.boxuegu.com/mock/3083/articles/:id
 请求方式: get
```

- 在created中发送请求 article.vue

```vue
//获取数据， 创建一个空对象
data() {
    return {
      articleDetail:{}
    }
  },
  async created() {
//获取id
    const id = this.$route.params.id
//换成 ${id} const{data} 换成 {data:{result}}
    const {data:{result}} = await axios.get(
      `https://mock.boxuegu.com/mock/3083/articles/${id}`
    )
//获取 result this.article = data.result
    this.articleDetail = result
  },
```

- 页面动态渲染


```vue
<template>
  <div class="article-detail-page">
    <nav class="nav">
      <span class="back" @click="$router.back()">&lt;</span> 面经详情
    </nav>
    <header class="header">
      <h1>{{articleDetail.stem}}</h1>
      <p>{{articleDetail.createAt}} | {{articleDetail.views}} 浏览量 | {{articleDetail.likeCount}} 点赞数</p>
      <p>
        <img
          :src="articleDetail.creatorAvatar"
          alt=""
        />
        <span>{{articleDetail.creatorName}}</span>
      </p>
    </header>
    <main class="body">
      {{articleDetail.content}}
    </main>
  </div>
</template>

```

- bug: 页面有一瞬间空白： 因为法请求需要时间，加上条件

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230926143143741.png" alt="image-20230926143143741" style="zoom:50%;" />

~~~html
 有id就进行下面代码
<div class="article-detail-page" v-if="article.id">
~~~



## 二十一、面经基础版-缓存组件

### 1.问题

- 从面经列表 点到 详情页，又点返回，数据重新加载了 →  **希望回到原来的位置**


<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230926152226273.png" alt="image-20230926152226273" style="zoom:50%;" />

### 2.原因

当路由被**跳转**后，原来所看到的组件就**被销毁**了（会执行组件内的beforeDestroy和destroyed生命周期钩子），**重新返回**后组件又被**重新创建**了（会执行组件内的beforeCreate,created,beforeMount,Mounted生命周期钩子），**所以数据被加载了**

### 3.解决方案

利用keep-alive把原来的组件给缓存下来

### 4.什么是keep-alive

- keep-alive 是 Vue 的内置组件，当它包裹动态组件时，**会缓存不活动的组件实例，而不是销毁**它们。


- keep-alive 是一个抽象组件：它自身不会渲染成一个 DOM 元素，也不会出现在父组件中。


- **优点：**

在组件切换过程中把切换出去的组件保留在内存中，防止重复渲染DOM，

减少加载时间及性能消耗，提高用户体验性。

- App.vue


```vue
<!-- 包裹了keep-alive 一级路由匹配的组件都会被缓存
         LayoutPage组件(被缓存) - 多两个生命周期钩子
          - actived 激活时，组件被看到时触发
          - deactived 失活时，离开页面组件看不见触发
         ArticleDetailPage组件(未被缓存)  
    -->
<template>
  <div class="h5-wrapper">
    <keep-alive>
      <router-view></router-view>
    </keep-alive>
  </div>
</template>
```

- **问题：**

缓存了所有被切换的组件（但是我们需要的是有些被缓存有些不被缓存）



### 5.keep-alive的三个属性

① include  ： 组件名数组，只有匹配的组件**会被缓存**

② exclude ： 组件名数组，任何匹配的组件都**不会被缓存**

③ max       ： 最多可以**缓存多少**组件实例

- 需求：只希望Layout被缓存，include配置

  ​     :include="组件名数组"

  - 组件名数组就是name，如果没有name 才用文件名

- App.vue


```vue

<template>
  <div class="h5-wrapper">
    <!-- 包裹了keep-alive 一级路由匹配的组件都会被缓存
         LayoutPage组件(被缓存) - 多两个生命周期钩子
          - actived 激活时，组件被看到时触发
          - deactived 失活时，离开页面组件看不见触发
         ArticleDetailPage组件(未被缓存)

         需求：只希望Layout被缓存，include配置
         :include="组件名数组"
    -->
    <keep-alive :include="keepArr">
      <router-view></router-view>
    </keep-alive>
  </div>
</template>

<script>
export default {
  name: 'h5-wrapper',
  //如果需要缓存的数据多可以直接写对象
  data () {
    return {
      // 缓存组件名的数组
      keepArr: ['LayoutPage']
    }
  }
}
</script>

```



### 6.额外的两个生命周期钩子

**keep-alive的使用会触发两个生命周期函数**

**activated** 当组件被激活（使用）的时候触发 →  进入这个页面的时候触发

**deactivated** 当组件不被使用的时候触发      →  离开这个页面的时候触发

组件**缓存后**就**不会执行**组件的**created, mounted, destroyed** 等钩子了

所以其提供了**actived 和deactived**钩子，帮我们实现业务需求

~~~javascript
<script>
export default {
  // 组件名(如果没有配置 name，才会找文件名作为组件名)
  name: 'LayoutPage',
  // 组件缓存了，就不会执行组件的created，mounted，destroyed等钩子
  // 所以提供了 actived 和 deactived
  created () {
    console.log('created 组件被加载了');
  },
  mounted () {
    console.log('mounted dom渲染完了');
  },
  destroyed () {
    console.log('destroyed 组件被销毁了');
  },

  activated () {
    alert('你好，欢迎回到首页')
    console.log('activated 组件被激活了，看到页面了');
  },
  deactivated () {
    console.log('deactivated 组件失活，离开页面了');
  }
}
</script>
~~~



### 7.总结

1.keep-alive是什么

Vue 的内置组件，包裹动态组件时，可以缓存 

2.keep-alive的优点

组件切换过程中 把切换出去的组件保留在内存中(提升性能) 

3.keep-alive的三个属性 (了解)

① include ： 组件名数组，只有匹配的组件会被缓存 

② exclude ： 组件名数组，任何匹配的组件都不会被缓存 

③ max ： 最多可以缓存多少组件实例

4.keep-alive的使用会触发两个生命周期函数(了解)

activated 当组件被激活（使用）的时候触发 → 进入页面触发 

deactivated 当组件不被使用的时候触发 → 离开页面触发

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230926153729281.png" alt="image-20230926153729281" style="zoom:50%;" />



## 二十二、VueCli 自定义创建项目

- 一次性创建目录

1.安装脚手架 (已安装)

```
npm i @vue/cli -g
```

2.创建项目

```
vue create hm-exp-mobile
```

+ 选项

```js
Vue CLI v5.0.8
? Please pick a preset:
  Default ([Vue 3] babel, eslint)
  Default ([Vue 2] babel, eslint)
> Manually select features     选自定义
```

+ 手动选择功能

![68294185617](/Users/guoguo/Documents/Typora笔记/VUE 2 ,3/code/06-day06/assets/1682941856172.png)

+ 选择vue的版本

```jsx
  3.x
> 2.x
```

+ 是否使用history模式

![image-20201025150602129](/Users/guoguo/Documents/Typora笔记/VUE 2 ,3/code/06-day06/assets/1682941888453.png)

+ 选择css预处理

![image-20220629175133593](/Users/guoguo/Documents/Typora笔记/VUE 2 ,3/code/06-day06/assets/1682941900018.png)

+ 选择eslint的风格 （eslint 代码规范的检验工具，检验代码是否符合规范）
+ 比如：const age = 18;   =>  报错！多加了分号！后面有工具，一保存，全部格式化成最规范的样子

![68294191856](/Users/guoguo/Documents/Typora笔记/VUE 2 ,3/code/06-day06/assets/1682941918562.png)

+ 选择校验的时机 （直接回车）

![68294193579](/Users/guoguo/Documents/Typora笔记/VUE 2 ,3/code/06-day06/assets/1682941935794.png)

+ 选择配置文件的生成方式 （直接回车）

![68294194798](/Users/guoguo/Documents/Typora笔记/VUE 2 ,3/code/06-day06/assets/1682941947985.png)

- 是否保存预设，下次直接使用？  =>   不保存，输入 N

![68294196155](/Users/guoguo/Documents/Typora笔记/VUE 2 ,3/code/06-day06/assets/1682941961551.png)

+ 等待安装，项目初始化完成

![68294197476](/Users/guoguo/Documents/Typora笔记/VUE 2 ,3/code/06-day06/assets/1682941974763.png)

+ 启动项目

```
npm run serve
```



## 二十三、ESlint代码规范及手动修复

代码规范：一套写代码的约定规则。例如：赋值符号的左右是否需要空格？一句结束是否是要加;？... 

>  没有规矩不成方圆  

ESLint:是一个代码检查工具，用来检查你的代码是否符合指定的规则(你和你的团队可以自行约定一套规则)。在创建项目时，我们使用的是 [JavaScript Standard Style](https://standardjs.com/readme-zhcn.html) 代码风格的规则。

#### 1.JavaScript Standard Style 规范说明

建议把：https://standardjs.com/rules-zhcn.html 看一遍，然后在写的时候,  遇到错误就查询解决。

下面是这份规则中的一小部分：

- *字符串使用单引号* – 需要转义的地方除外
- *无分号* – [这](http://blog.izs.me/post/2353458699/an-open-letter-to-javascript-leaders-regarding)[没什么不好。](http://inimino.org/~inimino/blog/javascript_semicolons)[不骗你！](https://www.youtube.com/watch?v=gsfbh17Ax9I)
- *关键字后加空格* `if (condition) { ... }`
- *函数名后加空格* `function name (arg) { ... }`
- 坚持使用全等 `===` 摒弃 `==` 一但在需要检查 `null || undefined` 时可以使用 `obj == null`
- ......

#### 2.代码规范错误

如果你的代码不符合standard的要求，eslint会跳出来刀子嘴，豆腐心地提示你。

下面我们在main.js中随意做一些改动：添加一些空行，空格。

```js
import Vue from 'vue'
import App from './App.vue'

import './styles/index.less'
import router from './router'
Vue.config.productionTip = false

new Vue ( {
  render: h => h(App),
  router
}).$mount('#app')


```

按下保存代码之后：

你将会看在控制台中输出如下错误：

![68294255431](/Users/guoguo/Documents/Typora笔记/VUE 2 ,3/code/06-day06/assets/1682942554314.png)

> eslint 是来帮助你的。心态要好，有错，就改

#### 3.手动修正

- 根据错误提示来一项一项手动修正。


- 如果你不认识命令行中的语法报错是什么意思，你可以根据错误代码（func-call-spacing, space-in-parens,.....）去 ESLint 规则列表中查找其具体含义。


- 打开 [ESLint 规则表](https://zh-hans.eslint.org/docs/latest/rules/)，使用页面搜索（Ctrl + F）这个代码，查找对该规则的一个释义。


![68294279221](/Users/guoguo/Documents/Typora笔记/VUE 2 ,3/code/06-day06/assets/1682942792219.png)

## 二十四、通过eslint插件来实现自动修正

> 1. eslint会自动高亮错误显示
> 2. 通过配置，eslint会自动帮助我们修复错误

+ 如何安装

![68294292098](/Users/guoguo/Documents/Typora笔记/VUE 2 ,3/code/06-day06/assets/1682942920986.png)

+ 如何配置

```js
// 当保存的时候，eslint自动帮我们修复错误
"editor.codeActionsOnSave": {
    "source.fixAll": true
},
// 保存代码，不自动格式化
"editor.formatOnSave": false
```

+ 注意：eslint的配置文件必须在根目录下，这个插件才能才能生效。打开项目必须以根目录打开，一次打开一个项目
+ 注意：使用了eslint校验之后，把vscode带的那些格式化工具全禁用了 Beatify

settings.json 参考

```jsx
{
    "window.zoomLevel": 2,
    "workbench.iconTheme": "vscode-icons",
    "editor.tabSize": 2,
    "emmet.triggerExpansionOnTab": true,
    // 当保存的时候，eslint自动帮我们修复错误
    "editor.codeActionsOnSave": {
        "source.fixAll": true
    },
    // 保存代码，不自动格式化
    "editor.formatOnSave": false
}
```















