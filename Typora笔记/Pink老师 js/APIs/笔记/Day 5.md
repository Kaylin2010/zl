# Web APIs - 第5天笔记

> 目标： 能够利用JS操作浏览器,具备利用本地存储实现学生就业表的能力

- BOM操作
- 综合案例


## * js组成

JavaScript的组成

- ECMAScript:
  - 规定了js基础语法核心知识。
  - 比如：变量、分支语句、循环语句、对象等等


- Web APIs :
  - DOM   文档对象模型， 定义了一套操作HTML文档的API
  - BOM   浏览器对象模型，定义了一套操作浏览器窗口的API

 <img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230904110123464.png" alt="image-20230904110123464" style="zoom:50%;" />

# 一，window对象

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230904110245907.png" alt="image-20230904110245907" style="zoom:50%;" />

**BOM** (Browser Object Model ) 是浏览器对象模型

- window对象是一个==全局对象==，也可以说是JavaScript中的顶级对象
- 像document、alert()、console.log()这些都是window的属性，基本BOM的属性和方法都是window的
- 所有通过var定义在全局作用域中的变量、函数都会变成window对象的属性和方法
- window对象下的属性和方法调用的时候可以省略window

~~~html
<script>
  //省略了window
    console.log(document === window.document)
    function fn() {
      console.log(11)
    }
    //用var声明，默认是挂在window下面 
    window.fn()
    var num = 10
    console.log(window.num)
  </script>
~~~

## 1 - 定时器-延迟函数

- JavaScript 内置的一个用来让代码延迟执行的函数，叫 ==setTimeout==


:notebook_with_decorative_cover:**语法：**

~~~JavaScript
setTimeout(回调函数, 延迟时间)
~~~

- setTimeout 仅仅只执行一次，所以可以理解为就是把一段代码延迟执行, 平时省略window

~~~html
<script>
    //意思是：刷新页面后2s后执行代码， 只执行一次
    setTimeout(function () {
      console.log('时间到了')
    }, 2000)
  </script>
~~~

- 间歇函数 ==setInterval== （区别） 每隔一段时间就执行一次， , 平时省略window

> - 两种定时器
>   - 轮播图自动换图片 === setInterval
>   - 过几秒后广告自动关闭（只出现一次）=== setTimeout
>
> 

:notebook_with_decorative_cover:**清除延时函数**

~~~JavaScript
clearTimeout(timerId)
~~~

>:o2:  注意点
>
>1. 延时函数需要等待,所以后面的代码先执行(到点才执行， 不影响到别的函数)
>2. 返回值是一个正整数，表示定时器的编号(ID)
>
><img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230904112301205.png" alt="image-20230904112301205" style="zoom:50%;" />

~~~html
<body>
  <script>
    // 定时器之延迟函数

    // 1. 开启延迟函数
    let timerId = setTimeout(function () {
      console.log('我只执行一次')
    }, 3000)

    // 1.1 延迟函数返回的还是一个正整数数字，表示延迟函数的编号
    console.log(timerId)

    // 1.2 延迟函数需要等待时间，所以下面的代码优先执行

    // 2. 关闭延迟函数
    clearTimeout(timerId)

  </script>
</body>
~~~

## 2 - JS 执行机制

- JS语言的一大特点就是单线程，也就是说，同一个时间只能做一件事

- 单线程就意味着，所有任务需要排队，前一个任务结束，才会执行后一个任务。这样所导致的问题是： 如果 JS 执行的时间过长，这样就会造成页面的渲染不连贯，导致页面渲染加载阻塞的感觉

- 为了解决这个问题，利用多核 CPU 的计算能力，HTML5 提出 Web Worker 标准，允许 JavaScript 脚本创建多个 

  线程。于是，JS 中出现了==同步和异步==

  - 同步：
    - 前一个任务结束后再执行后一个任务，程序的执行顺序与任务的排列顺序是一致的、同步的
    - 同步任务都在主线程上执行，形成一个**执行栈**
  - 异步：
    - 你在做一件事情时，因为这件事情会花费很长时间，在做这件事的同时，你还可以去处理其他事情
    - JS 的异步是通过回调函数实现的，一般而言，异步任务有以下三种类型：
      - 普通事件，如 click、resize 等
      - 资源加载，如 load、error 等
      - 定时器，包括 setInterval、setTimeout 等
    - 异步任务相关添加到**任务队列**

  :soon: **他们的本质区别： 这条流水线上各个流程的执行顺序不同**

  - JS执行步骤：
    - 先执行执行栈中的同步任务
    - 异步任务放入任务队列中
    - . 一旦执行栈中的所有同步任务执行完毕，系统就会按次序读取任务队列中的异步任务，于是被读取的异步任务结束等待状态，进入执行栈，开始执行

  > 由于主线程不断的重复获得任务、执行任务、再获取任务、再执行，所以这种机制被称为事件循环（ 
  >
  > event loop 

  <img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230905095105701.png" alt="image-20230905095105701" style="zoom:50%;" />

  - 例如

  <img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230905095218785.png" alt="image-20230905095218785" style="zoom: 25%;" />

  

  

