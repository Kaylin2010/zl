# Web APIs - 第4天

> 进一步学习 DOM 相关知识，实现可交互的网页特效

- 能够插入、删除和替换元素节点
- 能够依据元素节点关系查找节点

# 一，日期对象


掌握 Date 日期对象的使用，动态获取当前计算机的时间。


ECMAScript 中内置了获取系统时间的对象 Date，使用 Date 时与之前学习的内置对象 console 和 Math 不同，它需要借助 new 关键字才能使用。

### 1 - 实例化

- 代码中发现了 `new` 关键词时， 一般这个操作称为==实例化==

- 创建一个时间对象 并且获取时间

  - 获取当前时间：`const date = new Date()`
  - 获取指定时间：` const date = new Date('指定时间')` （指定到期时结束时间）

  > Date() ： 对象，new： 关键词

~~~javascript
  // 1. 实例化
  // const date = new Date(); // 系统默认时间
  const date = new Date('2020-05-01 08:00') // 指定时间
  // date 变量即所谓的时间对象
  console.log(typeof date)
~~~

### 2 - 日期对象方法

- 日期对象返回的数据我们不能直接使用，所以需要转换为实际开发中常用的格式

 ~~~javascript
   // 1. 实例化
  const date = new Date();
  // 2. 调用时间对象方法
  // 通过方法分别获取年、月、日，时、分、秒
  const year = date.getFullYear(); // 四位年份
  const month = date.getMonth(); // 0 ~ 11
 ~~~

- 
  getFullYear 获取四位年份


- ==getMonth 获取月份，取值为 0 ~ 11==

~~~html
<script>
    //现在是 9月
    const date = new Date()
    console.log(date.getMonth()) //结果：8
    //需要加1
    console.log(date.getMonth() + 1) //结果：9
  </script>
~~~

- getDate 获取月份中的每一天，不同月份取值也不相同


- ==getDay 获取星期，取值为 0 ~ 6==

~~~html
<script>
    //今天是星期一，数组需要从星期日开始
    const day = ['cn', 't2', 't3', 't4', 't5', 't6', 't7']
    const date = new Date()
    console.log(`今天是${day[date.getDay()]}`)
  </script>
~~~

- getHours 获取小时，取值为 0 ~ 23


- getMinutes 获取分钟，取值为 0 ~ 59


- getSeconds 获取秒，取值为 0 ~ 59


### 3 - 时间戳 /chuō/

- 时间戳是指1970年01月01日00时00分00秒起至现在的总秒数或毫秒数，它是一种特殊的计量时间的方式。


注：ECMAScript 中时间戳是以毫秒计的。

~~~javascript
    // 1. 实例化
  const date = new Date()
  // 2. 获取时间戳
  console.log(date.getTime())
// 还有一种获取时间戳的方法
  console.log(+new Date())
  // 还有一种获取时间戳的方法
  console.log(Date.now())

~~~

- 获取时间戳的方法

  - 使用  `getTime`()  方法

  ~~~html
  <script>
      const date = new Date()
      console.log(date.getTime()) //结果：1693805299676
    </script>
  ~~~

  - 使用 `+new Date()`  （建议使用）

    ~~~html
     <script>
        const date = new Date()
        console.log(+new Date()) //结果：1693805299676
      </script>
    ~~~

  - 使用 Date.now()

    - 缺点：只能得到当前的时间戳，上面两个方法可以反馈指定时间

  ~~~html
   <script>
      //不需要实例化
      console.log(Date.now()) 
     //指定时间
      const time = new Date()
      console.log(+new Date('2022-4-12')) //结果：1649692800000
    </script>
  ~~~

#### ？案例：毕业倒计时效果

# 二，节点操作

## 1 - DOM 节点

> 掌握元素节点创建、复制、插入、删除等操作的方法，能够依据元素节点的结构关系查找节点

- 回顾之前 DOM 的操作都是针对元素节点的属性或文本的，除此之外也有专门针对元素节点本身的操作，如插入、复制、删除、替换等
- 节点：DOM树每个内容 （标签， 属性。。。）
- 节点类型：
  - ==元素节点==：所有的标签，HTML是根元素节点
  - 属性节点：所有的属性（例如： href）
  - 文本节点：所有的文本
  - 其他

## 2 -  插入节点

- 很多情况下，我们需要再页面中添加元素（点击发布按钮，可以新增一条消息）
- 新增节点， 按照两个步骤：
  - 创建一个新的节点
  - 把新创建的节点放入指定的元素内部

### *创建节点

- 方法：`document.createElement('标签名')`

### *追加节点

** 插入到父元素的最后一个子元素 `父元素.appendChild(要插入的元素)`

~~~html
<script>
    const ul = document.querySelector('ul')
    //声明并且创建元素
    const li = document.createElement('li')
    ul.appendChild(li)
  </script>
~~~

> :octopus:注意点：
>
>  创建并且声明元素

