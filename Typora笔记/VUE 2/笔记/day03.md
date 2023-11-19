# day03

## 一、今日目标

### 1.生命周期

1. 生命周期介绍
2. 生命周期的四个阶段
3. 生命周期钩子
4. 声明周期案例



### 2.综合案例-小黑记账清单

1. 列表渲染
2. 添加/删除
3. 饼图渲染



### 3.工程化开发入门

1. 工程化开发和脚手架
2. 项目运行流程
3. 组件化
4. 组件注册



### 4.综合案例-小兔仙首页

1. 拆分模块-局部注册 
2. 结构样式完善 
3. 拆分组件 – 全局注册

4.render 语法

## 二、Vue生命周期

![image-20230928150017720](/Users/guoguo/Library/Application Support/typora-user-images/image-20230928150017720.png)

- 思考：什么时候可以发送初始化渲染请求？（越早越好）什么时候可以开始操作dom？（至少dom得渲染出来）


Vue生命周期：就是一个Vue实例从创建 到 销毁 的整个过程。

生命周期四个阶段：① 创建 ② 挂载 ③ 更新 ④ 销毁

1.创建阶段：创建响应式数据 （把普通数据变成响应式数据 - 一次）

2.挂载阶段：渲染模板（已经有数据， 渲染模版 - 一次）

3.更新阶段：修改数据，更新视图 （多次）

 4.销毁 /Xiāohuǐ/ 阶段：销毁Vue实例（例如：关闭页面）

## 三、Vue生命周期钩子

![image-20230928150053237](/Users/guoguo/Library/Application Support/typora-user-images/image-20230928150053237.png)

![image-20230928152342763](/Users/guoguo/Library/Application Support/typora-user-images/image-20230928152342763.png)

![image-20230928150140778](/Users/guoguo/Library/Application Support/typora-user-images/image-20230928150140778.png)

- Vue生命周期过程中，会**自动运行一些函数**，被称为【**生命周期钩子**】→  让开发者可以在【**特定阶段**】运行**自己的代码**
- 代码演示

> 1. 更新阶段f的第三第四个阶段需要修改数据才开始，区别是试图是否显示
> 2. `document.querySelector('h3')`用来找DOM值 （结果 100 ，101 修改前后的结果）
> 3. `app.$destroy` 卸载 ： 以前写的DOM还在，只有所有事件监听跟Vue有关系用不了

```html
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
```



### *练习：生命周期钩子小案例

#### 1.在created中发送数据

- 需求：一进页面发送请求，渲染数据

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230928161348260.png" alt="image-20230928161348260" style="zoom:50%;" />

- created 数据准备好了，可以开始发送初始化渲染请求


```html
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
```



#### 2.在mounted中获取焦点

- mounted 模板渲染完成（全部页面的结构渲染完成），可以开始操作DOM了


- 需求： 进去页面立刻获取焦点 （操作DOM）


<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230919153834691.png" alt="image-20230919153834691" style="zoom:50%;" />

```html
<script>
  const app = new Vue({
    el: '#app',
    data: {
      words: ''
    },
    // 核心思路：
    // 1. 等输入框渲染出来 mounted 钩子
    // 2. 让input框获取焦点 inp.focus()
    mounted () {
      //获取输入框 inp = 输入框id
      //获取的没有model - 被解析了
      document.querySelector('#inp').focus()
    }
  })
</script>
```

## 五、案例-小黑记账清单

#### 1.需求图示

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230919154157410.png" alt="image-20230919154157410" style="zoom:50%;" />

#### 2.需求分析

1.基本渲染

2.添加功能

3.删除功能

4.饼图渲染

#### 3.思路分析

1.基本渲染  

- 立刻发送请求获取数据 created
- 拿到数据，存到data的响应式数据中
- 结合数据，进行渲染 v-for
- 消费统计  —> 计算属性

2.添加功能

