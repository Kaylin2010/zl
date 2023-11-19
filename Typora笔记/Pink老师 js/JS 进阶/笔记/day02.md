# JavaScript 进阶 - 第2天

> 了解面向对象编程的基础概念及构造函数的作用，体会 JavaScript 一切皆对象的语言特征，掌握常见的对象属性和方法的使用。

- 了解面向对象编程中的一般概念
- 能够基于构造函数创建对象
- 理解 JavaScript 中一切皆对象的语言特征
- 理解引用对象类型值存储的的特征
- 掌握包装类型对象常见方法的使用

# 一，深入对象

> 了解面向对象的基础概念，能够利用构造函数创建对象。

## 1 - 创建对象三种方式

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230914200726137.png" alt="image-20230914200726137" style="zoom:50%;" />

## 2 - 构造函数

- 构造函数是专门用于==创建对象的函数==，如果一个函数使用 `new` 关键字调用，那么这个函数就是构造函数 （dùng cùng 1 cấu trúc để tạo nhiều 对象 mới)

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230914094729517.png" alt="image-20230914094729517" style="zoom:50%;" />

- 构造函数在技术上是常规函数，不过有两个约定
  1. 它们的命名以大写字母开头。
  2. 它们只能由 "new" 操作符来执行。

```html
<script>
  // 创建一个猪 构造函数 
    function Pig(uname, age) {
      this.uname = uname // 属性 - 形参
      this.age = age //this 对象
    }
  const p = new Pig('佩奇', 6)
    console.log(p)
</script>
```

==总结==

2. 使用 `new` 关键字调用函数的行为被称为实例化
3. 实例化构造函数时没有参数时可以省略 `()`
4. 构造函数的返回值即为新创建的对象
5. 构造函数内部的 `return` 返回的值无效！

6. 实践中为了从视觉上区分构造函数和普通函数，习惯将构造函数的首字母大写

### *案例 1：利用构造函数创建多个对象

- 需求：

①：写一个Goods构造函数

②：里面包含属性 name 商品名称 price 价格 count 库存数量

③：实例化多个商品对象，并打印到控制台，例如

小米 1999 20 

华为 3999 59 

vivo 1888 100

* **实例化执行过程**
  1. 创建空对象
  2. 构造函数this指向空对象
  3. 执行构造函数代码，修改this，添加新的属性
  4. 返回新对象

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230914100653128.png" alt="image-20230914100653128" style="zoom:50%;" />



## 3 - 实例成员

通过构造函数创建的对象称为实例对象，实例对象中的属性和方法称为实例成员。

```html
<script>
  // 构造函数
  function Person() {
    // 构造函数内部的 this 就是实例对象
    // 实例对象中动态添加属性
    this.name = '小明'
    // 实例对象动态添加方法
    this.sayHi = function () {
      console.log('大家好~')
    }
  }
  // 实例化，p1 是实例对象
  // p1 实际就是 构造函数内部的 this
  const p1 = new Person()
  console.log(p1)
  console.log(p1.name) // 访问实例属性
  p1.sayHi() // 调用实例方法
</script>
```

总结：

1. 构造函数内部 `this` 实际上就是实例对象，为其动态添加的属性和方法即为实例成员
2. 为构造函数传入参数，动态创建结构相同但值不同的对象

注：构造函数创建的实例对象彼此独立互不影响。

## 4 - 静态成员

- 在 JavaScript 中底层函数本质上也是对象类型，因此允许直接为函数动态添加属性或方法，构造函数的属性和方法被称为静态成员

```html
<script>
  // 构造函数
  function Person(name, age) {
    // 省略实例成员
  }
  // 静态属性
  Person.eyes = 2
  Person.arms = 2
  // 静态方法
  Person.walk = function () {
    console.log('^_^人都会走路...')
    // this 指向 Person
    console.log(this.eyes)
  }
</script>
```

总结：

1. 静态成员指的是添加到构造函数本身的属性和方法
2. 一般公共特征的属性或方法静态成员设置为静态成员
3. 静态成员方法中的 `this` 指向构造函数本身

# 二，内置构造函数

> 掌握各引用类型和包装类型对象属性和方法的使用。

