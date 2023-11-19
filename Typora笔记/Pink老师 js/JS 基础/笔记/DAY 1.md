# 一，JavaScript 介绍

## 1. Javascript 是什么？

- 是一种运行在用户端（浏览器）的变成语言，实现人机交互效果
- 作用
  - 页面特效（临听用户的一些行为让网页做出对应的反应）
  - 表单验证（针对表单数据的合法性进行判断）
    - 例如：输入手机号只能输入数字
  - 数据交互（获取后台数据，渲染到前端）
  - 服务端编程（note.js）
- Javacript的组成
  - ECMAScript: 规定了js基础语法核心知识
    - 例如：变量，分支语句，循环语句，对象等等
  - Web APIs：
    - DOM：操作文档，比如对页面元素进行移动，大小，添加删除等操作
    - BOM：操作浏览器，比如：页面弹窗，检测窗口宽度，存储数据到浏览器等等

## 2. Javascript引入方式

- 内部js：直接写在html文件里。用script标签包住

  - 规范：script标签在</body>上面

  ~~~html
  <body>
    <script>
      //页面弹出警示框
      alert('你好')
    </script>
  </body>
  ~~~

> 注意事项：我们将<script>放在HTML文件的地步附近的原因是浏览器会按照代码文件中的顺序加载HTML

- 内联js（行内）代码卸载标签内部

- 外部js： 代码卸 .js 结尾的文件里

  - 语法：通过script标签，引入到html页面中

  ~~~html
  //外部有js文件， 
  <body>
    <script src="./my.js"></script>
  </body>
  ~~~

  > 注意事项：
  >
  > 	1. script标签中无需要写代码，否则会被忽略
  > 	2. 外部js会使用代码更加有序，更易于服用，且没有了脚本的混合，HTML也会更加易读

## 3. Javascript注释

- 单行注释 //
- 块行注释 /* */

## 4. Javascript结束符

- 使用分号，可以用可以不用（需要统一）

## 5. Javascrip输入输出语法

- 输出语法：输出和输入也可以理为人和计算机的交互，用户通过鼠标，键盘等向计算机输入信息，计算机处理后再展示结果给用户

  :arrow_right: 这就是一次输入和输出的过程

  - 语法1：

    - 作用：向body内输出内容
    - 注意：如果输出的内容写的是标签，也会被解析成网页元素

    ~~~html
    <body>
      <script>
        document.write('我是div')
        document.write('<h1>okok</h1>')
      </script>
    </body>
    ~~~

  - 语法2：

    - 作用：页面弹出警告对话框

      ~~~html
       alert('okok')
      ~~~

  - 语法3：

  - 控制台输出语法，程序员调试使用

  ~~~html
  <script>
      console.log(okok)
    </script>
  ~~~

- 输入语法：

  - 显示一个对话框，对话框中包含一条文字信息，用来提示输入文字

    ~~~html
    prompt('请输入你的年龄')
    ~~~

    - 显示框：

    <img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230809143254883.png" alt="image-20230809143254883" style="zoom:50%;" />

## 6.代码这行顺序

- 按html文档流顺序执行JS代码
- alert() 和 prompt() 会跳过页面渲染先被执行

# 二，变量

## 1.字面量

- 在计算机科学中，字面量（literal) 是在计算机中描述 事/物
  - 例如： 
    - 我们的工资：1000 -》1000就是数字字面量
    - ‘黑马程序员’ 字符串字面量
    - []数组字面量 {}对象字面量

## 2.变量命名规则与规范

- 规则：
  - 不能用关键词
    - 关键词：有特意函数的字符，js内置的一些英语词汇
      - 例如：let，if，for。。。
  - 只能用下划线，字母，数字，$组成，而且==数字不能开头==
  - 字母严格==区分大小==
- 规范：
  - 起名有意义
  - 每个单词首字母小写，后面每个单词首字母大写
    - 例子：userName

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230810074652533.png" alt="image-20230810074652533" style="zoom:50%;" />

## 3.let - var的区别

- let为了解决var的一些问题：
  - 可以先使用 在声明 (不合理) ：不报错， 会显示没有值
  - var 声明过的变量可以重复声明(不合理) ： let重复报错
  -  比如变量提升、全局变量、没有块级作用域等等

## 4.数组 （[]）

- 数组（array）一种将数据存储在单个变量名下的优雅方式

`let arr = [数组字面量]`

- 数组的基本使用

  - 数组是按照顺序保存，所以每个数据都有自己的编号
  - 计算机中的编号==从0开始==
  - 在数组中，数据的编号也叫索引或下标
  - 数组可以存储任意类型的数据

- 取值语法

  - 通过下标取数据
  - 取出来是什么类型的， 就根据这种类型特点来访问

  ~~~html
  	<script>
      // 1.声明数组 有序（中文加‘’）
      let name = ['小明', '小红', '小李', '小刚']
      // 2.使用数组 数组明[索引号]从0开始算
       console.log(name[2])
    </script>
  ~~~

## 5.一些术语

- 元素：数组中保存的每个数据都叫数组元素（小明， 小李）
- ==下标：==数组中数据的编号
- ==长度：== 数组中数据的个数，通过数组的length属性获取

~~~html
	<script>
    let name = ['小明', '小红', '小李', '小刚']
    console.log(name.length) 
    // 控制台显示 4
  </script>
~~~

# 三，常量

## 1.常量的基本使用

- 概念：使用 `const` 声明的变量称为”常量“

- 使用场景： 摸个变量永远不会改变的时候，就使用`const`

  > 1.常量 不允许重新赋值（ 值固定），声明的时候必须赋值（初始化）
  >
  > 例如：可以写 `let age`没有给赋值，常量必须要给
  >
  > 2. 不需要重新赋值的数据使用 `const`

