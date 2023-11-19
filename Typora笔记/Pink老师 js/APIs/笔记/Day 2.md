# 一，时间监听

## 1-时间监听



- 什么事事件监听（注册事件）：让程序检测是否有时间产生，一旦有时间触发，就立即调用一个函数做出响应，称为绑定事件 或者 注册事件
  - 例如：鼠标经过现实下拉框，点击可以播放轮播图。。。
- 语法：
  - 元素对象：可以是一个按钮，一个盒子 
  - 事件类型： 加引号
  - 要执行函数：不会立马执行，有动作才执行

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230828091517698.png" alt="image-20230828091517698" style="zoom:50%;" />

- 事件监听三要素：例如：点击小点切换轮播图【事件源： 小圆点，事件类型：点击，事件调用函数：图片更换】

  - 事件源：那个dom元素被事件触发了，要获取dom元素（cái điều khiển tác động) 
  - **事件类型：** 用什么方式触发，比如鼠标单击 click、鼠标经过 mouseover 等 （tác động như thế nào)
  - **事件调用的函数：** 要做什么事 (tác động thì có gì xảy ra)

  <img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230828092933351.png" alt="image-20230828092933351" style="zoom:50%;" />

> 1.事件类型要加引号
>
> 2. 函数是点击之后再去执行，每次点击都会执行一次

### *练习：京东点击关闭顶部广告

需求：点击关闭之后，顶部关闭

~~~html
<body>
  <div class="box">
    我是广告
    <div class="box1">X</div>
    <script>
      //两个盒子都需要获取
      const box = document.querySelector('.box')
      const box1 = document.querySelector('.box1')
      //点击box1，关闭box2
      box1.addEventListener('click', function () {
        box.style.display = 'none'
      }
      )
    </script>
  </div>
</body>
~~~

> 1. 鼠标变成小手， 可点击 
>
> ​      cursor: pointer;
>
> 2. JS需要处理什么盒子都要获取过来

### *（重点）练习：随机点名案例

需求

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230831142820985.png" alt="image-20230831142820985" style="zoom:50%;" />

~~~javascript
 <script>
    // 数据数组
    const arr = ['马超', '黄忠', '赵云', '关羽', '张飞']
    //获取start按钮 - 点击start - 随机抽名字（random）
    const start = document.querySelector('.start')
    const end = document.querySelector('.end')
    const qs = document.querySelector('.qs')
    let timerId = 0
    let radom = 0
    start.addEventListener('click', function () {
      timerId = setInterval(function () {
        //随机数，已经声明的数组
        radom = (Math.floor(Math.random() * arr.length))
        qs.innerHTML = arr[radom]
      }, 35)
      if (arr.length === 1) {
        // start.disabled = true
        // end.disabled = true
        start.disabled = end.disabled = true
      }
    })
    end.addEventListener('click', function () {
      clearInterval(timerId)
      arr.splice(radom, 1)
      //显示删除后的数组
      // console.log(arr)
    })
  </script>
~~~

> :notebook_with_decorative_cover:练习的注意点：
>
> 1. 练习步骤：
>
>    1. 写开始按钮
>
>       *随机
>
>       +事件：点开始 -
>
>       +开始随机 （radom，外面声明因为外面还要复用） -
>
>       +随机结果写在内容里面（innerHTML - 换文本）
>
>    `radom = (Math.floor(Math.random() * arr.length))`
>
>    ​	*定时器：定时器声明名 = timerId (声明外面复用)
>
>    2. 写结束按钮：
>
>    * 关闭随机：
>
>    ​	+ 事件： 点击结束
>
>    ​	+ 关闭随机:  clearInterval(timerId)
>
>    * 删除已经抽到的名字：（指定删除对象元素）
>
>    arr.splice(radom, 1)
>
>    3. 没有名字了， 禁用两个按钮
>
>    - 如果数组元素的长度是1 --》没有名字了 --》置灰按钮
>
>      `start.disabled = end.disabled = true`

