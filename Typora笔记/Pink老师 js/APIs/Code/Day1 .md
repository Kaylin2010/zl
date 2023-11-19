# 01 - 复习

~~~javascript
<script>
    // let obj = {name: 'andy'}
    // let obj = new Object()
    // let obj = {}
    // let num = 10
    // num.age = 20
    // console.log(num)
    // let obj.name = 'andy'
    let arr = ['red', 'green']
    // 1.删除 数组.splice(起始位置, 删除个数)
    // arr.splice(1, 1)
    // console.log(arr)  // splice 修改原来数组
    // 2. 添加  数组.splice(起始位置, 删除个数, 添加的元素)
    // 把 pink 放到 red 和 green中间   
    // 起始位置 要放到的索引号位置
    // arr.splice(1, 0, 'pink')
    arr.splice(1, 0, 'pink', 'hotpink')
    console.log(arr)
  </script>
~~~

# 02 - letorconst

~~~javascript
<script>
    const arr = ['red', 'pink']
    // arr.push('blue')
    // console.log(arr)
    // arr = [1, 2, 4]
    // console.log(arr)  // 错误
  </script>
~~~

# 03 - dom对象

~~~javascript
<body>
  <div>123</div>
  <script>
    const div = document.querySelector('div')
    // 打印对象
    console.dir(div)  // dom 对象
  </script>
</body>
~~~

# 04 - 获取DOM元素

~~~javascript
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    .box {
      width: 200px;
      height: 200px;
    }
  </style>
</head>

<body>
  <div class="box">123</div>
  <div class="box">abc</div>
  <p id="nav">导航栏</p>
  <ul class="nav">
    <li>测试1</li>
    <li>测试2</li>
    <li>测试3</li>
  </ul>
  <script>
    // 1. 获取匹配的第一个元素
    // const box = document.querySelector('div')
    // const box = document.querySelector('.box')
    // console.log(box)
    // const nav = document.querySelector('#nav')
    // console.log(nav)
    // nav.style.color = 'red'
    // 1. 我要获取第一个小 ulli
    // const li = document.querySelector('ul li:first-child')
    // console.log(li)
    // 2. 选择所有的小li
    // const lis = document.querySelectorAll('ul li')
    // console.log(lis)

    // 1.获取元素
    const lis = document.querySelectorAll('.nav li')
    // console.log(lis)
    for (let i = 0; i < lis.length; i++) {
      console.log(lis[i]) // 每一个小li对象
    }

    const p = document.querySelectorAll('#nav')
    // console.log(p)
    // p[0].style.color = 'red'
  </script>
</body>

</html>
~~~



# 05 - 修改对象内容

~~~javascript
<body>
  <div class="box">我是文字的内容</div>
  <script>
    // const obj = {
    //   name: 'pink老师'
    // }
    // console.log(obj.name)
    // obj.name = 'red老师'
    // 1. 获取元素
    const box = document.querySelector('.box')
    // 2. 修改文字内容  对象.innerText 属性
    // console.log(box.innerText)  // 获取文字内容
    // // box.innerText = '我是一个盒子'  // 修改文字内容
    // box.innerText = '<strong>我是一个盒子</strong>'  // 不解析标签

    // 3. innerHTML 解析标签
    console.log(box.innerHTML)
    // box.innerHTML = '我要更换'
    box.innerHTML = '<strong>我要更换</strong>'
  </script>
</body>
~~~

# 06-随机抽奖案例

~~~javascript
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8" />
  <meta http-equiv="X-UA-Compatible" content="IE=edge" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>年会抽奖</title>
  <style>
    .wrapper {
      width: 840px;
      height: 420px;
      background: url(./images/bg01.jpg) no-repeat center / cover;
      padding: 100px 250px;
      box-sizing: border-box;
    }
  </style>
</head>

