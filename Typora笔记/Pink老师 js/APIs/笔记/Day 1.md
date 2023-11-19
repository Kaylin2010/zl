# 一， Web API基础认知

- 变量声明：建议使用 const
- 建议数组 跟 对象 用const声明，如果后面发现需要修改，再改成let

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230823094457539.png" alt="image-20230823094457539" style="zoom:50%;" />

- const 声明的值不能更改， 而且const声明变量需要里面进行初始化
- 对于引用数字类型，const 声明的变量，里面存的==不是值 是 地址==



<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230823094857511.png" alt="image-20230823094857511" style="zoom:50%;" />

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230823094959673.png" alt="image-20230823094959673" style="zoom:50%;" />

### 1.1 API作用 和 分类

- 作用：使用JS去操作html 和 浏览器
- 分类： DOM（文档对象模型）， BOM （浏览器对象模型）

### 1.2 什么是DOM

- 用来呈现以及与任意 HTML 或 XML 文档交互API， 浏览器提供一套专门操作网页内容法人功能
- DOM 作用：开发网页内容特效和实现用户交互

### 1.3 DOM 树

- 将HTML文档以树状结构直观的表现出来（ 标签与标签之间的关系）， 描述网页内容关系的名词

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230823095808747.png" alt="image-20230823095808747" style="zoom:50%;" />

### ==1.4 DOM对象==

- DOM 对象：浏览器根据HTML标签生成的JS对象
  - 所有的标签都可以在这个对象找到
  - 修改这个对象的属性会自动改到标签身上
- DOM的核心思想： 把网页内容当做对象来处理
- document 对象：
  - 是最大一个对象
  - 所以它提供的属性和方法都是用来访问和操作网页内容
  - 页面的所有内容都在document里面

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230823100559385.png" alt="image-20230823100559385" style="zoom:50%;" />

# 二，获取DOM对象

## 2.1 根据CSS选择器来获取DOM元素

### 2.1.1 选择匹配的第一元素

- 语法：querySelector（查询选择器），只选一个

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230823100923825.png" alt="image-20230823100923825" style="zoom:50%;" />

- 参数：包含一个或者多个有效的CSS选择器 字符串

- 返回值：CSS选择器第一个元素， 没有匹配到返回null

  <img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230823101415860.png" alt="image-20230823101415860" style="zoom:50%;" />

### 2.1.2 选择匹配的多元素

- 语法 （css选择器不许加引号）

  <img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230823101708962.png" alt="image-20230823101708962" style="zoom:50%;" />

- 参数： 包含一个或多个有效的CSS选择器 字符串

- 返回值： CSS选择器匹配的NodeList 对象集合 

~~~javascript
<body>
  <ul class="nav">
    <li>测试1</li>
    <li>测试2</li>
    <li>测试3</li>
  </ul>
  <script>
    const ok = document.querySelectorAll('.nav li')
    console.log(ok)  //NodeList(3) [li, li, li]
  </script>
</body>
~~~



> 1. 获取一个DOM元素可以直接修改
>
> ~~~javascript
> // const nav = document.querySelector('#nav')
>     // console.log(nav)
>     // nav.style.color = 'red'
> ~~~
>
> 2. 获取多个DOM不能直接修改要通过遍历方式

- 伪数组：（querySelectorAll 获取结果）·
  - 有长度有下标的数组
  - 但是没有push， pop等数组方法
  - 想得到里面的每个对象，要用遍历（for）的方式获取

# 三，操作元素内容

- 如果需要修改标签元素里面的内容， 可以使用如下方式：
  - .对象.innerText 属性
  - 对象.innerHTML 属性

> 1. 需要先获取元素， 再进行修改
> 2. 修改文字内容： ==对象.innerText 属性==

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230823103342480.png" alt="image-20230823103342480" style="zoom:50%;" />

## 3.1 元素innerText 属性

- 将文本内容添加， 更新到任意标签位置
- 显示纯文本， 不解析标签

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230823103717956.png" alt="image-20230823103717956" style="zoom:50%;" />

## 3.2 元素.innerHTML  属性

