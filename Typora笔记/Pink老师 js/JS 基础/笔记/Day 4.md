# 一，函数

## 1.为什么需要函数

- function 是被设置为执行特定任务的代码块， 有利于精简代码方便服用
  - 例如：alert() 、 prompt() 和 console.log() 函数已经封装好了， 我们直接使用 （特点： 小括号）

## 2. 函数使用

### 2.1 函数的声明语法

 <img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230818093326965.png" alt="image-20230818093326965" style="zoom:50%;" />

~~~html
 <script>
  // 1.声明函数
    function sayHi() {
      // 需要再使用的代码
      document.write('hi')
    }
  // 2. 调用函数（ 不调用不会执行）
    // 需要再使用调用就好
    sayHi()
  </script>
~~~

- 函数命名规范：
  - 和变量命名基本一样
  - 前缀应该为动词

![image-20230818094048085](/Users/guoguo/Library/Application Support/typora-user-images/image-20230818094048085.png)

### 2.2 函数的调用语法

- 声明（定义）的函数必须调用才会真正被执行，使用 () 调用函数

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230818094208668.png" alt="image-20230818094208668" style="zoom:50%;" />

### 2.3 函数体

- 函数体是函数的构成部分，它负责将相同或相似代码“包裹”起来，直到函数调用时函数体内的代码才会被执行。函数的功能代码都要写在函数体当中

![image-20230818094355427](/Users/guoguo/Library/Application Support/typora-user-images/image-20230818094355427.png)

> - 函数的服用和循环重复代码的区别：
>   - 循环代码写完即执行， 不能很方便控制执行位置
>   - 函数可以随时调用，随时执行

#### *练习 1：

需求： 把99乘法表封装到函数里面，重复调用3次

#### ？练习 2：

需求：

1. 封装一个函数，计算两个数的和
2. 封装一个函数，计算1-100之间所有数的和

## 3. 函数传参

![image-20230818095934519](/Users/guoguo/Library/Application Support/typora-user-images/image-20230818095934519.png)

- 调用语法

![image-20230818100044919](/Users/guoguo/Library/Application Support/typora-user-images/image-20230818100044919.png)

- 例如：

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230818100132189.png" alt="image-20230818100132189" style="zoom:50%;" />

- 形参：声明函数时写在**<u>函数名右边小括号</u>**里的叫形参（形式上的参数）
- 实参：调用函数时写在函数名右边小括号里的叫实参（实际上的参数）
  - 形参可以理解为是在这个c（比如 num1 = 10）实参可以理解为是给这个==变量赋值== 
  - 开发中尽量保持形参和实参个数一致（数量）

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230818100716690.png" alt="image-20230818100716690" style="zoom:50%;" />

### ？练习 1：

需求：采取函数封装的形式：输入2个数，计算两者的和，打印到页面中

- 参数默认值：不给值的情况下

  - undefined：但是如果做用户不输入实参，刚才的案例，则出现 undefined + undefined = NaN
  - 改进：用户不输入实参，可以给 **形参默认值**，可以默认为 0（x=0,y=0）
  - 这个默认值只会在缺少实参参数传递时 才会被执行，所以有参数会优先执行传递过来的实参, 否则默认为undefined

  ![image-20230818101703550](/Users/guoguo/Library/Application Support/typora-user-images/image-20230818101703550.png)

  ### ？ 练习2

  需求：学生的分数是一个数组,计算每个学生的总分

## 4. 函数返回值

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230818103440390.png" alt="image-20230818103440390" style="zoom:50%;" />

- 调用者 【（命名）（）】

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230818103933971.png" alt="image-20230818103933971" style="zoom:50%;" />

- 细节： 
  - 在函数体中使用 return 关键字能将内部的执行结果交给函数外部使用 （可以在funtion外面使用）
  - return 后面代码不会再被执行，会立即结束当前函数，所以 return 后面的==数据不要换行写== （1）
  - return函数可以没有 return，这种情况函数默认返回值为 undefined （2）

~~~html
 <script>
    // 求和函数的写法
    function getTotalPrice(x, y) {
      return x + y
（1）// return 后面的代码不会被执行
    }
    // console.log(getTotalPrice(1, 2))
    // console.log(getTotalPrice(1, 2))
    let sum = getTotalPrice(1, 2)
    console.log(sum)
    console.log(sum)
（2）//函数可以没有 return
    function fn() {

    }
    let re = fn()
    console.log(re)  // undefined
  </script>
~~~

### ？练习

需求：

1. 求任意2个数中的最大值, 并返回 
2. 求任意数组中的最大值并返回这个最大值 
3. 求任意数组中的最小值并返回这个最小值

> 断点调试： 进入函数内部看执行过程 F11

## 5. 函数细节补充

- 两个相同的函数后面的会覆盖前面的函数
  - 例如（1）： 打印结果： 2