<body>
  <div class="wrapper">
    <strong>传智教育年会抽奖</strong>
    <h1>一等奖：<span id="one">???</span></h1>
    <h3>二等奖：<span id="two">???</span></h3>
    <h5>三等奖：<span id="three">???</span></h5>
  </div>
  <script>
    // 1.声明数组
    const personArr = ['周杰伦', '刘德华', '周星驰', 'Pink老师', '张学友']
    // 2. 先做一等奖
    // 2.1 随机数 数组的下标
    const random = Math.floor(Math.random() * personArr.length)
    // console.log(personArr[random])
    // 2.2 获取one 元素 
    const one = document.querySelector('#one')
    // 2.3 把名字给 one
    one.innerHTML = personArr[random]
    // 2.4 删除数组这个名字
    personArr.splice(random, 1)
    // console.log(personArr)


    // 3. 二等奖
    // 2.1 随机数 数组的下标
    const random2 = Math.floor(Math.random() * personArr.length)
    // console.log(personArr[random])
    // 2.2 获取one 元素 
    const two = document.querySelector('#two')
    // 2.3 把名字给 one
    two.innerHTML = personArr[random2]
    // 2.4 删除数组这个名字
    personArr.splice(random2, 1)

    // 4. 三等奖
    // 2.1 随机数 数组的下标
    const random3 = Math.floor(Math.random() * personArr.length)
    // console.log(personArr[random])
    // 2.2 获取one 元素 
    const three = document.querySelector('#three')
    // 2.3 把名字给 one
    three.innerHTML = personArr[random3]
    // 2.4 删除数组这个名字
    personArr.splice(random3, 1)
  </script>
</body>

</html>
~~~

# 07-修改元素常见的属性

~~~javascript
<body>
  <img src="./images/1.webp" alt="">
  <script>
    // 1. 获取图片元素
    const img = document.querySelector('img')
    // 2. 修改图片对象的属性   对象.属性 = 值
    img.src = './images/2.webp'
    img.title = 'pink老师的艺术照'
  </script>
</body>
~~~

# 08-随机显示图片

~~~javascript
<body>
  <img src="./images/1.webp" alt="">
  <script>
    // 取到 N ~ M 的随机整数
    function getRandom(N, M) {
      return Math.floor(Math.random() * (M - N + 1)) + N
    }
    // 1. 获取图片对象
    const img = document.querySelector('img')
    // 2. 随机得到序号
    const random = getRandom(1, 6)
    // 3. 更换路径
    img.src = `./images/${random}.webp`
  </script>
</body>
~~~

# 09-通过style属性修改样式

~~~javascript
<style>
    .box {
      width: 200px;
      height: 200px;
      background-color: pink;
    }
  </style>
</head>

<body>
  <div class="box"></div>
  <script>
    // 1. 获取元素
    const box = document.querySelector('.box')
    //2. 修改样式属性 对象.style.样式属性 = '值'  别忘了跟单位
    box.style.width = '300px'
    // 多组单词的采取 小驼峰命名法
    box.style.backgroundColor = 'hotpink'
    box.style.border = '2px solid blue'
    box.style.borderTop = '2px solid red'
  </script>
</body>
~~~

# 10-随机更换背景图片

~~~javascript
body {
      background: url(./images/desktop_1.jpg) no-repeat top center/cover;
    }
  </style>
</head>

<body>
  <script>
    // console.log(document.body)
    // 取到 N ~ M 的随机整数
    function getRandom(N, M) {
      return Math.floor(Math.random() * (M - N + 1)) + N
    }
    // 随机数
    const random = getRandom(1, 10)
    document.body.style.backgroundImage = `url(./images/desktop_${random}.jpg)`
  </script>
</body>

~~~

# 11-通过类名修改样式

~~~javascript
<style>
    div {
      width: 200px;
      height: 200px;
      background-color: pink;
    }

    .nav {
      color: red;
    }

    .box {
      width: 300px;
      height: 300px;
      background-color: skyblue;
      margin: 100px auto;
      padding: 10px;
      border: 1px solid #000;
    }
  </style>
~~~

# 12-通过classList修改样式

~~~javascript
 <style>
    .box {
      width: 200px;
      height: 200px;
      color: #333;
    }

    .active {
      color: red;
      background-color: pink;
    }
  </style>
</head>

<body>
  <div class="box active">文字</div>
  <script>
    // 通过classList添加
    // 1. 获取元素
    const box = document.querySelector('.box')
    // 2. 修改样式
    // 2.1 追加类 add() 类名不加点，并且是字符串
    // box.classList.add('active')
    // 2.2 删除类  remove() 类名不加点，并且是字符串
    // box.classList.remove('box')
    // 2.3 切换类  toggle()  有还是没有啊， 有就删掉，没有就加上
    box.classList.toggle('active')
  </script>