- 将文本内容添加， 更新到任意标签位置
- 会分析标签，多标签建议使用模版字符 

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230823103939984.png" alt="image-20230823103939984" style="zoom:50%;" />

### ***练习：

需求：从数组随机抽取一等奖、二等奖和三等奖，显示到对应的标签里面

* 数组名单 '周杰伦', '刘德华', '周星驰', 'Pink老师', '张学友'

> 3个步骤：
>
> 1. 随机抽奖 （ radom）
> 2. 把结果获取到页面对应标签（ 指出来哪个标签， 文字获取出来）
> 3. 删除这个名字（在原来的数组伤处掉， 下一个等奖抽的时候没有这个名字了）

~~~javascript
<script>
    // 1.声明
    const personArr = ['周杰伦', '刘德华', '周星驰', 'Pink老师', '张学友']

    // 一等奖
    const random = Math.floor(Math.random() * personArr.length)
    // console.log(personArr[random]);
    // 获取一等奖数据
    const one = document.querySelector('#one')
    // 获取文字内容显示出来
    one.innerHTML = personArr[random]
    // 删除数组的这个名字呢
    personArr.splice(random, 1)
    console.log(personArr)
  </script>

~~~



# 四，操作元素属性

## 4.1 操作元素通常属性

- 通过js修改/ 设置标签的属性
- 语法： 

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230823141803275.png" alt="image-20230823141803275" style="zoom:50%;" />

~~~javascript
<body>
  <img src="./图片/anh 1 .png" alt="">
  <script>
    // 第一步必须要值出来需要修改的标签
    const pic = document.querySelector('img')
    // 记得修改内容加引号
    pic.src = './图片/anh 2.png'
  </script>
</body>
~~~

### ***练习： 页面刷新，图片随机更换

~~~javascript
<script>
    //用图片的名random
    function getRandom(N, M) {
      return Math.floor(Math.random() * (M - N + 1)) + N
    }
    const img = document.querySelector('img')
    //return 声明再调用, 取random的值
    //给图片序号对应的值
    const random = getRandom(1, 6)
    //使用模版字符串 ``
    img.src = `./images/${random}.webp`  
  </script>
~~~



> 1. 随机取图片（用公式）-- random图片的名字 （1.webp，2.webp...）
> 2. 获取图片（找出来）
> 3. 获取随机的数据 （random的调用）
> 4. 更换路径 （使用模版字符串 ``）

## 4.2 操作元素样式属性

### 4.2.1 通过style属性操作css

- 语法：

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230823145846579.png" alt="image-20230823145846579" style="zoom:50%;" />

~~~javascript
<script>
    const okok = document.querySelector('.box')
    （2）// 如果有连接符， 需要换成小驼峰命名法
    okok.style.backgroundColor = 'red'
    （3）// 记得加css单位
    okok.style.width = '30px'
  </script>
~~~

> 1. 修改样式通过style属性引出
> 2. 如果属性有-连接符，需要转换为小驼峰命名法
> 3. 赋值的时候，需要的时候不要忘记加css单位

#### ***练习：页面刷新，页面随机更换背景图片

~~~javascript
 <script>
    //用图片的名random
    function getRandom(N, M) {
      return Math.floor(Math.random() * (M - N + 1)) + N
    }
    const img = document.querySelector('img')
    //return 声明再调用, 取random的值
    //给图片序号对应的值
    const random = getRandom(1, 10)
    //打印用 document.标签.style.属性
    document.body.style.backgroundImage = `url(./images/desktop_${random}.jpg)` 
  </script>
~~~



### 4.2.2 操作类名（className）操作css

- 如果修改的样式比较多，直接通过style属性修改比较繁琐，我们可以通过借助于css类名的形式
- 语法：

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230823154506987.png" alt="image-20230823154506987" style="zoom:50%;" />

~~~javascript
<script>
    const helo = document.querySelector('div')
    // 语法： 变量名.className
    helo.className = 'box'  //box: 新的样式
    （1，2）// 已经有的样式要写进来， 要不然会被覆盖
    helo.className = 'nav box' //nav：div的class名
  </script>
~~~

