# 一，运算符

## 1，赋值运算符

- 对变量进行赋值的运算符
- 已经学过的赋值运算符： ”=“ 将等号右边的值给左边，需求左边必须是一个容器
- 其他赋值运算符：
  - +=
  - -=
  - *=
  - /=
  - %=
- 使用这些运算符可以在对变量赋值时进行快速操作

~~~html
// 让变量加数字	
	<script>
    let num = 1
    // 让变量加1
    num += 1
    console.log(num)
  </script>
~~~

## 2，一元运算符 （num = num + 1）

- 二元运算符：

  <img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230814164244305.png" alt="image-20230814164244305" style="zoom:50%;" />

- 一元运算符 （正负号）

  - 自增：++ 让变量值加1
  - 减增：-- 让变量值减1

- 使用场景：经常用于==计数==来使用

  - 例如：进行10次操作，用它来算进行了多少次

- 自增运算符的用法：

  - 前置自增：每执行一次，当前变量数值加1，其作用相当于 `num + = 1` (简写) 
  - 后置自增：每执行一次，当前变量数值加1，其作用相当于 `num + = 1` （简写）
  - 区别：单独使用没有区别，参与运算就由区别
    - 前置自增：先自加再使用
    - 后置自增：先使用再自加

  > 1. 前置自增和后置自增独立使用时每什么区别
  > 2. 一般开发中都是独立使用
  > 3. 后面 i++ 后置自增使用相对比较多， 并且都是单独使用

~~~html
	let num = 10
  num = num + 1
	//简写
	num += 1
~~~



## 3，比较运算符 （5>=3)

- 比较运算符的介绍
  - 使用场景：比较两个数据大小， 是否相等
- 比较运算符：
  - 大于， 小于， 大于等于， 小于等于：>, < ,>=, <=
  - 左右两边的值是否相等： ==
  - 左右两边是否 ==类型 和 值== 都相等：===
  - 左右两边是否不全等：!==
  - 比较结果为 boolean 类型，只会得到 true 或者 false

- 字符串也可以进行比较，比较的字符对应的ASCII码 （了解即可）
  - 从左向右一次比较
  - 如果第一位一样再比较第二位
- ==NaN不等于任何值==，包括它本身
  - 涉及到NaN都是false
- 尽量不要比较小数，因为小数右精度问题
  - 比较小数需要乘以*10然后又除以/10
- 不同类型之间比较会==发生隐式问题== （例如代码（1））
  - 最终把数据隐式转换为number类型再比较
  - 所以在开发中，如果进行准确的比较我们更喜欢用 === 
- = 和 == 和 === 的区别
  - = 是赋值 Ø
  - == 是判断 只要求值相等，不要求数据类型一样即可返回true
  - === 是全等 要求值和数据类型都一样返回的才是true

~~~html
 <script>
    console.log(3 > 5)
    console.log(3 >= 3)
    console.log(2 == 2)
（1）// 比较运算符有隐式转换 把'2' 转换为 2  双等号 只判断值
    console.log(2 == '2')  // true
（2）// console.log(undefined === null)
    // === 全等 判断 值 和 数据类型都一样才行
    // 以后判断是否相等 请用 ===  
    console.log(2 === '2')
    console.log(NaN === NaN) // NaN 不等于任何人，包括他自己
    console.log(2 !== '2')  // true  
    console.log(2 != '2') // false 
    console.log('-------------------------')
    console.log('a' < 'b') // true
    console.log('aa' < 'ab') // true
    console.log('aa' < 'aac') // true
    console.log('-------------------------')
  </script>
~~~



## 4，逻辑运算符 (num > 5 && num < 10)

- 使用场景：逻辑运算法用来解决多重条件判断

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230815155349904.png" alt="image-20230815155349904" style="zoom:50%;" />

~~~html
<script>
    // 逻辑与 一假则假 （只要有一个假 -》假）
    console.log(true && true)
    console.log(false && true)
    console.log(3 < 5 && 3 > 2)
    console.log(3 < 5 && 3 < 2)
    console.log('-----------------')
    // 逻辑或 一真则真 （只要有一个真 -》 真）
    console.log(true || true)
    console.log(false || true)
    console.log(false || false)
    console.log('-----------------')
    // 逻辑非  取反
    console.log(!true)
    console.log(!false)

    console.log('-----------------')

    let num = 6
    console.log(num > 5 && num < 10)
    console.log('-----------------')
  </script>
~~~

### ** 练习:

**判断一个数是4的倍数，且不是100的倍数**

- 需求：用户输入一个，判断这个数能被4整除，但是不能被100整除,满足条件，页面弹出true，否则弹出false

~~~html
	<script>
    let num = +prompt('请输入你的号码')
    //被整除（số bị chia）写在 % 前面
    alert(num % 4 === 0 && num % 100 !== 0)
  </script>
~~~



## 5，运算符优先级

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230815161318862.png" alt="image-20230815161318862"  />

- 一元运算符里面的逻辑优先级很高
- 逻辑&& 比 逻辑 || 有限级高

### *练习

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230815161627082.png" alt="image-20230815161627082" style="zoom:50%;" />