- 收集表单数据 v-model，使用指令修饰符处理数据
- 给添加按钮注册点击事件，对输入的内容做非空判断，发送请求
- 请求成功后，对文本框内容进行清空
- 重新渲染列表

3.删除功能

- 注册点击事件，获取当前行的id
- 根据id发送删除请求
- 需要重新渲染

4.饼图渲染

- 初始化一个饼图 echarts.init(dom)    mounted钩子中渲染
- 根据数据试试更新饼图 echarts.setOptions({...})

#### * 渲染 和 添加

```html
<body>
    <div id="app">
      <div class="contain">
        <!-- 左侧列表 -->
        <div class="list-box">

          <!-- 添加资产 -->
          <form class="my-form">
            <!-- 6.获取表单的数据 model -->
            <input v-model.trim="name" type="text" class="form-control" placeholder="消费名称" />
            <input v-model.number="price" type="text" class="form-control" placeholder="消费价格" />
            <!-- 7.绑定点击事件 -->
            <button @click="add" type="button" class="btn btn-primary">添加账单</button>
          </form>

          <table class="table table-hover">
            <thead>
              <tr>
                <th>编号</th>
                <th>消费名称</th>
                <th>消费价格</th>
                <th>操作</th>
              </tr>
            </thead>
            <tbody>
              <!-- 4. 循环渲染 -->
              <tr v-for="(item, index) in list" :key="item.id">
                <td>{{ index + 1 }}</td>
                <td>{{ item.name }}</td>
                <!-- 保留两位小数， 大于500高亮 -->
                <td :class="{ red: item.price > 500 }">{{ item.price.toFixed(2) }}</td>
                <td><a href="javascript:;">删除</a></td>
              </tr>
            </tbody>
            <tfoot>
              <!-- 5.计算总计 -->
                <td colspan="4">消费总计： {{ totalPrice.toFixed(2) }}</td>
              </tr>
            </tfoot>
          </table>
        </div>
        
        <!-- 右侧图表 -->
        <div class="echarts-box" id="main"></div>
      </div>
    </div>
    <script src="https://cdn.jsdelivr.net/npm/echarts@5.4.0/dist/echarts.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
    <script>
      /**
       * 接口文档地址：
       * https://www.apifox.cn/apidoc/shared-24459455-ebb1-4fdc-8df8-0aff8dc317a8/api-53371058
       * 
       * 功能需求：
       * 1. 基本渲染
       *    (1) 立刻发送请求获取数据 created
       *    (2) 拿到数据，存到data的响应式数据中
       *    (3) 结合数据，进行渲染 v-for
       *    (4) 消费统计 => 计算属性
       * 2. 添加功能
       *    (1) 收集表单数据 v-model
       *    (2) 给添加按钮注册点击事件，发送添加请求
       *    (3) 需要重新渲染
       * 3. 删除功能
       * 4. 饼图渲染
       */
      const app = new Vue({
        el: '#app',
        data: {
          // 2.准备数组
          list: [],
          // 6.转呗空数组存表单数据
          name: '',
          price: ''
        },
        // 5.计算属性算总计
        computed: {
          totalPrice () {
            return this.list.reduce((sum, item) => sum + item.price, 0)
          }
        },
        created () {
          // 1. 发送请求，记得后面的属性
          // const res = await axios.get('https://applet-base-api-t.itheima.net/bill', {
          //   params: {
          //     creator: '小黑'
          //   }
          // })
          // 3.存到已经准备的数组
          // this.list = res.data.data
          // 8.2上面还要调用这个方法
          this.getList()
        },
        methods: {
          // 8. 把上面赋值下来， 可以多次复用 （写在methods
          async getList () {
            const res = await axios.get('https://applet-base-api-t.itheima.net/bill', {
              params: {
                creator: '小黑'
              }
            })
            this.list = res.data.data
          },
          // 7.2 点击添加方法（methods）
          async add () {
            // 9. 修复 没有输入内容
            if (!this.name) {
              alert('请输入消费名称')
              return
            }
            if (typeof this.price !== 'number') {
              alert('请输入正确的消费价格')
              return
            }

            // 7.3 发送添加请求， 然后写上参数 get 请求需要额外写params， post 不要
            const res = await axios.post('https://applet-base-api-t.itheima.net/bill', {
              creator: '小黑',
              name: this.name,
              price: this.price
            })
            // 8.3 重新渲染一次 （复用再次渲染）
            this.getList()
            // 9. 添加后清空
              this.name = ''
            this.price = ''
          }
        }
      })
    </script>
  </body>
```