## 2-扩展阅读 - 事件临听版本

- DOM - L0

`事件源.on事件 = function() { }`

- DOM - L2

`事件源.addEventListener(事件， 事件处理函数)`

- 区别：on方式会被覆盖，addEventListener方式可绑定多次，拥有事件更多特性，推荐使用

- 发展史： 

  -  DOM L0 ：是 DOM 的发展的第一个版本； L：level 

  - DOM L1：DOM级别1 于1998年10月1日成为W3C推荐标准 

  -  DOM L2：使用addEventListener注册事件 

  - DOM L3： DOM3级事件模块在DOM2级事件的基础上重新定义了这些事件，也添加了一些新事件类型

# 二，事件类型

![image-20230831143343254](/Users/guoguo/Library/Application Support/typora-user-images/image-20230831143343254.png)

## 1 - 鼠标触发

click ： 鼠标点击 

mouseenter： 鼠标经过 

mouseleave： 鼠标离开

### * 练习（重点）：轮播图点击切换

~~~javascript
<script>
    // 1. 初始数据
    const Data = [
      { url: './images/slider01.jpg', title: '对人类来说会不会太超前了？', color: 'rgb(100, 67, 68)' },
      { url: './images/slider02.jpg', title: '开启剑与雪的黑暗传说！', color: 'rgb(43, 35, 26)' },
      { url: './images/slider03.jpg', title: '真正的jo厨出现了！', color: 'rgb(36, 31, 33)' },
      { url: './images/slider04.jpg', title: '李玉刚：让世界通过B站看到东方大国文化', color: 'rgb(139, 98, 66)' },
      { url: './images/slider05.jpg', title: '快来分享你的寒假日常吧~', color: 'rgb(67, 90, 92)' },
      { url: './images/slider06.jpg', title: '哔哩哔哩小年YEAH', color: 'rgb(166, 131, 143)' },
      { url: './images/slider07.jpg', title: '一站式解决你的电脑配置问题！！！', color: 'rgb(53, 29, 25)' },
      { url: './images/slider08.jpg', title: '谁不想和小猫咪贴贴呢！', color: 'rgb(99, 72, 114)' },
    ]
    //1.右边按钮
    const next = document.querySelector('.next')
    const img = document.querySelector('img')
    const p = document.querySelector('.slider-footer p')
    let i = 0
    next.addEventListener('click', function () {
      //1.1图片
      i++
      //next到最后一个，回第一个 (根据医德值，i>=8 -- 回i=0), i的条件要写在这里
      if (i >= 8) {
        i = 0
      }
      toggle()
      //按照i换图片
      // img.src = Data[i].url
      // //1.2title
      // p.innerHTML = Data[i].title
      // //1.3小圆点
      // //移除css写的active,获取 .active 然后直接删除
      // document.querySelector('.slider-indicator .active').classList.remove('active')
      // //把active给对应小点
      // document.querySelector(`.slider-indicator li:nth-child(${i + 1})`).classList.add('active')
    })
    //2.左边按钮
    const prev = document.querySelector('.prev')
    prev.addEventListener('click', function () {
      i--
      //回i的最后一个
      if (i < 0) {
        i = 7
      }
      //一样的代码可以用渲染函数以后复用
      toggle()
    })

    function toggle() {
      img.src = Data[i].url
      p.innerHTML = Data[i].title
      document.querySelector('.slider-indicator .active').classList.remove('active')
      document.querySelector(`.slider-indicator li:nth-child(${i + 1})`).classList.add('active')
    }
    //3.自动播放,只往前播放, 开起定时器
    let timeID = setInterval(function () {
      //click的意思是点击next按钮，click()是调用了所以可以自动
      //传统写法：next.onclick = function(从i++到active的代码)， 然后click() - 调用函数
      //自动调用点击事件
      next.click()
    }, 1000)

    //4.鼠标移入大盒子停止定时器
    const slider = document.querySelector('.slider')
    slider.addEventListener('mouseenter', function () {
      clearInterval(timeID)
    })

    //5.鼠标离开打开盒子，开启定时器,还是用timeID不要再声明
    slider.addEventListener('mouseleave', function () {
      //应该先关了再开新的（ 好习惯）
      clearInterval(timeID)
      timeID = setInterval(function () {
        next.click()
      }, 1000)
    })


  </script>
