# 页面弹框课程练习

需求：请用外部和内部两种 JavaScript 书写方式，页面弹出： 努力，奋斗

时间：5分钟

~~~html
<!-- 内部 -->
<body>
  <script>
    alert('奋斗')
  </script>
</body>
<!-- 外部 -->
<body>
  <script src="./my.js"> </script>
</body>
~~~

# 输入和输出法练习

- 浏览器中弹出对话框： 你好 JS~

- 页面中打印输出： JavaScript 我来了! 
-  页面控制台输出： 他会来吧

~~~html
// 对话框
	<script>
    alert('你好JS')
  </script>
// 打印输出
	<script>
    document.write('JS我来了')
  </script>
// 控制台输出
<script>
    prompt('他回来吧')
  </script>

~~~

# 变量复习

声明一个变量，用于存放用户购买的商品 数量 ( num ) 为 20 件,然后控制台打印输出变量

~~~html
<script>
    let num = 20
    console.log(num)
  </script>
~~~

# 弹出姓名

需求： 浏览器中弹出对话框： 请输入姓名， 页面中输出：刚才输入的姓名

~~~html
	<script>
    // 用户输入，内部处理保存数据
    let uname = prompt('请输入姓名')
    // 打印输出
    document.write(uname)
  </script>
~~~

# 交换变量的值

- 需求： 

  - 有2个变量： 

    - num1 里面放的是 10， num2 里面放的是20 

    - 最后变为 num1 里面放的是 20 ， num2 里面放的是 10

- 分析：使用一个临时变量用来做中间存储

- 步骤：

  - 声明一个临时变量temp
  - 把num1的值赋值给temp
  - 把num2的值赋值给num1
  - 把temp的值给num2

> 把右边给左边（temp=num1 - num1 给 temp）

~~~html
	<script>
    let num1 = 10
    let num2 = 20
    let temp
    temp = num1
    num1 = num2
    num2 = temp
    console.log(num1, num2)
  </script>
~~~

# 输出用户信息

需求：让用户输入自己的名字、年龄、性别，再输出到网页

~~~html
	<script>
    
    let uname = prompt('请输入您的用户名')
    let age = prompt('请输入您的年龄')
    let gender = prompt('请输入您的性别')
    document.write(uname.age, gender)
  </script>
~~~

# 数组练习

需求：定义一个数组，里面存放星期一、星期二…… 直到星期日（共7天），在控制台输出：星期日

~~~html
	<script>
    let week = ['星期一', '星期二', '星期三', '星期四', '星期五', '星期六', '星期日']
    console.log(week[6])
  </script>
~~~

# 计算圆的面积

需求：对话框中输入圆的半径，算出圆的面积并显示到页面

~~~html
	 <script>
    // 1.页面弹出输入框
    let r = prompt('请输入圆的半径')
    // 2.计算圆的面积
    let re = 3.14 * r * r
    // 3.页面输出
    document.write(re)
  </script>
~~~

# 字符串练习

需求：页面弹出对话框，输入名字和年龄，页面显示： 大家好，我叫xxx，今年xx岁了

~~~html
	<script>
    let age = prompt('请输入年龄')
    let name = prompt('请输入您的姓名')
    document.write(`大家好，我叫${name},今年${age}岁了`)
  </script>
~~~

# 类型转换练习

需求：输入2个数，计算两者的和，打印到页面中

~~~html
	<script>
    // 用户输入，转换类型 （用加号）
    let num1 = +(prompt('请输入'))
    let num2 = +(prompt('请输入'))
    // 输出,模版字符串（拼接字符串和变量）
    alert(`两个数相加的值是:${num1 + num2}`)
  </script>
~~~

# 综合案例

~~~html
    <!DOCTYPE html>
    <html lang="en">

    <head>
      <meta charset="UTF-8">
      <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>Document</title>
    </head>
    <style>
      h2 {
        text-align: center;
      }

      table {
        /* 合并表格边框 */
        border-collapse: collapse;
        height: 80px;
        margin: 0 auto;

      }

      th {
        /* 不给宽度，地址长了 */
        padding: 5px 30px;
      }

      table,
      th,
      td {
        border: 1px solid black;
      }
    </style>

    <body>
      <h2>订单确认</h2>
      <script>
        // 1，用户输入
        let price = prompt('请输入价格')
        let num = prompt('请输入数量')
        let address = prompt('请输入收货地址')
        // 2，计算总额
        let total = num * price
        // 3，页面打印宣传， 用模版字符串
        document.write(`
            <table>
            <tr>
              <th>产品名称</th>
              <th>产品价格</th>
              <th>商品数量</th>
              <th>总价</th>
              <th>收货地址</th>
            </tr>
            <tr>
              <td>小米手机</td>
              <td>${price}元</td>
              <td>${num}</td>
              <td>${total}元</td>
              <td>武汉</td>
            </tr>
          </table>
    `)
      </script>
    </body>

    </html>
~~~

复习题答案：[https://ks.wjx.top/wjx/join/completemobile2.aspx?activityid=YXyjrAe&joinactivity=118461946207&sojumpindex=3883&tvd=sX%2fKsf32awI%3d&costtime=511&comsign=849BDABF04535882224530BB8E6790EDD29A8F30&rfrr=1&nw=1&jpm=98%E2%80%98](https://ks.wjx.top/wjx/join/completemobile2.aspx?activityid=YXyjrAe&joinactivity=118461946207&sojumpindex=3883&tvd=sX%2fKsf32awI%3d&costtime=511&comsign=849BDABF04535882224530BB8E6790EDD29A8F30&rfrr=1&nw=1&jpm=98%E2%80%98)

# 增加年龄

~~~html
<body>
  <script>
    let age = +prompt('请输入您的年龄')
    // 直接加上5岁
    alert(`'据我估计,5年后,你可能'${age + 5}'岁'`)
    // 在输出的转换
    alert("你5年之后是:" + (Number(res) + 5))
  </script>
</body>
~~~

