#  Day04

## 一、学习目标

### 1.组件的三大组成部分（结构/样式/逻辑）

​    scoped解决样式冲突/data是一个函数

###  2.组件通信

1.  组件通信语法
2. 父传子
3. 子传父
4. 非父子通信（扩展）

### 3.综合案例：小黑记事本（组件版）

1. 拆分组件
2. 列表渲染
3. 数据添加
4. 数据删除
5. 列表统计
6. 清空
7. 持久化

### 4.进阶语法

1. v-model原理
2. v-model应用于组件
3. sync修饰符
4. ref和$refs
5. $nextTick

## 二、scoped解决样式冲突

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230929104227477.png" alt="image-20230929104227477" style="zoom:50%;" />

### **1.默认情况**：

- 写在组件中的样式会 **全局生效** →  因此很容易造成多个组件之间的样式冲突问题
  - 例如：样式选择器写的 是 div{} 那么其他组件有div 也会被影响



1. **全局样式**: 默认组件中的样式会作用到全局，任何一个组件中都会受到此样式的影响


2. **局部样式**: 可以给组件加上**scoped** 属性,可以**让样式只作用于当前组件**

> 1.style中的样式 默认是作用到全局的
>
>   2.加上scoped可以让样式变成局部样式
>
>   组件都应该有独立的样式，推荐加scoped（原理）
>
> -   scoped原理：
>
>   1.给当前组件模板的所有元素，都会添加上一个自定义属性
>
>   data-v-hash值
>
>   data-v-5f6a9d56  用于区分开不通的组件
>
>   2.css选择器后面，被自动处理，添加上了属性选择器
>
>   div[data-v-5f6a9d56]
>
> <img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230920135435722.png" alt="image-20230920135435722" style="zoom:50%;" />
>
> 

### 2.代码演示

BaseOne.vue

```vue
<template>
  <div class="base-one">
    BaseOne
  </div>
</template>

<script>
export default {

}
</script>
<style scoped>
</style>
```

BaseTwo.vue

```vue
<template>
  <div class="base-one">
    BaseTwo
  </div>
</template>

<script>
export default {

}
</script>

<style scoped>
</style>
```

App.vue

```vue
<template>
  <div id="app">
    <BaseOne></BaseOne>
    <BaseTwo></BaseTwo>
  </div>
</template>

<script>
import BaseOne from './components/BaseOne'
import BaseTwo from './components/BaseTwo'
export default {
  name: 'App',
  components: {
    BaseOne,
    BaseTwo
  }
}
</script>
```

### 3.scoped原理

![image-20230929104541124](/Users/guoguo/Library/Application Support/typora-user-images/image-20230929104541124.png)

1. 当前组件内标签都被添加**data-v-hash值** 的属性 
2. css选择器都被添加 [**data-v-hash值**] 的属性选择器

最终效果: **必须是当前组件的元素**, 才会有这个自定义属性, 才会被这个样式作用到 

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230920135530996.png" alt="image-20230920135530996" style="zoom:50%;" />

### 4.总结

1. style的默认样式是作用到哪里的？
2. scoped的作用是什么？
3. style中推不推荐加scoped？



## 三、data必须是一个函数

![image-20230929104949229](/Users/guoguo/Library/Application Support/typora-user-images/image-20230929104949229.png)

### 1、data为什么要写成函数

- 一个组件的 **data** 选项必须**是一个函数**。目的是为了：保证每个组件实例，维护**独立**的一份**数据**对象。


- 每次创建新的组件实例，都会新**执行一次data 函数**，得到一个新对象。
- 例如： 下面3个button， 每次点一个不会影响到其他的  是因为data是一个韩式每次点击都执行单独一次


<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230920135707883.png" alt="image-20230920135707883" style="zoom:50%;" />

### 2.代码演示

- BaseCount.vue


```vue
<template>
  <div class="base-count">
    <button @click="count--">-</button>
    <span>{{ count }}</span>
    <button @click="count++">+</button>
  </div>
</template>

<script>
export default {
   data() {
     console.log('函数执行了')
     return {
       count: 100,
     }
   },
  // 写成对象 （报错 -- 必须是函数），
  data: function () {
    return {
      count: 100,
    }
  },
}
</script>

<style>
.base-count {
  margin: 20px;
}
</style>
```

- App.vue


```vue
<template>
  <div class="app">
    <!-- 使用多次 baseCount 组件-->
    <baseCount></baseCount>
    <baseCount></baseCount>
    <baseCount></baseCount>
  </div>
</template>

<script>
import baseCount from './components/BaseCount'
export default {
  components: {
    baseCount,
  },
}
</script>

<style>
</style>



```

### 3.总结

组件三大组成部分的注意点：

1. 结构：有且只能一个根元素
2. 样式：默认全局样式，加上 scoped 局部样式
3. 逻辑：data是一个函数，保证数据独立。

## 四、组件通信

### 1.什么是组件通信？

组件通信，就是指**组件与组件**之间的**数据传递**

- 组件的数据是独立的，无法直接访问其他组件的数据。
- 想使用其他组件的数据，就需要组件通信

### 2.组件之间如何通信

思考：

1. 组件之间有哪些关系？
2. 对应的组件通信方案有哪几类？

### 3.组件关系分类

1. 父子关系
2. 非父子关系

![image-20230929105537834](/Users/guoguo/Library/Application Support/typora-user-images/image-20230929105537834.png)

### 4.通信解决方案

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230920141258701.png" alt="image-20230920141258701" style="zoom:50%;" />

### 5.父子通信流程

1. 父组件通过 **props** 将数据传递给子组件
2. 子组件利用 **$emit** 通知父组件修改更新

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230920141325977.png" alt="image-20230920141325977" style="zoom:50%;" />

### 6.父向子通信代码示例

- 讲解过程：

  - 父组件（有数据） -- data

  - 通过组件标签， 添加自定义组件传值 ， 动态属性加冒号

    `<Son :title="myTitle"></Son>`

  - 子组件通过props接受 

    - props：用数组可以接受多个传值
    - 数组名需要跟父组件的属性名一样

  ~~~html
  export default {
    name: 'Son-Child',
    // 2.通过props来接受
    props:['title']
  }
  ~~~

  - 子组件使用数据

  ~~~html
  <div>
    我是Son组件 {{title}}
  </div>
  ~~~

  