# 四，数据类型

## 1.数据类型

- JS 数据类型整体分为2大类：
  - 基本数据类型
    - number 数据型
    - string 字符串型
    - boolean 布尔型
    - undefined 未定义型
    - null 空类型
  - 引用数据类型：
    - object 对象

### 1.1 数据类型 - 数字类型

- 即我们数学中学到的数字：整数，小数，正数，负数

  > JS中正数，负数，小数等同一称为数字类型
  >
  > JS是弱数据类型，变量到底是那种类型，只有复制之后才知道

- 数字可以有很多操作，所以经常和算术运算符一起：减，加，除，乘，==取余==（求摸 số dư)

  - 取余： 经常作为某个数字是否被==整除==（chia hết)

- JS中优先级越高越先被执行，优先级相同以书从左向右执行

  - 乘，除，取余优先级相同
  
- ==NaN== 代表一个计算错误，它是一个不正确或者一个未定义的数学操作索得到的结果（会再结果显示）

  - NaN 是粘性，对NaN右任何操作多会返回NaN
  - 例如： 字 - 数字 = NaN
  

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230814094153470.png" alt="image-20230814094153470" style="zoom:50%;" />

### 1.2 数字类型 - 字符串类型 （string）

- 字符串拼接：
  - 场景：+ （运算符）可以实现字符串的拼接

~~~html
 <script>
    let age = 19
    <!-- 用加号 -->
    document.write('我今年' + age + '岁')
  </script>
~~~

- ==模版字符串==
  - 拼接字符和变量
  - 语法
    - ==``== （反引号）
    - 内容拼接变量时，用 ==${}==包住变量

~~~html
	<script>
    let age = 20
    document.write(`我今年${age}岁`)
  </script>
~~~

### 1.3 数字类型 - 未定义类型 (undefined)

- 未定义是比较特殊的类型，只有一个值undefined
- 值声明变量，不赋值的情况下，变量会默认为undefined
- 工作使用场景
  - 声明一个变量，等待转送来的数据
  - 如果不知道这个数据是否转移过来，此时可以通过检测这个变量是否undefined

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230814094634778.png" alt="image-20230814094634778" style="zoom:50%;" />

### 1.4 数据类型 - null （空类型）

- 代表 “无，空，值未知” 的特殊值

- null 跟undefined的区别

  - undefined： 表示没有赋值
  - null： 表示赋值了，但是内容为空

  ~~~css
  	<script>
      // 控制台显示 NaN （错）
      console.log(undefined + 1)
      // 控制台显示 1 （0+1）
      console.log(null + 1)
    </script>
  ~~~

- null使用场景:

  - 将来有一个变量里面存放是一个对象，但是对象还没创建好， 可以先给null

### 1.5 数据类型 - 布尔类型(boolean)

- 表示肯定或者否定，固定两个值 true 和 false

~~~html
	<script>
    // 控制台显示 false
    console.log(3 > 4)
  </script>
~~~

## 2.检测数据类型

- 控制台语句经常用于测试结果
- ==数字类型和布尔型==的颜色为==蓝色==，==字符串和underfined==颜色为==灰色==
- 通过 typeof 关键词检测数据类型
  - typeof 运算符可以返回被检测的数据类型，支持两种写法
    - 作为运算符： typeof x
    - 函数形式： typeof(x)

~~~html
 <script>
    // 控制台： boolean
    let flag = false
    console.log(typeof flag)
    // 控制台：object (对象)
    let ojb = null
    console.log(typeof ojb)
  </script>
~~~

# 五，类型转换

## 1.为什么要类型转换

- 使用表单， 输入prompt获取过来的数据==默认是字符串类型==，此时不能直接简单的进行加法运算

- 简单来说：把一种数据类型的变量转换为我们需要的数据类型

## 2.隐私转换

- 某些运算符被执行时，系统内部自动将数据类型进行转换

- 规则：

  - +号两边只有一个是字符串，会把另外一个转换成==字符串== （1）

  ~~~html
   <script>
      // 结果 1111
      console.log('11' + 11)
      // 结果 1111
      console.log('11' + '11');
    </script>
  ~~~

  - 除了+ 号以外的算术运算符都会把数据转换成==数字类型==

  > +号作为正号解析可以转换成数字型
  >
  > 任何数据和字符串相加结果都是字符串 （1）只要有一边是字符串， 都换成字符串
  >
  > ~~~html
  >  	<script>
  >     // 结果 数字型
  >     console.log(+'123');
  >   </script>
  > ~~~

## 3.显示转换

- 自己写代码告诉系统换成什么类型

- 转换为数字型

  - Number（数据）

    - 转换成数字类型

    ~~~html
     <script>
        // 字符串转换成数字型
        let str = '123'
        console.log(Number(str));
      </script>
    ~~~

    

    - 如果字符串内容里有非数字，转换失败时结果为NaN即不是一个数字
    - NaN也是number类型的数字，代表非数字

    ~~~html
    	<script>
        // 结果 ： NaN
        let str1 = 'pink'
        console.log(Number(str1));
      </script>
    ~~~

  - parseInt（数据）：只保留整数

  ~~~html
  <script>
      // 数字在字的中间，结果 NaN
      console.log(parseFloat('abc12.94px'))
    </script>
  ~~~
  
  - parseFloat（数据）：可以保留小数

~~~html
<script>
    // 字符串转换成数字型
    let str = '123'
    console.log(Number(str))
    // 结果 ： NaN
    let str1 = 'pink'
    console.log(Number(str1))
    let num = prompt('请输入您的年薪')
    // 输入后结果是字符串
    console.log(num)
    // 转换成数字型
    console.log(Number(num))
     // 更简单的写法
    let num = Number(prompt('请输入您的年薪'))
    console.log(num)
  </script>
~~~















