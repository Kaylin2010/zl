# 综合案例：消息提示对象封装（模态框）

需求： 多个模态框一样的， 而且每次点击都出来一个



<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230915114957109.png" alt="image-20230915114957109" style="zoom:50%;" />

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230915135355974.png" alt="image-20230915135355974" style="zoom:50%;" />

- 分析需求：

1. 定义模态框 Modal 构造函数，用来创建对象
2. 模态框具备 打开功能 open 方法 （按钮点击可以打开模态框）
3. 模态框 具备关闭功能 close 方法

①：Modal 构造函数 制作

\- 需要的公共属性： 标题（title）、提示信息内容（message） 可以设置默认参数

\- 在页面中创建模态框

(1) 创建div标签可以命名为：modalBox

(2) div标签的类名为 modal 

(3) 标签内部添加 基本结构，并填入相关数据

~~~html
 <script>
    // 1.  模态框的构造函数
    function Modal(title = '', message = '') { //实参，给默认值防止没有数据
      // 公共的属性部分
      this.title = title
      this.message = message
      // 因为盒子是公共的
      // 1. 创建div 一定不要忘了加 this （指定当前的盒子， 会有很多盒子）
      this.modalBox = document.createElement('div')
      // 2. 添加类名
      this.modalBox.className = 'modal'
      // 3. 填充内容 更换数据
      this.modalBox.innerHTML = `
        <div class="header">${this.title} <i>x</i></div>
        <div class="body">${this.message}</div>
      `
      // console.log(this.modalBox) 看看盒子是否创建成功 
      new Modal（）//调用 - 每次new创建一个新的div
    }
</script>
~~~

①：open方法 （显示弹窗）

\- 写到构造函数的原型对象身上

\- 把刚才创建的modalBox 添加到 页面 body 标签中

\- open 打开的本质就是 把创建标签添加到页面中

\- 点击按钮， 实例化对象，传入对应的参数，并执行 open 方法

~~~html
<script> 
// 2. 打开方法 挂载 到 模态框的构造函数原型身上
    2.1 Modal.prototype.open = function () {
      if (!document.querySelector('.modal')) {
        // 把刚才创建的盒子 modalBox  渲染到 页面中  父元素.appendChild(子元素) 
        //注意这个方法不用箭头函数（ 要用里面的this）
        2.2 document.body.appendChild(this.modalBox)
        4.2 获取 x  调用关闭方法 （要等盒子显示寄出来，就可以绑定点击事件）
        this.modalBox.querySelector('i').addEventListener('click', () => {
          // 箭头函数没有this 上一级作用域的this （2.2 的 this 实例对象 this.modalBox）
          // 这个this 指向 m 
          this.close()
        })
      }
    }
  //检测
  // 4. 按钮点击 （删除）
    document.querySelector('#delete').addEventListener('click', () => {
      //调用modal构造函数
      const m = new Modal('温馨提示', '您没有权限删除')
      // 调用 打开方法
      m.open()
    })

    // 5. 按钮点击 （注册）
    document.querySelector('#login').addEventListener('click', () => {
      const m = new Modal('友情提示', '您还么有注册账号')
      // 调用 打开方法
      m.open()
    })

</script>
~~~

①：close方法

\- 写到构造函数的原型对象身上

\- 把刚才创建的modalBox 从页面 body 标签中 删除

\- 需要注意，x 删除按钮绑定事件，要写到open里面添加

因为open是往页面中添加元素，同时顺便绑定事件

~~~html
<script>
  // 3. 关闭方法 挂载 到 模态框的构造函数原型身上
    Modal.prototype.close = function () {
      document.body.removeChild(this.modalBox)
    }
</script>
~~~

* BUG：一直点击按钮会显示很多盒子
  * 解决： 准备open显示的时候，先判断页面中有没有modal盒子，有就移除，没有就添加 （逻辑与终端）box： 假 -- 下面代码， box：真 --- 删除

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230915143656606.png" alt="image-20230915143656606" style="zoom:50%;" />

- 优点： 可以随便添加按钮，写div 然后new添加