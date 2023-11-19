# ？全选文本案例

# 一，事件流

## 1. 事件流与两个阶段说明

- 事件流： 事件完整执行过程中的领导路径
  - 捕获： 从父到子
  - 冒泡： 从子到父（实际上工作使用冒泡为主）

## 2. 事件捕获（了解）

- 从DOM的根元素开始去执行对应的事件（从外到里）
- addEventListener第三个参数传入 true 代表是捕获阶段触发（很少使用）
- 若传入false代表冒泡阶段触发，默认就是false
- 若是用 L0 事件监听，则只有冒泡阶段，没有捕获

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230831202403018.png" alt="image-20230831202403018" style="zoom:50%;" />

## 3. 事件冒泡

- 事件冒泡：当一个元素触发事件后，会依次向上调用所有父级元素的 同名事件
- 事件冒泡是默认存在
- L2事件监听第三个参数是 false，或者默认都是冒泡

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230831203015082.png" alt="image-20230831203015082" style="zoom:50%;" />



## 4. 阻止冒泡

### 4.1 防止冒泡

- 因为默认就有冒泡模式的存在，所以容易导致事件影响到父级元素

- 此方法可以阻断事件流动传播，不光在冒泡阶段有效，捕获阶段也有效
- 语法：

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230831203223222.png" alt="image-20230831203223222" style="zoom:50%;" />

- 例子

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230831203332397.png" alt="image-20230831203332397" style="zoom:50%;" />

### 4.2 防止默认行为

- 某些情况需要防止默认行为： 链接跳转，表单域跳转
- 语法:

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230831203714805.png" alt="image-20230831203714805" style="zoom:50%;" />

- 例如：填写信息错误， 阻止点击提交按钮

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230831203810561.png" alt="image-20230831203810561" style="zoom:50%;" />

- 阻止跳转链接：

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230831210311369.png" alt="image-20230831210311369" style="zoom:50%;" />

## 5. 解绑事件

### 5.1 - on事件方式

- 直接使用null覆盖就可以实现事件的解绑 （写在函数外面）
- 语法：

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230831204142379.png" alt="image-20230831204142379" style="zoom:50%;" />

### 5.2 - addEventListener方式

- 语法：

`removeEventListener(事件类型, 事件处理函数, [获取捕获或者冒泡阶段])`