### *删除功能

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230920065539166.png" alt="image-20230920065539166" style="zoom:50%;" />

~~~html
 <body>
    <div id="app">
      <div class="contain">
        <!-- 左侧列表 -->
        <div class="list-box">

          <!-- 添加资产 -->
          <form class="my-form">
            <input v-model.trim="name" type="text" class="form-control" placeholder="消费名称" />
            <input v-model.number="price" type="text" class="form-control" placeholder="消费价格" />
            <button @click="add" type="button" class="btn btn-primary">添加账单</button>
          </form>

          <table class="table table-hover">
            <thead>
              <tr>
                <th>编号</th>
                <th>消费名称</th>
                <th>消费价格</th>
                <th>操作</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(item, index) in list" :key="item.id">
                <td>{{ index + 1 }}</td>
                <td>{{ item.name }}</td>
                <td :class="{ red: item.price > 500 }">{{ item.price.toFixed(2) }}</td>
                <!-- 1.绑定点击事件，获取id -->
                <td><a @click="del(item.id)" href="javascript:;">删除</a></td>
              </tr>
            </tbody>
            <tfoot>
              <tr>
                <td colspan="4">消费总计： {{ totalPrice.toFixed(2) }}</td>
              </tr>
            </tfoot>
          </table>
        </div>
        
        <!-- 右侧图表 -->
        <div class="echarts-box" id="main"></div>
      </div>
    </div>
    <script src="https://cdn.jsdelivr.net/npm/echarts@5.4.0/dist/echarts.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
    <script>
      /**
       * 接口文档地址：
       * https://www.apifox.cn/apidoc/shared-24459455-ebb1-4fdc-8df8-0aff8dc317a8/api-53371058
       * 
       * 功能需求：
       * 1. 基本渲染
       *    (1) 立刻发送请求获取数据 created
       *    (2) 拿到数据，存到data的响应式数据中
       *    (3) 结合数据，进行渲染 v-for
       *    (4) 消费统计 => 计算属性
       * 2. 添加功能
       *    (1) 收集表单数据 v-model
       *    (2) 给添加按钮注册点击事件，发送添加请求
       *    (3) 需要重新渲染
       * 3. 删除功能
       *    (1) 注册点击事件，传参传 id
       *    (2) 根据 id 发送删除请求
       *    (3) 需要重新渲染
       * 4. 饼图渲染
       */
      const app = new Vue({
        el: '#app',
        data: {
          list: [],
          name: '',
          price: ''
        },
        computed: {
          totalPrice () {
            return this.list.reduce((sum, item) => sum + item.price, 0)
          }
        },
        created () {
          // const res = await axios.get('https://applet-base-api-t.itheima.net/bill', {
          //   params: {
          //     creator: '小黑'
          //   }
          // })
          // this.list = res.data.data

          this.getList()
        },
        methods: {
          async getList () {
            const res = await axios.get('https://applet-base-api-t.itheima.net/bill', {
              params: {
                creator: '小黑'
              }
            })
            this.list = res.data.data
          },
          async add () {
            if (!this.name) {
              alert('请输入消费名称')
              return
            }
            if (typeof this.price !== 'number') {
              alert('请输入正确的消费价格')
              return
            }

            // 发送添加请求
            const res = await axios.post('https://applet-base-api-t.itheima.net/bill', {
              creator: '小黑',
              name: this.name,
              price: this.price
            })
            // 重新渲染一次
            this.getList()

            this.name = ''
            this.price = ''
          },
          // 2. 删除函数
          async del (id) {
            // 2.1 根据 id 发送删除请求， 传id 给后台 写后面
            const res = await axios.delete(`https://applet-base-api-t.itheima.net/bill/${id}`)
            // 2.2 重新渲染
            this.getList()
          }
        }
      })
    </script>
  </body>