![image-20230929112632772](/Users/guoguo/Library/Application Support/typora-user-images/image-20230929112632772.png)

- 父组件通过**props**将数据传递给子组件 

- 父组件App.vue


```vue
<template>
  <div class="app" style="border: 3px solid #000; margin: 10px">
    我是APP组件
    <!-- 1.给组件标签，添加属性方式 赋值 
    动态属性前面加冒号-->
    <Son :title="myTitle"></Son>
  </div>
</template>

<script>
import Son from './components/Son.vue'
export default {
  name: 'App',
  data() {
    return {
      myTitle: '学前端，就来黑马程序员',
    }
  },
  components: {
    Son,
  },
}
</script>

<style>
</style>
```

- 子组件Son.vue


```vue
<template>
  <div class="son" style="border:3px solid #000;margin:10px">
    <!-- 3.直接使用props的值 -->
    我是Son组件 {{title}}
  </div>
</template>

<script>
export default {
  name: 'Son-Child',
  // 2.通过props来接受
  props:['title']
}
</script>

<style>

</style>
```

- 父向子传值步骤


1. 给子组件以添加属性的方式传值
2. 子组件内部通过props接收
3. 模板中直接使用 props接收的值

### 7.子向父通信代码示例

- 讲解过程：点击子元素的按钮后，修改title。但是因为title是从父组件获取过来， 所以也要通知父组件然后进行修改

  - 子组件

    - 绑定事件 = 函数
    - methods 写对应函数的逻辑，通过$emit （事件名，修改值）

    ~~~html
     <button @click="changeFn">修改title</button>
    ~~~

    ~~~javascript
    <script>
    export default {
      name: 'Son-Child',
      props: ['title'],
      methods: {
        changeFn() {
          // 2.通过this.$emit() 向父组件发送通知
          // this.$emit('干什么','怎么改')
          this.$emit('changTitle','传智教育')
        },
      },
    }
    </script>
    ~~~

    

  - 父组件

    - 子组件标签 事件监听 （绑定子组件的事件名） = 函数 （获取数据）

    ~~~html
    <Son :title="myTitle" @changTitle="handleChange"></Son>
    ~~~

    

    - methods 写对应函数 -- 修改data的数据

    ~~~javascript
    export default {
      name: 'App',
      data() {
        return {
          myTitle: '学前端，就来黑马程序员',
        }
      },
      components: {
        Son,
      },
      methods: {
        // 3.提供处理函数，提供逻辑
        // newTitle 获取到子元素的修改信息， 形参
        handleChange(newTitle) {
          this.myTitle = newTitle
        },
      },
    }
    ~~~

    



![image-20230929112604573](/Users/guoguo/Library/Application Support/typora-user-images/image-20230929112604573.png)

- 子组件利用 **$emit** 通知父组件，进行修改更新

- 子向父传值步骤


1. $emit触发事件，给父组件发送消息通知
2. 父组件监听$emit触发的事件
3. 提供处理函数，在函数的性参中获取传过来的参数

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230920143409948.png" alt="image-20230920143409948" style="zoom:50%;" />

- Son.vue

~~~html
<template>
  <div class="son" style="border: 3px solid #000; margin: 10px">
    <!-- 1. 点击事件，点击修改标题 -- 函数 -->
    我是Son组件 {{ title }}
    <button @click="changeFn">修改title</button>
  </div>
</template>

<script>
export default {
  name: 'Son-Child',
  props: ['title'],
  methods: {
    changeFn() {
      // 2.通过this.$emit() 向父组件发送通知
      // this.$emit('干什么','怎么改')
      this.$emit('changTitle','传智教育')
    },
  },
}
</script>

<style>
</style>
~~~

- App.vue

~~~html
<template>
  <div class="app" style="border: 3px solid #000; margin: 10px">
    我是APP组件
    <!-- 3.父组件对子组件的消息进行监听 
    @changTitle ： 要跟子元素的通知名要同名
    父组件的元素 （处理儿子的变化） - 实参
    -->
    <Son :title="myTitle" @changTitle="handleChange"></Son>
  </div>
</template>

<script>
import Son from './components/Son.vue'
export default {
  name: 'App',
  data() {
    return {
      myTitle: '学前端，就来黑马程序员',
    }
  },
  components: {
    Son,
  },
  methods: {
    // 3.提供处理函数，提供逻辑
    // newTitle 获取到子元素的修改信息， 形参
    handleChange(newTitle) {
      this.myTitle = newTitle
    },
  },
}
</script>

<style>
</style>
~~~



### 8.总结

1. 两种组件关系分类 和 对应的组件通信方案

父子关系 

→ props & $emit

非父子关系 

→ provide & inject 或 eventbus

通用方案 

→ vuex

2. 父子通信方案的核心流程

2.1 父传子props： 

① 父中给子添加属性传值 ② 子props 接收 ③ 子组件使用

2.2 子传父$emit： 

① 子$emit 发送消息 ②父中给子添加消息监听 ③ 父中实现处理函数



## 五、什么是props

### 1.Props 定义

组件上 注册的一些  自定义属性

### 2.Props 作用

向子组件传递数据

### 3.特点

1. 可以 传递 **任意数量** 的prop
2. 可以 传递 **任意类型** 的prop

![image-20230929113014036](/Users/guoguo/Library/Application Support/typora-user-images/image-20230929113014036.png)

### 4.代码演示

- 父组件App.vue


```vue
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

<style>
</style>
```

- 子组件UserInfo.vue

> 1. 可以直接在标签添逗号
>
> ~~~html
> <div>兴趣爱好：{{hobby.join('、')}}</div>
> ~~~
>
> 


```vue
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
  props:['username','age','isSingle','car','hobby']
}
</script>

<style>
.userinfo {
  width: 300px;
  border: 3px solid #000;
  padding: 20px;
}
.userinfo > div {
  margin: 20px 10px;
}
</style>
```



## 六、props校验

![image-20230929113103294](/Users/guoguo/Library/Application Support/typora-user-images/image-20230929113103294.png)

### 1.思考

组件的props可以乱传吗？

例如：需要传数字 但是 传的事布尔值 -->需要校验

