# 案例1： 用户名验证

需求：用户名要求用户英文字母,数字,下划线或者短横线组成，并且用户名长度为 6~16位

分析： 

①：首先准备好这种正则表达式模式 /^[a-zA-Z0-9-_]{6,16}$/ 

 /^[a-zA-Z0-9-_]{6,16}$/ ： _ 特殊字符号写后面

②：当表单失去焦点就开始验证 

③：如果符合正则规范, 则让后面的span标签添加 right 类. 

④：如果不符合正则规范, 则让后面的span标签添加 wrong 类

~~~html
<body>
    <input type="text">
    <span></span>
    <script>
        // 1. 准备正则 常用reg
        const reg = /^[a-zA-Z0-9-_]{6,16}$/
        //span 是input兄弟
        const span = input.nextElementSibling
        //失去焦点 ：blur
        input.addEventListener('blur', function () {
            // console.log(reg.test(this.value))
            //用户输入内容value
            if (reg.test(this.value)) {
                span.innerHTML = '输入正确'
                // input.nextElementSibling.innerHTML = '输入正确'
                //调类名
                span.className = 'right' //不要点，字符串
            } else {
                span.innerHTML = '请输入6~16位的英文数字下划线'
                //用classList.add 就算加了2个类名 会被前面的right覆盖， 所以用className
                span.className = 'error'
            }
        })
    </script>
~~~

# 案例 2：昵称案例

需求：要求用户只能输入中文 

分析： 

①：首先准备好这种正则表达式模式 /^[\u4e00-\u9fa5]{2,8}$/ 

②：当表单失去焦点就开始验证. 

③：如果符合正则规范, 则让后面的span标签添加 right 类. 

④：如果不符合正则规范, 则让后面的span标签添加 wrong 类

# 案例 3：过滤敏感字 

需求：要求用户不能输入敏感字 

比如，pink老师上课很有** 

分析： 

①：用户输入内容  -- 拿到内容去过滤 -- 再发布

②：内容进行正则替换查找，找到敏感词，进行** 

③：要全局替换使用修饰符 g

~~~html
<body>
  <textarea name="" id="" cols="30" rows="10"></textarea>
  <button>发布</button>
  <div></div>
  <script>
    //获取需要处理的元素
    const tx = document.querySelector('textarea')
    const btn = document.querySelector('button')
    const div = document.querySelector('div')
    btn.addEventListener('click', function () {
      // console.log(tx.value)
      // 如果输入内容敏感词，替换（过滤）为**
      div.innerHTML = tx.value.replace(/激情|基情/g, '**')
      //表单清空
      tx.value = ''
    })
  </script>
</body>
~~~

# 综合案例：小兔鲜页面注册

- 分析业务模块 


①： 发送验证码模块  （倒计时效果）

②： 各个表单验证模块 

③： 勾选已经阅读同意模块 （不勾选 -- 给提示）

④： 下一步验证全部模块 

只要上面有一个input验证不通过就不同意提交

## 1 - 需求 ①： 发送验证码 

用户点击之后，显示 05秒后重新获取 

时间到了，自动改为 重新获取 

用立即执行函数包起来， 下面可以重复使用code

~~~javascript
1.找到对应需要处理的标签, 点击发送验证码按钮（code）
    const code = document.querySelector('.code')
4. 通过一个变量来控制   节流阀 ，true才能点
     let flag = true  