# 二，语句

## 1.表达式和语句

- 表达式：可以被求值的代码，JS 引擎会将其==计算出一个结果==
- 语句：是可以执行的代码
  - 例如：prompt() 可以弹出一个输入框，还有 if语句 for 循环语句等等
- 区别：
  - 表达式：因为表达式可被求值，所以它可以写在==赋值语句的右侧==
  - 语句：而语句==不一定有值==，所以比如 alert() for和break 等语句就不能被用于赋值

## 2.分支语句

### 2.1 程序三大流程控制语句

- 以前我们写的代码，写几句就从上往下执行几句，这种叫顺序结构 

- 有的时候要==根据条件选择执行代码==，这种就叫==分支结构== 
- 某段代码被==重复执行==，就叫==循环结构==

### 2.2 分支语句 （条件）

- 分支语句可以让我们有选择性的执行想要的代码
- 分支语句包括：
  - If分支语句
  - 三元运算符
  - switch语句

#### 2.2.1 if语句

- if语句有3种：单分支，双分支，多分支
- ==单分支使==用语法：

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230815163806193.png" alt="image-20230815163806193" style="zoom:50%;" />

- ​		
  - （条件）为true时，进入大括号里执行代码	
  - （条件）结果若不是布尔类型时，==会发生隐式转换==为布尔类型
  - （1）除了0 所有的数字都为真
  - （2）除了 '空格' 所有的字符串都为真 true （''）

~~~html
<script>
    // 单分支语句
    // if (false) {
    //   console.log('执行语句')
    // }
    // if (3 > 5) {
    //   console.log('执行语句')
    // }
    // if (2 === '2') {
    //   console.log('执行语句')
    // }·
（1）//   除了0 所有的数字都为真 （flase）
    //   if (0) {·
    //     console.log('执行语句')
    //   }
（2）// 除了 '' 所有的字符串都为真 （true）
    // if ('pink老师') {
    //   console.log('执行语句')
    // }
    // if ('') {
    //   console.log('执行语句')
    // }
    // // if ('') console.log('执行语句')
  </script>
~~~

##### * 练习单分支

- 需求：用户输入高考成绩，如果分数大于700，则提示恭喜考入黑马程序员

~~~html
	<script>
    let num = +prompt('请输入分数')
    if (num > 700) {
      alert('恭喜你')
    }
  </script>
~~~

- ==双分支if==语法 （if else）

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230815165248890.png" alt="image-20230815165248890" style="zoom:50%;" />

##### *练习：双分支

- 需求：用户输入，用户名：pink，密码：123456， 则提示登录成功，否则提示登录失败分析

~~~html
	<script>
    let name = prompt('请输入用户名')
    let pass = prompt('请输入密码')
    if (name === 'pink' && pass === '123456') {
      alert('登录成功')
    } else {
      alert('登录失败')
    }
  </script>
~~~

- 多分支if语法（ if else 多次）
  - 使用场景：有多结果的时候，比如学习成绩分为： 优 良中 差
  - 先判断条件1， 满足就执行代码1， 其他不执行

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230816091655392.png" alt="image-20230816091655392" style="zoom:50%;" />

##### *练习：多分支

需求：根据输入不同的成绩，反馈不同的评价 注：

 ①：成绩90以上是 优秀 

 ②：成绩70~90是 良好 

③：成绩是60~70之间是 及格

 ④：成绩60分以下是 不及格

#### 2.2.2 三元运算符

- 使用场景：比if更简单的写法
- 符号：（？跟 ：）配合使用
- 语法：

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230816092208307.png" alt="image-20230816092208307" style="zoom:50%;" />

##### *练习 1：

需求：用户输入2个数，控制台输出最大的值

~~~html
 <script>
    let num1 = prompt('请输入数字1')
    let num2 = prompt('请输入数字2')
    console.log(num1 > num2 ? num1 : num2);
 </script>
~~~

##### *练习2：

需求：用户输入1个数，如果数字小于10，则前面进行补0， 比如 09 03 等

~~~html
	<script>
    let num = prompt('请输入数字')
    alert(num < 10 ? '0' + num : num)
    // 老师写的代码
    num1 > num2 ? alert(`最大值是: ${num1}`) : alert(`最大值是: ${num2}`)
  </script>
~~~

#### 2.2.3 switch 语句

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230816093711859.png" alt="image-20230816093711859" style="zoom: 33%;" />

- 找到（数据： 用户传过来）跟 case的值==全等== ，并执行里面对应的代码
- 如果没有找到全等的，会执行default里的代码

~~~html
 <script>
    switch (2) {
      case 1:
        console.log('您选择的是1')
        break  // 退出switch
      case 2:
        console.log('您选择的是2')
        break  // 退出switch
      case 3:
        console.log('您选择的是3')
        break  // 退出switch
      default:
        console.log('没有符合条件的')
    }
  </script>
~~~

> 1. switch case语句一般用于等值判断，不适合于区间判断
> 2. switch case 一般需要配合break关键词使用， 没有break会造成case穿透

##### *练习：简单计算机

需求：用户输入2个数字，然后输入 + - * / 任何一个，可以计算结果

