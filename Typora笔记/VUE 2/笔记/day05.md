# day05

## 一、学习目标

### 1.自定义指令

- 基本语法（全局、局部注册）
- 指令的值
- v-loading的指令封装

### 2.插槽

- 默认插槽
- 具名插槽
- 作用域插槽

### 3.综合案例：商品列表

- MyTag组件封装
- MyTable组件封装

### 4.路由入门

- 单页应用程序
- 路由
- VueRouter的基本使用

## 二、自定义指令

![image-20230929161516240](/Users/guoguo/Library/Application Support/typora-user-images/image-20230929161516240.png)

![image-20230929113737839](/Users/guoguo/Library/Application Support/typora-user-images/image-20230929113737839.png)

- 需求：需要某个元素获取焦点，就写 dom元素.focus()  但是需要再其他页面也有这个需求 --》需要封装方法 重用

### 1.指令介绍

- 内置指令：**v-html、v-if、v-bind、v-on**... 这都是Vue给咱们内置的一些指令，可以直接使用

- 自定义指令：同时Vue也支持让开发者，自己注册一些指令。这些指令被称为**自定义指令**

  每个指令都有自己各自独立的功能

### 2.自定义指令

概念：自己定义的指令，可以**封装一些DOM操作**，扩展额外的功能

### 3.自定义指令语法

-  "inserted"：是一个钩子，指的是所绑定的元素什么时候添加到页面就自动调用（跟mounted 相同：DOM渲染完）
  - 例如：进页面自定获取焦点
  - 然后 el ： 形参可以获取元素的值
  - 使用：写在标签里面会自动调用

- 全局注册

  ```js
  //在main.js中
  Vue.directive('指令名', {
    "inserted" (el) {
      // 可以对 el 标签，扩展额外功能
      el.focus()
    }
  })
  ```
  
- 局部注册

  ```vue
  //在Vue组件的配置项中
  directives: {
    "指令名": {
      inserted () {
        // 可以对 el 标签，扩展额外功能
        el.focus()
      }
    }
  }
  ```

- 使用指令

  注意：在使用指令的时候，一定要**先注册**，**再使用**，否则会报错
  使用指令语法： v-指令名

  <img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230922064830983.png" alt="image-20230922064830983" style="zoom:50%;" />
  
  ~~~html
  <input type="text"  v-focus/>
  ~~~
  
  **注册**指令时**不用**加**v-前缀**，但**使用时**一定要**加v-前缀**

### 4.指令中的配置项介绍

inserted:被绑定元素插入父节点时调用的钩子函数

el：使用指令的那个DOM元素

### 5.代码示例

需求：当页面加载时，让元素获取焦点（**autofocus在safari浏览器有兼容性**）

App.vue

> 1. ref="inp" 可以获取DOM元素，给名字， 然后下面找到
> 2. 获取焦点也可以用mounted获取
> 3. 指令名： 自己取
> 4. 其他页面还需要获取焦点 直接写 `v-focus` （全部注册）

```vue
  <template>
  <div>
    <h1>自定义指令</h1>
    <!-- 1. 添加 ref="inp 方便找到-->
    <!-- 2.2 使用 v-focus 指令名-->
    <input v-focus ref="inp" type="text">
  </div>
</template>

<script>
export default {
  // 2. 可以用这个方法
  // mounted () {
  //   this.$refs.inp.focus()
  // }
  
  // 3. 局部注册指令 - 只能在这个组件用
  directives: {
    // 指令名：指令的配置项
    focus: {
      inserted (el) {
        el.focus()
      }
    }
  }
}
</script>

<style>

</style>
```

main.js

~~~javascript
import Vue from 'vue'
import App from './App.vue'

Vue.config.productionTip = false
// 第二个方法： 注册自定义值指令
// // 1. 全局注册指令
// Vue.directive('focus', {
//   // inserted 会在 指令所在的元素，被插入到页面中时触发
//   inserted (el) {
//     // el 就是指令所绑定的元素
//     // console.log(el);
// 2.3 获取焦点
//     el.focus()
//   }
// })
// -》可以已多次使用， 需要再哪个页面复用 直接 v-focus

new Vue({
  render: h => h(App),
}).$mount('#app')

~~~

### 6.总结

- 自定义指令的作用? 
  - 封装一些 dom 操作，扩展额外功能，例如获取焦点 

- 自定义指令的使用步骤? 

1. 注册 (全局注册 或 局部注册) 

在 inserted 钩子函数中，配置指令dom逻辑 

2. 标签上 v-指令名 使用

## 三、自定义指令-指令的值

### 1.需求

实现一个 color 指令 - 传入不同的颜色, 给标签设置文字颜色

### 2.语法

1.在绑定指令时，可以通过“等号”的形式为指令 绑定 具体的参数值

- 这里证明指令可以拿到值

```html
<div v-color="color1">我是内容</div>
<div v-color="color2">我是内容</div>

<script>
export default {
  data () {
    return {
      // 3. 给实参传值
      color1: 'red',
      color2: 'orange'
    }
```

2.通过 binding.value 可以拿到指令值，**指令值修改会 触发 update 函数**

- 拿到值之后， 用这个值赋值给元素（修改元素的颜色）
- binding.value 可以获取指令的值
- el 获取元素（显示出来）

```js
directives: {
  color: {
    //el指令所绑定的元素
    inserted (el, binding) {
      el.style.color = binding.value
    },
    update (el, binding) {
      el.style.color = binding.value
    }
  }
}
```

- update：会再指令的值修改的时候触发，dom更新的逻辑 （目前inserted只是进页面后触发），逻辑跟inserted一样

### 3.代码示例

App.vue