~~~

> 1.新的知识点： 自动播放图片
>
> `对象.事件() 自动调用事件后面的代码`
>
> 2.开启定时器记得给声明， 为了方便关闭

## 2 - 鼠标获取光标

focus：获得焦点

blur：失去焦点

例如：点击边框亮，失去焦点不亮了

### ？练习（重点）小米搜索框案例

## 3 - 键盘触发

Keydown ： 键盘按下触发 （按下不放）

Keyup：键盘抬起触发 （按下然后放）

### ？练习（重点）： 评论字数统计

# 三，事件对象

## 1 - 获取事件对象

- 事件对象：

  - 也是对象，这个对象里有事件触发时的相关信息 （里面有很多对象和方法）

  ![image-20231109161655499](/Users/guoguo/Library/Application Support/typora-user-images/image-20231109161655499.png)

  - 例如：鼠标点击事件中，事件对象就存了鼠标点哪个位置等信息

- 使用场景：

  - 可以判断用户按下哪个键，比如： 按下回车可以发布评论
  - 可以判断鼠标点击哪个元素，从而做相应的操作 
  - 点击删除产品要知道点了哪个产品

  ![image-20231109161337869](/Users/guoguo/Library/Application Support/typora-user-images/image-20231109161337869.png)

- 语法：

  - 在事件绑定的回调函数的第一个参数就是事件对象
  - 一般命名为event、ev、e
  
  ![image-20230831170726635](/Users/guoguo/Library/Application Support/typora-user-images/image-20230831170726635.png)
  

## 2 - 事件对象常用属性

- 部分常用属性：

  - type：获取当前的事件类型，例如：click
  - clientX/clientY：获取光标相对于浏览器可见窗口左上角的位置 
  - offsetX/offsetY：获取光标相对于当前DOM元素左上角的位置
  - key：用户按下的键盘键的值

  <img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230831171904760.png" alt="image-20230831171904760" style="zoom:50%;" />

keyup：事件

key：属性

- 例如：按回车发送消息

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230831172217428.png" alt="image-20230831172217428" style="zoom:50%;" />

### ？ 练习（重点）评论回车发布

# 四，环境对象

- 环境对象：值是函数内部特殊的变量this，它代表着当前函数运行时所处的环境
- 【谁调用，this就是谁】
- 作用：让代码更简洁
  - 函数的调用方式不同，this指代的对象也不同
  - 直接调用函数，其实相当于是 window.函数，所以 this 指代 window
- 例如： this会指向命名的函数

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230831172913891.png" alt="image-20230831172913891" style="zoom:50%;" />

~~~javascript
 <script>
    // 每个函数里面都有this 环境对象  普通函数里面this指向的是window
    // function fn() {
    //   console.log(this)
    // }
    // window.fn()
    const btn = document.querySelector('button')
    btn.addEventListener('click', function () {
      // console.log(this)  // btn 对象
      // btn.style.color = 'red'
      this.style.color = 'red'
    })
  </script>
~~~

# 五，回调函数

- 当一个函数当做参数来传递给另外一个函数的时候，这个函数就是回调函数
- 回调函数本质还是函数，只不过把它当成参数使用
- 使用匿名函数做为回调函数比较常见

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230831173322946.png" alt="image-20230831173322946" style="zoom:50%;" />



<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230831173400499.png" alt="image-20230831173400499" style="zoom:50%;" />

# 六，综合案例

## ？tab切换练习