~~~

#### * 饼图渲染

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230920065758552.png" alt="image-20230920065758552" style="zoom: 25%;" />

- 怎么更新数据

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230920070808424.png" alt="image-20230920070808424" style="zoom: 25%;" />

~~~html
<body>
    <div id="app">
      <div class="contain">
        <!-- 左侧列表 -->
        <div class="list-box">

          <!-- 添加资产 -->
          <form class="my-form">
            <input v-model.trim="name" type="text" class="form-control" placeholder="消费名称" />
            <input v-model.number="price" type="text" class="form-control" placeholder="消费价格" />
            <button @click="add" type="button" class="btn btn-primary">添加账单</button>
          </form>

          <table class="table table-hover">
            <thead>
              <tr>
                <th>编号</th>
                <th>消费名称</th>
                <th>消费价格</th>
                <th>操作</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(item, index) in list" :key="item.id">
                <td>{{ index + 1 }}</td>
                <td>{{ item.name }}</td>
                <td :class="{ red: item.price > 500 }">{{ item.price.toFixed(2) }}</td>
                <td><a @click="del(item.id)" href="javascript:;">删除</a></td>
              </tr>
            </tbody>
            <tfoot>
              <tr>
                <td colspan="4">消费总计： {{ totalPrice.toFixed(2) }}</td>
              </tr>
            </tfoot>
          </table>
        </div>
        
        <!-- 右侧图表 -->
        <div class="echarts-box" id="main"></div>
      </div>
    </div>
    <script src="https://cdn.jsdelivr.net/npm/echarts@5.4.0/dist/echarts.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
    <script>
      /**
       * 接口文档地址：
       * https://www.apifox.cn/apidoc/shared-24459455-ebb1-4fdc-8df8-0aff8dc317a8/api-53371058
       * 
       * 功能需求：
       * 1. 基本渲染
       *    (1) 立刻发送请求获取数据 created
       *    (2) 拿到数据，存到data的响应式数据中
       *    (3) 结合数据，进行渲染 v-for
       *    (4) 消费统计 => 计算属性
       * 2. 添加功能
       *    (1) 收集表单数据 v-model
       *    (2) 给添加按钮注册点击事件，发送添加请求
       *    (3) 需要重新渲染
       * 3. 删除功能
       *    (1) 注册点击事件，传参传 id
       *    (2) 根据 id 发送删除请求
       *    (3) 需要重新渲染
       * 4. 饼图渲染
       *    (1) 初始化一个饼图 echarts.init(dom)  mounted钩子实现
       *    (2) 根据数据实时更新饼图 echarts.setOption({ ... })
       */
      const app = new Vue({
        el: '#app',
        data: {
          list: [],
          name: '',
          price: ''
        },
        computed: {
          totalPrice () {
            return this.list.reduce((sum, item) => sum + item.price, 0)
          }
        },
        created () {
          // const res = await axios.get('https://applet-base-api-t.itheima.net/bill', {
          //   params: {
          //     creator: '小黑'
          //   }
          // })
          // this.list = res.data.data

          this.getList()
        },
        mounted () {
          // 1. 创建饼图
          // let myChart = echarts.init(document.querySelector('#main'))
          // myChart.setOption，找需要的饼图 把样式复制过来
          // 3. 换成this 下面（2） 可以用
          this.myChart = echarts.init(document.querySelector('#main'))
          this.myChart.setOption({
            // 大标题
            title: {
              text: '消费账单列表',
              left: 'center'
            },
            // 提示框
            tooltip: {
              trigger: 'item'
            },
            // 图例
            legend: {
              orient: 'vertical',
              left: 'left'
            },
            // 数据项
            series: [
              {
                name: '消费账单',
                type: 'pie',
                radius: '50%', // 半径
                // 给空数组等数据
                data: [
                  // { value: 1048, name: '球鞋' },
                  // { value: 735, name: '防晒霜' }
                ],
                emphasis: {
                  itemStyle: {
                    shadowBlur: 10,
                    shadowOffsetX: 0,
                    shadowColor: 'rgba(0, 0, 0, 0.5)'
                  }
                }
              }
            ]
          })
        },

        methods: {
          async getList () {
            const res = await axios.get('https://applet-base-api-t.itheima.net/bill', {
              params: {
                creator: '小黑'
              }
            })
            this.list = res.data.data

            // 2. 更新图表， 用上面的 加this
            this.myChart.setOption({
              // 数据项， 要更新这个数据
              series: [
                {
                  // data: [
                  //   { value: 1048, name: '球鞋' },
                  //   { value: 735, name: '防晒霜' }
                  // ]
                  // 5. 获取新数组，value 跟name， 记得加括号
                  data: this.list.map(item => ({ value: item.price, name: item.name}))
                }
              ]
            })
          },
          async add () {
            if (!this.name) {
              alert('请输入消费名称')
              return
            }
            if (typeof this.price !== 'number') {
              alert('请输入正确的消费价格')
              return
            }

            // 发送添加请求
            const res = await axios.post('https://applet-base-api-t.itheima.net/bill', {
              creator: '小黑',
              name: this.name,
              price: this.price
            })
            // 重新渲染一次
            this.getList()

            this.name = ''
            this.price = ''
          },
          async del (id) {
            // 根据 id 发送删除请求
            const res = await axios.delete(`https://applet-base-api-t.itheima.net/bill/${id}`)
            // 重新渲染
            this.getList()
          }
        }
      })
    </script>
  </body>