> 1. 由于class是关键字, 所以使用className去代替
> 2. className是使用新值换旧值, 如果需要添加一个类,需要保留之前的类名

4.2.3 操作类名（classList）操作css

- 为了解决className 容易覆盖以前的类名，我们可以通过classList方式追加和删除类名

  - 追加：现有的样式不会被覆盖
  - 删除： 删除现有的样式
  - 切换： 有就删掉，没有就加上

  <img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230823161838051.png" alt="image-20230823161838051" style="zoom: 33%;" />

- 语法：

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230823161714619.png" alt="image-20230823161714619" style="zoom:50%;" />

> - 使用 className 和classList的区别
>   - 修改大量样式的更方便 （classList）
>   - 修改大量样式的更方便
>   - classList 是追加和删除不影响以前类名
> - 类名都不要加点号
> - const helo （对象） = document.querySelector('div')



## 4.3 操作表单元素属性

- 修改表单的属性， 本质就是把表单类型转换为文本框
- 获取： DOM对象.属性名
- 设置： DOM对象.属性名 = 新值
- 表单属性中添加就有效果,移除就没有效果,一律使用布尔值表示 如果为true 代表添加了该属性如果是false代表移除了该属性，比如： disabled、checked、selected

~~~javascript
<script>
    /* 文本框 */
    // 1.获取
    // const uname = document.querySelector('input')
    // 2.获取内容语法 对象.value (value 给后台数据)
    // console.log(uname.value)
    // 3.设置表单的值
    // 修改文本框的默认值
    // uname.value = 'hello' //重新赋值
    // 修改表单类型
    // uname.type = 'password'

    /* 多选框 */
    // 1. 获取
    const ipt = document.querySelector('input')
    // console.log(ipt.checked)
    ipt.checked = true // 选中不需要加引号

    /* 按钮 */
    const button = document.querySelector('button')
    // disabled：会禁用吗？ false： 不会， true： 会
    button.disabled = false //默认不禁用
  
  </script>
~~~

> 1. 获取表单内容，设置文本框的内容： 对象.value
> 2. 勾选状态： 对象.checked （false： 不勾选 - 默认， true：勾选）
> 3. 禁用点击按钮：对象.disabled （disabled：会禁用吗？ false： 不会， true： 会）

## 4.4 自定义属性

- 标签天生自带的属性 比如class id title等, 可以直接使用点语法操作比如：disabled、checked、selected

~~~javascript
<body>
  <div data-id="6">1</div>
  <div data-id="2">2</div>
  <div data-id="3">3</div>
  <div data-id="4">4</div>
  <div data-id="5">5</div>
  <script>
    // 1.获取
    const okok = document.querySelector('div')
    console.log(okok.dataset) // id: '6' 第一个id
    console.log(okok.dataset.id) // 6    第一个的值
  </script>
</body>
~~~

> - 在html5中推出来了专门的data-自定义属性
> - 在标签上一律以==data-开头==
> - 在DOM对象上一律以==dataset对象方式获取==

# 五，定时器 - 间歇函数

- 每个一点时间需要自动执行一段代码， 不需要手动去触发
- 定时器函数可以关闭和开启

## 5.1 开启定时器

- 作用：每隔一段时间调用这个函数
- 间隔时间单位是毫秒

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230824120338591.png" alt="image-20230824120338591" style="zoom:50%;" />

~~~javascript
<script>
    /* 开启定时器第一种方法 */
    // setInterval(function () {
    //   console.log('okok2')
    // }, 1000)

    /* 开启定时器第二种方法 */
    function fn() {
      console.log('okok di ')
    }
    // 声明， 开启
    let n = setInterval(fn, 1000)  （1）
    // 每个定时子有自己的id
    // console.log(n) //1           （2）

    /* 关闭定时器 */
    // 需要声明函才能关闭
    clearInterval(n)

  </script>
~~~

> 1. 函数名字不需要加括号 
> 2. 定时器返回的是一个id数字
> 3. 开启定时器有2个办法， 第二个可以声明方便关闭定时器

## 5.2 关闭定时器

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230825153919105.png" alt="image-20230825153919105" style="zoom:50%;" />