## 3 - location对象 

- location (地址) 它拆分并保存了 URL 地址的各个组成部分， 它是一个对象


| 属性/方法    | 说明                                                 |
| ------------ | ---------------------------------------------------- |
| ==href==     | 属性，获取完整的 URL 地址，赋值时用于地址的跳转      |
| search       | 属性，获取地址中携带的参数，符号 ？后面部分          |
| hash         | 属性，获取地址中的啥希值，符号 # 后面部分            |
| ==reload()== | 方法，用来刷新当前页面，传入参数 true 时表示强制刷新 |

~~~html
<body>
  <form>
    <input type="text" name="search"> <button>搜索</button>
  </form>
  <a href="#/music">音乐</a>
  <a href="#/download">下载</a>

  <button class="reload">刷新页面</button>
  <script>
    // location 对象  
    // 1. href属性 （重点） 得到完整地址，赋值则是跳转到新地址
    console.log(location.href)
    // location.href = 'http://www.itcast.cn'

    // 2. search属性  得到 ? 后面的地址 
    console.log(location.search)  // ?search=笔记本

    // 3. hash属性  得到 # 后面的地址
    console.log(location.hash)

    // 4. reload 方法  刷新页面
    const btn = document.querySelector('.reload')
    btn.addEventListener('click', function () {
      // location.reload() // 页面刷新
      location.reload(true) // 强制页面刷新 ctrl+f5
    })
  </script>
</body>
~~~

### ？练习： 5秒后自动跳转页面

## 4 - navigator对象

navigator是对象，该对象下记录了浏览器自身的相关信息

常用属性和方法：

- 通过 userAgent 检测浏览器的版本及平台，换地址就好

~~~javascript
// 检测 userAgent（浏览器信息）
(function () {
  const userAgent = navigator.userAgent
  // 验证是否为Android或iPhone
  const android = userAgent.match(/(Android);?[\s\/]+([\d.]+)?/)
  const iphone = userAgent.match(/(iPhone\sOS)\s([\d_]+)/)
  // 如果是Android或iPhone，则跳转至移动站点
  if (android || iphone) {
    location.href = 'http://m.itcast.cn'
  }})();
~~~

## 5 - histroy对象 

- history (历史)是对象，主要管理历史记录， 该对象与浏览器地址栏的操作相对应，如前进、后退等


- **使用场景**

- history对象一般在实际开发中比较少用，但是会在一些OA 办公系统中见到。


<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230905142652477.png" alt="image-20230905142652477" style="zoom:50%;" />

- 常见方法：


<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230905142715992.png" alt="image-20230905142715992" style="zoom:50%;" />

~~~html
<body>
  <button class="back">←后退</button>
  <button class="forward">前进→</button>
  <script>
    // histroy对象

    // 1.前进
    const forward = document.querySelector('.forward')
    forward.addEventListener('click', function () {
      // history.forward() 
      history.go(1)
    })
    // 2.后退
    const back = document.querySelector('.back')
    back.addEventListener('click', function () {
      // history.back()
      history.go(-1)
    })
  </script>
</body>

~~~

# 二，本地存储（今日重点）

## 1 - 本地存储介绍

- 常见的使用场景


<https://todomvc.com/examples/vanilla-es6/>    页面==刷新数据不丢失==

- 好处


1、页面刷新或者关闭不丢失数据，实现数据持久化

2、容量较大，sessionStorage和 localStorage 约 5M 左右

## 2 - 本地存储分类

- 本地存储：将数据存储在本地浏览器中


###  * localStorage（重点）

`Storage : tích trữ (dữ liệu)`

**作用:** 数据可以长期保留在本地浏览器中，刷新页面和关闭页面，数据也不会丢失