~~~

> <img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230920071734144.png" alt="image-20230920071734144" style="zoom:50%;" />

## 六、工程化开发和脚手架

### 1.开发Vue的两种方式

- 核心包传统开发模式：基于html / css / js 文件，直接引入核心包，开发 Vue。
- **工程化开发模式：基于构建工具（例如：webpack）的环境中开发Vue。**

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230920092558411.png" alt="image-20230920092558411" style="zoom:50%;" />

- 工程化开发模式优点：


   提高编码效率，比如使用JS新语法、Less/Sass、Typescript等通过webpack都可以编译成浏览器识别的ES3/ES5/CSS等

- 工程化开发模式问题：

  - webpack配置**不简单**

  - **雷同**的基础配置

  - 缺乏**统一的标准**


为了解决以上问题，所以我们需要一个工具，生成标准化的配置

### 2. 脚手架Vue CLI

- ####   基本介绍：


   Vue CLI 是Vue官方提供的一个**全局命令工具**

   可以帮助我们**快速创建**一个开发Vue项目的**标准化基础架子**。【集成了webpack配置】

- ####    好处：


1. 开箱即用，零配置
2. 内置babel等工具
3. 标准化的webpack配置

- ####    使用步骤：


1. 全局安装（只需安装一次即可） yarn global add @vue/cli 或者 npm i @vue/cli -g
2. 查看vue/cli版本： vue --version
3. 创建项目架子：**vue create project-name**(项目名不能使用中文)
4. 启动项目：**yarn serve** 或者 **npm run serve**(命令不固定，找package.json)
5. serve 有可能会修改 `yarn div`

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230928163618412.png" alt="image-20230928163618412" style="zoom:50%;" />

## 七、项目目录介绍和运行流程

![image-20230928203500995](/Users/guoguo/Library/Application Support/typora-user-images/image-20230928203500995.png)

### 1.项目目录介绍

- 虽然脚手架中的文件有很多，目前咱们只需人事三个文件即可


**1.main.js  入口文件**

- 核心作用：导入App. vue，基于App. vue创建结构渲染 index.html