- 例如：[https://github.com/go-pingping/JS/blob/master/2023JavaScript/JavaScript/Web%20API/web%20APIs%E7%AC%AC%E4%B8%89%E5%A4%A9/04-code/05-%E4%BA%8B%E4%BB%B6%E8%A7%A3%E7%BB%91.html](https://github.com/go-pingping/JS/blob/master/2023JavaScript/JavaScript/Web%20API/web%20APIs%E7%AC%AC%E4%B8%89%E5%A4%A9/04-code/05-%E4%BA%8B%E4%BB%B6%E8%A7%A3%E7%BB%91.html)

~~~javascript
 <script>
    const btn = document.querySelector('button')
    function fn() {
      alert('an vao day')
    }
    btn.addEventListener('click', fn)
    //解绑add的事件
    btn.removeEventListener('click', fn)
  </script>
~~~

- 匿名函数无法解绑

### 5.3 - 区别

- 鼠标经过事件的区别 

  - mouseover 和 mouseout 会有冒泡效果

    - 例如：https://github.com/go-pingping/JS/blob/master/2023JavaScript/JavaScript/Web%20API/web%20APIs%E7%AC%AC%E4%B8%89%E5%A4%A9/04-code/06-mouseover%E5%92%8Cmouseenter%E7%9A%84%E5%8C%BA%E5%88%AB.html

    <img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230831210641374.png" alt="image-20230831210641374" style="zoom:50%;" />

  - mouseenter 和 mouseleave 没有冒泡效果 (推荐)

- 两种注册事件的区别

  - 传统on注册（L0）：
    - 同一个对象,后面注册的事件会覆盖前面注册(同一个事件)
    - 直接使用null覆盖偶就可以实现事件的解绑
    - 都是冒泡阶段执行的 
  - 事件监听注册（L2）
    - 语法: addEventListener(事件类型, 事件处理函数, 是否使用捕获) 
    - 后面注册的事件不会覆盖前面注册的事件(同一个事件)
    - 可以通过第三个参数去确定是在冒泡或者捕获阶段执行 （默认false）
    - 必须使用removeEventListener(事件类型, 事件处理函数, 获取捕获或者冒泡阶段)
    - 匿名函数无法被解绑

# 二，事件委托

- 减少注册次数，可以提高程序性能

~~~html
<script>
    const ul = document.querySelector('ul')
    ul.addEventListener('click', function (e) {
      console.dir(e.target)
    })
  </script>
~~~



- 原理：事件委托是利用事件委托的特点 （必须要用e）
  - 给父元素注册事件，当我们触发子元素，会冒泡到父元素身上，从而触发父元素的事件 

> - target只的事我们点击的对象（这样只要点击会变成红色）
>
> ` e.target.style.color = 'red'`:
>
> - 如果需求是点击的如果是li才变成颜色， 要找出来LI然后给条件
>
> ~~~javascript
> if (e.target.tagName === 'LI') {
>         e.target.style.color = 'red'
>       }
> ~~~
>
> - 查看一个对象的全部属性
>
> `console.dir(*e*.target)`

### ？练习：tab切换改造

# 三，其他事件

### 3.1 - 页面加载事件

#### * load 事件

- 加载外部资源（图片，外联CSS和JS等）加载完毕时触发的事件
- 有时候需要等页面资源全部处理完做一些事情
- 事件名：load
- 监听页面所有资源加载完毕：给window添加load
- 例如：等待页面所有资源加载完毕，就回去执行回调函数 （load给window）

~~~html
 <script>
    window.addEventListener('load', function(){
      const btn = document.querySelector('button')
      btn.addEventListener('click', function () {
        alert(11)
    })
  })
  </script>
~~~

- 不光可以监听整个页面资源加载完毕，也可以针对某个资源绑定load事件 (window 换成 img)

~~~html
<script>
img.addEventListener('load', function () {
      // 等待图片加载完毕，再去执行里面的代码
    })
</script>
~~~

#### *DOMContentLoaded 事件 （只要等加载标签）

- 当初始的 HTML 文档被完全加载和解析完成之后，DOMContentLoaded 事件被触发，而==无需等待样式表、图像等完 全加载==

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230901111700216.png" alt="image-20230901111700216" style="zoom:50%;" />

## 3.2 - 元素滚动事件

- 滚动条在滚动的时候待续触发的事件
  - 例如：把页面滚动到摸个区域后做一些处理：固定导航栏，回顶部。。。
- 事件名：`scroll`
- 监听某个元素的内部直接给某个元素加即可，给window或document添加`scroll` 事件

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230901112601532.png" alt="image-20230901112601532" style="zoom:50%;" />

- 使用场景：
  - 我们需要滚动一段距离然后显示隐藏， 可以使用scroll来检测滚动距离

#### * 获取滚动位置（具体滚动多少）

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230901113232522.png" alt="image-20230901113232522" style="zoom:50%;" />

- 属性：scrollLeft和scrollTop
  - 获取被卷去的大小
  - 获取元素内容往左、往上滚出去看不到的距离
  - 这两个值是可**读写**的 （直接给值，需要多少距离）
- 尽量在scroll事件里面获取被卷去的距离

 <img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230901113244846.png" alt="image-20230901113244846" style="zoom:50%;" />

#### *.`documentElement`  HTML文档返回对象

- 开发中，我们经常检测页面滚动的距离，比如页面滚动100像素，就可以显示一个元素，或者固定一个元素

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230901113448390.png" alt="image-20230901113448390" style="zoom:50%;" />

- 可读：可以直接符值（给值）
- 得到的数据： 是数字型 （注意给值的时候要给数字类型）

~~~html
<script>
  // 打开页面就滚到800了
    document.documentElement.scrollTop = 800
    window.addEventListener('scroll', function () {
      // 必须写到里面因为外面没有写scroll -》距离=0
      const n = document.documentElement.scrollTop
      // 得到是什么数据   数字型 不带单位
      // console.log(n)
    })
  </script>
~~~

> 1. 被卷去的头部或左侧用：scrollLeft和scrollTop，可以读取和修改
> 2. 检测页面面滚动的头部距离（被卷去的头部）：`document.documentElement.scrollTop`

- 例如：页面滚动到100px以上（不要写单位）会显示，100以下隐藏

  （获取n必须写在事件里面）

~~~html
<script>
    const div = document.querySelector('div')
    // 页面滚动事件
    window.addEventListener('scroll', function () {
      //检测：  
      // console.log(document.documentElement.scrollTop)
      //给声明， 方便获取给条件
      const n = document.documentElement.scrollTop
      if (n >= 100) {
        div.style.display = 'block'
      } else {
        div.style.display = 'none'
      }
    })
  </script>
~~~

#### ？练习：页面滚动显示隐藏侧边栏

### * scrollTo()：回哪个位置（回顶部）

- 把内容滚动到指定坐标
- 语法：`元素：.scrollTo(x, y)`  （页面还是元素）

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230901115134329.png" alt="image-20230901115134329" style="zoom:50%;" />

- 例如：点击按钮回顶部 `windown.scrollTo(0,0)`

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230901115418151.png" alt="image-20230901115418151" style="zoom:50%;" />

#### ?练习：回顶部

## 3.3 - 页面尺寸事件

- 在窗口尺寸改变的时候触发事件

- 事件：`resize`

  <img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230901115727155.png" alt="image-20230901115727155" style="zoom:50%;" />

- 检测屏幕宽度：

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230901115752367.png" alt="image-20230901115752367" style="zoom:50%;" />

### *获取宽高（clientWidth和clientHeight）

- 获取元素的可见部分宽高（不包含边框，margin，滚动条等）

~~~html
<script>
    const div = document.querySelector('div')
    //只检测内容大小
    console.log(div.clientWidth)
    // resize 浏览器窗口大小发生变化的时候触发的事件
    window.addEventListener('resize', function () {
      console.log(1) // 浏览器窗口大小改变 --- 结果：1
    })
  </script>
~~~



# 四，元素尺寸与位置

- 使用场景：（上面是写固定的距离=800，现在需要n自动计算）
  - 前面案例滚动多少距离，都是我们自己算的，最好是页面滚动到某个元素，就可以做某些事
  - 简单说，就是通过js的方式，==得到元素在页面中的位置==
  - 这样我们可以做，页面滚动到这个位置，就可以做某些操作，省去计算了

## *尺寸：

- 获取宽高：
  - 获取元素的自身宽高、包含元素自身设置的宽高、padding、border
  - offsetWidth和offsetHeight （包含边框，margin，滚动条等 跟 clientWidth和clientHeight 不一样）
  - 注意: 获取的是可视宽高, 如果盒子是隐藏的,获取的结果是0
- 获取位置：
  - 获取元素距离自己定位符元素的左，上距离 （如果祖先有定位， 按照父元素计算）
  
  ~~~html
  * margin: 50px; p的左侧离div左侧 = 50 （p是div的子元素）
  <script>
      const div = document.querySelector('div')
      const p = document.querySelector('p')
      // console.log(div.offsetLeft)
      // 检测盒子的位置  最近一级带有定位的祖先元素
      console.log(p.offsetLeft) //50
    </script>
  ~~~
  
  - offsetLeft和offsetTop 注意是只读属性 （不能给值）
- 例如：不需要给固定值， entry的offsetTop固定， 所以不要直接给值

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230901120914970.png" alt="image-20230901120914970" style="zoom:50%;" />

# 五，综合案例

## 1 - 仿京东固定导航栏案例

需求：当页面滚动到秒杀模块，导航栏自动滑入，否则滑出 

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230904205430319.png" alt="image-20230904205430319" style="zoom: 25%;" />

分析： 

①：顶部不是隐藏， 是上下滑动 --- 条定位的top（负数往上移 = 盒子高度）

`top: -80px;`

②：绑定事件（window（scroll）--- 获取高度 n（srcollTop）

> document.documentElement ：检测页面滚动的距离

~~~html
 window.addEventListener('scroll', function () {
		const n = document.documentElement.scrollTop
~~~

③：给条件

`header.style.top = n >= sk.offsetTop ? 0 : '-80px'`

> - n >= sk.offsetTop吗 ？0 不是 '-80px'
> - 字符串：数字+文字必须要用字符串