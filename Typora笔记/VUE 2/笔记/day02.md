# day02

## 一、今日学习目标

一.指令补充

1. 指令修饰符
2. v-bind对样式增强的操作
3. v-model应用于其他表单元素

二.computed计算属性

1. 基础语法
2. 计算属性vs方法
3. 计算属性的完整写法
4. 成绩案例

三.watch侦听器

1. 基础写法
2. 完整写法

四.综合案例 （演示）

1. 渲染  /  删除  /  修改数量 / 全选 / 反选 / 统计总价 /  持久化

## 二、指令修饰符

### 1.什么是指令修饰符？

- 所谓指令修饰符就是通过“.”指明一些指令**后缀** 不同的**后缀**封装了不同的处理操作  —> 简化代码


### 2.按键修饰符

- @keyup.enter  —>当点击enter键的时候才触发

- 代码演示：


```html
<body>
  <div id="app">
    <h3>@keyup.enter  →  监听键盘回车事件</h3>
    <!-- 没有绑定 -->
    <input @keyup.enter="fn" v-model="username" type="text">
  </div>
  <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
  <script>
    const app = new Vue({
      el: '#app',
      data: {
        username: ''
      },
      methods: {
        fn (e) {
          // keyup.enter 相当于 下面的函数
          // if (e.key === 'Enter') {
          //   console.log('键盘回车的时候触发', this.username)
          // }

          console.log('键盘回车的时候触发', this.username)
        }
      }
    })
  </script>
</body>
```

### 3.v-model修饰符

- v-model.trim  —>去除首位空格
- v-model.number —>转数字

~~~html
 <!-- v-model.trim  —>去除首位空格，v-model.number —>转数字   -->
    姓名：<input v-model.trim="username" type="text"><br>
    年纪：<input v-model.number="age" type="text"><br>
~~~



### 4.事件修饰符

- @事件名.stop —> 阻止冒泡 （点击子元素如果父元素也绑定事件， 触发在父元素）

~~~html
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

~~~html
<a @click.prevent href="http://www.baidu.com">阻止默认行为</a>
~~~

- @事件名.stop.prevent —>可以连用 即阻止事件冒泡也阻止默认行为

## 三、v-bind对样式控制的增强-操作class

- 为了方便开发者进行样式控制， Vue 扩展了 v-bind 的语法，可以针对 **class 类名** 和 **style 行内样式** 进行控制 。


### 1.语法：

```html
<div> :class = "对象/数组">这是一个div</div>
```

### 2.对象语法

- 当class动态绑定的是**对象**时，**键就是类名，值就是布尔值**，如果值是**true**，就有这个类，否则没有这个类


```html
<div class="box" :class="{ 类名1: 布尔值, 类名2: 布尔值 }"></div>  
```

- 适用场景：一个类名，来回切换 （切换导航栏的高亮）

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230918154425187.png" alt="image-20230918154425187" style="zoom:50%;" />

### 3.数组语法

当class动态绑定的是**数组**时 → 数组中所有的类，都会添加到盒子上，本质就是一个 class 列表

```html
<div class="box" :class="[ 类名1, 类名2, 类名3 ]"></div>
```

-    使用场景:批量添加或删除类 （同时删除多个类）

·<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230918154516256.png" alt="image-20230918154516256" style="zoom:50%;" />

~~~html
<body>
  <div id="app">
    <div class="box" :class="{ pink: true, big: true }">黑马程序员</div>
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



## *练习：京东秒杀-tab栏切换导航高亮

### 1.需求：

​	当我们点击哪个tab页签时，哪个tab页签就高亮

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230918154658160.png" alt="image-20230918154658160" style="zoom:50%;" />

### *思路

1.基于数据，动态渲染tab（v-for）

2.准备一个下标 记录高亮的是哪一个 tab （activeIndex）

3.基于下标动态切换class的类名 （ v-bind:class）