### 2.作用

为组件的 prop 指定**验证要求**，不符合要求，控制台就会有**错误提示**  → 帮助开发者，快速发现错误

### 3.语法

- **类型校验**
  - 把props改成对象


<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230921095231453.png" alt="image-20230921095231453" style="zoom:50%;" />

父组件

~~~html
<div class="app">
    <BaseProgress :w="width"></BaseProgress>
~~~

子组件

~~~html
<script>
export default {
  props: {
    w: Number, //string,array,object...
  },
}
</script>
~~~



- 非空校验
- 默认值
- 自定义校验 （直接写会校验什么）

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230921095241674.png" alt="image-20230921095241674" style="zoom:50%;" />

### 4.代码演示

- App.vue


```vue
<template>
  <div class="app">
    <BaseProgress :w="width"></BaseProgress>
  </div>
</template>

<script>
import BaseProgress from './components/BaseProgress.vue'
export default {
  data() {
    return {
      width: 30,
    }
  },
  components: {
    BaseProgress,
  },
}
</script>

<style>
</style>
```

- BaseProgress.vue


```vue
<template>
  <div class="base-progress">
    <div class="inner" :style="{ width: w + '%' }">
      <span>{{ w }}%</span>
    </div>
  </div>
</template>

<script>
export default {
  // props转换成对象， 要求： 对类型给要求 Number
  props: {
    w: Number, //string,array,object...
  },
}
</script>
```



## 七、props校验完整写法

### 1.语法

```vue
props: {
  校验的属性名: {
    type: 类型,  // Number String Boolean ...
    required: true, // 是否必填
    default: 默认值, // 默认值
    validator (value) {
      // 自定义校验逻辑
      return 是否通过校验
    }
  }
},
```

### 2.代码实例

> 1. validator(val) 里面的形参val 可以拿到 props接受的值， 然后给值写条件
> 2. r          c
> 3. return true --- 校验通过
> 4. console.error 加上内容会给具体的报错内容（校验不通过）

```vue
<script>
export default {
  // 完整写法（类型、默认值、非空、自定义校验）
  props: {
    w: {
      type: Number,
      //required: true,
      default: 0,
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
```

### 3.注意

1.default和required一般不同时写（因为当时必填项时，肯定是有值的）

2.default后面如果是简单类型的值，可以直接写默认。如果是复杂类型的值，则需要以函数的形式return一个默认值

## 八、props&data、单向数据流

![image-20230929113452200](/Users/guoguo/Library/Application Support/typora-user-images/image-20230929113452200.png)



### 1.共同点

都可以给组件提供数据

### 2.区别

- data 的数据是**自己**的  →   随便改  

  ~~~html
   //可以直接修改
  <button @click="count++">+</button>
  ~~~

  

- prop 的数据是**外部**的  →   不能直接改，要遵循 **单向数据流**

  - 子组件： 写怎么修改函数然后通知到父组件
    - this.count 就是props获取的数据

  - 父组件： 监听事假 -- 修改

  ~~~javascript
  <script>
  export default {
    props: {
      count: {
        type: Number,
      },
    },
    methods: {
      handleSub() {
        this.$emit('changeCount', this.count - 1)
      },
      handleAdd() {
        this.$emit('changeCount', this.count + 1)
      },
    },
  }
  </script>
  
  
  ~~~

  

### 3.单向数据流：

父级props 的数据更新，会向下流动，影响子组件。这个数据流动是单向的

### 4.代码演示

- App.vue


```vue
<template>
  <div class="app">
    <!-- 3.添加监听事件 @changeCount -->
    <BaseCount :count="count" @changeCount="handleChange"></BaseCount>
  </div>
</template>

<script>
import BaseCount from './components/BaseCount.vue'
export default {
  components:{
    BaseCount
  },
  data(){
    return {
      count:100
    }
  },
  // 4.添加函数得到新值
  methods:{
    handleChange(newVal){
      // console.log(newVal);
      // 5.修改数据
      this.count = newVal
    }
  }
}
</script>

<style>

</style>
```

- BaseCount.vue


```vue
<template>
  <div class="base-count">
    <!-- 1.创建函数 （ 函数为了给父组件通知） -->
    <button @click="handleSub">-</button>
    <span>{{ count }}</span>
    <button @click="handleAdd">+</button>
  </div>
</template>

<script>
export default {
  // 1.自己的数据随便修改  （谁的数据 谁负责）
  // data () {
  //   return {
  //     count: 100,
  //   }
  // },
  // 2.外部传过来的数据 不能随便修改
  props: {
    count: {
      type: Number,
    },
  },
  // 2. 用子传父的$emit通知给父， 怎么改
  methods: {
    handleSub() {
      this.$emit('changeCount', this.count - 1)
    },
    handleAdd() {
      this.$emit('changeCount', this.count + 1)
    },
  },
}
</script>

<style>
.base-count {
  margin: 20px;
}
</style>
```

- 单向数据流：父级 prop 的数据更新，会向下流动，影响子组件。这个数据流动是单向的

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230921104352637.png" alt="image-20230921104352637" style="zoom:50%;" />

### 5.口诀

**谁的数据谁负责**

## * 综合案例

### 1. 组件拆分

- 需求说明

  - 拆分基础组件

  - 渲染待办任务

  - 添加任务

  -  删除任务

  -  底部合计 和 清空功能

  -  持久化存储


- 拆分基础组件

- 咱们可以把小黑记事本原有的结构拆成三部分内容：头部（TodoHeader）、列表(TodoMain)、底部(TodoFooter)


<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230921104558687.png" alt="image-20230921104558687" style="zoom:50%;" />

- 在 main.js 导入css

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230921105254438.png" alt="image-20230921105254438" style="zoom:50%;" />

1.先在 components·创建三个文件

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230921105509688.png" alt="image-20230921105509688" style="zoom:50%;" />

- 从app.vue 把每个部分的结构复制过去 （temple)
- 在app.vue进行导入每个组件

~~~html
<script>
import TodoHeader from './components/TodoHeader.vue'
import TodoMain from './components/TodoMain.vue'
import TodoFooter from './components/TodoFooter.vue'
</script>
~~~

- 注册成局部属性

~~~html
<script>
  components: {
    TodoHeader,
    TodoMain,
    TodoFooter,
  },
    