```vue
<template>
  <div>
    <!-- 2.使用写上指令， 两个传不同的值 -->
    <h1 v-color="color1">指令的值1测试</h1>
    <h1 v-color="color2">指令的值2测试</h1>
  </div>
</template>

<script>
export default {
  data () {
    return {
      // 3. 给实参传值
      color1: 'red',
      color2: 'orange'
    }
  },
  // 1. 局部指令
  directives: {
    color: {
      // 1. inserted 提供的是元素被添加到页面中时的逻辑
      inserted (el, binding) {
        // console.log(el, binding.value);
        // binding.value 就是指令的值， 不能直接给值
        el.style.color = binding.value
      },
      // 除了初始化（一打开页面）还有数据变化的时候也要变颜色
      // 2. update 指令的值修改的时候触发，提供值变化后，dom更新的逻辑
      update (el, binding) {
        console.log('指令的值修改了');
        el.style.color = binding.value
      }
    }
  }
}
</script>

<style>

</style>
```

*总结

![image-20230929165645793](/Users/guoguo/Library/Application Support/typora-user-images/image-20230929165645793.png)

## 四、自定义指令-v-loading指令的封装

![image-20230929165718340](/Users/guoguo/Library/Application Support/typora-user-images/image-20230929165718340.png)

![image-20230929165730216](/Users/guoguo/Library/Application Support/typora-user-images/image-20230929165730216.png)

### 1.场景

-  实际开发过程中，发送请求需要时间，在请求的数据未回来时，页面会处于**空白状态**  =>  用户体验不好
- 其他页面需要用这个指令可以封装后直接使用

### 2.需求

封装一个 v-loading 指令，实现加载中的效果

### 3.分析

1.本质 loading效果就是一个蒙层，盖在了盒子上

2.数据请求中，开启loading状态，添加蒙层

3.数据请求完毕，关闭loading状态，移除蒙层

### 4.实现

1.准备一个 loading类，通过伪元素定位，设置宽高，实现蒙层

2.开启关闭 loading状态（添加移除蒙层），本质只需要添加移除类即可

3.结合自定义指令的语法进行封装复用

```css
.loading:before {
  content: "";
  position: absolute;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  background: #fff url("./loading.gif") no-repeat center;
} 
```

### 5.准备代码

- 场景： 发请求需要等很久之后才宣传完成

- v-loading="isLoading"：如果true显示，false隐藏

  ~~~html
  setTimeout(() => {
        // 更新到 list 中，用于页面渲染 v-for
        this.list = res.data.data
        // 3. 请求完成正常显示
        this.isLoading = false
      }, 2000)
  ~~~

  

  - 数据已经变但是试图没变：

- 当请求完了，显示出来 update 

~~~html
update (el, binding) {
        binding.value ? el.classList.add('loading') : el.classList.remove('loading')
      }
~~~