~~~html
<body>
  <div id="app">
    <ul>
      <!-- 1. 循环渲染页面
      5. activeIndex = index 点击时把循环的下标给记录高亮
      -->
      <li v-for="(item, index) in list" :key="item.id" @click="activeIndex = index">
       <!-- 2.修改 item.name
      4. active: true  -- 都有，active: false -- 都没有，index === activeIndex 如果当前的跟记录的一样就高亮其他的没有
      -->
        <a :class="{ active: index === activeIndex }" href="#">{{ item.name }}</a>
      </li>
    </ul>
  </div>
  <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
  <script>
    const app = new Vue({
      el: '#app',
      data: {
        //3.准备记录高亮， 0 默认是第一个
        activeIndex: 2, // 记录高亮
        list: [
          { id: 1, name: '京东秒杀' },
          { id: 2, name: '每日特价' },
          { id: 3, name: '品类秒杀' }
        ]
      }
    })
  </script>
</body>
~~~



## 五、v-bind对有样式控制的增强-操作style

### 1.语法

- 控制单个属性的变化

```html
<div class="box" :style="{ CSS属性名1: CSS属性值, CSS属性名2: CSS属性值 }"></div>
```

### 2.代码练习

```html
<body>
  <div id="app">
    <!-- backgroundColor 或者 'backgroundColor'  -->
    <div class="box" :style="{ width: '400px', height: '400px', backgroundColor: 'green' }"></div>
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
```

### *练习：进度条案例

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230918160626446.png" alt="image-20230918160626446" style="zoom:50%;" />

```html
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
```



## 六、v-model在其他表单元素的使用

### 1.讲解内容：

- 常见的表单元素都可以用 v-model 绑定关联  →  快速 ==**获取** 或 **设置**== 表单元素的值


- 它会根据  **控件类型** 自动选取  **正确的方法** 来更新元素


```js
输入框  input:text   ——> value
文本域  textarea	 ——> value
复选框  input:checkbox  ——> checked
单选框  input:radio   ——> checked
下拉菜单 select    ——> value
...
```

### 2. 代码演示

```html
<body>

  <div id="app">
    <h3>小黑学习网</h3>

    姓名：
      <input type="text" v-model="username"> 
      <br><br>

    是否单身：
      <input type="checkbox" v-model="isSingle"> 
      <br><br>

    <!-- 
      前置理解：
        1. name:  给单选框加上 name 属性 可以分组 → 同一组互相会互斥 (只能选一个)
        2. value: 给单选框加上 value 属性，用于提交给后台的数据
      结合 Vue 使用 → v-model
    -->
    性别: 
      <input v-model="gender" type="radio" name="gender" value="1">男
      <input v-model="gender" type="radio" name="gender" value="2">女
      <br><br>

    <!-- 
      前置理解：
        1. option 需要设置 value 值，提交给后台
        2. select 的 value 值，关联了选中的 option 的 value 值
      结合 Vue 使用 → v-model
    -->
    所在城市:
    <!--  select 跟 option 有关联， v-model 绑定给 select -->
      <select v-model="cityId">
        <option value="101">北京</option>
        <option value="102">上海</option>
        <option value="103">成都</option>
        <option value="104">南京</option>
      </select>
      <br><br>

    自我描述：
      <textarea v-model="desc"></textarea> 

    <button>立即注册</button>
  </div>
  <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
  <script>
    const app = new Vue({
      el: '#app',
      data: {
        username: '',
        isSingle: false,
        //按照value 取值
        gender: "2",
        cityId: '102',
        desc: ""
      }
    })
  </script>
</body>
```



## 七、computed计算属性

### 1.概念

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230928095812899.png" alt="image-20230928095812899" style="zoom:50%;" />

- 基于**==现有的数据==**，计算出来的**==新属性==**。 **==依赖==**的数据变化，**自动**重新计算。


### 2.语法