~~~javascript
// 文件核心作用：导入App.vue，基于App.vue创建结构渲染index.html
// 1. 导入 Vue 核心包
import Vue from 'vue'

// 2. 导入 App.vue 根组件
import App from './App.vue'

// 提示：当前处于什么环境 (生产环境 / 开发环境)
//true 控制台会显示哪个环境
Vue.config.productionTip = false

// 3. Vue实例化，提供render方法 → 基于App.vue创建结构渲染index.html
new Vue({
  // el: '#app', 作用：和$mount('选择器')作用一致，用于指定Vue所管理容器
  // render: h => h(App),
  render: (createElement) => {
    // 基于App创建元素结构，获取创建完的结果 return
    return createElement(App)
  }
}).$mount('#app')
~~~

> 1. 以前的 `el: '#app'` 跟 `.$mount('#app')` 一样
> 2. `render: h => h(App)` ：基于App.vue创建结构渲染index.html
>
> - 完整写法：
>
> ~~~javascript
> //createElemen 形参
> render: (createElement) => {
>     // 基于App创建元素结构
>     return createElement(App)
> //把createElemen 形参 改成 h 然后写箭头函数
> render: h => h(App)
> ~~~
>
> 3. 创建结构渲染然后渲染'#app'的盒子

**2.App.vue  App根组件** 

~~~html
<template>
  <div class="App">
    <div class="box" @click="fn"></div>
  </div>
</template>

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

<style lang="less">
/* 让style支持less
   1. 给style加上 lang="less"
   2. 安装依赖包 less less-loader
      yarn add less less-loader -D (开发依赖)
*/
.App {
  width: 400px;
  height: 400px;
  background-color: pink;
  .box {
    width: 100px;
    height: 100px;
    background-color: skyblue;
  }
}
</style>
~~~



**3. index.html 模板文件**  （以前）

1 - 兼容： 给不支持js的浏览器一个提示

2 - Vue所管理的容器：将来创建结构动态渲染这个容器

3 - 工程化开发模式中： 这里不再直接编写模版语法，通过App.vue 来提供结构渲染

~~~html
<!DOCTYPE html>
<html lang="">
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width,initial-scale=1.0">
    <link rel="icon" href="<%= BASE_URL %>favicon.ico">
    <title><%= htmlWebpackPlugin.options.title %></title>
  </head>
  <body>
    <!-- 兼容：给不支持js的浏览器一个提示 -->
    <noscript>
      <strong>We're sorry but <%= htmlWebpackPlugin.options.title %> doesn't work properly without JavaScript enabled. Please enable it to continue.</strong>
    </noscript>

    <!-- Vue所管理的容器：将来创建结构动态渲染这个容器 -->
    <div id="app">
      <!-- 工程化开发模式中：这里不再直接编写模板语法，通过 App.vue 提供结构渲染 -->
    </div>

    <!-- built files will be auto injected -->
  </body>
</html>

~~~



### 2.运行流程

![image-20230928203534957](/Users/guoguo/Library/Application Support/typora-user-images/image-20230928203534957.png)

## 八、组件化开发

![image-20230928210209609](/Users/guoguo/Library/Application Support/typora-user-images/image-20230928210209609.png)

- 组件化：一个页面可以拆分成一个个组件，每个组件有着自己独立的结构、样式、行为。
  - 好处：
    - 便于维护，利于复用 → 提升开发效率。
    -  组件分类：普通组件 （里面的小组件）、根组件（这个一大块包括全部小组件）

- 比如：下面这个页面，可以把所有的代码都写在一个页面中，但是这样显得代码比较混乱，难易维护。咱们可以按模块进行组件划分

## 九、根组件 App.vue

### 1.根组件介绍

- 整个应用最上层的组件，包裹所有普通小组件


<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230920101150926.png" alt="image-20230920101150926" style="zoom:50%;" />

### 2.组件是由三部分构成

- 语法高亮插件

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230920101312521.png" alt="image-20230920101312521" style="zoom:50%;" />

- 三部分构成

  - template：结构 （有且只能一个根元素）
  - script:   js逻辑 
  -  style： 样式 (可支持less，需要装包)

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230920101345912.png" alt="image-20230920101345912" style="zoom:50%;" />

- export default： 导出当前组件的配置项，里面可以提供methods data 

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230929093855360.png" alt="image-20230929093855360" style="zoom:50%;" />

- 让组件支持less

  （1） style标签，lang="less" 开启less功能  （css 学过 嵌套盒子）

  （2） 装包: yarn add less less-loade  r -D 或者npm i less less-loader -D

> - VUE 2 temple 只能有一个div包其他div
>

### 3.总结

**(1) 组件化：**

页面可拆分成一个个组件，每个组件有着独立的结构、样式、行为

① 好处：便于维护，利于复用 → 提升开发效率。 

② 组件分类：普通组件、根组件。

**(2) 根组件：**

整个应用最上层的组件，包裹所有普通小组件。

一个根组件App.vue，包含的三个部分： 

① template 结构 (只能有一个根节点) 

② style 样式 (可以支持less，需要装包 less 和 less-loader )

③ script 行为

## 十、普通组件的注册使用-局部注册

![image-20230929094320946](/Users/guoguo/Library/Application Support/typora-user-images/image-20230929094320946.png)

### 1.特点：

- 只能在注册的组件内使用 （像局部变量）

### 2.步骤：

![image-20230929094606010](/Users/guoguo/Library/Application Support/typora-user-images/image-20230929094606010.png)

- import （组件对象） from './components/HmHeader'
- 组件名跟组件对象取同一个名字

1.创建.vue文件（三个组成部分）

2.在使用的组件内先导入再注册，最后使用

### 3.使用方式：

- 当成html标签使用即可  `<组件名></组件名>`


### 4.注意：

- 组件名规范 —> 大驼峰命名法， 如 HmHeader

- 一般都用局部注册，如果发现确实是通用组件，再定义到全局

### 5.语法：

```js
// 导入需要注册的组件
import 组件对象 from '.vue文件路径'
import HmHeader from './components/HmHeader'

export default {  // 局部注册
  components: {
   '组件名': 组件对象,
    HmHeader:HmHeaer,
    HmHeader
  }
}
```

### 6.练习

- 在App组件中，完成以下练习。在App.vue中使用组件的方式完成下面布局
- App 结构 快捷键： <Vue

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230920103322614.png" alt="image-20230920103322614" style="zoom: 33%;" />

- App.vue
  - import导入每个小组件
  - 必须要写components给小组件注册名字

```vue
<template>
<!-- 1.加类 -->
  <div class="App">
<!-- 3.添加组件  去 conponents 添加组件 -- 然后去每个组件写结构样式-->
<!-- 6.渲染到页面 -->
    <!-- 头部组件 -->
    <HmHeader></HmHeader>
    <!-- 主体组件 -->
    <HmMain></HmMain>
    <!-- 底部组件 -->
    <HmFooter></HmFooter>

    <!-- 如果 HmFooter + tab 出不来 → 需要配置 vscode
         设置 搜索trigger on tab → 勾上 Emmet
    -->
  </div>
</template>

<script>
// 4. 进行导入和注册
import HmHeader from './components/HmHeader.vue'
import HmMain from './components/HmMain.vue'
import HmFooter from './components/HmFooter.vue'
export default {
  components: {
    // 5.注册组件
    // '组件名': 组件对象， 可以简写
    HmHeader: HmHeader,
    HmMain,
    HmFooter
  }
}
</script>

<style>
/* 2.样式 */
.App {
  width: 600px;
  height: 700px;
  background-color: #87ceeb;
  margin: 0 auto;
  padding: 20px;
}
</style>
```

```vue
<template>
  <div class="hm-main">
    我是hm-main
  </div>
</template>

<script>
export default {

}
</script>

<style>
.hm-main {
  height: 400px;
  line-height: 400px;
  text-align: center;
  font-size: 30px;
  background-color: #f79646;
  color: white;
  margin: 20px 0;
}
</style>
```

```vue
<template>
  <div class="hm-footer">
    我是hm-footer
  </div>
</template>

<script>
export default {

}
</script>

<style>
.hm-footer {
  height: 100px;
  line-height: 100px;
  text-align: center;
  font-size: 30px;
  background-color: #4f81bd;
  color: white;
}
</style>
```

## 十一、普通组件的注册使用-全局注册

![image-20230929095618949](/Users/guoguo/Library/Application Support/typora-user-images/image-20230929095618949.png)

### 1.特点：

- 全局注册的组件，在项目的**任何组件**中都能使用


### 2.步骤

1.创建.vue组件（三个组成部分）

![image-20230920105048183](/Users/guoguo/Library/Application Support/typora-user-images/image-20230920105048183.png)

2.**main.js**中进行全局注册

- 一次只能注册一个全局组件

`Vue.component(组件名, 组件对象）`

### 3.使用方式

- 当成HTML标签直接使用


> <组件名></组件名>

### 4.注意

- 组件名规范 —> 大驼峰命名法， 如 HmHeader


### 5.语法

- Vue.component('组件名', 组件对象)


例：

```js
// 导入需要全局注册的组件
import HmButton from './components/HmButton'
Vue.component('HmButton', HmButton)
```

### 6.练习

在以下3个局部组件中是展示一个通用按钮

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230920105015373.png" alt="image-20230920105015373" style="zoom: 33%;" />

- HmMain.vue (可以你在任何组件导入使用)

```vue
*把全部 变成 局部（ 先把全部注了 在这里写）
<template>
  <div class="hm-header">
    我是hm-header
    <HmButton></HmButton>
  </div>
</template>

<script>
import HmButton from './HmButton.vue'
export default {
  // 局部注册: 注册的组件只能在当前的组件范围内使用
  components: {
    HmButton
  }
}
</script>
```

- main.js （全局注册）

~~~javascript
// 文件核心作用：导入App.vue，基于App.vue创建结构渲染index.html
import Vue from 'vue'
import App from './App.vue'
// 2.1编写导入的代码，往代码的顶部编写(规范)
import HmButton from './components/HmButton'
Vue.config.productionTip = false

// 2.2 进行全局注册 → 在所有的组件范围内都能直接使用
//  <HmButton></HmButton> 找到对应的地方调用
// Vue.component(组件名，组件对象)
// 如果下面注了， 没有全局组件了
Vue.component('HmButton', HmButton)


// Vue实例化，提供render方法 → 基于App.vue创建结构渲染index.html
new Vue({
  // render: h => h(App),
  render: (createElement) => {
    // 基于App创建元素结构
    return createElement(App)
  }
}).$mount('#app')

~~~



### 7.总结

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230929100318635.png" alt="image-20230929100318635" style="zoom:50%;" />

**普通组件的注册使用：**

1. 两种注册方式：

① 局部注册：

(1) 创建.vue组件 (单文件组件)

(2) 使用的组件内导入，并局部注册 components: { 组件名：组件对象 } 

② 全局注册：

(1) 创建.vue组件 (单文件组件)

(2) main.js内导入，并全局注册 Vue.component(组件名, 组件对象)

2. 使用：

<组件名></组件名>

**技巧：**

一般都用局部注册，如果发现确实是通用组件，再抽离到全局。



## 十二、综合案例

### 1.小兔仙首页启动项目演示

### 2.小兔仙组件拆分示意图

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230920132641657.png" alt="image-20230920132641657" style="zoom:50%;" />



### 3.开发思路

1. 分析页面，按模块拆分组件，搭架子  (局部或全局注册)

2. 根据设计图，编写组件 html 结构 css 样式 (已准备好)

3. 拆分封装通用小组件  (局部或全局注册)

   将来 → 通过 js 动态渲染，实现功能