在 JavaScript 中**最主要**的数据类型有 6 种，分别是字符串、数值、布尔、undefined、null 和 对象，常见的对象类型数据包括数组和普通对象。其中字符串、数值、布尔、undefined、null 也被称为简单类型或基础类型，对象也被称为引用类型。

在 JavaScript 内置了一些构造函数，绝大部的数据处理都是基于这些构造函数实现的，JavaScript 基础阶段学习的 `Date` 就是内置的构造函数。

```html
<script>
  // 实例化
	let date = new Date();
  
  // date 即为实例对象
  console.log(date);
</script>
```

甚至字符串、数值、布尔、数组、普通对象也都有专门的构造函数，用于创建对应类型的数据。

## 1 - Object

`Object` 是内置的构造函数，用于创建普通对象。

```html
<script>
   const o = { uname: 'pink', age: 18 }
    // 1.获得所有的属性名
    console.log(Object.keys(o))  //返回数组['uname', 'age']
    // 2. 获得所有的属性值
    console.log(Object.values(o))  //  ['pink', 18]
    // 3. 对象的拷贝
    // const oo = {}
    // Object.assign(oo, o)
    // console.log(oo)
    Object.assign(o, { gender: '女' }) //添加属性
    console.log(o)
</script>
```

。

总结：

1. 推荐使用字面量方式声明对象，而不是 `Object` 构造函数
2. `Object.assign` 静态方法创建新的对象
3. `Object.keys` 静态方法获取对象中所有属性
4. `Object.values` 表态方法获取对象中所有属性值

## 2 - Array

### 2.1**数组常见实例方法-核心方法**

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230914112309567.png" alt="image-20230914112309567" style="zoom:50%;" />

- `Array` 是内置的构造函数，用于创建数组
- reduce 返回函数累计处理的结果，经常用于求和等
  - 基本语法（return返回值）
  - 起始值可以省略，如果写就作为第一次累计的起始值

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230914111043632.png" alt="image-20230914111043632" style="zoom:50%;" />

```html
  <script>
    arr.reduce(function(累计值, 当前元素){}, 起始值)
    arr.reduce(function (prev, item) {
      // console.log(11)
      // console.log(prev)
      return prev + item
    }, 0)
    arr.reduce(function (prev, item) {
      console.log(11)
      // console.log(prev)
      return prev + item
    })
    //箭头函数
    const arr = [1, 2, 3]
    const re = arr.reduce((prev, item) => prev + item)
    console.log(re)
  </script>
```

- reduce 执行过程

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230914111603814.png" alt="image-20230914111603814" style="zoom:50%;" />

#### *练习：员工涨薪计算成本

- 需求：

①：给员工每人涨薪 30%

②：然后计算需要支出的费用

数据：

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230914112546809.png" alt="image-20230914112546809" style="zoom:50%;" />

- 分析

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230914112715363.png" alt="image-20230914112715363" style="zoom:50%;" />

~~~html
<script>
    //是一个数组对象所以要把salary拿出来 item.salary， 不能省略初始值 设置为0
    const arr = [{
      name: '张三',
      salary: 10000
    }, {
      name: '李四',
      salary: 10000
    }, {
      name: '王五',
      salary: 20000
    },
    ]
    // 涨薪的钱数  10000 * 0.3 
    // const money = arr.reduce(function (prev, item) {
    //   return prev + item.salary * 1.3
    //执行过程： 上一个 + 当前值（每个人涨薪后的值） = （item.salary * 1.3） = 返回值
    // }, 0)
    const money = arr.reduce((prev, item) => prev + item.salary * 0.3, 0)
    console.log(money)
  </script>
~~~



- 数组赋值后，无论修改哪个变量另一个对象的数据值也会相当发生改变

总结：

1. 推荐使用字面量方式声明数组，而不是 `Array` 构造函数

2. 实例方法 `forEach` 用于遍历数组，替代 `for` 循环 (重点)

3. 实例方法 `filter` 过滤数组单元值，生成新数组(重点)

4. 实例方法 `map` 迭代原数组，生成新数组(重点)

5. 实例方法 `join` 数组元素拼接为字符串，返回字符串(重点)