1. 声明在 **computed 配置项**中，一个计算属性对应一个函数
2. 使用起来和普通属性一样使用  {{ 计算属性名}}  

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230928100727620.png" alt="image-20230928100727620" style="zoom:50%;" />

### 3.注意

1. computed配置项和data配置项是**同级**的
2. computed中的计算属性**==虽然是函数的写法==**，但他**==依然是个属性==**
3. computed中的计算属性==**不能**和data中的属性**同名**==
4. 使用computed中的计算属性和使用data中的属性是一样的用法
5. computed中计算属性内部的**this**依然**==指向的是Vue实例==**

### *练习：

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230919060206970.png" alt="image-20230919060206970" style="zoom:50%;" />

```html
body>

  <div id="app">
    <h3>小黑的礼物清单</h3>
    <table>
      <tr>
        <th>名字</th>
        <th>数量</th>
      </tr>
      <tr v-for="(item, index) in list" :key="item.id">
        <td>{{ item.name }}</td>
        <td>{{ item.num }}个</td>
      </tr>
    </table>

    <!-- 目标：统计求和，求得礼物总数 -->
    <p>礼物总数：{{ totalCount }} 个</p>
  </div>
  <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
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
          // 基于现有的数据，编写求值逻辑
          // 计算属性函数内部，可以直接通过 this 访问到 app 实例
          // console.log(this.list)

          // 需求：对 this.list 数组里面的 num 进行求和 → reduce
          let total = this.list.reduce((sum, item) => sum + item.num, 0)
          return total
        }
      }
    })
  </script>
</body>
```

> * 案例总结
>
> 1.computed可以直接通过this访问到app实例
>
> ` console.log(this.list)` //结果打印list
>
> 2.算总和用 reduce()
>
> `this.list.reduce((sum, item) => sum + item.num, 0)` 
>
> sum = 每次计算总和
>
> item.num = 当前的值

## 八、computed计算属性 VS methods方法

![image-20230928102827143](/Users/guoguo/Library/Application Support/typora-user-images/image-20230928102827143.png)

### 1.computed计算属性

- 作用（thuộc tính được tính toán）：封装了一段对于**数据**的处理，求得一个**结果**


- 语法：


1. 写在computed配置项中
2. ==作为属性==，直接使用
   - js中使用计算属性： this.计算属性
   - 模板中使用计算属性：{{计算属性}}
3. 代码演示

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230928103711010.png" alt="image-20230928103711010" style="zoom:50%;" />

~~~html
//1.重新使用多次 
<div id="app">
    <h3>小黑的礼物清单🛒<span>{{ totalCount }}</span></h3>
    <h3>小黑的礼物清单🛒<span>{{ totalCount }}</span></h3>
</div>

//2.会缓存下来，然后直接使用缓存数据，如果修改数量数据 会重新计算一次（总共执行2次）
//console.log('计算属性执行了') //只打印一次
<script>
computed: {
        // 计算属性：有缓存的，一旦计算出来结果，就会立刻缓存
        // 下一次读取 → 直接读缓存就行 → 性能特别高
         totalCount () {
           console.log('计算属性执行了') //只打印一次
           let total = this.list.reduce((sum, item) => sum + item.num, 0)
           return total
         }
      }
</script>
//3.如果用methods会执行很多次
~~~

### 2.methods计算属性

- 作用：给Vue实例提供一个**方法**，调用以**处理业务逻辑**。


- 语法：


1. 写在methods配置项中
2. ==作为方法==调用
   - js中调用：this.方法名()
   - 模板中调用 {{方法名()}}  或者 ==@事件名=“方法名”==  

### 3.计算属性的优势

1. 缓存特性（提升性能）

   - 计算属性会对计算出来的结果缓存，再次使用直接读取缓存，


   - 依赖项变化了，会自动重新计算 → 并再次缓存

2. methods没有缓存特性

3. 通过代码比较

### 4.总结

1.computed**有缓存特性**，methods**没有缓存**

2.当一个结果依赖其他多个值时，推荐使用计算属性