- 给loading指令写逻辑

  - 一进页面 inserted： 给当前的元素 el添加类`el.classList.add('loading')`
  - 要给初始值限制条件 binding.value （默认值）：true会怎么样， false会怎么样

  ~~~html
  directives: {
      loading: {
        // inserted 只控制初始状态
        inserted (el, binding) {
          binding.value ? el.classList.add('loading') : el.classList.remove('loading')
        },
        
  ~~~

  - 怎么复用：准备好盒子之后

  ~~~html
  <div class="box2" v-loading="isLoading2"></div>
  ~~~

  ~~~javascript
  data () {
      return {
        list: [],
        // 1.2 isLoading true 显示图片， false显示正常数据
        isLoading: true,
        // 复用给默认值
        isLoading2: true
      }
  ~~~

  

```html
<template>
  <div class="main">
    <!-- 1. 创建  v-loading 指令 跟 isLoading  -->
    <div class="box" v-loading="isLoading">
      <ul>
        <li v-for="item in list" :key="item.id" class="news">
          <div class="left">
            <div class="title">{{ item.title }}</div>
            <div class="info">
              <span>{{ item.source }}</span>
              <span>{{ item.time }}</span>
            </div>
          </div>

          <div class="right">
            <img :src="item.img" alt="">
          </div>
        </li>
      </ul>
    </div>
    <!-- 复用：添加指令 v-loading="isLoading2 -->
    <div class="box2" v-loading="isLoading2"></div>
  </div>
</template>

<script>
// 安装axios =>  yarn add axios
import axios from 'axios'

// 接口地址：http://hmajax.itheima.net/api/news
// 请求方式：get
export default {
  data () {
    return {
      list: [],
      // 1.2 isLoading true 显示图片， false显示正常数据
      isLoading: true,
      // 复用给默认值
      isLoading2: true
    }
  },
  async created () {
    //  发送请求获取数据
    const res = await axios.get('http://hmajax.itheima.net/api/news')
    
    setTimeout(() => {
      // 更新到 list 中，用于页面渲染 v-for
      this.list = res.data.data
      // 3. 请求完成正常显示
      this.isLoading = false
    }, 2000)
  },
  // 2.给v-loading写逻辑
  // el.classList.add('loading') 打开页面显示loading
  // 默认值是 isLoading: true 所以要给初始值条件
  //  binding.value 初始值
  directives: {
    loading: {
      // inserted 只控制初始状态
      inserted (el, binding) {
        binding.value ? el.classList.add('loading') : el.classList.remove('loading')
      },
      update (el, binding) {
        binding.value ? el.classList.add('loading') : el.classList.remove('loading')
      }
    }
  }
}
</script>

<style>
/* 3.准备loading类 */
.loading:before {
  content: '';
  position: absolute;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  background: #fff url('./loading.gif') no-repeat center;
}

.box2 {
  width: 400px;
  height: 400px;
  border: 2px solid #000;
  position: relative;
}



.box {
  width: 800px;
  min-height: 500px;
  border: 3px solid orange;
  border-radius: 5px;
  position: relative;
}
.news {
  display: flex;
  height: 120px;
  width: 600px;
  margin: 0 auto;
  padding: 20px 0;
  cursor: pointer;
}
.news .left {
  flex: 1;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  padding-right: 10px;
}
.news .left .title {
  font-size: 20px;
}
.news .left .info {
  color: #999999;
}
.news .left .info span {
  margin-right: 20px;
}
.news .right {
  width: 160px;
  height: 120px;
}
.news .right img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}
</style>
```



## 五、插槽-默认插槽

- 有时候结构， 语法一样但是组件里面的内容不要写死 -- 可以修改 ---> 使用插槽自定义

![image-20230930094934971](/Users/guoguo/Library/Application Support/typora-user-images/image-20230930094934971.png)

### 1.作用

- 让组件内部的一些 **结构** 支持 **自定义**


![image-20230922103646132](/Users/guoguo/Library/Application Support/typora-user-images/image-20230922103646132.png)

### 2.需求

将需要多次显示的对话框,封装成一个组件

### 3.问题

组件的内容部分，**不希望写死**，希望能使用的时候**自定义**。怎么办

### 4.插槽的基本语法

![image-20230930094955892](/Users/guoguo/Library/Application Support/typora-user-images/image-20230930094955892.png)

1. 组件内需要定制的结构部分，改用**<slot></slot>**占位
2. 使用组件时, **<MyDialog></MyDialog>**标签内部, 传入结构替换slot
3. 给插槽传入内容时，可以传入**纯文本、html标签、组件**

### 5.代码示例

> 1. 用<MyDialog>组件填写需要修改内容，可以给标签，文本

- MyDialog.vue


```vue
<template>
  <div class="dialog">
    <div class="dialog-header">
      <h3>友情提示</h3>
      <span class="close">✖️</span>
    </div>

    <div class="dialog-content">
      <!-- 1. 在需要定制的位置，使用slot占位,写了之后里面的两个盒子内容消失 -->
      <slot></slot>
    </div>
    <div class="dialog-footer">
      <button>取消</button>
      <button>确认</button>
    </div>
  </div>
</template>
```

- App.vue


```vue
<template>
  <div>
    <!-- 2. 在使用组件时，组件标签内填入内容 -->
    <MyDialog>
      <div>你确认要删除么</div>
    </MyDialog>

    <MyDialog>
      <p>你确认要退出么</p>
    </MyDialog>
  </div>
</template>
```

### 6.总结

场景：组件内某一部分结构不确定，想要自定义怎么办

- 用插槽 slot 占位封装

使用：插槽的步骤分为哪几步？

1. 先在组件内用 slot 占位 
2. 使用组件时, 传入具体标签内容插入

## 六、插槽-后备内容（默认值）

-  后备内容：默认值（slot占位置了， 但是没传内容 --> 给后备内容）
- 语法：直接写在slot标签

![image-20230930095728461](/Users/guoguo/Library/Application Support/typora-user-images/image-20230930095728461.png)

![image-20230930095944043](/Users/guoguo/Library/Application Support/typora-user-images/image-20230930095944043.png)



### 1.问题 

通过插槽完成了内容的定制，传什么显示什么, 但是如果不传，则是空白

能否给插槽设置 默认显示内容 呢？

### 2.插槽的后备内容

封装组件时，可以为预留的 `<slot>` 插槽提供后备内容（默认内容）。

### 3.语法

在 <slot> 标签内，放置内容, 作为默认显示内容

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230922104952676.png" alt="image-20230922104952676" style="zoom:50%;" />

### 4.代码示例

App.vue

```vue
<template>
  <div>
    <MyDialog></MyDialog>
    <MyDialog>
      你确认要退出么
    </MyDialog>
  </div>
</template>

<script>
import MyDialog from './components/MyDialog.vue'
export default {
  data () {
    return {

    }
  },
  components: {
    MyDialog
  }
}
</script>

<style>
body {
  background-color: #b3b3b3;
}
</style>
```

*总结

![image-20230930100104419](/Users/guoguo/Library/Application Support/typora-user-images/image-20230930100104419.png)

## 七、插槽-具名插槽

- 需要修改多个内容，需要外部传标签 --> 具名插槽 （给slot起名字 方便使用）

![image-20230930100121700](/Users/guoguo/Library/Application Support/typora-user-images/image-20230930100121700.png)

### 1.需求

一个组件内有多处结构，需要外部传入标签，进行定制 

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230922105915935.png" alt="image-20230922105915935" style="zoom:50%;" />

上面的弹框中有**三处不同**，但是**默认插槽**只能**定制一个位置**，这时候怎么办呢?

### 2.具名插槽语法

![image-20230930100135792](/Users/guoguo/Library/Application Support/typora-user-images/image-20230930100135792.png)

- 多个slot使用name属性区分名字  `<template v-slot:head>` head 就是slot起的名字

- template配合v-slot:名字来分发对应标签 

### 3.v-slot的简写

![image-20230930100153981](/Users/guoguo/Library/Application Support/typora-user-images/image-20230930100153981.png)     

v-slot写起来太长，vue给我们提供一个简单写法 

`v-slot:插槽名 可以简化成 #插槽名`

### 4.代码

- MyDialog.vue

~~~html
<template>
  <div class="dialog">
    <div class="dialog-header">
      <!-- 一旦插槽起了名字，就是具名插槽，只支持定向分发 -->
      <slot name="head"></slot>
    </div>

    <div class="dialog-content">
      <!-- 加上name -->
      <slot name="content"></slot>
    </div>
    <div class="dialog-footer">
      <!-- 加上name -->
      <slot name="footer"></slot>
    </div>
  </div>
</template>
~~~

- App.vue

~~~html
<template>
  <div>
    <MyDialog>
      <!-- 需要通过template标签包裹需要分发的结构，包成一个整体 -->
      <template v-slot:head>
        <div>我是大标题</div>
      </template>
      <!-- 用template 然后用name制定 -->
      <template v-slot:content>
        <div>我是内容</div>
      </template>
    <!-- 简写 # -->
      <template #footer>
        <button>取消</button>
        <button>确认</button>
      </template>
    </MyDialog>
  </div>
</template>

~~~

### 5.总结

- 组件内 有多处不确定的结构 怎么办?

具名插槽 

1. slot占位, 给name属性起名字来区分 
2. template配合 v-slot:插槽名 分发内容

- 具名插槽的语法是什么？
- v-slot:插槽名可以简化成什么?

## 八、作用域插槽

- 不是插槽分类， 就是插槽的==传值语法== 

![image-20230930101052761](/Users/guoguo/Library/Application Support/typora-user-images/image-20230930101052761.png)

### 1.插槽分类

- 默认插槽

- 具名插槽

  插槽只有两种，作用域插槽不属于插槽的一种分类

### 2.作用

定义slot 插槽的同时, 是可以**传值**的。给 **插槽** 上可以 **绑定数据**，将来 **使用组件时可以用**

### 3.场景

封装表格组件：删除 和 查看 需要用插槽分别， 但是删除，查看都需要用内部的id进行传参 --> 用作用域传id

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230922112207449.png" alt="image-20230922112207449" style="zoom:50%;" />

### 4.使用步骤

![image-20230930101109299](/Users/guoguo/Library/Application Support/typora-user-images/image-20230930101109299.png)

1. 给 slot 标签, 以 添加属性的方式传值

   ```vue
   <slot :id="item.id" msg="测试文本"></slot>
   ```

2. 所有添加的属性, 都会被收集到一个对象中 （obj）

   ```vue
   { id: 3, msg: '测试文本' }
   ```

3. 在template中, 通过  ` #插槽名= "obj"` 接收，默认插槽名为 default （接受上面对象的数据），然后获取使用 `"del(obj.id)"`

   ```vue
   <MyTable :list="list">
     <template #default="obj">
       <button @click="del(obj.id)">删除</button>
     </template>
   </MyTable>
   ```

### 5.代码示例

> 这个案例需要解决的问题就是表格组件已经封装好， 里面有查看 删除 按钮需要 用插槽分别
>
> 但是删除 需要用id来删除，在父组件封装的删除按钮 不能直接拿对应表格内部的id
>
> 解决方案：通过slot 标签 传值 #插槽名= "obj" 接受来进行内部组件的id传给父组件
>
> ![image-20230930111635086](/Users/guoguo/Library/Application Support/typora-user-images/image-20230930111635086.png)

MyTable.vue

```vue
<template>
  <table class="my-table">
    <thead>
      <tr>
        <th>序号</th>
        <th>姓名</th>
        <th>年纪</th>
        <th>操作</th>
      </tr>
    </thead>
    <tbody>
      <!-- 3.渲染数据 -->
      <tr v-for="(item, index) in data" :key="item.id">
        <td>{{ index + 1 }}</td>
        <td>{{ item.name }}</td>
        <td>{{ item.age }}</td>
        <td>
          <!-- 5.  已经有for循环写一个就好 -->
          <!-- 8.1 给slot标签，添加属性的方式传值 :row="item" msg="测试文本" -->
          <slot :row="item" msg="测试文本"></slot>

          <!-- 8.2. 将所有的属性，添加到一个对象中 -->
          <!-- 
             {
              //row 是一行的数据
               row: { id: 2, name: '孙大明', age: 19 },
               msg: '测试文本'
             }
           -->
        </td>
      </tr>
    </tbody>
  </table>
</template>

<script>
// 2.接受父传子数据
export default {
  props: {
    data: Array
  }
}
```

App.vue

```vue
<template>
  <div>
    <!-- 父传子 -->
    <MyTable :data="list">
      <!-- 3. 通过template #插槽名="变量名" 接收 -->
      <!-- 6. 这个表格需要 删除 -->
      <!-- 7.绑定点击事件 
      @click="del(item.id) 是子的数据不能直接拿 - 需要写个方法获取数据
      -->
      <!-- 8.3 用 template 包裹 获取刚才对象， 可以随便给变量名（obj）-->
      <template #default="obj">
        <!-- 8.4 可以拿子的值 obj.row.id -->
        <button @click="del(obj.row.id)">
            <!-- {{obj}} 测试是否取到数据 -->
          删除
        </button>
      </template>
    </MyTable>
    <!-- 4.第二哥表哥的结构 -->
    <!-- 6. 这个表格需要 查看 -->
    <!-- 7.绑定点击事件 -->
    <MyTable :data="list2">
      <!-- 9.查看功能 用 template 包裹-->
      <!-- 直接给 row 进行解构 -->
      <template #default="{ row }">
        <!-- 把row 传过去 -->
        <button @click="show(row)">查看</button>
      </template>
    </MyTable>
  </div>
</template>

<script>
import MyTable from './components/MyTable.vue'
export default {
  data () {
    return {
      list: [
        { id: 1, name: '张小花', age: 18 },
        { id: 2, name: '孙大明', age: 19 },
        { id: 3, name: '刘德忠', age: 17 },
      ],
      list2: [
        { id: 1, name: '赵小云', age: 18 },
        { id: 2, name: '刘蓓蓓', age: 19 },
        { id: 3, name: '姜肖泰', age: 17 },
      ]
    }
  },
  // 7. 对应的方法获取数据
  methods: {
    del (id) {
      // console.log(id) // 报错 - 不能直接拿值 - 需要再slot添加属性
      // 8.4 写删除按钮逻辑
      this.list = this.list.filter(item => item.id !== id)
    },
    // 9. 查看 - 写对应的语法
    show (row) {
      // console.log(row); //检测是否接受到数据
      alert(`姓名：${row.name}; 年纪：${row.age}`)
    }
  },
  components: {
    MyTable
  }
}
</script>

```

### 6.总结

- 作用域插槽的作用是什么？ 

可以给插槽上绑定数据，供将来使用组件时使用 

- 作用域插槽使用步骤？ 

（1）给 slot 标签, 以 添加属性的方式传值 

（2）所有属性都会被收集到一个对象中 

（3）template中, 通过 ` #插槽名= "obj" ` 接收

## 九、综合案例 - 商品列表-MyTag组件抽离

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230922132942936.png" alt="image-20230922132942936" style="zoom:50%;" />

### 1.需求说明

1. **my-tag 标签组件封装**

​    (1) 双击显示输入框，输入框获取焦点

​    (2) 失去焦点，隐藏输入框

​    (3) 回显标签信息

​    (4) 内容修改，回车 → 修改标签信息

2. **my-table 表格组件封装**

​    (1) 动态传递表格数据渲染

​    (2) 表头支持用户自定义

​    (3) 主体支持用户自定义

~~~html
// my-tag 标签组件的封装
// 1-4 . 创建组件 - 初始化
// 2. 实现功能
//    (5) 双击显示，（6）并且自动聚焦
//        v-if v-else @dbclick 操作 isEdit
//        自动聚焦：
//        1. $nextTick => $refs 获取到dom，进行focus获取焦点
//        2. 封装v-focus指令

//    (7) 失去焦点，隐藏输入框
//        @blur 操作 isEdit 即可

//    (8) 双击后 - 回显标签信息
//        回显的标签信息是父组件传递过来的
//        v-model实现功能 (简化代码)  v-model => :value 和 @input
//        组件 内部通过props接收, :value设置给输入框

//    (9) 内容修改了，回车 => 修改标签信息
//        @keyup.enter, 触发事件 $emit('input', e.target.value)
	（10）修改完输入框隐藏
(11)内容为空给条件

// ---------------------------------------------------------------------

// 11.my-table 表格组件的封装 （先把Mytag 注释）
// 12. 数据不能写死，动态传递表格渲染的数据  props
//  结构不能写死 - 多处结构自定义 【具名插槽】
//    (13) 表头支持自定义
//    (14) 主体支持自定义
（15）换Mytag 数据
~~~

**12.1**

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230922162312846.png" alt="image-20230922162312846" style="zoom:50%;" />

### 2.代码准备

```vue
<template>
  <div class="table-case">
    <table class="my-table">
      <thead>
        <tr>
          <th>编号</th>
          <th>名称</th>
          <th>图片</th>
          <th width="100px">标签</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>1</td>
          <td>梨皮朱泥三绝清代小品壶经典款紫砂壶</td>
          <td>
            <img src="https://yanxuan-item.nosdn.127.net/f8c37ffa41ab1eb84bff499e1f6acfc7.jpg" />
          </td>
          <td>
            <div class="my-tag">
              <!-- <input 
                class="input"
                type="text"
                placeholder="输入标签"
              /> -->
              <div class="text">
                茶具
              </div>
            </div>
          </td>
        </tr>
        <tr>
          <td>1</td>
          <td>梨皮朱泥三绝清代小品壶经典款紫砂壶</td>
          <td>
            <img src="https://yanxuan-item.nosdn.127.net/221317c85274a188174352474b859d7b.jpg" />
          </td>
          <td>
            <div class="my-tag">
              <!-- <input
                ref="inp"
                class="input"
                type="text"
                placeholder="输入标签"
              /> -->
              <div class="text">
                男靴
              </div>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: 'TableCase',
  components: {},
  data() {
    return {
      goods: [
        {
          id: 101,
          picture:
            'https://yanxuan-item.nosdn.127.net/f8c37ffa41ab1eb84bff499e1f6acfc7.jpg',
          name: '梨皮朱泥三绝清代小品壶经典款紫砂壶',
          tag: '茶具',
        },
        {
          id: 102,
          picture:
            'https://yanxuan-item.nosdn.127.net/221317c85274a188174352474b859d7b.jpg',
          name: '全防水HABU旋钮牛皮户外徒步鞋山宁泰抗菌',
          tag: '男鞋',
        },
        {
          id: 103,
          picture:
            'https://yanxuan-item.nosdn.127.net/cd4b840751ef4f7505c85004f0bebcb5.png',
          name: '毛茸茸小熊出没，儿童羊羔绒背心73-90cm',
          tag: '儿童服饰',
        },
        {
          id: 104,
          picture:
            'https://yanxuan-item.nosdn.127.net/56eb25a38d7a630e76a608a9360eec6b.jpg',
          name: '基础百搭，儿童套头针织毛衣1-9岁',
          tag: '儿童服饰',
        },
      ],
    }
  },
}
</script>

<style lang="less" scoped>
.table-case {
  width: 1000px;
  margin: 50px auto;
  img {
    width: 100px;
    height: 100px;
    object-fit: contain;
    vertical-align: middle;
  }

  .my-table {
    width: 100%;
    border-spacing: 0;
    img {
      width: 100px;
      height: 100px;
      object-fit: contain;
      vertical-align: middle;
    }
    th {
      background: #f5f5f5;
      border-bottom: 2px solid #069;
    }
    td {
      border-bottom: 1px dashed #ccc;
    }
    td,
    th {
      text-align: center;
      padding: 10px;
      transition: all 0.5s;
      &.red {
        color: red;
      }
    }
    .none {
      height: 100px;
      line-height: 100px;
      color: #999;
    }
  }
  .my-tag {
    cursor: pointer;
    .input {
      appearance: none;
      outline: none;
      border: 1px solid #ccc;
      width: 100px;
      height: 40px;
      box-sizing: border-box;
      padding: 10px;
      color: #666;
      &::placeholder {
        color: #666;
      }
    }
  }
}
</style>
```

### 3.my-tag组件封装-创建组件

MyTag.vue

```vue
<template>
  <div class="my-tag">
  <!--  <input
      class="input"
      type="text"
      placeholder="输入标签" 
    /> -->
    <div  
      class="text">
       茶具
    </div>
  </div>
</template>

<script>
export default {
 
}
</script>

<style lang="less" scoped>
.my-tag {
  cursor: pointer;
  .input {
    appearance: none;
    outline: none;
    border: 1px solid #ccc;
    width: 100px;
    height: 40px;
    box-sizing: border-box;
    padding: 10px;
    color: #666;
    &::placeholder {
      color: #666;
    }
  }
}
</style>
```

App.vue

```vue
<template>
  ...
 <tbody>
       <tr>
          ....
          <td>
            <MyTag></MyTag>
          </td>
       </tr>
 </tbody>
 ...
</template>
<script>
import MyTag from './components/MyTag.vue'
export default {
  name: 'TableCase',
  components: {
    MyTag,
  },
 ....
 </script>
```



## 十、综合案例-MyTag组件控制显示隐藏

MyTag.vue

```vue
<template>
  <div class="my-tag">
    <input
      v-if="isEdit"
      v-focus
      ref="inp"
      class="input"
      type="text"
      placeholder="输入标签" 
      @blur="isEdit = false" 
    />
    <div 
      v-else
      @dblclick="handleClick"
      class="text">
       茶具
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      isEdit: false
    }
  },
  methods: {
    handleClick () {
      this.isEdit = true
    }
  }
}
</script> 
```

main.js

```js
// 封装全局指令 focus
Vue.directive('focus', {
  // 指令所在的dom元素，被插入到页面中时触发
  inserted (el) {
    el.focus()
  }
})
```



## 十一、综合案例-MyTag组件进行v-model绑定

App.vue

```vue
<MyTag v-model="tempText"></MyTag>
<script>
    export default {
        data(){
            tempText:'水杯'
        }
    }
</script>
```

MyTag.vue

```
<template>
  <div class="my-tag">
    <input
      v-if="isEdit"
      v-focus
      ref="inp"
      class="input"
      type="text"
      placeholder="输入标签"
      :value="value"
      @blur="isEdit = false"
      @keyup.enter="handleEnter"
    />
    <div 
      v-else
      @dblclick="handleClick"
      class="text">
      {{ value }}
    </div>
  </div>
</template>

<script>
export default {
  props: {
    value: String
  },
  data () {
    return {
      isEdit: false
    }
  },
  methods: {
    handleClick () {
      this.isEdit = true
    },
    handleEnter (e) {
      // 非空处理
      if (e.target.value.trim() === '') return alert('标签内容不能为空')
      this.$emit('input', e.target.value)
      // 提交完成，关闭输入状态
      this.isEdit = false
    }
  }
}
</script> 
```



## 十二、综合案例-封装MyTable组件-动态渲染数据

App.vue

```vue
<template>
  <div class="table-case">
    <MyTable :data="goods"></MyTable>
  </div>
</template>

<script>
import MyTable from './components/MyTable.vue'
export default {
  name: 'TableCase',
  components: { 
    MyTable
  },
  data(){
    return {
        ....
    }
  },
}
</script> 
```

MyTable.vue

```vue
<template>
  <table class="my-table">
    <thead>
      <tr>
        <th>编号</th>
        <th>名称</th>
        <th>图片</th>
        <th width="100px">标签</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(item, index) in data" :key="item.id">
       <td>{{ index + 1 }}</td>
        <td>{{ item.name }}</td>
        <td>
          <img
            :src="item.picture"
          />
        </td>
        <td>
          标签内容
         <!-- <MyTag v-model="item.tag"></MyTag> -->
        </td>
      </tr>
    </tbody>
  </table>
</template>

<script>
export default {
  props: {
    data: {
      type: Array,
      required: true
    }
  }
};
</script>

<style lang="less" scoped>

.my-table {
  width: 100%;
  border-spacing: 0;
  img {
    width: 100px;
    height: 100px;
    object-fit: contain;
    vertical-align: middle;
  }
  th {
    background: #f5f5f5;
    border-bottom: 2px solid #069;
  }
  td {
    border-bottom: 1px dashed #ccc;
  }
  td,
  th {
    text-align: center;
    padding: 10px;
    transition: all .5s;
    &.red {
      color: red;
    }
  }
  .none {
    height: 100px;
    line-height: 100px;
    color: #999;
  }
}

</style>
```



## 十三、综合案例-封装MyTable组件-自定义结构

App.vue

```vue
<template>
  <div class="table-case">
    <MyTable :data="goods">
      <template #head>
        <th>编号</th>
        <th>名称</th>
        <th>图片</th>
        <th width="100px">标签</th>
      </template>

      <template #body="{ item, index }">
        <td>{{ index + 1 }}</td>
        <td>{{ item.name }}</td>
        <td>
          <img
            :src="item.picture"
          />
        </td>
        <td>
          <MyTag v-model="item.tag"></MyTag>
        </td>
      </template>
    </MyTable>
  </div>
</template>

<script>
import MyTag from './components/MyTag.vue'
import MyTable from './components/MyTable.vue'
export default {
  name: 'TableCase',
  components: {
    MyTag,
    MyTable
  },
  data () {
    return {
      ....
  }
}
</script>
 
```

MyTable.vue

```vue
<template>
  <table class="my-table">
    <thead>
      <tr>
        <slot name="head"></slot>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(item, index) in data" :key="item.id">
        <slot name="body" :item="item" :index="index" ></slot>
      </tr>
    </tbody>
  </table>
</template>

<script>
export default {
  props: {
    data: {
      type: Array,
      required: true
    }
  }
};
</script>
```



## 十四、单页应用程序介绍

| 单页                                              | 多页                                     |
| :------------------------------------------------ | :--------------------------------------- |
| 1.每次切换导航页面都不会更新， 只改了导航下面部分 | 1.每次切换整个页面会重新下载（页面切换） |
| 2.移动端常用                                      | 2.需要对外的网站                         |

![image-20230930111830292](/Users/guoguo/Library/Application Support/typora-user-images/image-20230930111830292.png)

![image-20230930111848440](/Users/guoguo/Library/Application Support/typora-user-images/image-20230930111848440.png)

### 1.概念

单页应用程序：SPA【Single Page Application】是指所有的功能都在**一个html页面**上实现

### 2.具体示例

单页应用网站： 网易云音乐  <https://music.163.com/>

多页应用网站：京东  https://jd.com/

### 3.单页应用 VS 多页面应用

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230925090726344.png" alt="image-20230925090726344" style="zoom:50%;" />

单页应用类网站：系统类网站 / 内部网站 / 文档类网站 / 移动端站点

多页应用类网站：公司官网 / 电商类网站 

### 4.总结

1.什么是单页面应用程序?

所有功能在一个html页面上实现

2.单页面应用优缺点?

优点：按需更新性能高，开发效率高，用户体验好 

缺点：学习成本，首屏加载慢，不利于SEO

3.单页应用场景？

系统类网站 / 内部网站 / 文档类网站 /移动端站点

## 十五、路由介绍

### 1.思考

![image-20230930113140820](/Users/guoguo/Library/Application Support/typora-user-images/image-20230930113140820.png)

- 单页面应用程序，之所以开发效率高，性能好，用户体验好


- 最大的原因就是：**页面按需更新 ** (只显示需要的)

- 比如当点击【发现音乐】和【关注】时，**只是更新下面部分内容**，对于头部是不更新的


- 要按需更新，首先就需要明确：**访问路径**和 **组件**的对应关系


- 访问路径 和 组件的对应关系如何确定呢？ ==**路由**==

### 2.路由的介绍

- 路由器会给每个手机对应的ip 然后通过id访问 （可以分别是哪个设备）

![image-20230930113158965](/Users/guoguo/Library/Application Support/typora-user-images/image-20230930113158965.png)

- 生活中的路由：设备和ip的映射关系

- Vue中的路由：**路径和组件**的**映射**关系


![image-20230930113211891](/Users/guoguo/Library/Application Support/typora-user-images/image-20230930113211891.png)

### 3.总结

- 什么是路由：路由是一种映射关系
- Vue中的路由是什么：路径 和 组件 的映射关系，根据路由就能知道不同路径的，应该匹配渲染哪个组件

## 十六、路由的基本使用

![image-20230930113705856](/Users/guoguo/Library/Application Support/typora-user-images/image-20230930113705856.png)

### 1.目标

- 认识插件 VueRouter，掌握 VueRouter 的基本使用步骤


### 2.作用

- **修改**地址栏路径时，**切换显示**匹配的**组件**

### 3.说明

- Vue 官方的一个路由插件，是一个第三方包


### 4.官网

<https://v3.router.vuejs.org/zh/>

### 5.VueRouter的使用（5+2）

![image-20230930113718459](/Users/guoguo/Library/Application Support/typora-user-images/image-20230930113718459.png)

- 固定5个固定的步骤（不用死背，熟能生巧）
- App.vue

1.下载 VueRouter 模块到当前工程，版本3.6.5

```bash
yarn add vue-router@3.6.5
```

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230925093148815.png" alt="image-20230925093148815" style="zoom:50%;" />

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230925093243790.png" alt="image-20230925093243790" style="zoom:50%;" />

- main.js

2.main.js中引入VueRouter

```vue
import VueRouter from 'vue-router'
```

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230925093328055.png" alt="image-20230925093328055" style="zoom:50%;" />

- main.js

3.安装注册 Vue.use(Vue插件)

```vue
Vue.use(VueRouter)
```

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230925093435243.png" alt="image-20230925093435243" style="zoom:50%;" />

- main.js

4.创建路由对象

```vue
const router = new VueRouter()
```

- main.js

5.注入，将路由对象注入到new Vue实例中，建立关联

- 跟上面 const router 一样，如果是router 可以简写

```vue
new Vue({
  render: h => h(App),
  router:router
//简写
	router
}).$mount('#app')

```

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230925093736480.png" alt="image-20230925093736480" style="zoom:50%;" />

- 当我们配置完以上5步之后 就可以看到浏览器地址栏中的路由 变成了 /#/的形式。表示项目的路由已经被Vue-Router管理了
  - router没有配东西，只显示 `#`


<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230925093833584.png" alt="image-20230925093833584" style="zoom:50%;" />

### 6.代码示例

main.js

```vue
// 路由的使用步骤 5 + 2
// 5个基础步骤
// 1. 下载 v3.6.5
// yarn add vue-router@3.6.5
// 2. 引入
// 3. 安装注册 Vue.use(Vue插件)
// 4. 创建路由对象
// 5. 注入到new Vue中，建立关联


import VueRouter from 'vue-router'
Vue.use(VueRouter) // VueRouter插件初始化

const router = new VueRouter()

new Vue({
  render: h => h(App),
  router
}).$mount('#app')
```

### 7.两个核心步骤 (路由规则)

1. 创建需要的组件 (views目录)，配置路由规则

   - 在 src 创建views 然后创建对应的组件

   <img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230925100038986.png" alt="image-20230925100038986" style="zoom: 25%;" />

   - main.js
     - `path: '/find'` ：路径（ 没有点号）
     - `component: Find` ：组件，要多个单词不能只写Find，在子组件配上名字
     - routes： 多规则的意思，数组包对象的格式


   ~~~javascript
   const router = new VueRouter({
     // routes 路由规则们
     // route  一条路由规则 { path: 路径, component: 组件 }
     routes: [
       { path: '/find', component: Find },
       { path: '/my', component: My },
       { path: '/friend', component: Friend },
     ]
   })
   ~~~

   - 子组件写名字：需要在vue文件添加名字

   ~~~html
   <script>
   export default {
     name: 'FindMusic'
   }
   </script>
   ~~~

   - 对应的导入  - main.js

   ~~~html
   import Find from './views/Find'
   import My from './views/My'
   import Friend from './views/Friend'
   import VueRouter from 'vue-router'
   ~~~

   

   <img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230925095410318.png" alt="image-20230925095410318" style="zoom:50%;" />

2. 配置导航，配置路由出口(路径匹配的组件显示的位置)

   - App.vue （给对应的a标签绑定路径，记得写#）

   
   ```vue
   <!-- <router-view></router-view> 导航会在下面 -->
   <div class="footer_wrap">
     <a href="#/find">发现音乐</a>
     <a href="#/my">我的音乐</a>
     <a href="#/friend">朋友</a>
   </div>
   <div class="top">
     <!-- 路由出口 → 匹配的组件所展示的位置 
     写哪里所匹配的组件会再哪里
     -->
     <router-view></router-view>
   </div>
   ```

3.匹配的组件所展示的位置

- `<router-view></router-view>`

### 8.总结

1. 如何实现 路径改变，对应组件 切换,应该使用哪个插件?
2. Vue-Router的使用步骤是什么(5+2)?



## 十七、组件的存放目录问题

- 都是.vue文件为什么要分 views 和 /components？

![image-20230930120210553](/Users/guoguo/Library/Application Support/typora-user-images/image-20230930120210553.png)

注意： **.vue文件** 本质无区别

### 1.组件分类

![image-20230930120237715](/Users/guoguo/Library/Application Support/typora-user-images/image-20230930120237715.png)

 .vue文件分为2类，都是 **.vue文件（本质无区别）**

- 页面组件 （配置路由规则时使用的组件）-  views
- 复用组件（多个组件中都使用到的组件） - components

### 2.存放目录

分类开来的目的就是为了 **更易维护**

1. src/views文件夹

   页面组件 - 页面展示 - 配合路由用

2. src/components文件夹

   复用组件 - 展示数据 - 常用于复用
   
   - **复用组件** ，放在 components 目录 （复用组件）
   
   <img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230925102448137.png" alt="image-20230925102448137" style="zoom:50%;" />
   
   - **页面组件** 配合路由用 ，放在 views 目录  （用路由，切换页面）
   
   <img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230925102414654.png" alt="image-20230925102414654" style="zoom:50%;" />



### 3.总结

- 组件分类有哪两类？分类的目的？

页面组件 和 复用组件，便于维护

- 不同分类的组件应该放在什么文件夹？作用分别是什么？

页面组件 - views文件夹 => 配合路由，页面展示 

复用组件 - components文件夹 => 封装复用

## 十八、路由的封装抽离 （day6）

- 跟路由相关东西拆分，方在一个文件夹
- 然后导入到main.js就能使用， 输入到 new Vue

![image-20230930131546110](/Users/guoguo/Library/Application Support/typora-user-images/image-20230930131546110.png)

问题：所有的路由配置都在 中合适吗？

目标：将路由模块抽离出来。  好处：**拆分模块，利于维护**

- 新建 router文件夹 -- index 文件

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230925110653530.png" alt="image-20230925110653530" style="zoom:25%;" />

> 1. 因为 index 还没有导入Vue，需要再导入
>
> `import Vue from 'vue'`
>
> 2. 需要到处 `export default router` 然后在main.js才能导入
> 3. `'../views/Find'` 因为 Find 在index里面所以要两个点号回到上一层
> 4. 路径简写：`'@/views/Find'`
>
> **脚手架环境下** @指代src目录，可以用于快速引入组件

~~~javascript
// 1.跟路由相关的代码,要用..从当前目录回到上一级找Finds
import Find from '../views/Find'
import My from '../views/My'
import Friend from '../views/Friend'
// **可以使用@代表绝对路径，@就是src下面是views
import Find from '@/views/Find'
// 2.再导入Vue
import Vue from 'vue'
import VueRouter from 'vue-router'
Vue.use(VueRouter) // VueRouter插件初始化

// 3.创建了一个路由对象
const router = new VueRouter({
  // routes 路由规则们
  // route  一条路由规则 { path: 路径, component: 组件 }
  routes: [
    { path: '/find', component: Find },
    { path: '/my', component: My },
    { path: '/friend', component: Friend },
  ]
})
// 4.导出 router 然后去main导入
export default router
~~~

- 导入到main.js

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230925105442231.png" alt="image-20230925105442231" style="zoom:50%;" />



<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230925105519952.png" alt="image-20230925105519952" style="zoom:50%;" />

- main.js

~~~javascript
import Vue from 'vue'
import App from './App.vue'
//1.导入router 
import router from './router/index'

Vue.config.productionTip = false
//2.导入router
new Vue({
  render: h => h(App),
  router
}).$mount('#app')
~~~

![image-20230930132601419](/Users/guoguo/Library/Application Support/typora-user-images/image-20230930132601419.png)