- 在Javascript中 实参的个数和形参的个数可以不一致
  - 如果形参过多 会自动填上undefined (了解即可)
  - 如果实参过多 那么多余的实参会被忽略 (函数内部有一个arguments,里面装着所有的实参) 
- 函数一旦碰到return就不会在往下执行了 函数的结束用return

~~~html
<script>
    // function getSum(x, y) {
    //   return x + y
    //   // 返回值返回给了谁？ 函数的调用者  getSum(1, 2)
    //   // getSum(1, 2) = 3
    // }
    // // let result = getSum(1, 2) = 3
    // // let num = parseInt('12px')
    // let result = getSum(1, 2)
    // console.log(result)
  
    （1）// 1. 函数名相同， 后面覆盖前面

    // function fn() {
    //   console.log(1)
    // }
    // function fn() {
    //   console.log(2)
    // }
    // fn()
    （2）// 2. 参数不匹配

    function fn(a, b) {
      console.log(a + b)
    }
    // (1). 实参多余形参   剩余的实参不参与运算
    // fn(1, 2, 3)
    // (2). 实参少于形参   剩余的实参不参与运算
    fn(1)   // 1 + undefined  = NaN 
  </script>
~~~



## 6. 作用城

- 在JavaScript中，根据作用域的不同，变量可以分为：

![image-20230818105311120](/Users/guoguo/Library/Application Support/typora-user-images/image-20230818105311120.png)

- 变量有一个坑， 特殊情况：

  - 如果函数内部，变量没有声明，直接赋值，也当全局变量看，但是强烈不推荐
    - 例如： 如果外面在声明一个num = 20 ，还是按照里面的值打印（10）

  <img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230818110008620.png" alt="image-20230818110008620" style="zoom:50%;" />

  - 但是有一种情况，函数内部的形参可以看做是局部变量。

  <img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230818110221430.png" alt="image-20230818110221430" style="zoom:50%;" />

~~~html
<script>
    let num = 10  // 1. 全局变量，let在函数外面
    console.log(num)
    function fn() {
      console.log(num)
    }
    fn()

    // 2. 局部变量， let在函数里面
    function fun() {
      let str = 'pink'
    }
    console.log(str)  // 错误
  </script>
~~~

- 变量访问的原则

  - 只要是代码，就至少有一个作用域
  - 写在函数内部的局部作用域
  - 如果函数中还有函数，那么在这个作用域中就又可以诞生一个作用域
  - ==访问原则==：**在能够访问到的情况下 先局部， 局部没有在找全局** （从里面然后找外面）

  <img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230818110458263.png" alt="image-20230818110458263" style="zoom:50%;" />

## 7. 匿名函数

- 函数可以分为：

![image-20230818110726262](/Users/guoguo/Library/Application Support/typora-user-images/image-20230818110726262.png)

- 匿名函数：无法直接使用， 使用方式
  - 函数表达式
  - 立即执行函数

### 7.1 函数表达式

- 将匿名函数 我们将这个称为**函数表达式**（1）

![image-20230818111011785](/Users/guoguo/Library/Application Support/typora-user-images/image-20230818111011785.png)

~~~html
<script>
    // console.log(num)
    // let num = 10

    // 3 + 4
    // num = 10
    // 1. 函数表达式
    fn(1, 2)  //错误 （不能写在这里）
   （1）let fn = function (x, y) {
      // console.log('我是函数表达式')
      console.log(x + y)
    }

    // 函数表达式和 具名函数的不同   function fn() {}
    // 1. 具名函数的调用可以写到任何位置
    // 2. 函数表达式，必须先声明函数表达式，后调用
    ===》调用不能写在前面
  ***具名的函数：
    // function fun() {
    //   console.log(1)
    // }
    // fun()
  </script>
~~~



### 7.2 立即执行函数

- 场景介绍：避免·全局变量之间的污染 
- 多个立即执行函数要用 ; 隔开，要不然会报错 （一样声明命名抱起来就可以）

![·](/Users/guoguo/Library/Application Support/typora-user-images/image-20230818111621136.png)

~~~html
<script>
  *声明一样的会报错
    // let num = 10
    // let num = 20
  *解决上面的问题  (function（）{}) |  ()  第一小括号： 函数，第二小括号： 调用 - 实参
  		+ 先写2个小括号然后把函数写里面
    // (function () {
    //   let num = 10
    // })();
  * 加分号（；）可以加前面-- 不报错了
    // (function () {
    //   let num = 20
    // })();
    // 1. 第一种写法
    (function (x, y) {
      console.log(x + y)
      *声明一样 - 不影响
      let arr = []
    })(1, 2);
    // (function(){})();
    // 2.第二种写法
    // (function () { }()); 一个小括号（函数+ 调用）
    (function (x, y) {
      *声明一样 - 不影响·
      let arr = []
      console.log(x + y)
    }(1, 3));


    // (function(){})()
    // (function(){}())
  </script>
~~~



# 二，综合案例