2.一点开启倒计时 --- 开启定时器: 从5开始
	//4.1 if控制点击事件
	if (flag) {
    code.addEventListener('click', function () {
      //4.2 取反了，不能马上第二次点击
          flag = false
      let i = 5
   	// 点击完毕之后立马触发 -- （目前是点击1s后才生效）
          code.innerHTML = `0${i}秒后重新获取`   
      let timerId = setInterval(function () {
         i--
3. 改文档 - 不能用this因为在调用定时器
	code.innerHTML = `0${i}秒后重新获取`
  //为0关定时器 并且  修改文档
   if (i === 0) {
              // 清除定时器
              clearInterval(timerId)
              // 从新获取
              code.innerHTML = `重新获取`
              // 4.3 到时间了，可以开启 flag了
              flag = true
    },1000)
  
~~~

> -  节流阀: 控制不能点击第一次立马可以再点击， 要等时间到了再开启 `let flag = true `



## 2 - 需求②： 用户名验证

（注意封装函数 verifyxxx） , 失去焦点触发这个函数 

正则 /^[a-zA-Z0-9-_]{6,10}$/ 

如果不符合要求，则出现提示信息 并 return false 中断程序 

否则 则返回return true 

之所以返回 布尔值，是为了 最后的提交按钮做准备 

侦听使用change事件，当鼠标离开了表单，并且表单值发生了变化时触发（类似京东效 

果）

* 结构：input username 文本框， input的兄弟 span 显示输入错误提示

~~~javascript
// 2. 验证的是用户名
    // 2.1 获取用户名表单  - name="username" (属性)
    const username = document.querySelector('[name=username]')
    // 2.2 使用change事件  值发生变化的时候 - 用有名字函数方便以后调用 - 直接写名字
    username.addEventListener('change', verifyName)
    // 2.3 封装verifyName函数， 不用调用，只要change出发自动调用
    function verifyName() {
      // console.log(11) 输入内容 -- 触发
      // 2.4 声明兄弟元素，因为有很多span， 不能单独获取
      const span = username.nextElementSibling
      // 2.4 定规则  用户名 不要用this 后面要调用函数
      const reg = /^[a-zA-Z0-9-_]{6,10}$/
      //!如果不通过
      if (!reg.test(username.value)) {
        // console.log(11) 输入错会显示11， 对没显示什么
        //innerText 只能写字
        span.innerText = '输入不合法,请输入6~10位'
        //不合法不再去执行，返回 true false 为了后面取值做判断
        return false
      }
      // 2.5 合法的 就清空span - 写外面不要写esle因为有return
      // 先清空再判断
      span.innerText = ''
      return true
    }
~~~

## 需求③： 手机号验证 

正则: /^1(3\d|4[5-9]|5[0-35-9]|6[567]|7[0-8]|8\d|9[0-35-9])\d{8}$/ 

其余同上

~~~javascript
 *复制过来然后修改：
// 3. 验证的是手机号
    // 2.1 获取手机表单
    const phone = document.querySelector('[name=phone]')
    // 2.2 使用change事件  值发生变化的时候
    phone.addEventListener('change', verifyPhone)
    // 2.3 verifyPhone
    function verifyPhone() {
      // console.log(11)
      const span = phone.nextElementSibling
      // 2.4 定规则  用户名
      const reg = /^1(3\d|4[5-9]|5[0-35-9]|6[567]|7[0-8]|8\d|9[0-35-9])\d{8}$/
      if (!reg.test(phone.value)) {
        // console.log(11)
        span.innerText = '输入不合法,请输入正确的11位手机号码'
        return false
      }
      // 2.5 合法的 就清空span
      span.innerText = ''
      return true
    }

~~~

## 需求④： 验证码验证 

正则 /^\d{6}$/ 

其余同上

~~~javascript
// 4. 验证的是验证码
    // 4.1 获取验证码表单
    const codeInput = document.querySelector('[name=code]')
    //4.2 使用change事件  值发生变化的时候
    codeInput.addEventListener('change', verifyCode)
    // 4.3 verifyPhone
    function verifyCode() {
      // console.log(11)
      const span = codeInput.nextElementSibling
      // 4.4 定规则  验证码
      const reg = /^\d{6}$/
      if (!reg.test(codeInput.value)) {
        // console.log(11)
        span.innerText = '输入不合法,6 位数字'
        return false
      }
      // 4.5 合法的 就清空span
      span.innerText = ''
      return true
    }
~~~

> - 立即执行函数抱起来，重名也没问题

## 需求⑤： 密码验证 

正则 

/^[a-zA-Z0-9-_]{6,20}$/ 

其余同上

~~~javascript
// 5. 验证的是密码框
    // 5.1 获取密码表单
    const password = document.querySelector('[name=password]')
    //5.2 使用change事件  值发生变化的时候
    password.addEventListener('change', verifyPwd)
    // 5.3 verifyPhone
    function verifyPwd() {
      // console.log(11)
      const span = password.nextElementSibling
      // 5.4 定规则  密码
      const reg = /^[a-zA-Z0-9-_]{6,20}$/
      if (!reg.test(password.value)) {
        // console.log(11)
        span.innerText = '输入不合法,6~20位数字字母符号组成'
        return false
      }
      // 5.5 合法的 就清空span
      span.innerText = ''
      return true
    }
~~~

## 需求⑥： 再次密码验证 

只有一种情况可以通过， 所以不要验证(不需要规则）只要核对就好了

如果本次密码不等于上面输入的密码则返回错误信息 

其余同上 

~~~javascript
// 6. 密码的再次验证
    // 6.1 获取再次验证表单
    const confirm = document.querySelector('[name=confirm]')
    //6.2 使用change事件  值发生变化的时候
    confirm.addEventListener('change', verifyConfirm)
    // 6.3 verifyPhone
    function verifyConfirm() {
      // console.log(11)
      const span = confirm.nextElementSibling
      ***// 6.4 当前表单的值不等于 密码框的值就是错误的
      if (confirm.value !== password.value) {
        // console.log(11)
        span.innerText = '两次密码输入不一致'
        return false
      }
      // 6.5 合法的 就清空span
      span.innerText = ''
      return true
    }
~~~

## 需求⑦： 我同意模块 

添加类 .icon-queren2 则是默认选中样式 可以使用 ==toggle切换类== 

~~~javascript
// 7. 我同意
    const queren = document.querySelector('.icon-queren')
    queren.addEventListener('click', function () {
      // 切换类  原来有的就删掉，原来没有就添加
      this.classList.toggle('icon-queren2')
      //i标签切换类名 .icon-queren icon-queren2
    })
~~~

## 需求⑧： 表单提交模块 

使用 submit 提交事件 ：

1，如果没有勾选同意协议，则提示 需要勾选 

2，一个没有填写

classList.contains() 看看有没有包含某个类，如果有则返回true，么有则返回false 

如果上面input表单 只要有模块，返回的是 false 则 阻止提交

~~~javascript
 // 8. 提交模块
    const form = document.querySelector('form')
    //用对象事件， 获取鼠标点击，阻止提交
    form.addEventListener('submit', function (e) {
      // 判断是否勾选我同意模块 ，如果有 icon-queren2说明就勾选了，否则没勾选
      if (!queren.classList.contains('icon-queren2')) {
        alert('请勾选同意协议')
        // 阻止提交， 检测提不提交：看看链接后面有没有信息
        e.preventDefault()
      }
      // 依次判断上面的每个框框 是否通过，只要有一个没有通过的就阻止
			调用函数会拿到返回的结果
      // console.log(verifyName())
      // !verifyName() = ! True = False （if） 不执行（要相反） ---不会被阻止，if True 执行 e.preventDefault()
      if (!verifyName()) e.preventDefault()
      if (!verifyPhone()) e.preventDefault()
      if (!verifyCode()) e.preventDefault()
      if (!verifyPwd()) e.preventDefault()
      if (!verifyConfirm()) e.preventDefault()
    })
~~~

> .classList.contains() : 检测有没有这个类 // 结果： true false

# 阶段案例 1：小兔鲜登录页面

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230907104553986.png" alt="image-20230907104553986" style="zoom:50%;" />

## 需求 ①： tab切换

~~~javascript
  1. tab栏切换  事件委托
    const tab_nav = document.querySelector('.tab-nav')
    const pane = document.querySelectorAll('.tab-pane')
    // 1.1 事件监听
    tab_nav.addEventListener('click', function (e) {
      if (e.target.tagName === 'A') {
        // 取消上一个active - 点击谁就是下划线
        tab_nav.querySelector('.active').classList.remove('active')
        // 当前元素添加active
        e.target.classList.add('active')
~~~

~~~javascript
2. 下面的显示和隐藏 （ 集中一个盒子已经有些隐藏display none）
-->用for循环把全部盒子获取出来，都隐藏，然后用id序号拿出来
2.1 获取 tab-pane：两个盒子class
  const pane = document.querySelectorAll('.tab-pane')//数组
   // 先干掉所有人  for循环
        for (let i = 0; i < pane.length; i++) {
          pane[i].style.display = 'none'
        }
        // 让对应序号的 大pane 显示，a的id
        pane[e.target.dataset.id].style.display = 'block'
	//检测：  pane[1].style.display = 'block' 显示二维码
      }
    })
~~~



> - 事件委托只适合资源数再没有子元素（例如： a里面有图片，点击有可能点的是图片不是a）
> - 切换显示是对应页面
>
> <img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230907130811633.png" alt="image-20230907130811633" style="zoom:50%;" />

## 需求②： 点击登录可以跳转页面 

-  先阻止默认行为 

- 如果没有勾选同意，则提示要勾选 

-  required 属性不能为空 

- 假设登录成功 

把用户名记录到本地存储中 

同时跳转到首页 location.href

~~~javascript
 // 点击提交模块
    const form = document.querySelector('form')
    //获取 name=agree 勾选框
    const agree = document.querySelector('[name=agree]')
    //获取用户名
    const username = document.querySelector('[name=username]')
    form.addEventListener('submit', function (e) {
      //不要提交，先跳转页面
      e.preventDefault()
      // 判断是否勾选同意协议， 表单的checked 检测勾不勾上
      if (!agree.checked) {
        return alert('请勾选同意协议')
      }

      // 记录用户名到本地存储 （为了跳转后，页面显示用户名） -- 上面要获取uname
      localStorage.setItem('xtx-uname', username.value)
      // 跳转到首页   同时跳转到首页 location.href
      location.href = './index.html'
    })
~~~



> autocomplete = 'off' :关自动记录

## 阶段案例 2：小兔鲜首页页面

需求： 

1. 从登录页面跳转过来之后，自动显示用户名 
2. 如果点击退出，则不显示用户名

步骤： 

最好写个渲染函数 （render），因为一会的退出还需要用到 

①：如果本地存储有记录的用户名， 读取本地存储数据 

需要把用户名写到 第一个li里面  （显示用户名）

格式：`<a href="javascript:;"><i class="iconfont icon-user"> pink老师</i></a>`

因为登录了，所以第二个 里面的文字变为，退出登录 

格式：`<a href="javascript:;">退出登录</a> ` 

②：如果本地存储没有数据，**则复原为默认的结构**



~~~javascript
 // 1、 获取第一个小li（li1：请先登录， li2：免费注册）
    const li1 = document.querySelector('.xtx_navs li:first-child')
    const li2 = li1.nextElementSibling
    // 2. 最好做个渲染函数 因为退出登录需要重新渲染
    function render() {
      // 2.1 读取本地存储的用户名 （设定有用户名怎么显示， 没有怎么显示）
      const uname = localStorage.getItem('xtx-uname')
      // console.log(uname) 调用再检测
      if (uname) {
        li1.innerHTML = `<a href="javascript:;"><i class="iconfont icon-user">${uname
          }</i></a>
        `
        li2.innerHTML = '<a href="javascript:;">退出登录</a>'
      } else {
        li1.innerHTML = '<a href="./login.html">请先登录</a>'
        li2.innerHTML = '<a href="./register.html">免费注册</a>'
      }
    }
    render()  // 调用函数

~~~

④： 点击 退出登录 

删除本地存储对应的用户名数据 

重新调用渲染函数即可

~~~javascript
// 2. 点击退出登录模块
    li2.addEventListener('click', function () {
      // 删除本地存储的数据
      localStorage.removeItem('xtx-uname')
      // 重新渲染 - 显示没有用户名的页面
      render()
    })
~~~