~~~html
	<script>
    // 1.用户输入  2个数字 +  操作符号  + - *  / 
    let num1 = +prompt('请您输入第一个数字:')
    let num2 = +prompt('请您输入第二个数字:')
    let sp = prompt('请您输入 + - * / 其中一个:')
    // 2. 判断输出
    switch (sp) {
       // 用引号跟sp全等
      case '+':
        alert(`两个数的加法操作是${num1 + num2}`)
        break
      case '-':
        alert(`两个数的减法操作是${num1 - num2}`)
        break
      case '*':
        alert(`两个数的乘法操作是${num1 * num2}`)
        break
      case '/':
        alert(`两个数的除法操作是${num1 / num2}`)
        break
      default:
        alert(`你干啥咧，请输入+-*/`)

    } 
  </script>
~~~



## 3. 循环语句

### 3.1 循环结构

#### 3.1.2 断点调试

- 作用：帮助更好了解代码运行，工作时可以更快找到bug
- 浏览器打开调试页面：
  - 按F12打开开发者工具
  - 点到sources一栏
  - 选择代码文件

- 断点：在某句代码上加的标记就叫断点，当程序员执行到这句有标记的代码时会展示停下来

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230816101111691.png" alt="image-20230816101111691" style="zoom:50%;" />

#### 3.1.3 while 循环

- 循环：重复执行一些操作， while : 在…. 期间， 所以 while循环 就是==在满足条件期间，重复执行==某些代码。
  - 比如：我们运行相同的代码输出5次（输出5句 “我学的很棒”）
- while基本语法
  - 跟if语句很像，都要满足条件true才能执行代码
  - while大括号里代码执行完毕后不会跳出，而是继续回到小括号里判断条件是否满足，若满足又执行大括号里的代码，然后再回到 小括号判断条件，==直到括号内条件不满足，即跳出==

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230816101531871.png" alt="image-20230816101531871"  />

- ==while循环三要素==：
  - 循环的本质就是以某个变量为起始值，然后不断产生变化量，慢慢靠近终止条件的过程
    - 变量起始值（1）
    - 终止条件（没有终止条件会一直执行，造成死循环）（2）
    - 变量变化量（用自增或者减增）（3）

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230816102401084.png" alt="image-20230816102401084" style="zoom:50%;" />

~~~html
 <script>
    // // 1. 变量的起始值
    // let i = 1
    // // 2. 终止条件
    // while (i <= 3) {
    //   document.write('我要循环三次 <br>')
    //   // 3. 变量的变化量
    //   i++
    // }
    // 1. 变量的起始值
    let end = +prompt('请输入次数:')
    let i = 1
    // 2. 终止条件
    while (i <= end) {
      document.write('我要循环三次 <br>')
      // 3. 变量的变化量
      i++
    }

  </script>
~~~

##### *练习 1：

需求：使用while循环，页面中打印，可以添加换行效果

~~~html
	<script>
    let i = 1
    while (i <= 10) {
      document.write('你很棒<br>')
      i++
    }
  </script>
~~~

##### ***练习 2：

需求：使用while循环，页面中打印，可以添加换行效果

**1. 计算从1加到100的总和并输出**

~~~html
<script>
    let i = 1
    let sum = 0
    while (i <= 100) {
      if (i % 2 === 0) {
        sum += i
      }
      i++
    }
    document.write(sum)
  </script>
~~~

- 循环退出：
  - 循环结束 
    - break：退出循环 
    - continue：结束本次循环，继续下次循环 
    - 区别：
      -  continue 退出本次循环，一般用于排除或者跳过某一个选项的时候, 可以使用continue （跳过这一条，不打印出来）
      -  break 退出整个循环，一般用于结果已经得到, 后续的循环不需要的时候可以使用 （打印出来，结束）

##### ***练习 3：

需求：页面弹出对话框，‘你爱我吗’，如果输入‘爱’，则结束，否则一直弹出对话框

~~~html
<script>
    while (true) {
      // 只要输入会一直执行这个代码
      let ai = prompt('你爱我吗')
      // 满足条件不弹了
      if (ai === '爱') {
        break
      }
    }
  </script>
~~~

# 三，综合案例：简易ATM取款机案例 

需求：用户可以选择存钱、取钱、查看余额和退出功能

~~~html
<script>
    // 1. 开始循环 输入框写到 循环里面
    // 3. 准备一个总的金额,写在里面会一直循环成100
    let money = 100
    while (true) {
      let re = +prompt(`
        请您选择操作：
        1.存钱
        2.取钱
        3.查看余额
        4.退出
        `)
      // 2. 如果用户输入的 4 则退出循环， break  写到if 里面，没有写到switch里面， 因为4需要break退出循环
      if (re === 4) {
        break
      }
      // 4. 根据输入做操作
      switch (re) {
        case 1:
          // 存钱
          let cun = +prompt('请输入存款金额')
          money = money + cun
          break
        case 2:
          // 存钱
          let qu = +prompt('请输入取款金额')
          money = money - qu
          break
        case 3:
          // 存钱
          alert(`您的银行卡余额是${money}`)
          break
      }
    }
  </script>
~~~