**特性：**以==键值对的形式==存储，并且==存储的是字符串==， 省略了window

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230905150618149.png" alt="image-20230905150618149" style="zoom:50%;" />

~~~html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>本地存储-localstorage</title>
</head>

<body>
  <script>
    // 本地存储 - localstorage 存储的是字符串 
    // 1. 存储 - （改 + 增）
    localStorage.setItem('age', 18)

    // 2. 获取 -（查）
    console.log(typeof localStorage.getItem('age'))

    // 3. 删除
    localStorage.removeItem('age')
  </script>
</body>

</html>
~~~

> 1， 存储的是字符串
>
> 2，（键，值）除了数字型其他的都需要加引号

- 浏览器查看本地数据

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230905155751386.png" alt="image-20230905155751386" style="zoom:50%;" />

### * sessionStorage（了解）

特性：

- 用法跟localStorage基本相同
- 区别是：当页面浏览器被关闭时，存储在 sessionStorage 的数据会被清除

存储：sessionStorage.setItem(key,value)

获取：sessionStorage.getItem(key)

删除：sessionStorage.removeItem(key)

## 3 - 存储复杂数据类型

### * localStorage 存储复杂数据类型

- 第一步：复杂数据类型存储转换为 JSON字符串存储

  - **问题：**本地只能存储字符串,无法存储复杂数据类型 （对象，数组）
  - **解决：**需要将复杂数据类型转换成 JSON字符串,在存储到本地
  - **语法：**`JSON.stringify(不加引号)`

  * JSON字符串特点

    `*{"uname":"pink老师","age":18,"gender":"女"}*`

    * 先是1个字符串
    * 属性名使用双引号引起来，不能单引号
    * 属性值如果是字符串型也必须双引号

- 第二步：获取然后把JSON字符串转换为 对象 JSON.parse(方法)

  - **问题：**因为本地存储里面取出来的是字符串，不是对象，无法直接使用

  - **解决： **把取出来的字符串转换为对象

  - **语法：**`JSON.parse(不加引号)`

~~~html
<script>
    const obj = {
      uname: 'pink老师',
      age: 18,
      gender: '女'
    }
    /* 第一步：复杂数据类型存储必须转换为 JSON字符串存储
     JSON对象 JSON.stringify(方法 - 不要加引号) */
    localStorage.setItem('obj', JSON.stringify(obj))
    // console.log(localStorage.getItem('obj'))
    //结果 JSON对象： {"uname":"pink老师","age":18,"gender":"女"}

    //获取
    const str = localStorage.getItem('obj')
    /* 第二步： 把JSON字符串转换为 对象 JSON.parse(方法) */
    console.log(JSON.parse(str))
    //结果 {uname: 'pink老师', age: 18, gender: '女'}
  </script>
~~~

# 三，综合案例：学生就业信息表

### 1 - 数组map 方法

**使用场景：**

map 可以遍历数组==处理数据==，并且==返回新的数组==

~~~html
map(function (ele, index) {`
		return ele + '颜色'}
~~~

**语法：**

~~~javascript
<body>
  <script>
  const arr = ['red', 'blue', 'pink']
  // 1. 数组 map方法 处理数据并且 返回一个数组
   const newArr = arr.map(function (ele, index) {
    // console.log(ele)  // 数组元素
    // console.log(index) // 索引号
    return ele + '颜色'
	})
console.log(newArr)
</script>
</body>
~~~

>map 也称为映射。映射是个术语，指两个元素的集之间元素相互“对应”的关系。
>
>map重点在于有返回值，forEach没有返回值（undefined）

### 2 - 数组join方法

**作用：**join() 方法用于把==数组==中的所有元素==转换一个字符串==

**语法：**

`对象.join()`

~~~html
<body>
  <script>
    const arr = ['red', 'blue', 'pink']

    // 1. 数组 map方法 处理数据并且 返回一个数组
    const newArr = arr.map(function (ele, index) {
      // console.log(ele)  // 数组元素
      // console.log(index) // 索引号
      return ele + '颜色'
    })
    console.log(newArr)

    // 2. 数组join方法  把数组转换为字符串
    // 小括号为空则逗号分割
    console.log(newArr.join())  // red颜色,blue颜色,pink颜色
    // 小括号是空字符串，则元素之间没有分隔符
    console.log(newArr.join(''))  //red颜色blue颜色pink颜色
    console.log(newArr.join('|'))  //red颜色|blue颜色|pink颜色
  </script>
</body>
~~~