~~~

- 使用组件

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230921110204201.png" alt="image-20230921110204201" style="zoom:50%;" />

### 2 .综合案例-列表渲染

- 思路分析：


1. 提供数据：提供在公共的父组件 App.vue（因为除了main其他地方也用到这里的数据）
2. 通过父传子，将数据传递给TodoMain
3. 利用v-for进行渲染

- 4 - 给app.vue 数据

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230921111852207.png" alt="image-20230921111852207" style="zoom:50%;" />

- 5 - 从app 给 mian传数据 
- 6 - for循环渲染数据

### 3. 综合案例-添加 功能

- 思路分析：


1.收集表单数据  ==v-model==

2.监听时间 （回车+点击 都要进行添加） 

3.子传父，将任务名称传递给父组件App.vue

- header 输入后不能直接修改main的值， 要通过app

4.父组件接受到数据后 进行添加 **unshift**(自己的数据自己负责)

- 代码实现

7-  Header data 创建存储表单数据的函数 -- 在temple 用model获取数据

8- 回车事件跟点击事件：

`@keyup.enter="handleAdd"`  input绑定回车事件

`@click="handleAdd"` 按钮绑定点击事件

9- 传给父app 然后修改了再传给 子

`this.$emit('add',this.todoName)`

10- 去header 添加事件监听获取数据

`<TodoHeader @add="handleAdd"></TodoHeader>`

然后methors 函数获取子组件数据

处理收到的数据