3.当处理业务逻辑时，推荐使用methods方法，比如事件的处理函数

~~~html
 <!-- 方法需要调用， 要加小括号 -->
    <p>礼物总数：{{ totalCountFn() }} 个</p>
 <script>
methods: {
        //方法
        totalCountFn () {
          console.log('methods方法执行了') // 会打印好多次
          let total = this.list.reduce((sum, item) => sum + item.num, 0)
          return total
        }
      }
 <script>
~~~



## 九、计算属性的完整写法

![image-20230928104155028](/Users/guoguo/Library/Application Support/typora-user-images/image-20230928104155028.png)

- **既然计算属性也是属性，能访问，应该也能修改了？**

1. 计算属性默认的简写，只能读取访问，不能 "修改"
2. 如果要 "修改"  → 需要写计算属性的完整写法

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230928105030552.png" alt="image-20230928105030552" style="zoom:50%;" />

- 写上两个属性
  - get（）： 配置逻辑 - 怎么获取数据
  - set（）：修改逻辑 - 怎么获取修改数据
- 完整写法代码演示

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230919194016369.png" alt="image-20230919194016369" style="zoom:50%;" />

```html
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
```

> * 综合案例的总结
>
> 1. JS 的加号
>
> <img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230928110212951.png" alt="image-20230928110212951" style="zoom:50%;" />
>
> `this.firstName + this.lastName = '刘' + '备' `
>
> 2. get 直接 return 返回值作为求值的结果 （求data 的 值）
> 3. set 修改 data 的值
>
> set (value) - 形参 = （this.fullName = '黄忠'）- 实参
>
> set 获取实参后 （value），然后修改 firstName 跟 lastName 的值
>
> 4. slice(0, 1)  分出来 获取 [0,1) --> 没有获取1

## 十、综合案例-成绩案例

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230919062459983.png" alt="image-20230919062459983" style="zoom:50%;" />

- 功能描述：


1.渲染功能

2.删除功能

3.添加功能

4.统计总分，求平均分

- 思路分析：


1.渲染功能  v-for  :key  v-bind:动态绑定class的样式

2.删除功能 v-on绑定事件， 阻止a标签的默认行为

3.v-model的修饰符 .trim、 .number、  判断数据是否为空后 再添加、添加后清空文本框的数据

4.使用计算属性computed 计算总分和平均分的值

#### * 渲染， 删除， 添加

