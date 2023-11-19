# 01 - 复习

~~~html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>

  </style>
</head>

<body>
  <ul>
    <li class="item">1</li>
    <li class="item">2</li>
    <li class="item">3</li>
  </ul>
  <script>
    const lis = document.querySelectorAll('.item')
    // lis[0].classList.add('active')
    console.log(lis)
    for (let i = 0; i < lis.length; i++) {
      lis[i].style.color = 'red'
    }
  </script>
</body>

</html>
~~~

# 02 - 事件监听

~~~html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>

<body>
  <button>点击</button>
  <script>
    // 需求： 点击了按钮，弹出一个对话框
    // 1. 事件源   按钮  
    // 2.事件类型 点击鼠标   click 字符串
    // 3. 事件处理程序 弹出对话框
    const btn = document.querySelector('button')
    btn.addEventListener('click', function () {
      alert('你早呀~')
    })

  </script>
</body>

</html>
~~~

# 03-点击关闭

~~~html
李伟兴 09:31:13
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    .box {
      position: relative;
      width: 1000px;
      height: 200px;
      background-color: pink;
      margin: 100px auto;
      text-align: center;
      font-size: 50px;
      line-height: 200px;
      font-weight: 700;
    }

    .box1 {
      position: absolute;
      right: 20px;
      top: 10px;
      width: 20px;
      height: 20px;
      background-color: skyblue;
      text-align: center;
      line-height: 20px;
      font-size: 16px;
//鼠标变成小手， 可点击
      cursor: pointer;
    }
  </style>
</head>

<body>
  <div class="box">
    我是广告
    <div class="box1">X</div>
  </div>
  <script>
    // 1. 获取事件源
    const box1 = document.querySelector('.box1')
    //  关闭的是大盒子
    const box = document.querySelector('.box')
    // 2. 事件侦听
    box1.addEventListener('click', function () {
      //关闭的是box， 样式给box，隐藏用display
      box.style.display = 'none'
    })
  </script>
</body>

</html>
~~~

# 04-随机点名案例

~~~html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        * {
            margin: 0;
            padding: 0;
        }

        h2 {
            text-align: center;
        }

        .box {
            width: 600px;
            margin: 50px auto;
            display: flex;
            font-size: 25px;
            line-height: 40px;
        }

        .qs {

            width: 450px;
            height: 40px;
            color: red;

        }

        .btns {
            text-align: center;
        }

        .btns button {
            width: 120px;
            height: 35px;
            margin: 0 50px;
        }
    </style>
</head>

<body>
    <h2>随机点名</h2>
    <div class="box">
        <span>名字是：</span>
        <div class="qs">这里显示姓名</div>
    </div>
    <div class="btns">
        <button class="start">开始</button>
        <button class="end">结束</button>
    </div>

    <script>
        // 数据数组
        const arr = ['马超', '黄忠', '赵云', '关羽', '张飞']
        // 定时器的全局变量
        let timerId = 0
        // 随机号要全局变量
        let random = 0
        // 业务1.开始按钮模块
        const qs = document.querySelector('.qs')
        // 1.1 获取开始按钮对象
        const start = document.querySelector('.start')
        // 1.2 添加点击事件
        start.addEventListener('click', function () {
            timerId = setInterval(function () {
                // 随机数
                random = parseInt(Math.random() * arr.length)
                // console.log(arr[random])
                qs.innerHTML = arr[random]
            }, 35)
            // 如果数组里面只有一个值了，还需要抽取吗？  不需要  让两个按钮禁用就可以
            if (arr.length === 1) {
                // start.disabled = true
                // end.disabled = true
                start.disabled = end.disabled = true
            }
        })

        // 2. 关闭按钮模块
        const end = document.querySelector('.end')
        end.addEventListener('click', function () {
            clearInterval(timerId)
            // 结束了，可以删除掉当前抽取的那个数组元素
            arr.splice(random, 1)
            console.log(arr)
        })
    </script>
</body>

</html>
~~~

# 05-解惑

~~~html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>

<body>
  <button>点击</button>
  <script>
    const btn = document.querySelector('button')
    btn.addEventListener('click', function () {
      const num = Math.random()
      console.log(num)
    })
  </script>
</body>

</html>
~~~

# 06-事件监听方法

~~~html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>

<body>
  <button>点击</button>
  <script>
    const btn = document.querySelector('button')
    // btn.onclick = function () {
    //   alert(11)
    // }
    // btn.onclick = function () {
    //   alert(22)
    // }

    // let num = 10
    // num = 20
    btn.addEventListener('click', function () {
      alert(11)
    })
    btn.addEventListener('click', function () {
      alert(22)
    })
  </script>