</body>

~~~

# 13-随机轮播图

~~~javascript
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8" />
  <meta http-equiv="X-UA-Compatible" content="IE=edge" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>轮播图点击切换</title>
  <style>
    * {
      box-sizing: border-box;
    }

    .slider {
      width: 560px;
      height: 400px;
      overflow: hidden;
    }

    .slider-wrapper {
      width: 100%;
      height: 320px;
    }

    .slider-wrapper img {
      width: 100%;
      height: 100%;
      display: block;
    }

    .slider-footer {
      height: 80px;
      background-color: rgb(100, 67, 68);
      padding: 12px 12px 0 12px;
      position: relative;
    }

    .slider-footer .toggle {
      position: absolute;
      right: 0;
      top: 12px;
      display: flex;
    }

    .slider-footer .toggle button {
      margin-right: 12px;
      width: 28px;
      height: 28px;
      appearance: none;
      border: none;
      background: rgba(255, 255, 255, 0.1);
      color: #fff;
      border-radius: 4px;
      cursor: pointer;
    }

    .slider-footer .toggle button:hover {
      background: rgba(255, 255, 255, 0.2);
    }

    .slider-footer p {
      margin: 0;
      color: #fff;
      font-size: 18px;
      margin-bottom: 10px;
    }

    .slider-indicator {
      margin: 0;
      padding: 0;
      list-style: none;
      display: flex;
      align-items: center;
    }

    .slider-indicator li {
      width: 8px;
      height: 8px;
      margin: 4px;
      border-radius: 50%;
      background: #fff;
      opacity: 0.4;
      cursor: pointer;
    }

    .slider-indicator li.active {
      width: 12px;
      height: 12px;
      opacity: 1;
    }
  </style>
</head>

<body>
  <div class="slider">
    <div class="slider-wrapper">
      <img src="./images/slider01.jpg" alt="" />
    </div>
    <div class="slider-footer">
      <p>对人类来说会不会太超前了？</p>
      <ul class="slider-indicator">
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
      </ul>
      <div class="toggle">
        <button class="prev">&lt;</button>
        <button class="next">&gt;</button>
      </div>
    </div>
  </div>
  <script>
    // const arr = [1, 3]
    // arr[0]
    // 1. 初始数据
    const sliderData = [
      { url: './images/slider01.jpg', title: '对人类来说会不会太超前了？', color: 'rgb(100, 67, 68)' },
      { url: './images/slider02.jpg', title: '开启剑与雪的黑暗传说！', color: 'rgb(43, 35, 26)' },
      { url: './images/slider03.jpg', title: '真正的jo厨出现了！', color: 'rgb(36, 31, 33)' },
      { url: './images/slider04.jpg', title: '李玉刚：让世界通过B站看到东方大国文化', color: 'rgb(139, 98, 66)' },
      { url: './images/slider05.jpg', title: '快来分享你的寒假日常吧~', color: 'rgb(67, 90, 92)' },
      { url: './images/slider06.jpg', title: '哔哩哔哩小年YEAH', color: 'rgb(166, 131, 143)' },
      { url: './images/slider07.jpg', title: '一站式解决你的电脑配置问题！！！', color: 'rgb(53, 29, 25)' },
      { url: './images/slider08.jpg', title: '谁不想和小猫咪贴贴呢！', color: 'rgb(99, 72, 114)' },
    ]




    // 1. 需要一个随机数 
    const random = parseInt(Math.random() * sliderData.length)
    // console.log(sliderData[random])
    // 2. 把对应的数据渲染到标签里面
    // 2.1 获取图片
    const img = document.querySelector('.slider-wrapper img')
    // 2.2. 修改图片路径  =  对象.url
    img.src = sliderData[random].url
    // 3. 把p里面的文字内容更换
    // 3.1 获取p
    const p = document.querySelector('.slider-footer p')
    // 3.2修改p
    p.innerHTML = sliderData[random].title
    // 4. 修改背景颜色
    const footer = document.querySelector('.slider-footer')
    footer.style.backgroundColor = sliderData[random].color
    // 5. 小圆点
    const li = document.querySelector(`.slider-indicator li:nth-child(${random + 1})`)
    // 让当前这个小li 添加 active这个类
    li.classList.add('active')
  </script>