~~~html
<body>
    <div id="app" class="score-case">
      <div class="table">
        <table>
          <thead>
            <tr>
              <th>编号</th>
              <th>科目</th>
              <th>成绩</th>
              <th>操作</th>
            </tr>
          </thead>
          <!-- 1. tbody只能显示一个 -->
          <tbody v-if="list.length > 0">
            <!-- 2.for 循环 拿数组数据-->
            <tr v-for="(item, index) in list" :key="item.id">
              <!-- 序号建议使用index -->
              <td>{{ index + 1 }}</td>
              <td>{{ item.subject }}</td>
              <!-- 3. 需求：不及格的标红, < 60 分, 加上 red 类 -->
              <td :class="{ red: item.score < 60 }">{{ item.score }}</td>
              <!-- 4.删除对应的id，prevent 防止跳转， 点击 获取 id -->
              <td><a @click.prevent="del(item.id)" href="http://www.baidu.com">删除</a></td>
            </tr>
          </tbody>
          <!-- 1.无数据的 tbody -->
          <tbody v-else>
            <tr>
              <td colspan="5">
                <span class="none">暂无数据</span>
              </td>
            </tr>
          </tbody>

          <tfoot>
            <tr>
              <td colspan="5">
                <span>总分：246</span>
                <span style="margin-left: 50px">平均分：79</span>
              </td>
            </tr>
          </tfoot>
        </table>
      </div>
      <div class="form">
        <div class="form-item">
          <div class="label">科目：</div>
          <div class="input">
            <!-- 5. 添加 - model获取表单数据，去掉去空格 trim-->
            <input
              type="text"
              placeholder="请输入科目"
              v-model.trim="subject"
            />
          </div>
        </div>
        <div class="form-item">
          <div class="label">分数：</div>
          <div class="input">
             <!-- 5. 添加 - model获取表单数据， 转换为数字 number-->
            <input
              type="text"
              placeholder="请输入分数"
              v-model.number="score"
            />
          </div>
        </div>
        <div class="form-item">
          <div class="label"></div>
          <div class="input">
            <!-- 6.绑定点击事件， 调用add 函数-->
            <button @click="add" class="submit" >添加</button>
          </div>
        </div>
      </div>
    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>

    <script>
      const app = new Vue({
        el: '#app',
        data: {
          list: [
            { id: 1, subject: '语文', score: 62 },
            { id: 7, subject: '数学', score: 39 },
            { id: 12, subject: '英语', score: 70 },
          ],
          subject: '',
          score: ''
        },
        // 4. 删除
        methods: {
          del (id) {
            // console.log(id) filter 跟 当前 id 不一样的id 保留， 删除当前id
            this.list = this.list.filter(item => item.id !== id)
          },
          add () {
            // 优化没有输入内容仍然添加，取反
            if (!this.subject) {
              alert('请输入科目')
              return
            }
            // 如果输入不是数字 并且没有输入
            if (typeof this.score !== 'number') {
              alert('请输入正确的成绩')
              return
            }
            // 7. unshift 添加到前面， 写对应模版
            this.list.unshift({
              id: +new Date(),
              subject: this.subject,
              score: this.score
            })
            // 添加后自动清空
            this.subject = ''
            this.score = ''
          }
        }
      })
    </script>
  </body>
~~~

#### *总分，评分

~~~HTML
<body>
    <div id="app" class="score-case">
      <div class="table">
        <table>
          <thead>
            <tr>
              <th>编号</th>
              <th>科目</th>
              <th>成绩</th>
              <th>操作</th>
            </tr>
          </thead>

          <tbody v-if="list.length > 0">
            <tr v-for="(item, index) in list" :key="item.id">
              <td>{{ index + 1 }}</td>
              <td>{{ item.subject }}</td>
              <!-- 需求：不及格的标红, < 60 分, 加上 red 类 -->
              <td :class="{ red: item.score < 60 }">{{ item.score }}</td>
              <td><a @click.prevent="del(item.id)" href="http://www.baidu.com">删除</a></td>
            </tr>
          </tbody>

          <tbody v-else>
            <tr>
              <td colspan="5">
                <span class="none">暂无数据</span>
              </td>
            </tr>
          </tbody>

          <tfoot>
            <tr>
              <td colspan="5">
                <!-- 1.改成动态结果  -->
                <span>总分：{{ totalScore }}</span>
                <span style="margin-left: 50px">平均分：{{ averageScore }}</span>
              </td>
            </tr>
          </tfoot>
        </table>
      </div>
      <div class="form">
        <div class="form-item">
          <div class="label">科目：</div>
          <div class="input">
            <input
              type="text"
              placeholder="请输入科目"
              v-model.trim="subject"
            />
          </div>
        </div>
        <div class="form-item">
          <div class="label">分数：</div>
          <div class="input">
            <input
              type="text"
              placeholder="请输入分数"
              v-model.number="score"
            />
          </div>
        </div>
        <div class="form-item">
          <div class="label"></div>
          <div class="input">
            <button @click="add" class="submit" >添加</button>
          </div>
        </div>
      </div>
    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>

    <script>
      const app = new Vue({
        el: '#app',
        data: {
          list: [
            { id: 1, subject: '语文', score: 62 },
            { id: 7, subject: '数学', score: 89 },
            { id: 12, subject: '英语', score: 70 },
          ],
          subject: '',
          score: ''
        },
        computed: {
          // 2. 用reduce 算总计 
          totalScore() {
            return this.list.reduce((sum, item) => sum + item.score, 0)
          },
          // 3.可以直接用totalScore值
          averageScore () {
            // 长度 0 直接返回0 
            if (this.list.length === 0) {
              return 0
            }
            // toFixed 保留两位小数
            return (this.totalScore / this.list.length).toFixed(2)
          }
        },
        methods: {
          del (id) {
            // console.log(id)
            this.list = this.list.filter(item => item.id !== id)
          },
          add () {
            if (!this.subject) {
              alert('请输入科目')
              return
            }
            if (typeof this.score !== 'number') {
              alert('请输入正确的成绩')
              return
            }
            this.list.unshift({
              id: +new Date(),
              subject: this.subject,
              score: this.score
            })

            this.subject = ''
            this.score = ''
          }
        }
      })
    </script>
  </body>