**插入到父元素中某个子元素的前面 `父元素.insertBefore（要插入元素，在哪个元素前面）`

~~~html
<script>
    const div = document.querySelector('div')
    const ul = document.querySelector('ul')
    //声明并且创建元素
    const li = document.createElement('li')
    //1，添加到都需要是ul的子元素
    //2，用.children[1]选出来
    //2，用.children[0]选出来 --- 一直会在前面
    ul.insertBefore(li, ul.children[0])
  </script>
~~~

> :octopus:注意点
>
> 1，添加到都需要是ul的子元素
> 2，用.children[1]选出来

### *克隆节点

- 新增节点的步骤：
  - 复制一个原有的节点
  - 把复制的节点放入到指定的额元素内部
- 语法： `要复制元素.cloneNode(布尔值)`

~~~html
 <script>
    const ul = document.querySelector('ul')
    const li1 = ul.children[0].cloneNode(true)
    console.log(li1) //结果：<li>1</li>，需要用就调用
    //复制后放在指定的额位置
    ul.appendChild(li1) //加上li1在最后面
  </script>
=====简洁代码=====
	<script>
    const ul = document.querySelector('ul')
    //直接写在里面，只要获取ul
    ul.appendChild(ul.children[0].cloneNode(true))
  </script>
~~~

- cloneNode会克隆出来一个跟原标签一样的元素，括号内传入布尔值
  - 若为true，则代表克隆时会包含后代节点一起克隆（包括内容）
  - 若为false，则代表克隆时不包含后代节点 （只有标签没有内容）
  - 默认为false

- 如下代码演示：

```html
<body>
  <h3>插入节点</h3>
  <p>在现有 dom 结构基础上插入新的元素节点</p>
  <hr>
  <!-- 普通盒子 -->
  <div class="box"></div>
  <!-- 点击按钮向 box 盒子插入节点 -->
  <button class="btn">插入节点</button>
  <script>
    // 点击按钮，在网页中插入节点
    const btn = document.querySelector('.btn')
    btn.addEventListener('click', function () {
      // 1. 获得一个 DOM 元素节点
      const p = document.createElement('p')
      p.innerText = '创建的新的p标签'
      p.className = 'info'
      
      // 复制原有的 DOM 节点
      const p2 = document.querySelector('p').cloneNode(true)
      p2.style.color = 'red'

      // 2. 插入盒子 box 盒子
      document.querySelector('.box').appendChild(p)
      document.querySelector('.box').appendChild(p2)
    })
  </script>
</body>
```

:octopus: 结论

- `createElement` 动态创建任意 DOM 节点

- `cloneNode` 复制现有的 DOM 节点，传入参数 true 会复制所有子节点

- `appendChild` 在末尾（结束标签前）插入节点

再来看另一种情形的代码演示：

```html
<body>
  <h3>插入节点</h3>
  <p>在现有 dom 结构基础上插入新的元素节点</p>
	<hr>
  <button class="btn1">在任意节点前插入</button>
  <ul>
    <li>HTML</li>
    <li>CSS</li>
    <li>JavaScript</li>
  </ul>
  <script>
    // 点击按钮，在已有 DOM 中插入新节点
    const btn1 = document.querySelector('.btn1')
    btn1.addEventListener('click', function () {

      // 第 2 个 li 元素
      const relative = document.querySelector('li:nth-child(2)')

      // 1. 动态创建新的节点
      const li1 = document.createElement('li')
      li1.style.color = 'red'
      li1.innerText = 'Web APIs'

      // 复制现有的节点
      const li2 = document.querySelector('li:first-child').cloneNode(true)
      li2.style.color = 'blue'

      // 2. 在 relative 节点前插入
      document.querySelector('ul').insertBefore(li1, relative)
      document.querySelector('ul').insertBefore(li2, relative)
    })
  </script>
</body>
```

:octopus:  结论

- `createElement` 动态创建任意 DOM 节点

- `cloneNode` 复制现有的 DOM 节点，传入参数 true 会复制所有子节点

- `insertBefore` 在父节点中任意子节点之前插入新节点

### ?案例：学成在线文案传染

## 3 -  删除节点

- 删除现有的 DOM 节点，也需要关注两个因素：首先由父节点删除子节点，其次是要删除哪个子节点（必须要通过父元素）
- 语法：` 父元素.removeChild(要删除的元素)`
- 如果不存在父子关系则删除不成功
- removeChild：删除，标签也删除，display：none --- 是隐藏， 标签还在

```html
<body>
  <!-- 点击按钮删除节点 -->
  <button>删除节点</button>
  <ul>
    <li>HTML</li>
    <li>CSS</li>
    <li>Web APIs</li>
  </ul>

  <script>
    const btn = document.querySelector('button')
    btn.addEventListener('click', function () {
      // 获取 ul 父节点
      let ul = document.querySelector('ul')
      // 待删除的子节点
      let lis = document.querySelectorAll('li')

      // 删除节点
      ul.removeChild(lis[0])
    })
  </script>
</body>
```