</body>

</html>
~~~

# 14-修改表单属性

~~~javascript
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>

<body>
  <!-- <input type="text" value="电脑"> -->
  <input type="checkbox" name="" id="">
  <button>点击</button>
  <script>
    // 1 获取元素
    // const uname = document.querySelector('input')
    // 2. 获取值  获取表单里面的值 用的  表单.value
    // console.log(uname.value) // 电脑
    // console.log(uname.innerHTML)  innertHTML 得不到表单的内容
    // 3. 设置表单的值
    // uname.value = '我要买电脑'
    // console.log(uname.type)
    // uname.type = 'password'

    // 1. 获取
    const ipt = document.querySelector('input')
    // console.log(ipt.checked)  // false   只接受布尔值
    ipt.checked = true
    // ipt.checked = 'true'  // 会选中，不提倡  有隐式转换

    // 1.获取
    const button = document.querySelector('button')
    // console.log(button.disabled)  // 默认false 不禁用
    button.disabled = true   // 禁用按钮

  </script>
</body>

</html>
~~~

# 15-自定义属性

~~~javascript
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>

<body>
  <div data-id="1" data-spm="不知道">1</div>
  <div data-id="2">2</div>
  <div data-id="3">3</div>
  <div data-id="4">4</div>
  <div data-id="5">5</div>
  <script>
    const one = document.querySelector('div')
    console.log(one.dataset.id)  // 1
    console.log(one.dataset.spm)  // 不知道
  </script>
</body>

</html>
~~~

# 16-定时器-间歇函数

~~~javascript
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>

<body>
  <script>
    // setInterval(函数, 间隔时间)
    // setInterval(function () {
    //   console.log('一秒执行一次')
    // }, 1000)

    function fn() {
      console.log('一秒执行一次')
    }
    // setInterval(函数名, 间隔时间)  函数名不要加小括号
    let n = setInterval(fn, 1000)
    // setInterval('fn()', 1000)
    console.log(n)
    // 关闭定时器
    clearInterval(n)

    // let m = setInterval(function () {
    //   console.log(11)
    // }, 2000)
    // console.log(m)

    // const num = 10
    // num = 10
    // console.log(num)
    let i = 1
    setInterval(function () {
      i++
      document.write(`${i} `)
    }, 200)
  </script>
</body>

</html>
~~~

# 17-用户注册倒计时

~~~javascript
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>

<body>
    <textarea name="" id="" cols="30" rows="10">
        用户注册协议
        欢迎注册成为京东用户！在您注册过程中，您需要完成我们的注册流程并通过点击同意的形式在线签署以下协议，请您务必仔细阅读、充分理解协议中的条款内容后再点击同意（尤其是以粗体或下划线标识的条款，因为这些条款可能会明确您应履行的义务或对您的权利有所限制）。
        【请您注意】如果您不同意以下协议全部或任何条款约定，请您停止注册。您停止注册后将仅可以浏览我们的商品信息但无法享受我们的产品或服务。如您按照注册流程提示填写信息，阅读并点击同意上述协议且完成全部注册流程后，即表示您已充分阅读、理解并接受协议的全部内容，并表明您同意我们可以依据协议内容来处理您的个人信息，并同意我们将您的订单信息共享给为完成此订单所必须的第三方合作方（详情查看
    </textarea>
    <br>
    <button class="btn" disabled>我已经阅读用户协议(5)</button>
    <script>
        // 1. 获取元素
        const btn = document.querySelector('.btn')
        // console.log(btn.innerHTML)  butto按钮特殊用innerHTML
        // 2. 倒计时
        let i = 5
        // 2.1 开启定时器
        let n = setInterval(function () {
            i--
            btn.innerHTML = `我已经阅读用户协议(${i})`
            if (i === 0) {
                clearInterval(n)  // 关闭定时器
                // 定时器停了，我就可以开按钮
                btn.disabled = false
                btn.innerHTML = '同意'
            }
        }, 1000)

    </script>
</body>

</html>
~~~

# 18-轮播图定时版