~~~

> *总结
>
> 1. 渲染功能（不及格高亮）
>
> v-if v-else v-for v-bind:class
>
> 2. 删除功能
>
> 点击传参 filter过滤覆盖原数组
>
> .prevent 阻止默认行为
>
> 3. 添加功能
>
> v-model v-model修饰符(.trim .number)
>
> unshift 修改数组更新视图
>
> 4. 统计总分，求平均分
>
> 计算属性 reduce求和

## 十一、watch侦听器（监视器）

![image-20230928111955345](/Users/guoguo/Library/Application Support/typora-user-images/image-20230928111955345.png)

### 1.作用：

- ​	**监视数据变化**，执行一些业务逻辑或异步操作
- 使用场景：输入内容， 实时翻译 （谷歌翻译）

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230919062646526.png" alt="image-20230919062646526" style="zoom:50%;" />

### 2.语法：

1. watch同样声明在跟data同级的配置项中
2. 简单写法： 简单类型数据直接监视

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230928113048243.png" alt="image-20230928113048243" style="zoom:50%;" />

3. 代码演示：watch语法 

> 1. newValue新值, oldValue老值（一般不用）
>
>    `words (newValue)`
>
> 2. watch  该方法会在数据变化时调用执行
>
> 3. 对象的值，方法名加引号

~~~html
 <script>    
      const app = new Vue({
        el: '#app',
        data: {
          // words: ''，对象里面的值
          obj: {
            words: ''
          }
        },
        watch: {
          //   console.log('变化了', newValue)
          'obj.words' (newValue) {
            console.log('变化了', newValue)
          }
        }
      })
    </script>
~~~

* 发请求，数据改变马上发请求

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230928121345664.png" alt="image-20230928121345664" style="zoom:50%;" />

> 1. 用 axious 发送请求
>
> - url： 请求链接
> - params：get的请求参数 （属性名 = 值（新输入的值）
>
> <img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230928121611223.png" alt="image-20230928121611223" style="zoom:50%;" />
>
> 2.获取当前请求结果 asyrc await 拿到当前翻译的结果 `res.datda.data`

* 渲染翻译结果

> 1.在data创建空属性存结果 (result)
>
> 2.渲染到div
>
> 3.把结果给result 
>
> `this.result = res.data.data`

- 问题：每次输入一个次都发一次请求 （防抖优化： 延迟执行）

> 1. await 要写在async 函数里面
> 2. 要删除旧的定时器把clearTimeout 写在前面
> 3. 在data创建一个 timer 属性获取定时器id （定时器的返回值是执行数次）
> 4. 声明 this.timer 获取id执行定时器
> 5. 也可以不要在data 创建timer 直接下面写，因为不要渲染到页面， this相当于一个对象， 然后this.timer相当于往对象去数据
>
> <img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230928125327699.png" alt="image-20230928125327699" style="zoom:50%;" />