6. 实例方法  `find`  查找元素， 返回符合测试条件的第一个数组元素值，如果没有符合条件的则返回 undefined(重点)

~~~html
 <script>
   const arr = ['red', 'blue', 'green']
    const re = arr.find(function (item) {
      return item === 'blue'
    })
    console.log(re) // blue
</script>
<script>
</script>
~~~

~~~html
* find的使用场景：找小米相关的信息（ 通过小米关键词找出来对象）
<script>
  onst arr = [
      {
        name: '小米',
        price: 1999
      },
      {
        name: '华为',
        price: 3999
      },
    ]
    找小米 这个对象，并且返回这个对象，find返回一个数组
    const mi = arr.find(function (item) {
      // console.log(item)  //
      // console.log(item.name)  //
      console.log(111)
      return item.name === '华为'
    })
    console.log(mi) //结果：小米对象所有信息
  1. find 查找 （简写）
     const mi = arr.find(item => item.name === '小米')
     console.log(mi)
</script>
~~~



7.实例方法`every` 检测数组所有元素是否都符合指定条件，==如果**所有元素**都通过==检测返回 true，只要有一个不是返回 false(重点)

~~~html
<script>
  // 2. every 每一个是否都符合条件，如果都符合返回 true ，否则返回false
  //item 返回值
    const arr1 = [10, 20, 30]
    const flag = arr1.every(item => item >= 20)
    console.log(flag)
</script>
~~~

8.实例方法`some` 检测数组中的元素是否满足指定条件   **如果数组中有**元素满足条件返回 true，否则返回 false

9.实例方法 `concat`  合并两个数组，返回生成新数组

10.实例方法 `sort` 对原数组单元值排序

11.实例方法 `splice` 删除或替换原数组单元

12.实例方法 `reverse` 反转数组

13.实例方法 `findIndex`  查找元素的索引值

#### *练习 : 从对象里面取出来数据然后渲染到页面

const spec = { size: '40cm*40cm' , color: '黑色'}

请将里面的值写到div标签里面，展示内容如下：

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230914144829761.png" alt="image-20230914144829761" style="zoom:50%;" />

* 思路： 获得所有的属性值，然后拼接字符串就可以了

①： 获得所有属性值是： Object.values() 返回的是数组

②： 拼接数组是 join(‘’) 这样就可以转换为字符串了

~~~html
<script>
    const spec = { size: '40cm*40cm', color: '黑色' }
    //1. 所有的属性值回去过来  数组
    console.log(Object.values(spec))
    //2. 转换为字符串   数组join('/') 把数组根据分隔符转换为字符串
    console.log(Object.values(spec).join('/'))
    document.querySelector('div').innerHTML = Object.values(spec).join('/')
  </script>
~~~

### 2.2 **数组常见方法- 伪数组转换为真数组**

- 静态方法 `Array.from()`

  ~~~html
  <body>
    <ul>
      <li>1</li>
      <li>2</li>
      <li>3</li>
    </ul>
    <script>
      //  Array.from(lis) 把伪数组转换为真数组 （伪数组没有删除pop方法）
      const lis = document.querySelectorAll('ul li')
      // console.log(lis)
      // lis.pop() 报错（只有真数组有）
      const liss = Array.from(lis)
      liss.pop()
      console.log(liss)
    </script>
  </body>
  ~~~

### * 包装类型

在 JavaScript 中的字符串、数值、布尔具有对象的使用特征，如具有属性和方法，如下代码举例：

```html
<script>
  // 字符串类型
  const str = 'hello world!'
 	// 统计字符的长度（字符数量）
  console.log(str.length)
  
  // 数值类型
  const price = 12.345
  // 保留两位小数
  price.toFixed(2) // 12.34
</script>
```

之所以具有对象特征的原因是字符串、数值、布尔类型数据是 JavaScript 底层使用 Object 构造函数“包装”来的，被称为包装类型。

## 3 - String

`String` 是内置的构造函数，用于创建字符串。

```html
<script>
  // 使用构造函数创建字符串
  let str = new String('hello world!');

  // 字面量创建字符串
  let str2 = '你好，世界！';

  // 检测是否属于同一个构造函数
  console.log(str.constructor === str2.constructor); // true
  console.log(str instanceof String); // false
</script>
```