:octopus:  结论：

> 1, `removeChild` 删除节点时一定是由父子关系
>
> 2, children[0] --- 数组， 获取对一个的子元素

## 4 - 查找节点

- DOM 树中的任意节点都不是孤立存在的，它们要么是父子关系，要么是兄弟关系，不仅如此，我们可以依据节点之间的关系查找节点
- 节点关系：
  - 父节点
  - 子节点
  - 兄弟节点

### *父节点

- 语法：`子元素.parentNode`  （通过子元素找到父元素）

~~~html
// parentNode 不是 Note
	<script>
    const baby = document.querySelector('.baby')
    console.log(baby)  // 返回dom对象
    console.log(baby.parentNode) // 返回dom对象,dad
    console.log(baby.parentNode.parentNode) // 返回dom对象,yeye
  </script>
~~~

### *子节点

- 子节点查询：
  - childNodes：获取所有子节点，包括文本节点（空格，换行。。。）
  - ==children 属性（重点）==：`父元素.children`
    - 仅获得所有元素节点
    - 返回的还是一个伪数组, 所以只先亲儿子

```html
<body>
  <button class="btn1">所有的子节点</button>
  <!-- 获取 ul 的子节点 -->
  <ul>
    <li>HTML</li>
    <li>CSS</li>
    <li>JavaScript 基础</li>
    <li>Web APIs</li>
  </ul>
  <script>
    const btn1 = document.querySelector('.btn1')
    btn1.addEventListener('click', function () {
      // 父节点
      const ul = document.querySelector('ul')

      // 所有的子节点
      console.log(ul.childNodes)
      // 只包含元素子节点
      console.log(ul.children)
    })
  </script>
</body>
```

:octopus:  结论

- `childNodes` 获取全部的子节点，回车换行会被认为是空白文本节点
- `children` 只获取元素类型节点

```html
<body>
  <table>
    <tr>
      <td width="60">序号</td>
      <td>课程名</td>
      <td>难度</td>
      <td width="80">操作</td>
    </tr>
    <tr>
      <td>1</td>
      <td><span>HTML</span></td>
      <td>初级</td>
      <td><button>变色</button></td>
    </tr>
    <tr>
      <td>2</td>
      <td><span>CSS</span></td>
      <td>初级</td>
      <td><button>变色</button></td>
    </tr>
    <tr>
      <td>3</td>
      <td><span>Web APIs</span></td>
      <td>中级</td>
      <td><button>变色</button></td>
    </tr>
  </table>
  <script>
    // 获取所有 button 节点，并添加事件监听
    const buttons = document.querySelectorAll('table button')
    for(let i = 0; i < buttons.length; i++) {
      buttons[i].addEventListener('click', function () {
        // console.log(this.parentNode); // 父节点 td
        // console.log(this.parentNode.parentNode); // 爷爷节点 tr
        this.parentNode.parentNode.style.color = 'red'
      })
    }
  </script>
</body>
```

:octopus:   结论：`parentNode` 获取父节点，以相对位置查找节点，实际应用中非常灵活。

### *兄弟关系

- 下一个兄弟节点：`nextSibling`
- 上一个兄弟节点：`previousSibling`

```html
<body>
  <ul>
    <li>HTML</li>
    <li>CSS</li>
    <li>JavaScript 基础</li>
    <li>Web APIs</li>
  </ul>
  <script>
    // 获取所有 li 节点
    const lis = document.querySelectorAll('ul li')

    // 对所有的 li 节点添加事件监听
    for(let i = 0; i < lis.length; i++) {
      lis[i].addEventListener('click', function () {
        // 前一个节点
        console.log(this.previousSibling)
        // 下一下节点
        console.log(this.nextSibling)
      })
    }
  </script>
</body>
```

:octopus:  结论

- `previousSibling` 获取前一个节点，以相对位置查找节点，实际应用中非常灵活。
- `nextSibling` 获取后一个节点，以相对位置查找节点，实际应用中非常灵活。

# 三， M端事件 （手机端）

- 手机端有自己独特的地方，比如触屏事件 touch（触摸事件）
- touchstart：手指触摸到一个DOM元素时触发
- touchmove：手指在一个DOM元素上滑动时触发
- touchend：手指从一个DOM元素上移开触发

# 四，JS插件

- 插件：别人写好的代买，我们想只要赋值对应代码， 就可以直接实现对应效果

https://www.swiper.com.cn/

- 使用：
  - 下载Swiper
  - 找packgage（css 跟 js）
  - js 跟 css获取过来
  - 找对应的效果 --- 获取源代码
  - 引入文件： css （head - link） 跟 js (script src 里面)， js有2个script  （一个引用js， 一个指定js）
  - 没有小圆点 --- 看定位
- 定制：API文档找对应的效果 - 复制到第二个 script里面