~~~html
<script>
'obj.words' (newValue) {
            // console.log('变化了', newValue)
            // 防抖: 延迟执行 → 干啥事先等一等，延迟一会，一段时间内没有再次触发，才执行
            clearTimeout(this.timer)
            this.timer = setTimeout(async () => {
              const res = await axios({
                url: 'https://applet-base-api-t.itheima.net/api/translate',
                params: {
                  words: newValue
                }
              })
              this.result = res.data.data
              console.log(res.data.data)
            }, 300)
  
~~~



3.完整写法：添加额外配置项

![image-20230928112041789](/Users/guoguo/Library/Application Support/typora-user-images/image-20230928112041789.png)

- 需求：同时输入词或者换语言都要实时翻译，这时obj已经有两个属性需要处理

- 完整写法 → 添加额外配置项

  (1) deep: true 对复杂类型深度监视 （表示会监视所有对象的属性）

  (2) immediate: true 初始化立刻执行一次handler方法

- 代码演示

> 1. 绑定 v-model 获取复选框的值 `"obj.lang"` ， data写对应的属性
> 2. 不要重复写代码， 一次可以监视到所有对象 `deep: true`
> 3.  handler (newValue) 获取最新的值，请求 params 的 lang （要传的值）
> 4. 可以直接写 `params: newValue` 获取对象
> 5. handler 函数是修改之后才执行

- Bug： 如果给words默认值打开页面没有给翻译
  - immediate: 一进来马上执行一次handler方法 （打开页面就翻译）

~~~html
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

#### * 总结

![image-20230928131757007](/Users/guoguo/Library/Application Support/typora-user-images/image-20230928131757007.png)

### 3.侦听器代码准备

```html
 <style>
      * {
        margin: 0;
        padding: 0;
        box-sizing: border-box;
        font-size: 18px;
      }
      #app {
        padding: 10px 20px;
      }
      .query {
        margin: 10px 0;
      }
      .box {
        display: flex;
      }
      textarea {
        width: 300px;
        height: 160px;
        font-size: 18px;
        border: 1px solid #dedede;
        outline: none;
        resize: none;
        padding: 10px;
      }
      textarea:hover {
        border: 1px solid #1589f5;
      }
      .transbox {
        width: 300px;
        height: 160px;
        background-color: #f0f0f0;
        padding: 10px;
        border: none;
      }
      .tip-box {
        width: 300px;
        height: 25px;
        line-height: 25px;
        display: flex;
      }
      .tip-box span {
        flex: 1;
        text-align: center;
      }
      .query span {
        font-size: 18px;
      }

      .input-wrap {
        position: relative;
      }
      .input-wrap span {
        position: absolute;
        right: 15px;
        bottom: 15px;
        font-size: 12px;
      }
      .input-wrap i {
        font-size: 20px;
        font-style: normal;
      }
    </style>

 <div id="app">
      <!-- 条件选择框 -->
      <div class="query">
        <span>翻译成的语言：</span>
        <select>
          <option value="italy">意大利</option>
          <option value="english">英语</option>
          <option value="german">德语</option>
        </select>
      </div>

      <!-- 翻译框 -->
      <div class="box">
        <div class="input-wrap">
          <textarea v-model="words"></textarea>
          <span><i>⌨️</i>文档翻译</span>
        </div>
        <div class="output-wrap">
          <div class="transbox">mela</div>
        </div>
      </div>
    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
    <script>
      // 接口地址：https://applet-base-api-t.itheima.net/api/translate
      // 请求方式：get
      // 请求参数：
      // （1）words：需要被翻译的文本（必传）
      // （2）lang： 需要被翻译成的语言（可选）默认值-意大利
      // -----------------------------------------------
      
      const app = new Vue({
        el: '#app',
        data: {
          words: ''
        },
        // 具体讲解：(1) watch语法 (2) 具体业务实现
      })
    </script>
```



## 十二、翻译案例-代码实现

```js
  <script>
      // 接口地址：https://applet-base-api-t.itheima.net/api/translate
      // 请求方式：get
      // 请求参数：
      // （1）words：需要被翻译的文本（必传）
      // （2）lang： 需要被翻译成的语言（可选）默认值-意大利
      // -----------------------------------------------
      
      const app = new Vue({
        el: '#app',
        data: {
           //words: ''
           obj: {
            words: ''
          },
          result: '', // 翻译结果
          // timer: null // 延时器id
        },
        // 具体讲解：(1) watch语法 (2) 具体业务实现
        watch: {
          // 该方法会在数据变化时调用执行
          // newValue新值, oldValue老值（一般不用）
          // words (newValue) {
          //   console.log('变化了', newValue)
          // }

          'obj.words' (newValue) {
            // console.log('变化了', newValue)
            // 防抖: 延迟执行 → 干啥事先等一等，延迟一会，一段时间内没有再次触发，才执行
            clearTimeout(this.timer)
            this.timer = setTimeout(async () => {
              const res = await axios({
                url: 'https://applet-base-api-t.itheima.net/api/translate',
                params: {
                  words: newValue
                }
              })
              this.result = res.data.data
              console.log(res.data.data)
            }, 300)
          }
        }
      })
    </script>
```



## 十三、watch侦听器

![image-20231013101043181](/Users/guoguo/Library/Application Support/typora-user-images/image-20231013101043181.png)

### 1.语法

- 使用 [`watch` 函数](https://cn.vuejs.org/api/reactivity-core.html#watch)在每次响应式状态发生变化时触发回调函数

- 完整写法 —>添加额外的配置项

![image-20231013101101933](/Users/guoguo/Library/Application Support/typora-user-images/image-20231013101101933.png)

1. deep:true 对复杂类型进行深度监听
2. immdiate:true 初始化 立刻执行一次

```js
data: {
  obj: {
    words: '苹果',
    lang: 'italy'
  },
},

watch: {// watch 完整写法
  对象: {
    deep: true, // 深度监视
    immdiate:true,//立即执行handler函数
    handler (newValue) {
      console.log(newValue)
    }
  }
}

```

### 2.需求

- 当文本框输入的时候 右侧翻译内容要时时变化
- 当下拉框中的语言发生变化的时候 右侧翻译的内容依然要时时变化
- 如果文本框中有默认值的话要立即翻译

### 3.代码实现

```js
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
        }
      })
    </script>
```

### 4.总结

watch侦听器的写法有几种？

1.简单写法

```js
watch: {
  数据属性名 (newValue, oldValue) {
    一些业务逻辑 或 异步操作。 
  },
  '对象.属性名' (newValue, oldValue) {
    一些业务逻辑 或 异步操作。 
  }
}
```

2.完整写法

```js
watch: {// watch 完整写法
  数据属性名: {
    deep: true, // 深度监视(针对复杂类型)
    immediate: true, // 是否立刻执行一次handler
    handler (newValue) {
      console.log(newValue)
    }
  }
}
```

![image-20231013101139433](/Users/guoguo/Library/Application Support/typora-user-images/image-20231013101139433.png)

## 十四、综合案例

购物车案例

![68205100897](assets/1682051008978.png)



需求说明：

1. 渲染功能
2. 删除功能
3. 修改个数
4. 全选反选
5. 统计 选中的 总价 和 总数量 
6. 持久化到本地



实现思路：

1.基本渲染：  v-for遍历、:class动态绑定样式

2.删除功能 ： v-on 绑定事件，获取当前行的id

3.修改个数 ： v-on绑定事件，获取当前行的id，进行筛选出对应的项然后增加或减少

4.全选反选 

1. 必须所有的小选框都选中，全选按钮才选中 → every
2. 如果全选按钮选中，则所有小选框都选中
3. 如果全选取消，则所有小选框都取消选中

声明计算属性，判断数组中的每一个checked属性的值，看是否需要全部选

5.统计 选中的 总价 和 总数量 ：通过计算属性来计算**选中的**总价和总数量

6.持久化到本地： 在数据变化时都要更新下本地存储 watch