</body>

</html>
~~~

# 07-事件类型

~~~html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    div {
      width: 200px;
      height: 200px;
      background-color: pink;
    }
  </style>
</head>

<body>
  <div></div>
  <script>
    const div = document.querySelector('div')
    // 鼠标经过
    div.addEventListener('mouseenter', function () {
      console.log(`轻轻的我来了`)
    })
    // 鼠标离开
    div.addEventListener('mouseleave', function () {
      console.log(`轻轻的我走了`)
    })

  </script>
</body>

</html>
~~~

# 08-轮播图完整版

~~~html
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
    const data = [
      { url: './images/slider01.jpg', title: '对人类来说会不会太超前了？', color: 'rgb(100, 67, 68)' },
      { url: './images/slider02.jpg', title: '开启剑与雪的黑暗传说！', color: 'rgb(43, 35, 26)' },
      { url: './images/slider03.jpg', title: '真正的jo厨出现了！', color: 'rgb(36, 31, 33)' },
      { url: './images/slider04.jpg', title: '李玉刚：让世界通过B站看到东方大国文化', color: 'rgb(139, 98, 66)' },
      { url: './images/slider05.jpg', title: '快来分享你的寒假日常吧~', color: 'rgb(67, 90, 92)' },
      { url: './images/slider06.jpg', title: '哔哩哔哩小年YEAH', color: 'rgb(166, 131, 143)' },
      { url: './images/slider07.jpg', title: '一站式解决你的电脑配置问题！！！', color: 'rgb(53, 29, 25)' },
      { url: './images/slider08.jpg', title: '谁不想和小猫咪贴贴呢！', color: 'rgb(99, 72, 114)' },
    ]
    // 获取元素
    const img = document.querySelector('.slider-wrapper img')
    const p = document.querySelector('.slider-footer p')
    const footer = document.querySelector('.slider-footer')
    // 1. 右按钮业务
    // 1.1 获取右侧按钮 
    const next = document.querySelector('.next')
    let i = 0  // 信号量 控制播放图片张数
    // 1.2 注册点击事件

    next.addEventListener('click', function () {
      // console.log(11)
      i++
      // 1.6判断条件  如果大于8 就复原为 0
      // if (i >= 8) {
      //   i = 0
      // }
      i = i >= data.length ? 0 : i
      // 1.3 得到对应的对象
      // console.log(data[i])
      // 调用函数
      toggle()
    })

    // 2. 左侧按钮业务
    // 2.1 获取左侧按钮 
    const prev = document.querySelector('.prev')
    // 1.2 注册点击事件
    prev.addEventListener('click', function () {
      i--
      // 判断条件  如果小于0  则爬到最后一张图片索引号是 7
      // if (i < 0) {
      //   i = 7
      // }
      i = i < 0 ? data.length - 1 : i
      // 1.3 得到对应的对象
      // console.log(data[i])
      // 调用函数
      toggle()
    })

    // 声明一个渲染的函数作为复用
    function toggle() {
      // 1.4 渲染对应的数据
      img.src = data[i].url
      p.innerHTML = data[i].title
      footer.style.backgroundColor = data[i].color
      // 1.5 更换小圆点    先移除原来的类名， 当前li再添加 这个 类名
      document.querySelector('.slider-indicator .active').classList.remove('active')
      document.querySelector(`.slider-indicator li:nth-child(${i + 1})`).classList.add('active')
    }



    // 3. 自动播放模块
    let timerId = setInterval(function () {
      // 利用js自动调用点击事件  click()  一定加小括号调用函数
      next.click()
    }, 1000)


    // 4. 鼠标经过大盒子，停止定时器
    const slider = document.querySelector('.slider')
    // 注册事件
    slider.addEventListener('mouseenter', function () {
      // 停止定时器
      clearInterval(timerId)
    })

    // 5. 鼠标离开大盒子，开启定时器
    // 注册事件
    slider.addEventListener('mouseleave', function () {
      // 停止定时器
      if (timerId) clearInterval(timerId)
      // 开启定时器
      timerId = setInterval(function () {
        // 利用js自动调用点击事件  click()  一定加小括号调用函数
        next.click()
      }, 1000)
    })
  </script>
</body>

</html>
~~~

# 09-焦点事件

~~~html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>

<body>
  <input type="text">
  <script>
    const input = document.querySelector('input')
    input.addEventListener('focus', function () {
      console.log('有焦点触发')
    })
    input.addEventListener('blur', function () {
      console.log('失去焦点触发')
    })
  </script>