总结：

1. 实例属性 `length` 用来获取字符串的度长(重点)
2. 实例方法 `split('分隔符')` 用来将字符串拆分成数组(重点)

~~~html
<script>
//1. split 把字符串 转换为 数组  和 join() 相反
    const str = 'pink,red'
    const arr = str.split(',')
    console.log(arr) //[pink,red]
  
  const str1 = '2022-4-8'
     const arr1 = str1.split('-')
     console.log(arr1) // ['2022','4','8']
  </script>
~~~



3.实例方法 `substring（需要截取的第一个字符的索引[,结束的索引号]）` 用于字符串==截取==(重点)

~~~html
<script>
   //2. 字符串的截取   substring(开始的索引号[， 结束的索引号])
    //2.1 如果省略 结束的索引号，默认取到最后
    //2.2 结束的索引号不包含想要截取的部分
    const str = '今天又要做核酸了'
    console.log(str.substring(5, 7)) //核算 （只有5,6）
</script>
~~~



4.实例方法 `startsWith(检测字符串[, 检测位置索引号])` 检测是否以某字符开头(重点)

~~~html
<script>
  // 3. startsWith 判断是不是以某个字符开头 （是不是它开头）
     const str = 'pink老师上课中'
     console.log(str.startsWith('pink'))
</script>
~~~



5.实例方法 `includes(搜索的字符串[, 检测位置索引号])` 判断一个字符串是否包含在另一个字符串中，根据情况返回 true 或 false(重点) ，区分大小写

~~~html
<script>
  // 4. includes 判断某个字符是不是包含在一个字符串里面
    const str = '我是pink老师'
    console.log(str.includes('pink')) // true
</script>
~~~



6.实例方法 `toUpperCase` 用于将字母转换成大写

7.实例方法 `toLowerCase` 用于将就转换成小写

8.实例方法 `indexOf`  检测是否包含某字符

9.实例方法 `endsWith` 检测是否以某字符结尾

10.实例方法 `replace` 用于替换字符串，支持正则匹配

11. 实例方法 `match` 用于查找字符串，支持正则匹配

注：String 也可以当做普通函数使用，这时它的作用是强制转换成字符串数据类型

#### * 转换为字符串：

~~~html
 <script>
    const num = 10
    console.log(String(num)) //字符串10
    console.log(num.toString()) //字符串10
  </script>
~~~



#### * 练习：显示赠品练习

请将下面字符串渲染到准备好的 p标签内部，结构必须如左下图所示，展示效果如右图所示：

const gift = '50g茶叶,清洗球'

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230914162730765.png" alt="image-20230914162730765" style="zoom:50%;" />

- 思路：

①：把字符串拆分为数组，这样两个赠品就拆分开了 用那个方法？

②：利用map遍历数组，同时把数组元素生成到span里面，并且返回

③：因为返回的是数组，所以需要 转换为字符串, 用那个方法？

④：p的innerHTML 存放刚才的返回值

~~~html
<body>
  <div></div>
  <script>
    const gift = '50g的茶叶,清洗球'
    // 1. 把字符串拆分为数组
     console.log(gift.split(',')) [,]
    // 2. 根据数组元素的个数，生成 对应 span标签
    //返回2个数组，添加到对应的2个span， 用map遍历
     const str = gift.split(',').map(function (item) {
       return `<span>【赠品】 ${item}</span> <br>`
     }).join('')

     // console.log(str)
     document.querySelector('div').innerHTML = str
    //简写
    document.querySelector('div').innerHTML = gift.split(',').map(item => `<span>【赠品】 ${item}</span> <br>`).join('')
  </script>
</body>
~~~

## 4 - Number

`Number` 是内置的构造函数，用于创建数值。

```html
<script>
  // 使用构造函数创建数值
   // toFixed 方法可以让数字指定保留的小数位数
    const num = 10.923
    // console.log(num.toFixed())  // 11
    console.log(num.toFixed(1))
    const num1 = 10
    console.log(num1.toFixed(2))  //10.00

</script>
```

总结：

1. 推荐使用字面量方式声明数值，而不是 `Number` 构造函数
2. 实例方法 `toFixed` 用于设置保留小数位的长度