~~~html
// 10. 函数获取子组件的函数
  methods: {
    handleAdd(todoName) {
      // 检测父组件是否拿到数据
      // console.log(todoName)
      // 10.处理获取到的数据
      this.list.unshift({
        id: +new Date(),
        name: todoName,
      })
~~~

11- 清空输入框（header）

`this.todoName = ''`

12- 没有输入内容的限制

~~~html
 if(this.todoName.trim() === ''){
        alert('任务不能为空')
      }
~~~

### 4. 综合案例-删除功能

需求： 点叉号进行删除

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230921114834668.png" alt="image-20230921114834668" style="zoom:50%;" />

- 思路分析：


1. 监听时间（监听删除的点击）携带id
2. 子传父，将删除的id传递给父组件App.vue
3. 进行删除 **filter**  (自己的数据自己负责)

- 代码实现：

13 - 绑定点击事件（删除） - 获取对应的id

`<button class="destroy" @click="handleDel(item.id)"></button>`

- 写对应的删除函数 - $emit传给父app

~~~html
 methods: {
    handleDel(id) {
      this.$emit('del', id)
    },
  },
~~~

14 - app 在 main组件绑定事件监听获取id

` <TodoMain :list="list" @del="handelDel"></TodoMain>`

- methods 获取删除id
- 在methods写删除条件

~~~html
this.list = this.list.filter((item) => item.id !== id);
    }
~~~

### 5. 综合案例-底部功能及持久化存储

- 思路分析：


1. 底部合计：父组件传递list到底部组件  —>展示合计
2. 清空功能：监听事件 —> **子组件**通知父组件 —>父组件清空
3. 持久化存储：watch监听数据变化，持久化到本地

- 代码实现

***合计功能**

15 - list数组的数据传给footer

` <TodoFooter :list="list" @clear="clear"></TodoFooter>`

16 - footer 接受数据 props

~~~html
 props: {
    list: {
      type: Array,
    },
  },
~~~

- 更新数据到span ` {{ list.length }}`

***清空功能**

17 -绑定点清空事件击事件

`<button class="clear-completed" @click="clear">清空任务</button>`

- methods 提供对应方法 - 通知父组件去改

~~~html
methods:{
    clear(){
      this.$emit('clear')
    }
  }
~~~

18 - 在app给footer 添加监听事件 - 获取数据

`<TodoFooter :list="list" @clear="clear"></TodoFooter>`

- 清楚对应方法 - 空数组

   ~~~html
   clear() {
         this.list = [];
       },
   
   ~~~

***待久化存储 **（刷新页面还是暴力当前数据）

watch深度监视list的变化 -> 往本地存储 ->进入页面优先读取本地数据

~~~html
watch: {
    list: {
      deep: true,
      handler(newVal) {
        localStorage.setItem("list", JSON.stringify(newVal));
      },
    },
~~~

### 6.代码

- App.vue

~~~html
<template>
  <!-- 主体区域 -->
  <section id="app">
    <!-- 2.使用组件 -->
    <!-- 10.添加事件监听获取子组件的数据 -->
    <TodoHeader @add="handleAdd"></TodoHeader>
    <!-- 5. :list="list" 创建事件 给 main数据 -->
    <!-- 14.事件监听获取id -->
    <TodoMain :list="list" @del="handelDel"></TodoMain>
    <!-- 18.添加事件监听- 获取数据 -->
    <TodoFooter :list="list" @clear="clear"></TodoFooter>
  </section>
</template>

<script>
// 1. 导入对应的组件
import TodoHeader from "./components/TodoHeader.vue";
import TodoMain from "./components/TodoMain.vue";
import TodoFooter from "./components/TodoFooter.vue";

export default {
  data() {
    return {
      // 4.给数组数据
      // 19. 读本地数据， json 转换成 对象 
      list: JSON.parse(localStorage.getItem("list")) || [
        { id: 1, name: "打篮球" },
        { id: 2, name: "看电影" },
        { id: 3, name: "逛街" },
      ],
    };
  },
  components: {
    TodoHeader,
    TodoMain,
    TodoFooter,
  },
  // 19.待久化存储
  watch: {
    list: {
      deep: true,
      handler(newVal) {
        localStorage.setItem("list", JSON.stringify(newVal));
      },
    },
  },
  // 10。 函数获取子组件的函数
  methods: {
    handleAdd(todoName) {
      // 检测父组件是否拿到数据
      // console.log(todoName)
      // 10.处理获取到的数据
      this.list.unshift({
        id: +new Date(),
        name: todoName,
      });
    },
    // 14. 获取删除id
    handelDel(id) {
      // console.log(id);
      this.list = this.list.filter((item) => item.id !== id);
    },
    // 18. 清楚对应方法 - 空数组
    clear() {
      this.list = [];
    },
  },
};
</script>

<style>
</style>
~~~

- TodoFooter.vue

~~~html
<template>
  <!-- 统计和清空 -->
  <footer class="footer">
    <!-- 统计 -->
    <!-- 16 - 更新数据 -->
    <span class="todo-count"
      >合 计:<strong> {{ list.length }} </strong></span
    >
    <!-- 清空 -->
    <button class="clear-completed" @click="clear">清空任务</button>
  </footer>
</template>

<script>
export default {
  // 16- 接受数据
  props: {
    list: {
      type: Array,
    },
  },
  // 17.提供对应方法 - 通知到父组件去改
  methods:{
    clear(){
      this.$emit('clear')
    }
  }
}
</script>

<style>
</style>
~~~



- TodoMain.vue

~~~html
<template>
  <!-- 列表区域 -->
  <section class="main">
    <ul class="todo-list">
      <!-- 6.for 循环渲染数据 -->
      <li class="todo" v-for="(item, index) in list" :key="item.id">
        <div class="view">
          <!-- 更改序号 -->
          <span class="index">{{ index + 1 }}.</span>
        <!-- 更改内容 -->
          <label>{{ item.name }}</label>
          <!-- 12.绑定点击删除事件 - 获取对应的id -->
          <button class="destroy" @click="handleDel(item.id)"></button>
        </div>
      </li>
    </ul>
  </section>
</template>

<script>
export default {
  // 5. props 接受app的值，写对象去校验， 限制必须是数组
  props: {
    list: {
      type: Array,
    },
  },
  // 13. 删除函数 -- 获取id -- $emit传给app
  methods: {
    // 检测是否获取到
    handleDel(id) {
      this.$emit('del', id)
    },
  },
}
</script>

<style>
</style>
~~~



- TodoHeader.vue

~~~html
<template>
  <!-- 输入框 -->
  <header class="header">
    <h1>小黑记事本</h1>
    <!-- 7. 绑定 model -->
    <!-- 8.绑定回车事件 -->
    <input
      placeholder="请输入任务"
      class="new-todo"
      v-model="todoName"
      @keyup.enter="handleAdd"
    />
    <!-- 8.绑定点击事件 -->
    <button class="add" @click="handleAdd">添加任务</button>
  </header>
</template>

<script>
export default {
  // 7.创建存储表单数据的函数
  data() {
    return {
      todoName: "",
    };
  },
  // 8.给点击跟回车事件写函数获取数据
  methods: {
    handleAdd() {
      // 8. 检测能不能拿到输入内容数据
      // console.log(this.todoName)
      // 12 - 输入框不能为空
      if (this.todoName.trim() === "") {
        alert("任务不能为空");
      }
      // 9. $emit 子传给 父 然后在父修改 $emit('事件名',要传的值)
      this.$emit("add", this.todoName);
      // 11.清空输入框
      this.todoName = "";
    },
  },
};
</script>

<style>
</style>
~~~



## 十四、非父子通信-event bus 事件总线

![image-20230929114041279](/Users/guoguo/Library/Application Support/typora-user-images/image-20230929114041279.png)

### 1.作用

非父子组件之间，进行简易消息传递。(复杂场景→ Vuex)

### 2.步骤

1. 创建一个都能访问的事件总线 （空Vue实例）

   ```js
   import Vue from 'vue'
   const Bus = new Vue()
   export default Bus
   ```

2. A组件（接受方），监听Bus的 $on事件

   ```vue
   created () {
     Bus.$on('sendMsg', (msg) => {
       this.msg = msg
     })
   }
   ```

3. B组件（发送方），触发Bus的$emit事件

   ```vue
   Bus.$emit('sendMsg', '这是一个消息')
   ```


### 3.代码示例

- EventBus.js （添加到 utils）


```js
import Vue from 'vue'
const Bus  =  new Vue()
export default Bus
```

- BaseA.vue(接受方)


```vue
<template>
  <div class="base-a">
    我是A组件（接收方）
    <p>{{msg}}</p>  
  </div>
</template>

<script>
import Bus from "../utils/EventBus";
export default {
  data() {
    return {
      msg: "",
    };
  },
  created() {
    // 2.在A组件接收方),进行监听Bus 的事件 （订阅消息）
    //Bus.$on('事件名', (msg)  => 拿到信息了要干嘛
    Bus.$on("sendMsg", (msg) => {
      // 检测有没有触发
      // console.log(msg)
      // 6.获取数据
      this.msg = msg;
    });
  },
};
</script>

<style scoped>
.base-a {
  width: 200px;
  height: 200px;
  border: 3px solid #000;
  border-radius: 3px;
  margin: 10px;
}
</style>
```

BaseB.vue(发送方)

```vue
<template>
  <div class="base-b">
    <div>我是B组件（发布方）</div>
    <!-- 4.绑定点击事件， 点击后发送信息 -->
    <button @click="sendMsgFn">发送消息</button>
  </div>
</template>

<script>
// 3.导入事件总线
import Bus from "../utils/EventBus";
export default {
  methods: {
    sendMsgFn() {
      // 5. B组件发送方出发事件方式传递对应参数（发布消息）
      // Bus.$emit('跟A的事件名', '今天天气不错，适合旅游')
      Bus.$emit("sendMsg", "今天天气不错，适合旅游");
    },
  },
};
</script>


<style scoped>
.base-b {
  width: 200px;
  height: 200px;
  border: 3px solid #000;
  border-radius: 3px;
  margin: 10px;
}
</style>
```

App.vue (在添加C组件，B发送后可以多个组件收到)

```vue
<template>
  <div class="app">
    <BaseA></BaseA>
    <BaseB></BaseB>
    <BaseC></BaseC>
  </div>
</template>

<script>
import BaseA from './components/BaseA.vue'
import BaseB from './components/BaseB.vue'
import BaseC from './components/BaseC.vue'
export default {
  components:{
    BaseA,
    BaseB,
    BaseC
  }
}
</script>

<style>

</style>
```

### 4.总结

1.非父子组件传值借助什么？

2.什么是事件总线

3.发送方应该调用事件总线的哪个方法

4.接收方应该调用事件总线的哪个方法

5.一个组件发送数据，可不可以被多个组件接收



## 十五、非父子通信-provide&inject

### 1.作用

跨层级共享数据 （祖先关系）

### 2.场景

![image-20230929114125642](/Users/guoguo/Library/Application Support/typora-user-images/image-20230929114125642.png)

### 3.语法

1. 父组件 provide提供数据

```js
export default {
  provide () {
    return {
       // 普通类型【非响应式】
       color: this.color, 
       // 复杂类型【响应式】
       userInfo: this.userInfo, 
    }
  }
}
```

2.子/孙组件 inject获取数据

```js
export default {
  inject: ['color','userInfo'],
  created () {
    console.log(this.color, this.userInfo)
  }
}
```

### 4.注意

- provide提供的简单类型的数据不是响应式的，复杂类型数据是响应式。（推荐提供复杂类型数据）
- 子/孙组件通过inject获取的数据，不能在自身组件内修改

### 5.代码演示

- 需求：从app 传给 孙子

App.vue （provide() 创建函数）

> 1.推荐使用复杂类型 （对象）

~~~html
<template>
  <div class="app">
    我是APP组件
    <!-- 需要修改数据 - 绑定点击事件 -->
    <button @click="change">修改数据</button>
    <SonA></SonA>
    <SonB></SonB>
  </div>
</template>

<script>
import SonA from "./components/SonA.vue";
import SonB from "./components/SonB.vue";
export default {
  // 1. 用provide创建函数， 写上共用的属性
  provide() {
    return {
      // 简单类型 是非响应式的 （数据改 - 视图没改）
      color: this.color,
      // 复杂类型 是响应式的 - 推荐 （数据改 - 视图改）
      userInfo: this.userInfo,
    };
  },
  data() {
    return {
      color: "pink",
      userInfo: {
        name: "zs",
        age: 18,
      },
    };
  },
  // 修改数据方法
  methods: {
    change() {
      // 修改
      this.color = "red"; //结果没改变
      this.userInfo.name = "ls"; //改变（响应式）
    },
  },
  components: {
    SonA,
    SonB,
  },
};
</script>
~~~

grandSon.vue （inject 直接接受值）

~~~html
<template>
  <div class="grandSon">
    我是GrandSon
    <!-- 3. 渲染数据 - userInfo 数数组渲染需要 userInfo.name -->
    {{ color }} -{{ userInfo.name }} -{{ userInfo.age }}
  </div>
</template>

<script>
export default {
  // 2.需要什么数据直接引入
  inject: ["color", "userInfo"],
};
</script>

<style>
.grandSon {
  border: 3px solid #000;
  border-radius: 6px;
  margin: 10px;
  height: 100px;
}
</style>
~~~

## 十六、v-model 原理

- input: 用户输入内容，input事件触发执行后面代码， 获取用户输入的值， 然后赋值给msg
  - 视图修改 - 数据修改
  - `$event = $e` e是形参的名字 （以前表单可以这样写）这里必须要写event
- **原理：**v-model本质上是一个语法糖。例如==应用在输入框==上，就是 value属性 和 input事件 的合写

~~~html
@input="msg2 = $event.target.value"
~~~

- value：数据修改 - 视图修改

![image-20230929142645713](/Users/guoguo/Library/Application Support/typora-user-images/image-20230929142645713.png)

- **作用：**提供数据的双向绑定

  ① 数据变，视图跟着变 :value 

  ② 视图变，数据跟着变 @input

- **注意：**$event 用于在模板中，获取事件的形参

~~~html
<template>
  <div class="app">
    <!-- v-model 两向： 试图改 -- 数据改， 数据改 -- 试图改 -->
    <input type="text" v-model="msg1" />
    <br />
    <!-- v-model的底层其实就是：value和 @input的简写 -->
    <!-- value="msg2" 数据改 -- 试图改 没有反向 -->
    <!-- 获取试图数据 $event.target.value  -->
    <input type="text" :value="msg2" @input="msg2 = $event.target.value" />
  </div>
</template>

<script>
export default {
  data() {
    return {
      msg1: '',
      msg2: '',
    }
  },
}
</script>

<style>
</style>
~~~



## 十八、表单组件封装 和 v-model简化代码

### 1.表单类组件 封装

- 父组件（使用）
  - 传值 `:cityId="selectId"` 并且绑定事件更新最新的值
- 子组件
  - 修改值之后传给父组件

![image-20230929143917256](/Users/guoguo/Library/Application Support/typora-user-images/image-20230929143917256.png)

- 实现 子组件 和 父组件数据 的双向绑定

① 父传子：数据 应该是父组件 props 传递 过来的，拆解 v-model 绑定数据

② 子传父：监听输入，子传父传值给父组件修改

- 代码演示：复选框选省份 -- 父元素修改

App.vue

> 1.直接拿到最新的值 $event = e.target.value

~~~html
<template>
  <div class="app">
    <!-- 2.获取id 传给子组件 :selectId="selectId" -->
    <!-- 8. 获取修改值， 要更新 selectId 直接给新值， 通过 $event 可以拿到值 -->
    <BaseSelect
      :selectId="selectId"
      @changeCity="selectId = $event"
    ></BaseSelect>
  </div>
</template>

<script>
import BaseSelect from './components/BaseSelect.vue'
export default {
  // 1。数据是app提供
  data() {
    return {
      selectId: '102',
    }
  },
  components: {
    BaseSelect,
  },
}
</script>

<style>
</style>
~~~

BaseSelect.vue

> 1. `:value="selectId"`动态属性，获取父组件传过来的值
> 2. 子传父：
>    - 绑定事件监听 获取复选框的最近新值 `@change="selectCity"`
>    - 写对应的函数 selectCity(e)， 用e形参 e.target 获取事件源，e.target.value 获取到值 value="101"
>    - target 事件属性可返回事件的目标节点（触发该事件的节点），如生成事件的元素、文档或窗口

~~~html
<template>
  <div>
    <!--4. 绑定 model获取id 但不能直接修改 props -->
    <!-- 所以要拆出来 :value="selectId"  数据改 -- 试图改-->
    <!-- 5. 改试图 - 数据改： 绑定监听事件 给父通知 @change="selectCity"（形参）-->
    <select :value="selectId" @change="selectCity">
      <option value="101">北京</option>
      <option value="102">上海</option>
      <option value="103">武汉</option>
      <option value="104">广州</option>
      <option value="105">深圳</option>
    </select>
  </div>
</template>

<script>
export default {
  //3.接受数据 通过selectId接受， 类型：对象
  props: {
    selectId: String,
  },
  // 5.对应处理函数
  methods: {
    // 6.用e拿到选中的项 e.target 拿到复选框  e.target.value 拿到值
    selectCity(e) {
      // 7。子给父
      this.$emit('changeCity', e.target.value)
    },
  },
}
</script>

<style>
</style>
~~~



### 2.父组件 v-model 简化代码

- 讲解

  - 父组件：直接用model 又获取值， 又给传值 （value 传，input收）

  ~~~html
  <BaseSelect
        v-model="selectId"
      ></BaseSelect>
  ~~~

  - 子组件：
    - props 通过 value 接收
    - 事件触发 input （获取最近的值）

  ~~~javascript
  <script>
  export default {
    // 改成value
    props: {
      value: String,
    },
    methods: {
      selectCity(e) {
        // 改成input
        this.$emit('input', e.target.value)
      },
    },
  }
  </script>
  ~~~

  

![image-20230929143928853](/Users/guoguo/Library/Application Support/typora-user-images/image-20230929143928853.png)

- v-model 简化代码，实现 子组件 和 父组件数据 双向绑定

① 子组件中：props 通过 value 接收，事件触发 input 

② 父组件中：v-model 给组件直接绑数据（:value + @input）

- App.vue

~~~html
<template>
  <div class="app">
    <!-- 1.需求需要这里直接用model -->
    <!-- v-model = :value + @input 按照这个结构改子组件-->
    <BaseSelect
      v-model="selectId"
    ></BaseSelect>
  </div>
</template>

<script>
import BaseSelect from './components/BaseSelect.vue'
export default {
  data() {
    return {
      selectId: '102',
    }
  },
  components: {
    BaseSelect,
  },
}
</script>

<style>
</style>
~~~



- BaseSelect.vue

~~~html
<template>
  <div>
    <!-- 改成value -->
    <select :value="value" @change="selectCity">
      <option value="101">北京</option>
      <option value="102">上海</option>
      <option value="103">武汉</option>
      <option value="104">广州</option>
      <option value="105">深圳</option>
    </select>
  </div>
</template>

<script>
export default {
  // 改成value
  props: {
    value: String,
  },
  methods: {
    selectCity(e) {
      // 改成input
      this.$emit('input', e.target.value)
    },
  },
}
</script>

<style>
</style>
~~~

*总结

![image-20230929151847973](/Users/guoguo/Library/Application Support/typora-user-images/image-20230929151847973.png)

## 十九、.sync修饰符

![image-20230929151904681](/Users/guoguo/Library/Application Support/typora-user-images/image-20230929151904681.png)

### 1.作用

- 可以实现 **子组件** 与 **父组件数据** 的 **双向绑定**，简化代码


- 简单理解：**子组件可以修改父组件传过来的props值 ** (v- model不能改)
- **特点：**prop属性名，可以自定义，非固定为 value

### 2.场景

封装弹框类的基础组件， visible属性 true显示 false隐藏

### 3.本质

.sync修饰符 就是 `:属性名` 和 `**@update:属性名**`合写

### 4.语法

父组件

```vue
//.sync写法 - 简写
<BaseDialog :visible.sync="isShow" />
--------------------------------------
//完整写法
<BaseDialog 
  :visible="isShow"  //传值
  @update:visible="isShow = $event"  //获取值
/>
```

子组件

```vue
props: {
  visible: Boolean //接受值
},

this.$emit('update:visible', false) //传值
```

### 5.代码示例

- 需求： 默认隐藏， 点击按钮显示

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230921155713596.png" alt="image-20230921155713596" style="zoom: 33%;" />

- 分析

  - 显示隐藏切换需要用 v-show = true （true，false不要写死）
  - 子自检接受时值接受布尔值

- App.vue

  > 1.点击退出按钮 是父组件的按钮 -- 直接绑定事件， 点击显示（true）
  >
  > 


```vue
<template>
  <div class="app">
    <!-- 5. 绑定点击事件 @click="isShow"  可以直接诶自己改值-->
    <button @click="openDialog">退出按钮</button>
    <!-- 2.传给子
    :visible (显示， 隐藏的意思)= "isShow" -->
    <!-- 6.子也需要传给父 直接加上 visible.sync -->
    <!-- isShow.sync  => :isShow="isShow" @update:isShow="isShow=$event" -->
    <BaseDialog :isShow.sync="isShow"></BaseDialog>
  </div>
</template>

<script>
import BaseDialog from './components/BaseDialog.vue'
export default {
  data() {
    return {
      // 1.在父提供数据
      isShow: false,
    }
  },
  methods: {
    openDialog() {
      this.isShow = true
      // console.log(document.querySelectorAll('.box')); 
    },
  },
  components: {
    BaseDialog,
  },
}
</script>

<style>
</style>
```

- BaseDialog.vue

> 1.点叉号关闭（false），修改值传给父组件即可


```vue
<template>
<!-- 4. 渲染  v-show="visible" -->
  <div class="base-dialog-wrap" v-show="isShow">
    <div class="base-dialog">
      <div class="title">
        <h3>温馨提示：</h3>
        <!-- 7.绑定点击事件 -->
        <button class="close" @click="closeDialog">x</button>
      </div>
      <div class="content">
        <p>你确认要退出本系统么？</p>
      </div>
      <div class="footer">
        <button>确认</button>
        <button>取消</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  // 3.接受父的值 visible: Boolean
  props: {
    isShow: Boolean,
  },
  // 7.对应语法， 事件名要跟父一样 update:visible，false
  methods:{
    closeDialog(){
      this.$emit('update:isShow',false)
    }
  }
}
</script>

<style scoped>
.base-dialog-wrap {
  width: 300px;
  height: 200px;
  box-shadow: 2px 2px 2px 2px #ccc;
  position: fixed;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  padding: 0 10px;
}
.base-dialog .title {
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-bottom: 2px solid #000;
}
.base-dialog .content {
  margin-top: 38px;
}
.base-dialog .title .close {
  width: 20px;
  height: 20px;
  cursor: pointer;
  line-height: 10px;
}
.footer {
  display: flex;
  justify-content: flex-end;
  margin-top: 26px;
}
.footer button {
  width: 80px;
  height: 40px;
}
.footer button:nth-child(1) {
  margin-right: 10px;
  cursor: pointer;
}
</style>
```



### 6.总结

1.父组件如果想让子组件修改传过去的值 必须加什么修饰符？

2.子组件要修改父组件的props值 必须使用什么语法？

3.可以用value建议用model value

## 二十、ref和$refs

- 使用场景：平时通过querySelector查找元素， 但是会查找整个页面，如果重名 -- 查找错

![image-20230929153414203](/Users/guoguo/Library/Application Support/typora-user-images/image-20230929153414203.png)

### *获取dom：

#### 1.作用

利用ref 和 $refs 可以用于 获取 dom 元素 或 组件实例

#### 2.特点

查找范围 →  当前组件内(更精确稳定)

#### 3.语法

1.给要获取的盒子添加ref属性

```html
<div ref="chartRef">我是渲染图表的容器</div>
```

2.获取时通过 $refs获取  this.\$refs.chartRef 获取

- 要写在mounted因为需要等DOM已经有了才能获取

```html
mounted () {
  console.log(this.$refs.chartRef)
}
```

#### 4.注意

之前只用document.querySelect('.box') 获取的是整个页面中的盒子

#### 5.代码示例

- App.vue


```vue
<template>
  <div class="app">
    <BaseChart></BaseChart>
  </div>
</template>

<script>
import BaseChart from './components/BaseChart.vue'
export default {
  components:{
    BaseChart
  }
}
</script>

<style>
</style>
```

- BaseChart.vue


```vue
<template>
<!-- 1.添加 ref="baseChartBox"-->
  <div class="base-chart-box" ref="baseChartBox">子组件</div>
</template>

<script>
// yarn add echarts 或者 npm i echarts
import * as echarts from 'echarts'

export default {
  mounted() {
    // 基于准备好的dom，初始化echarts实例
    var myChart = echarts.init(document.querySelect('.base-chart-box'))
    // 绘制图表
    myChart.setOption({
      title: {
        text: 'ECharts 入门示例',
      },
      tooltip: {},
      xAxis: {
        data: ['衬衫', '羊毛衫', '雪纺衫', '裤子', '高跟鞋', '袜子'],
      },
      yAxis: {},
      series: [
        {
          name: '销量',
          type: 'bar',
          data: [5, 20, 36, 10, 10, 20],
        },
      ],
    })
  },
}
</script>

<style scoped>
.base-chart-box {
  width: 400px;
  height: 300px;
  border: 3px solid #000;
  border-radius: 6px;
}
</style>
```

### *获取组件

- 父组件向用子组件里面写好的方法（methods）-- 用ref 属性

例如：已经写账号和密码的方法，通过ref 属性可以获取到用户输入的账号和密码

![image-20230929153516304](/Users/guoguo/Library/Application Support/typora-user-images/image-20230929153516304.png)

1.目标组件 – 添加 ref 属性 

~~~html
<BaseForm ref = "baseForm"></BaseForm>
~~~



2.恰当时机, 通过 this.$refs.xxx, 获取目标组件，就可以调用组件对象里面的方法

- this.$ref.baseForm ： 获取到子组件的全部方法， 所以直接 `.getFormData` 就可以获取到 子组件里面的getFormData方法

~~~javascript
methods: {
   handleGet(){
    console.log(this.$ref.baseForm.getFormData);
   }
  }
~~~



3.代码实现

需求：子已经写好 重置 跟获取数据的方法， 父获取使用

- App.vue

~~~html
<template>
<!-- 1.获取数据， 加上ref -->
  <div class="app">
    <h4>父组件 -- <button>获取组件实例</button></h4>
    <!-- 1. 创建 ref = "baseForm"-->
    <BaseForm ref = "baseForm"></BaseForm>
    <button @click = "handleGet"></button>
    <button @click = "handleReset"></button>
  </div>
</template>

<script>
import BaseForm from './components/BaseForm.vue'
export default {
  components: {
    BaseForm,
  },
  // 2.获取对应的方法
  methods: {
   handleGet(){
    console.log(this.$ref.baseForm.getFormData);
   }
  }
}
</script>

<style>
</style>
~~~

- BaseForm.vue

~~~html
 <script>
methods: {
    // 方法1： 获取表单数据， 返回一个对象
    getFormData() {
      console.log('获取表单数据', this.username, this.password);
    },
    // 方法2： 重置
    resetFormData() {
      this.username = ''
      this.password = ''
      console.log('重置表单数据成功');
    },
  }
</script>
~~~



## 二十一、异步更新 & $nextTick

![image-20230929160150581](/Users/guoguo/Library/Application Support/typora-user-images/image-20230929160150581.png)

### 1.需求

编辑标题,  编辑框自动聚焦 （只显示集中一个）

1. 点击编辑，显示编辑框
2. 让编辑框，立刻获取焦点

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230922061644463.png" alt="image-20230922061644463" style="zoom:50%;" />

问题："显示之后"，立刻获取焦点是不能成功的！

**原因：**Vue 是 异步更新 DOM (提升性能)

- 讲解：this.isShowEdit = true Dom还没获取完 所以后面写 this.$refs.inp.focus() 或报错获取不到

解决：怎么知道什么时候获取完了再去获取？---使用$nextTick

### 2.代码实现

```vue
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
      this.$nextTick((){
        this.$refs.inp.focus()
         })       
    }  },
}
</script> 
```



### 3.问题

"显示之后"，立刻获取焦点是不能成功的！

原因：Vue 是异步更新DOM  (提升性能)

### 4.解决方案

![image-20230929161128240](/Users/guoguo/Library/Application Support/typora-user-images/image-20230929161128240.png)

$nextTick：**等 DOM更新后**,才会触发执行此方法里的函数体

**语法:** this.$nextTick(函数体)

```js
this.$nextTick(() => {
  this.$refs.inp.focus() //等这个更新完了执行下面
})
```

**注意：**$nextTick 内的函数体 一定是**箭头函数**，这样才能让函数内部的this指向Vue实例