</body>

</html>
~~~

# 10-小米搜索框

~~~html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        ul {

            list-style: none;
        }

        .mi {
            position: relative;
            width: 223px;
            margin: 100px auto;
        }

        .mi input {
            width: 223px;
            height: 48px;
            padding: 0 10px;
            font-size: 14px;
            line-height: 48px;
            border: 1px solid #e0e0e0;
            outline: none;
        }

        .mi .search {
            border: 1px solid #ff6700;
        }

        .result-list {
            display: none;
            position: absolute;
            left: 0;
            top: 48px;
            width: 223px;
            border: 1px solid #ff6700;
            border-top: 0;
            background: #fff;
        }

        .result-list a {
            display: block;
            padding: 6px 15px;
            font-size: 12px;
            color: #424242;
            text-decoration: none;
        }

        .result-list a:hover {
            background-color: #eee;
        }
    </style>

</head>

<body>
    <div class="mi">
        <input type="search" placeholder="小米笔记本">
        <ul class="result-list">
            <li><a href="#">全部商品</a></li>
            <li><a href="#">小米11</a></li>
            <li><a href="#">小米10S</a></li>
            <li><a href="#">小米笔记本</a></li>
            <li><a href="#">小米手机</a></li>
            <li><a href="#">黑鲨4</a></li>
            <li><a href="#">空调</a></li>
        </ul>
    </div>
    <script>
        // 1. 获取元素
        const input = document.querySelector('[type=search]')
        const ul = document.querySelector('.result-list')
        // console.log(input)
        // 2. 监听事件 获得焦点
        input.addEventListener('focus', function () {
            // ul显示
            ul.style.display = 'block'
            // 添加一个带有颜色边框的类名
            input.classList.add('search')
        })
        // 3. 监听事件 失去焦点
        input.addEventListener('blur', function () {
            ul.style.display = 'none'
            input.classList.remove('search')
        })
    </script>
</body>

</html>
~~~

# 11-键盘事件类型

~~~html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>

<body>
  <input type="text">
  <script>
    const input = document.querySelector('input')
    // 1. 键盘事件
    // input.addEventListener('keydown', function () {
    //   console.log('键盘按下了')
    // })
    // input.addEventListener('keyup', function () {
    //   console.log('键盘谈起了')
    // })
    // 2. 用户输入文本事件  input
    input.addEventListener('input', function () {
      console.log(input.value)
    })
  </script>
</body>

</html>
~~~

# 12-focus选择器

~~~html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    input {
      width: 200px;
      transition: all .3s;
    }

    /* focus伪类选择器  获得焦点 */
    input:focus {
      width: 300px;
    }
  </style>
</head>

<body>
  <input type="text">
</body>

</html>
~~~

# 13-评论回车发布

~~~html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>评论回车发布</title>
  <style>
    .wrapper {
      min-width: 400px;
      max-width: 800px;
      display: flex;
      justify-content: flex-end;
    }

    .avatar {
      width: 48px;
      height: 48px;
      border-radius: 50%;
      overflow: hidden;
      background: url(./images/avatar.jpg) no-repeat center / cover;
      margin-right: 20px;
    }

    .wrapper textarea {
      outline: none;
      border-color: transparent;
      resize: none;
      background: #f5f5f5;
      border-radius: 4px;
      flex: 1;
      padding: 10px;
      transition: all 0.5s;
      height: 30px;
    }

    .wrapper textarea:focus {
      border-color: #e4e4e4;
      background: #fff;
      height: 50px;
    }

    .wrapper button {
      background: #00aeec;
      color: #fff;
      border: none;
      border-radius: 4px;
      margin-left: 10px;
      width: 70px;
      cursor: pointer;
    }

    .wrapper .total {
      margin-right: 80px;
      color: #999;
      margin-top: 5px;
      opacity: 0;
      transition: all 0.5s;
    }

    .list {
      min-width: 400px;
      max-width: 800px;
      display: flex;
    }

    .list .item {
      width: 100%;
      display: flex;
    }

    .list .item .info {
      flex: 1;
      border-bottom: 1px dashed #e4e4e4;
      padding-bottom: 10px;
    }

    .list .item p {
      margin: 0;
    }

    .list .item .name {
      color: #FB7299;
      font-size: 14px;
      font-weight: bold;
    }

    .list .item .text {
      color: #333;
      padding: 10px 0;
    }

    .list .item .time {
      color: #999;
      font-size: 12px;
    }
  </style>
</head>