~~~javascript
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8" />
  <meta http-equiv="X-UA-Compatible" content="IE=edge" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>轮播图点击切换</title>
  <style>
    * {
      box-sizing: border-box;
    }

    .slider {
      width: 560px;
      height: 400px;
      overflow: hidden;
    }

    .slider-wrapper {
      width: 100%;
      height: 320px;
    }

    .slider-wrapper img {
      width: 100%;
      height: 100%;
      display: block;
    }

    .slider-footer {
      height: 80px;
      background-color: rgb(100, 67, 68);
      padding: 12px 12px 0 12px;
      position: relative;
    }

    .slider-footer .toggle {
      position: absolute;
      right: 0;
      top: 12px;
      display: flex;
    }

    .slider-footer .toggle button {
      margin-right: 12px;
      width: 28px;
      height: 28px;
      appearance: none;
      border: none;
      background: rgba(255, 255, 255, 0.1);
      color: #fff;
      border-radius: 4px;
      cursor: pointer;
    }

    .slider-footer .toggle button:hover {
      background: rgba(255, 255, 255, 0.2);
    }

    .slider-footer p {
      margin: 0;
      color: #fff;
      font-size: 18px;
      margin-bottom: 10px;
    }

    .slider-indicator {
      margin: 0;
      padding: 0;
      list-style: none;
      display: flex;
      align-items: center;
    }

    .slider-indicator li {
      width: 8px;
      height: 8px;
      margin: 4px;
      border-radius: 50%;
      background: #fff;
      opacity: 0.4;
      cursor: pointer;
    }

    .slider-indicator li.active {
      width: 12px;
      height: 12px;
      opacity: 1;
    }
  </style>
</head>

<body>
  <div class="slider">
    <div class="slider-wrapper">
      <img src="./images/slider01.jpg" alt="" />
    </div>
    <div class="slider-footer">
      <p>对人类来说会不会太超前了？</p>
      <ul class="slider-indicator">
        <li class="active"></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
        <li></li>
      </ul>
      <div class="toggle">
        <button class="prev">&lt;</button>
        <button class="next">&gt;</button>
      </div>
    </div>
  </div>
  <script>
    // 1. 初始数据
    const sliderData = [
      { url: './images/slider01.jpg', title: '对人类来说会不会太超前了？', color: 'rgb(100, 67, 68)' },
      { url: './images/slider02.jpg', title: '开启剑与雪的黑暗传说！', color: 'rgb(43, 35, 26)' },
      { url: './images/slider03.jpg', title: '真正的jo厨出现了！', color: 'rgb(36, 31, 33)' },
      { url: './images/slider04.jpg', title: '李玉刚：让世界通过B站看到东方大国文化', color: 'rgb(139, 98, 66)' },
      { url: './images/slider05.jpg', title: '快来分享你的寒假日常吧~', color: 'rgb(67, 90, 92)' },
      { url: './images/slider06.jpg', title: '哔哩哔哩小年YEAH', color: 'rgb(166, 131, 143)' },
      { url: './images/slider07.jpg', title: '一站式解决你的电脑配置问题！！！', color: 'rgb(53, 29, 25)' },
      { url: './images/slider08.jpg', title: '谁不想和小猫咪贴贴呢！', color: 'rgb(99, 72, 114)' },
    ]
    // 1. 获取元素 
    const img = document.querySelector('.slider-wrapper img')
    const p = document.querySelector('.slider-footer p')
    let i = 0  // 信号量 控制图片的张数
    // 2. 开启定时器
    // console.log(sliderData[i])  拿到对应的对象啦
    setInterval(function () {
      i++
      // 无缝衔接位置  一共八张图片，到了最后一张就是 8， 数组的长度就是 8
      if (i >= sliderData.length) {
        i = 0
      }
      // console.log(i)
      // console.log(sliderData[i])
      // 更换图片路径  
      img.src = sliderData[i].url
      // 把字写到 p里面
      p.innerHTML = sliderData[i].title
      // 小圆点
      // 先删除以前的active
      document.querySelector('.slider-indicator .active').classList.remove('active')
      // 只让当前li添加active
      document.querySelector(`.slider-indicator li:nth-child(${i + 1})`).classList.add('active')
    }, 1000)

  </script>
</body>

</html>
~~~



