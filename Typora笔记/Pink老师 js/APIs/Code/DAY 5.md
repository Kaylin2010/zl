# **案例1： 5秒后自动跳转页面**

| 分析                                         |
| -------------------------------------------- |
| 到期时需要用setInterval ，因为页面有到期时跑 |

| 链接                                                         |
| ------------------------------------------------------------ |
| [file:///Users/guoguo/Documents/JS/js%20code/APIs/Day%205%20/code/06-5%E7%A7%92%E9%92%9F%E4%B9%8B%E5%90%8E%E8%87%AA%E5%8A%A8%E8%B7%B3%E8%BD%AC%E9%A1%B5%E9%9D%A2.html](file:///Users/guoguo/Documents/JS/js%20code/APIs/Day%205%20/code/06-5%E7%A7%92%E9%92%9F%E4%B9%8B%E5%90%8E%E8%87%AA%E5%8A%A8%E8%B7%B3%E8%BD%AC%E9%A1%B5%E9%9D%A2.html) |

| 步骤 |
| ---- |

1，准备链接 a标签  （注意文档）

~~~html
`<a href="http://www.itcast.cn">支付成功<span>5</span>秒钟之后跳转到首页</a>`
~~~

> `<span>5</span>` ： 需要显示倒计事件

2，获取a元素

~~~html
const a = document.querySelector('a')
~~~

3，声明倒计时变量

~~~html
 let num = 5
~~~

4，开启定时器

~~~html
<!-- timerId：给定时器声明方便关闭  -->
let timerId = setInterval(function () {
      num--
      a.innerHTML = `支付成功<span>${num}</span>秒钟之后跳转到首页`
<!--时间： 1000ms，过1s再减去1，所以一开始还是5  -->
~~~

5，给条件关倒计时

如果 n ===0 则停止定时器，并且完成跳转功能

~~~html
 <!-- 关闭并且跳转 -->
if (num === 0) {
        clearInterval(timerId)
        // 4. 跳转  location.href
        location.href = 'http://www.itcast.cn'
      }
    }
~~~

~~~html
 <a href="http://www.itcast.cn">支付成功<span>5</span>秒钟之后跳转到首页</a>
  <script>
    // 1. 获取元素
    const a = document.querySelector('a')
    // 2.开启定时器
    // 3. 声明倒计时变量
    let num = 5
    let timerId = setInterval(function () {
      num--
      a.innerHTML = `支付成功<span>${num}</span>秒钟之后跳转到首页`
      // 如果num === 0 则停止定时器，并且完成跳转功能
      if (num === 0) {
        clearInterval(timerId)
        // 4. 跳转  location.href
        location.href = 'http://www.itcast.cn'
      }
    }, 1000)
~~~



> :octopus:注意
>
> 1，

# 综合案例：学生就业信息表

- 需求： 录入学生信息，页面刷新数据不丢失（已经填写的数据， 刷新后不丢失）

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230905163113014.png" alt="image-20230905163113014" style="zoom:50%;" />

*思路分析：

①：因为页面刷新不丢失数据，所以可能存在已有数据，所以第一步，我们先找本地存储里面查找是 否有数据，如果有数据先进行渲染页面，如果没有数据，我们放一个空数组，用来存放数据 

②：渲染模块，数据会渲染到页面中 

③：新增模块， 输入学生信息，数据会存储到本地存储中，然后渲染页面 

④：删除模块，点击删除按钮，会删除对应的数据，然后渲染页面

| 分析结构                                               |
| ------------------------------------------------------ |
| `form` ：填写信息，点击添加 -- 添加到表格 （渲染内容） |

- 分析步骤

  1.渲染业务

  - 从本地存储数据 （页面刷新不丢失）

  2.新增业务

  - 点击新增， 页面会多一条数据 --- 发生修改 --》存到本地
  - 最新的数据 --- 渲染

  3.删除业务

  - 点击删除 - 少了一条信息（ 修改）---》 存到本地里面
  - 最新数据 --- 渲染

====================================================================================================================================================================================================================================================================================

## 1 - 渲染业务

~~~javascript
 // 1. 渲染业务
   1.1 先读取本地存储的数据
    (1). 本地存储有数据则记得转换为对象然后存储到变量里面，后期用于渲染页面
     (2). 如果没有数据，则用 空数组来代替
 =========================================================
(1). 本地存储有数据则记得转换为对象然后存储到变量里面，后期用于渲染页面
// 用测试数据 把数据存到本地并且转换为Json对象
		localStorage.setItem('data', JSON.stringify(initData))
// 声明对象使用： Json对象转换为对象，localStorage.getItem 获取 'data'
		const arr = JSON.parse(localStorage.getItem('data')) 
// 检测 （数组也是对象， 都转换为对象）
    console.log(arr)
 (2). 如果没有数据，则用 空数组来代替
// 后面加上，代码从上往下执行 ||：如果有数据就是真的，没有数据（假）就为空 []
		const arr = JSON.parse(localStorage.getItem('data')) || []
// 检测： 注释存储代码 --- 返回空数组 （下注释因为打开页面时是没有数据）
    // localStorage.setItem('data', JSON.stringify(initData))
~~~

* 根据持久化数据渲染页面， 核心步骤：
  * 根据数据渲染页面，遍历数组，根据tr，页面填充数据，最后追加给tbody
* 利用map（）和 join（）数组方法实现字符串拼接

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230906073308597.png" alt="image-20230906073308597" style="zoom:50%;" />

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230906073332088.png" alt="image-20230906073332088" style="zoom:50%;" />

~~~html
(1). 利用map遍历数组，返回对应tr的数组
map遍历有一个空数组等着return的返回值， 然后给tr换值
(2). 把数组转换为字符串 join
(3). 把生成的字符串追加给tbody 

~~~

~~~javascript
// 1.2 利用map和join方法来渲染页面
    const tbody = document.querySelector('tbody')
   // 遍历函数 渲染函数
    function render() {
      // (1). 利用map遍历数组，返回对应tr的数组 （ ele： 数组元素）uname，age
      // ele.stuId 对象里面的值， 声明map
      const trArr = arr.map(function (ele, index) {
        return `
          <tr>
            <td>${ele.stuId}</td> 
            <td>${ele.uname}</td>
            <td>${ele.age}</td>
            <td>${ele.gender}</td>
            <td>${ele.salary}</td>
            <td>${ele.city}</td>
            <td>${ele.time}</td>
            <td>
              <a href="javascript:" data-id="${index}">
                <i class="iconfont icon-shanchu"></i>
                删除
              </a>
            </td>
          </tr>
        `
      })
      //检测结果 需要调用函数
      console.log(trArr) //空数组 -- 没数据 initData解开
      render() //渲染内容
~~~

> :octopus:注意点
>
> map的优点就是把return的值保存为数组

~~~javascript
//(2). 把数组转换为字符串 join (tbody 输入信息)
//(3). 把生成的字符串追加给tbody 
		//获取tbody
 const tbody = document.querySelector('tbody')
 //innerHTML不能用数组， 必须要转换为字符串
 tbody.innerHTML = trArr.join('')
//验证去看数据显示了么
~~~



~~~javascript
// 显示共计有几条数据 (就是数组的长度)
      document.querySelector('.title span').innerHTML = arr.length
    }
~~~

> :octopus:注意点
>
> `localStorage.setItem('data', JSON.stringify(initData))`
>
> 这条数据必须要注释起来， 因为数据已经存到本地里面，不要再显示了

## 2 - 新增业务

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230906093359208.png" alt="image-20230906093359208" style="zoom:50%;" />

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230906103220522.png" alt="image-20230906103220522" style="zoom:50%;" />

~~~html
2. 新增业务
	2.1 form表单注册提交事件，阻止
	2.2 非空判断
	2.3 给 arr 数组追加对象，里面存储 表单获取过来的数据
	2.4 渲染页面和重置表单（reset()方法）

~~~

~~~javascript
// 2.1 form表单注册提交事件，阻止默认行为 （还没填写点添加没有用）
    info.addEventListener('submit', function (e) {
      e.preventDefault()
~~~

> 阻止默认行为 （e）：事件对象
>
>  e.preventDefault()
>
> <img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230906094213449.png" alt="image-20230906094213449" style="zoom:50%;" />

~~~javascript
// 2.2 非空判断 (内容为空给提示填写)
//获取 uname age salary
// !uname.value ： 内容为空
      if (!uname.value || !age.value || !salary.value) {
        return alert('输入内容不能为空')
      }
~~~

> :octopus:注意点：
>
> 1. 内容为空语法
>
> `!uname.value`
>
> 2. **`return`** 语句终止函数的执行，并返回一个指定的值给函数调用者
>
> ~~~javascript
> //给指定的值 return 后面的代码不执行， 所以不能换行写
> function getRectArea(width, height) {
>   if (width > 0 && height > 0) {
>     return width * height;
>   }
>   return 0;
> }
> 
> console.log(getRectArea(3, 4));
> // Expected output: 12
> 
> console.log(getRectArea(-3, 4));
> // Expected output: 0
> ~~~

~~~javascript

// 2.3 给 arr 数组追加对象，里面存储 表单获取过来的数据
// push: 添加元素， 因为点击后才添加 arr.length + 1
//修改数组元素的内容内容  数组.push(新增内容)
      arr.push({
        // 处理 stuId：数组最后一条数据的stuId + 1      
        stuId: arr.length ? arr[arr.length - 1].stuId + 1 : 1,
        //表单的值 value
        uname: uname.value,
        age: age.value,
        salary: salary.value,
        gender: gender.value,
        city: city.value,
        //自动获取当前日期  toLocaleString()：换成字符串
        time: new Date().toLocaleString()
      })

~~~

> :octopus:注意点
>
> 1，序号每次添加要加1
>
> 2，获取当前时间并且转换为字符串

~~~javascript
 // 2.4 渲染页面和重置表单（reset()方法）
      render()
//添加后，重置数据 -- 为空 （this = infor）
      this.reset() // 重置表单
~~~

![image-20230906102412715](/Users/guoguo/Library/Application Support/typora-user-images/image-20230906102412715.png)

~~~javascript
 数据刷新就没了， 要把新数据存储在本地
// 2.5 把数组重新存入本地存储里面，记得转换为JSON字符串存储
 //把最新的数据存进去
      localStorage.setItem('data', JSON.stringify(arr))
    })
~~~

~~~javascript
//执行过程
1.提交先判断用户提交的内容够不够 （ 不够给提示）
2.把用户输入的相关信息生成一条对象， 追加给 arr （空数组）
3. 把最新的数据存到本地
~~~

## 3 - 删除业务

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230906105657916.png" alt="image-20230906105657916" style="zoom:50%;" />

> 1. 要给tbody注册事件而不是给删除按钮是因为会有新数据产生
> 2. 要知道点了哪个删除按钮 --- 索引号

~~~javascript
// 3. 删除业务
3.1 采用事件委托形式，给 tbody 注册点击事件
3.2 得到当前点击链接的索引号。渲染数据的时候，动态给a链接添加自定义属性例如 data-id="0"
3.3 根据索引号，利用 splice 删除数组这条数据
3.4 重新渲染页面 
3.5 把最新 arr 数组存入本地存储
~~~

~~~javascript
// 3.1 采用事件委托形式，给 tbody 注册点击事件
    tbody.addEventListener('click', function (e) {
      // 判断是否点击的是删除按钮  A 链接， 用事件对象
      //tagName 要大写（a链接）
      if (e.target.tagName === 'A') {
        // alert(11)
// 3.2 得到当前点击链接的索引号。渲染数据的时候，动态给a链接添加自定义属性例如 data-id="0"  
        //index : 数组的下标
<a href="javascript:" data-id="${index}">
        <i class="iconfont icon-shanchu"></i>
                删除
</a>
        console.log(e.target.dataset.id) // 0,2,1 
        // 确认框 确认是否要真的删除
        // confirm: 返回2个值 确认 取消 false ： 不执行
        if (confirm('您确定要删除这条数据吗？')) {
          // 3.3 根据索引号，利用 splice 删除数组这条数据
          arr.splice(e.target.dataset.id, 1)
          // 3.4 重新渲染页面 
          render()
          // 3.5 把最新 arr 数组存入本地存储
          localStorage.setItem('data', JSON.stringify(arr))
        }
      }
    })

~~~

~~~javascript
BUG：删除第一条第二条的ID:2,再添加一条ID 还是 2 （ID是唯一）
1. 数组[数组长度 - 1].stuld = 获取到当前最后ID + 1 = 添加一条
2. 没有数据（都删除）， 没有stuld -- 报错， 直接赋值为1
	stuId: arr.length ? arr[arr.length - 1].stuId + 1 : 1
//arr.length you数据吗？三元表达式
~~~

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230906112141672.png" alt="image-20230906112141672" style="zoom:50%;" />