<body>
  <div class="wrapper">
    <i class="avatar"></i>
    <textarea id="tx" placeholder="发一条友善的评论" rows="2" maxlength="200"></textarea>
    <button>发布</button>
  </div>
  <div class="wrapper">
    <span class="total">0/200字</span>
  </div>
  <div class="list">
    <div class="item" style="display: none;">
      <i class="avatar"></i>
      <div class="info">
        <p class="name">清风徐来</p>
        <p class="text">大家都辛苦啦，感谢各位大大的努力，能圆满完成真是太好了[笑哭][支持]</p>
        <p class="time">2022-10-10 20:29:21</p>
      </div>
    </div>
  </div>
  <script>
    const tx = document.querySelector('#tx')
    const total = document.querySelector('.total')
    // 1. 当我们文本域获得了焦点，就让 total 显示出来
    tx.addEventListener('focus', function () {
      total.style.opacity = 1
    })
    // 2. 当我们文本域失去了焦点，就让 total 隐藏出来
    tx.addEventListener('blur', function () {
      total.style.opacity = 0
    })
    // 3. 检测用户输入
    tx.addEventListener('input', function () {
      // console.log(tx.value.length)  得到输入的长度
      total.innerHTML = `${tx.value.length}/200字`
    })

    // const str = 'andy'
    // console.log(str.length)
  </script>
</body>

</html>
~~~

# 14-事件对象

~~~html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>

<body>
  <!-- <button>点击</button> -->
  <input type="text">
  <script>
    // const btn = document.querySelector('button')
    // btn.addEventListener('click', function (e) {
    //   console.log(e)
    // })

    const input = document.querySelector('input')
    input.addEventListener('keyup', function (e) {
      // console.log(11)
      // console.log(e.key)
      if (e.key === 'Enter') {
        console.log('我按下了回车键')
      }
    })
  </script>
</body>

</html>
~~~

# 15-按下回车发布评论

~~~html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>评论回车发布</title>
  <style>
    .wrapper {
      min-width: 400px;
      max-width: 800px;
      display: flex;
      justify-content: flex-end;
    }

    .avatar {
      width: 48px;
      height: 48px;
      border-radius: 50%;
      overflow: hidden;
      background: url(./images/avatar.jpg) no-repeat center / cover;
      margin-right: 20px;
    }

    .wrapper textarea {
      outline: none;
      border-color: transparent;
      resize: none;
      background: #f5f5f5;
      border-radius: 4px;
      flex: 1;
      padding: 10px;
      transition: all 0.5s;
      height: 30px;
    }

    .wrapper textarea:focus {
      border-color: #e4e4e4;
      background: #fff;
      height: 50px;
    }

    .wrapper button {
      background: #00aeec;
      color: #fff;
      border: none;
      border-radius: 4px;
      margin-left: 10px;
      width: 70px;
      cursor: pointer;
    }

    .wrapper .total {
      margin-right: 80px;
      color: #999;
      margin-top: 5px;
      opacity: 0;
      transition: all 0.5s;
    }

    .list {
      min-width: 400px;
      max-width: 800px;
      display: flex;
    }

    .list .item {
      width: 100%;
      display: flex;
    }

    .list .item .info {
      flex: 1;
      border-bottom: 1px dashed #e4e4e4;
      padding-bottom: 10px;
    }

    .list .item p {
      margin: 0;
    }

    .list .item .name {
      color: #FB7299;
      font-size: 14px;
      font-weight: bold;
    }

    .list .item .text {
      color: #333;
      padding: 10px 0;
    }

    .list .item .time {
      color: #999;
      font-size: 12px;
    }
  </style>
</head>

<body>
  <div class="wrapper">
    <i class="avatar"></i>
    <textarea id="tx" placeholder="发一条友善的评论" rows="2" maxlength="200"></textarea>
    <button>发布</button>
  </div>
  <div class="wrapper">
    <span class="total">0/200字</span>
  </div>
  <div class="list">
    <div class="item" style="display: none;">
      <i class="avatar"></i>
      <div class="info">
        <p class="name">清风徐来</p>
        <p class="text">大家都辛苦啦，感谢各位大大的努力，能圆满完成真是太好了[笑哭][支持]</p>
        <p class="time">2022-10-10 20:29:21</p>
      </div>
    </div>
  </div>
  <script>
    const tx = document.querySelector('#tx')
    const total = document.querySelector('.total')
    const item = document.querySelector('.item')
    const text = document.querySelector('.text')
    // 1. 当我们文本域获得了焦点，就让 total 显示出来
    tx.addEventListener('focus', function () {
      total.style.opacity = 1
    })
    // 2. 当我们文本域失去了焦点，就让 total 隐藏出来
    tx.addEventListener('blur', function () {
      total.style.opacity = 0
    })
    // 3. 检测用户输入
    tx.addEventListener('input', function () {
      // console.log(tx.value.length)  得到输入的长度
      total.innerHTML = `${tx.value.length}/200字`
    })

    // 4. 按下回车发布评论
    tx.addEventListener('keyup', function (e) {
      // 只有按下的是回车键，才会触发
      // console.log(e.key)
      if (e.key === 'Enter') {
        // 如果用户输入的不为空就显示和打印
        if (tx.value.trim()) {
          // console.log(11)
          item.style.display = 'block'
          // console.log(tx.value)  // 用户输入的内容
          text.innerHTML = tx.value

        }
        // 等我们按下回车，结束，清空文本域
        tx.value = ''
        // 按下回车之后，就要把 字符统计 复原
        total.innerHTML = '0/200字'
      }

    })
  </script>
</body>

</html>
~~~

# 16-trim方法

~~~html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>

<body>
  <textarea name="" id="" cols="30" rows="10"></textarea>
  <script>
    const str = '          im a teacher  '
    // console.log(str.trim())  // 去除字符串左右的空格
    const tx = document.querySelector('textarea')
    tx.addEventListener('keyup', function (e) {
      // console.log(tx.value)
      if (e.key === 'Enter') {
        // console.log(tx.value)
        console.log(tx.value.trim() === '')
      }
    })
  </script>
</body>

</html>
~~~

# 17-环境对象

~~~html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>

<body>
  <button>点击</button>
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
</body>

</html>
~~~

# 18-tab栏切换

~~~html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8" />
  <meta http-equiv="X-UA-Compatible" content="IE=edge" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>tab栏切换</title>
  <style>
    * {
      margin: 0;
      padding: 0;
    }

    .tab {
      width: 590px;
      height: 340px;
      margin: 20px;
      border: 1px solid #e4e4e4;
    }

    .tab-nav {
      width: 100%;
      height: 60px;
      line-height: 60px;
      display: flex;
      justify-content: space-between;
    }

    .tab-nav h3 {
      font-size: 24px;
      font-weight: normal;
      margin-left: 20px;
    }

    .tab-nav ul {
      list-style: none;
      display: flex;
      justify-content: flex-end;
    }

    .tab-nav ul li {
      margin: 0 20px;
      font-size: 14px;
    }

    .tab-nav ul li a {
      text-decoration: none;
      border-bottom: 2px solid transparent;
      color: #333;
    }

    .tab-nav ul li a.active {
      border-color: #e1251b;
      color: #e1251b;
    }

    .tab-content {
      padding: 0 16px;
    }

    .tab-content .item {
      display: none;
    }

    .tab-content .item.active {
      display: block;
    }
  </style>
</head>

<body>
  <div class="tab">
    <div class="tab-nav">
      <h3>每日特价</h3>
      <ul>
        <li><a class="active" href="javascript:;">精选</a></li>
        <li><a href="javascript:;">美食</a></li>
        <li><a href="javascript:;">百货</a></li>
        <li><a href="javascript:;">个护</a></li>
        <li><a href="javascript:;">预告</a></li>
      </ul>
    </div>
    <div class="tab-content">
      <div class="item active"><img src="./images/tab00.png" alt="" /></div>
      <div class="item"><img src="./images/tab01.png" alt="" /></div>
      <div class="item"><img src="./images/tab02.png" alt="" /></div>
      <div class="item"><img src="./images/tab03.png" alt="" /></div>
      <div class="item"><img src="./images/tab04.png" alt="" /></div>
    </div>
  </div>
  <script>
    // 1. a 模块制作 要给 5个链接绑定鼠标经过事件
    // 1.1 获取 a 元素 
    const as = document.querySelectorAll('.tab-nav a')
    // console.log(as) 
    for (let i = 0; i < as.length; i++) {
      // console.log(as[i])
      // 要给 5个链接绑定鼠标经过事件
      as[i].addEventListener('mouseenter', function () {
        // console.log('鼠标经过')
        // 排他思想  
        // 干掉别人 移除类active
        document.querySelector('.tab-nav .active').classList.remove('active')
        // 我登基 我添加类 active  this 当前的那个 a 
        this.classList.add('active')

        // 下面5个大盒子 一一对应  .item 
        // 干掉别人
        document.querySelector('.tab-content .active').classList.remove('active')
        // 对应序号的那个 item 显示 添加 active 类
        document.querySelector(`.tab-content .item:nth-child(${i + 1})`).classList.add('active')

      })
    }
  </script>
</body>

</html>
~~~

