> 学习作用域、变量提升、闭包等语言特征，加深对 JavaScript 的理解，掌握变量赋值、函数声明的简洁语法，降低代码的冗余度。

- 理解作用域对程序执行的影响
- 能够分析程序执行的作用域范围
- 理解闭包本质，利用闭包创建隔离作用域
- 了解什么变量提升及函数提升
- 掌握箭头函数、解析剩余参数等简洁语法

# 一，作用域

> 了解作用域对程序执行的影响及作用域链的查找机制，使用闭包函数创建隔离作用域避免全局变量污染。

作用域（scope）规定了变量能够被访问的“范围”，离开了这个“范围”变量便不能被访问，作用域分为全局作用域和局部作用域。

## 1 - 局部作用域

局部作用域分为函数作用域和块作用域。

### * 函数作用域

在函数内部声明的变量只能在函数内部被访问，外部无法直接访问。

```html
<script>
  // 声明 counter 函数
  function counter(x, y) {
    // 函数内部声明的变量
    const s = x + y
    console.log(s) // 18
  }
  // 设用 counter 函数
  counter(10, 8)
  // 访问变量 s
  console.log(s)// 报错
</script>
```

总结：

1. 函数内部声明的变量，在函数外部无法被访问
2. 函数的参数也是函数内部的局部变量
3. 不同函数内部声明的变量无法互相访问
4. 函数执行完毕后，函数内部的变量实际被清空了

### * 块作用域

在 JavaScript 中使用 `{}` 包裹的代码称为代码块，代码块内部声明的变量外部将【有可能】无法被访问。

```html
<script>
  {
    // age 只能在该代码块中被访问
    let age = 18;
    console.log(age); // 正常
  }
  
  // 超出了 age 的作用域
  console.log(age) // 报错
  
  let flag = true;
  if(flag) {
    // str 只能在该代码块中被访问
    let str = 'hello world!'
    console.log(str); // 正常
  }
  
  // 超出了 age 的作用域
  console.log(str); // 报错
  
  for(let t = 1; t <= 6; t++) {
    // t 只能在该代码块中被访问
    console.log(t); // 正常
  }
  
  // 超出了 t 的作用域
  console.log(t); // 报错
</script>
```

JavaScript 中除了变量外还有常量，常量与变量本质的区别是【常量必须要有值且不允许被重新赋值】，常量值为对象时其属性和方法允许重新赋值。

```html
<script>
  // 必须要有值
  const version = '1.0.0';

  // 不能重新赋值
  // version = '1.0.1';

  // 常量值为对象类型
  const user = {
    name: '小明',
    age: 18
  }

  // 不能重新赋值
  user = {};

  // 属性和方法允许被修改
  user.name = '小小明';
  user.gender = '男';
</script>
```

总结：

1. `let` 声明的变量会产生块作用域，`var` 不会产生块作用域
2. `const` 声明的常量也会产生块作用域
3. 不同代码块之间的变量无法互相访问
4. 推荐使用 `let` 或 `const`

注：开发中 `let` 和 `const` 经常不加区分的使用，如果担心某个值会不小被修改时，则只能使用 `const` 声明成常量。

## 2 - 全局作用域

`<script>` 标签和 `.js` 文件的【最外层】就是所谓的全局作用域，在此声明的变量在函数内部也可以被访问。

```html
<script>
  // 此处是全局
  
  function sayHi() {
    // 此处为局部
  }

  // 此处为全局
</script>
```

全局作用域中声明的变量，任何其它作用域都可以被访问，如下代码所示：

```html
<script>
    // 全局变量 name
    const name = '小明'
  
  	// 函数作用域中访问全局
    function sayHi() {
      // 此处为局部
      console.log('你好' + name)
    }

    // 全局变量 flag 和 x
    const flag = true
    let x = 10
  
  	// 块作用域中访问全局
    if(flag) {
      let y = 5
      console.log(x + y) // x 是全局的
    }
</script>
```

总结：

1. 为 `window` 对象动态添加的属性默认也是全局的，不推荐！
2. 函数中未使用任何关键字声明的变量为全局变量，不推荐！！！
3. 尽可能少的声明全局变量，防止全局变量被污染

JavaScript 中的作用域是程序被执行时的底层机制，了解这一机制有助于规范代码书写习惯，避免因作用域导致的语法错误。

## 3 - 作用域链

在解释什么是作用域链前先来看一段代码：

```html
<script>
  // 全局作用域
  let a = 1
  let b = 2
  // 局部作用域
  function f() {
    let c
    // 局部作用域
    function g() {
      let d = 'yo'
    }
  }
</script>
```

函数内部允许创建新的函数，`f` 函数内部创建的新函数 `g`，会产生新的函数作用域，由此可知作用域产生了嵌套的关系。

如下图所示，父子关系的作用域关联在一起形成了链状的结构，作用域链的名字也由此而来。

作用域链本质上是底层的变量查找机制，在函数被执行时，会优先查找当前函数作用域中查找变量，如果当前作用域查找不到则会依次逐级查找父级作用域直到全局作用域，如下代码所示：

```html
<script>
  // 全局作用域
  let a = 1
  let b = 2

  // 局部作用域
  function f() {
    let c
    // let a = 10;
    console.log(a) // 1 或 10
    console.log(d) // 报错
    
    // 局部作用域
    function g() {
      let d = 'yo'
      // let b = 20;
      console.log(b) // 2 或 20
    }
    
    // 调用 g 函数
    g()
  }

  console.log(c) // 报错
  console.log(d) // 报错
  
  f();
</script>
```

总结：

1. 嵌套关系的作用域串联起来形成了作用域链
2. 相同作用域链中按着从小到大的规则查找变量
3. 子作用域能够访问父作用域，父级作用域无法访问子级作用域

## 4 - JS垃圾回收机制     

## 5 - 闭包

- 闭包是一种比较特殊和函数，使用闭包能够访问函数作用域中的变量。从代码形式上看闭包是一个做为返回值的函数，如下代码所示：


:octopus:  总结

1.怎么理解闭包？

- 闭包 = 内层函数 + 外层函数的变量

~~~javascript
// 常见的闭包的形式   外部可以访问使用 函数内部的变量
//里面的函数用外面函数的变量，return里面函数
    function outer() {
       let a = 100
       function fn() {
         console.log(a) //使用外层韩式变量a
       }
       return fn
     }
// outer()   ===  fn   ===  function fn() {}
//外面函数使用局部函数
    const fun = function fn() { }

~~~

~~~javascript
 // 常见的写法2
     function outer() {
       let a = 100
       return function () {
         console.log(a) //结果100
       }
     }
//fun 可以使用 outer() 的 a 变量
     const fun = outer()
     fun() // 调用函数 结果 100
~~~



2.闭包的作用？

- 封闭数据，实现数据私有，==外部也可以访问函数内部的变量==
- 闭包很有用，因为它允许将函数与其所操作的某些数据（环境）关联起来

~~~javascript
// 外面要使用这个 100

    // 闭包的应用 
    // 普通形式 统计函数调用的次数
     let i = 0
     function fn() {
       i++
       console.log(`函数被调用了${i}次`)
     }
    //  因为 i 是全局变量，容易被修改
    // 闭包形式 统计函数调用的次数
    // 把i改成1000 也不会影响到调用次数结果 （1000 + 1 = 1001 错）
	function count() {
      let i = 0
      function fn() {
        i++
        console.log(`函数被调用了${i}次`)
      }
      return fn
    }
    const fun = count()
~~~



3.闭包可能引起的问题？

- 内存泄漏 （改收回不收回）

~~~javascript
//i不会被回收， 应该被回收 fun 找到 fn 找到 i （泄漏）
function count() {
      let i = 0
      function fn() {
        i++
        console.log(`函数被调用了${i}次`)
      }
  //一直调用i不会被回收
      return fn
    }
    const fun = count() 
~~~



![image-20231112065854304](/Users/guoguo/Library/Application Support/typora-user-images/image-20231112065854304.png)

## 6 - 变量提升

变量提升是 JavaScript 中比较“奇怪”的现象，它允许在变量声明之前即被访问，

```html
<script>
  // 访问变量 str
  console.log(str + 'world!');

  // 声明变量 str
  var str = 'hello ';
</script>

```

~~~html
	<script>
    function fn() {
      //提升里面函数
      console.log(num)
      var num = 10
    }
    fn() //undefined
  </script>
~~~



总结：

1. 变量在未声明即被访问时会报语法错误
2. 变量在声明之前即被访问，变量的值为 `undefined`
3. `let` 声明的变量不存在变量提升，推荐使用 `let`
4. 变量提升出现在相同作用域当中
5. 实际开发中推荐先声明再访问变量

注：关于变量提升的原理分析会涉及较为复杂的词法分析等知识，而开发中使用 `let` 可以轻松规避变量的提升，因此在此不做过多的探讨，有兴趣可[查阅资料](https://segmentfault.com/a/1190000013915935)。

# 二，函数进阶

> 知道函数参数默认值、动态参数、剩余参数的使用细节，提升函数应用的灵活度，知道箭头函数的语法及与普通函数的差异。

## 1 - 函数提升

函数提升与变量提升比较类似，是指函数在声明之前即可被调用。

```html
<script>
  // 调用函数
  foo()
  // 声明函数
  function foo() {
    console.log('声明之前即被调用...')
  }

  // 不存在提升现象 -函数表达式
  bar()  // 错误
  var bar = function () {
    console.log('函数表达式不存在提升现象...')
  }
</script>
```

总结：

1. 函数提升能够使函数的声明调用更灵活 (调用在哪里都可以)
2. 函数表达式不存在提升的现象 （给函数声明赋值）
3. 函数提升出现在相同作用域当中

## 2 - 函数参数

函数参数的使用细节，能够提升函数应用的灵活度。

### 2.1 默认值

```html
<script>
  // 设置参数默认值
  function sayHi(name="小明", age=18) {
    document.write(`<p>大家好，我叫${name}，我今年${age}岁了。</p>`);
  }
  // 调用函数
  sayHi();
  sayHi('小红');
  sayHi('小刚', 21);
</script>
```

总结：

1. 声明函数时为形参赋值即为参数的默认值
2. 如果参数未自定义默认值时，参数的默认值为 `undefined`
3. 调用函数时没有传入对应实参时，参数的默认值被当做实参传入

### 2.2 动态参数

`arguments` 是函数内部内置的伪数组变量，它包含了调用函数时传入的所有实参。

```html
***需求吧用户输入的不管多少都要算出来总和
<script>
  // 求生函数，计算所有参数的和
  function sum() {
    // arguments 动态参数 只存在于 函数里面
    // 是伪数组 里面存储的是传递过来的实参
    // console.log(arguments) 结果： 伪数组
    let s = 0
    for(let i = 0; i < arguments.length; i++) {
      s += arguments[i]
    }
    console.log(s)
  }
  // 调用求和函数
  sum(5, 10)// 两个参数
  sum(1, 2, 4) // 两个参数
</script>
```

总结

1. `arguments` 是一个伪数组
2. `arguments` 的作用是动态获取函数的实参
3. 可以通过for循环依次得到传递过来的实参

#### 2.3 - 剩余参数 (提倡使用)

```html
<script>
    function getSum(a, b, ...arr) { //没有a，b会取全部
      console.log(arr)  // 使用的时候不需要写 ...
    }
    getSum(2, 3)
    getSum(1, 2, 3, 4, 5) //结果： [3,4,5] 存剩下的数字
  </script>
```

总结：

1. `...` 是语法符号，置于最末函数形参之前，用于获取多余的实参
2. 使用场景：用于获取多余的参数
3. 借助 `...` 获取的剩余实参，是个==真数组==

### 2.4 - 展开运算符

- 展开运算符 (...)， 将一个数组进行展开， 不会修改元素组
  - 就和数组最大，最小值 （以前只有对象 Math.max）
  - 合并数组
- 跟剩余参数的区别
  - 剩余参数：函数参数使用，得到真数组 
  - 展开运算符：数组中使用，数组展开

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230913115949208.png" alt="image-20230913115949208" style="zoom:50%;" />

~~~html
 <script>
    const arr1 = [1, 2, 3]
    // 展开运算符 可以展开数组
    // console.log(...arr)

    // console.log(Math.max(1, 2, 3)) 
    // ...arr1  === 1,2,3
    // 1 求数组最大值
    console.log(Math.max(...arr1)) // 3
    console.log(Math.min(...arr1)) // 1
    // 2. 合并数组
    const arr2 = [3, 4, 5]
    const arr = [...arr1, ...arr2]
    console.log(arr)
  </script>
~~~



## 3 - 箭头函数 （重要）

### 3.1 基本语法

- 箭头函数是一种声明函数的简洁语法，它与普通函数并无本质的区别，差异性更多体现在语法格式上
- **语法1：基本写法**
  - 去掉function

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230913132727038.png" alt="image-20230913132727038" style="zoom:50%;" />

- **语法2：只有一个参数可以省略小括号**
  - 去掉 function + ()

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230913132822526.png" alt="image-20230913132822526" style="zoom:50%;" />

- **语法3：如果函数体只有一行代码，可以写到一行上，并且无需写 return 直接返回值**

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230913132940950.png" alt="image-20230913132940950" style="zoom:50%;" />

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230913133026832.png" alt="image-20230913133026832" style="zoom:50%;" />

- **语法4：加括号的函数体返回对象字面量表达式**

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230913133215880.png" alt="image-20230913133215880" style="zoom:50%;" />

- 总结：

1. 箭头函数属于表达式函数，因此不存在函数提升
2. 箭头函数只有一个参数时可以省略圆括号 `()`
3. 箭头函数函数体只有一行代码时可以省略花括号 `{}`，并自动做为返回值被返回

~~~html
 <script>
     const fn = function () {
       console.log(123)
     }
    // 1. 箭头函数 基本语法
     const fn = () => {
       console.log(123)
     }
     fn()
     const fn = (x) => {
       console.log(x)
     }
     fn(1)

    // 2. 只有一个形参的时候，可以省略小括号()
     const fn = x => {
       console.log(x)
     }
     fn(1)

    //  3. 只有一行代码的时候，我们可以省略大括号{}
     const fn = x => console.log(x)
     fn(1)

    // 4. 只有一行代码的时候，可以省略return
     const fn = x => x + x
     console.log(fn(1))

    // 5. 箭头函数可以直接返回一个对象
     const fn = (uname) => ({ name: uname }) name： 属性名
     console.log(fn('刘德华')) //返回对象

  
  </script>
~~~



#### 3.2 - 箭头函数参数

- 箭头函数中没有 `arguments`，只能使用 `...` 动态获取实参


~~~html
 <script>
    // 1. 利用箭头函数来求和
    const getSum = (...arr) => {
      let sum = 0
      for (let i = 0; i < arr.length; i++) {
        sum += arr[i]
      }
      return sum
    }
    const result = getSum(2, 3, 4) // = sum
    console.log(result) // 9
  </script>
~~~

#### 3.3 - 箭头函数 this

- 箭头函数不会创建自己的this,它只会从自己的作用域链的==上一层沿用this==。


~~~html
 <script>
    // 以前this的指向：  谁调用的这个函数，this 就指向谁
     console.log(this)  // window
    
    // 普通函数
     function fn() {
       console.log(this)  // window
     }
     window.fn()
    
     // 对象方法里面的this
     const obj = {
       name: 'andy',
       sayHi: function () {
         console.log(this)  // obj
       }
     }
     obj.sayHi()

    // 2. 箭头函数的this  是上一层作用域的this 指向
     const fn = () => {
       console.log(this)  // window
     }
     fn()

    // 对象方法箭头函数 this
     const obj = {
       uname: 'pink老师',
       sayHi: () => {
        console.log(this)  // this 指向谁？ window
      }
     }
     obj.sayHi()
   
	// 两个函数，对象方法箭头函数 this
    const obj = {
      uname: 'pink老师',
      sayHi: function () {
        console.log(this)  // obj 普通函数
        let i = 10
        const count = () => {
          console.log(this)  // obj  上一层
        }
        count()
      }
    }
    obj.sayHi()

  </script>
~~~

> 1. 箭头函数里面有this吗？ 
>
> -  箭头函数不会创建自己的this,它只会从自己的作用域链的上一层沿用this 
>
> 2. DOM事件回调函数推荐使用箭头函数吗？ 
>
> -  不太推荐，特别是需要用到this的时候 
>
> -  事件回调函数使用箭头函数时，this 为全局的 window

# 三，解构赋值 (重点)

> 知道解构的语法及分类，使用解构简洁语法快速为变量赋值。

解构赋值是一种快速为变量赋值的简洁语法，本质上仍然是为变量赋值，分为数组解构、对象解构两大类型。

### 3.1 - 数组解构

- 数组解构是将数组的单元值快速批量赋值给一系列变量的简洁语法，如下代码所示：


```html
<script>
  // 普通的数组
  let arr = [1, 2, 3]
  // 批量声明变量 a b c 
  // 同时将数组单元值 1 2 3 依次赋值给变量 a b c
  let [a, b, c] = arr
  console.log(a); // 1
  console.log(b); // 2
  console.log(c); // 3
  
  // 交换2个变量的值
    let a = 1
    let b = 2;  //必须加分号
    [b, a] = [a, b]
    console.log(a, b)
</script>
```

- 注意： js 前面必须加分号情况

  - 立即执行函数

  <img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230913144948520.png" alt="image-20230913144948520" style="zoom:50%;" />

  - 数组解构

~~~html
	<script>
//为什么需要分号，没有分号就算两个连在一起，后面不是函数了
    //分号表示结束
    //前面有代码 后面以数组开头 必须用分号隔开
    let a = 1
    let b = 2;      [b, a] = [a, b]
    console.log(a, b)
	</script>
~~~

1. 赋值运算符 `=` 左侧的 `[]` 用于批量声明变量，右侧数组的单元值将被赋值给左侧的变量
2. 变量的顺序对应数组单元值的位置依次进行赋值操作
3. 变量的数量大于单元值数量时，多余的变量将被赋值为  `undefined`
4. 变量的数量小于单元值数量时，可以通过 `...` 获取剩余单元值，但只能置于最末位
5. 允许初始化变量的默认值，且只有单元值为 `undefined` 时默认值才会生效

注：支持多维解构赋值，比较复杂后续有应用需求时再进一步分析

> 1. 数组解构赋值的作用是什么？ 
>
> -  是将数组的单元值快速批量赋值给一系列变量的简洁语法 
>
> 2. Js 前面有两哪种情况需要加分号的？ 
>
> - 立即执行函数 
>
> - 数组解构

#### *练习 1： 

需求①： 有个数组： const pc = ['海尔', '联想', '小米', '方正'] 

解构为变量: hr lx mi fz

~~~html
<script>
   const pc = ['海尔', '联想', '小米', '方正']  
    const [hr, lx, mi, fz] = ['海尔', '联想', '小米', '方正']
    // console.log(hr)
</script>
~~~

需求②：请将最大值和最小值函数返回值解构 max 和min 两个变量

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230913145506151.png" alt="image-20230913145506151" style="zoom:50%;" />

~~~html
<script>
   function getValue() {
      return [100, 60] //只返回给调用者
    }
    const [max, min] = getValue()
    console.log(max)
</script> 
~~~

#### * 数组结构细节

1. 变量多， 单元值少 ， undefined

~~~html
	<script>
    const [a, b, c, d] = [1, 2, 3]
    console.log(a) // 1
    console.log(b) // 2
    console.log(c) // 3
    console.log(d) // undefined
  </script> 

~~~

2. 变量少， 单元值多

~~~html
	<script>
    const [a, b] = [1, 2, 3]
    console.log(a) // 1
    console.log(b) // 2
  </script> 

~~~

3. 剩余参数 变量少， 单元值多，剩下的都给c

~~~html
 	<script>
    const [a, b, ...c] = [1, 2, 3, 4]
    console.log(a) // 1
    console.log(b) // 2
    console.log(c) // [3, 4]  真数组
  </script> 

~~~

4. ==防止 undefined 传递  给 a，b默认值==

~~~html
	<script>
    const [a = 0, b = 0] = [1, 2]
    const [a = 0, b = 0] = []
    console.log(a) // 1
    console.log(b) // 2
  </script> 

<script>
</script> 
~~~

5. 按需导入赋值 (中间有空格)

~~~html
<script>
  const [a, b, , d] = [1, 2, 3, 4]
    console.log(a) // 1
    console.log(b) // 2
    console.log(d) // 4
</script>
~~~

6.  ==支持多维数组的结构==

~~~html
<script>
  const [a, b, [c, d]] = [1, 2, [3, 4]]
    console.log(a) // 1
    console.log(b) // 2
    console.log(c) // 3
    console.log(d) // 4
</script> 
~~~



### 3.2 - 对象解构

- 对象解构是将对象属性和方法快速批量赋值给一系列变量的简洁语法，如下代码所示：


```html
<script>
    //对象解构
    const obj = {
      uname: 'pink老师',  // uname ： 属性 或者 变量
      age: 18
    }
    obj.uname
    obj.age 


    const uname = 'red老师' //外面不能有一样的变量名
    //解构的语法
    const { uname, age } = {age: 18, uname: 'pink老师' } 
    // 等价于 const uname =  obj.uname
    // 要求属性名和变量名必须一直才可以
    console.log(uname)
    console.log(age)

    //1. 对象解构的变量名 可以重新改名  旧变量名: 新变量名
    const { uname: username, age } = { uname: 'pink老师', age: 18 }
    console.log(username)
    console.log(age)

    //2. 解构数组对象
    const pig = [
      {
        uname: '佩奇',
        age: 6
      }
    ]
    const [{ uname, age }] = pig
    console.log(uname)
    console.log(age)
  </script>
```

> 1. 要求属性名和变量名必须一直才可以
>
> ` const { uname, age } = {age: 18, uname: 'pink老师' }` 
>
> 2. 修改变量名 `旧变量名: 新变量名`
>
>    ` const { uname: username, age } = { uname: 'pink老师', age: 18 }`

==总结==

1. 赋值运算符 `=` 左侧的 `{}` 用于批量声明变量，右侧对象的属性值将被赋值给左侧的变量
2. 对象属性的值将被赋值给与属性名相同的变量
3. 对象中找不到与变量名一致的属性时变量值为 `undefined`
4. 允许初始化变量的默认值，属性不存在或单元值为 `undefined` 时默认值才会生效

#### ***案例：支持多维解构赋值

~~~html
<body>
  <script>
    // 1. 这是后台传递过来的数据
    const msg = {
      "code": 200,
      "msg": "获取新闻列表成功",
      "data": [
        {
          "id": 1,
          "title": "5G商用自己，三大运用商收入下降",
          "count": 58
        },
        {
          "id": 2,
          "title": "国际媒体头条速览",
          "count": 56
        },
        {
          "id": 3,
          "title": "乌克兰和俄罗斯持续冲突",
          "count": 1669
        },

      ]
    }

    // 需求1： 请将以上msg对象  采用对象解构的方式 只选出  data 方面后面使用渲染页面
     //意思是 data = mgs 的 data, 重名才可以
     const { data } = msg
     console.log(data)
    
    // 需求2： 上面msg是后台传递过来的数据，我们需要把data选出当做参数传递给 函数
     //const { data } = msg
    // msg 虽然很多属性，但是我们利用解构只要 data值
    function render({ data }) {
       //const { data } = arr
      // 我们只要 data 数据
      // 内部处理
      console.log(data)

    }
    render(msg) //实参用msg,有几个对象 形参只要data （少与实参）
    ***如果没有用data形参代码如下：
     function render(msg) {
      const { data } = msg
      console.log({ data })
    }
    render(msg)

    // 需求3， 为了防止msg里面的data名字混淆，要求渲染函数里面的数据名改为 myData
    function render({ data: myData }) {
      // 要求将 获取过来的 data数据 更名为 myData
      // 内部处理
      console.log(myData)

    }
    render(msg)

  </script>
~~~

#### * 多级对象结构

- 用冒号隔开

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230913163318492.png" alt="image-20230913163318492" style="zoom:50%;" />

~~~html
<script>
    const pig = {
      name: '佩奇',
      family: {
        mother: '猪妈妈',
        father: '猪爸爸',
        sister: '乔治'
      },
      age: 6
    }
    // 多级对象解构 - 用冒号隔开
    const { name, family: { mother, father, sister } } = pig
    console.log(name)
    console.log(mother)
    console.log(father)
    console.log(sister)

    //数组对象里面 - 按顺序写 数组 - 对象 - 对象
    const person = [
      {
        name: '佩奇',
        family: {
          mother: '猪妈妈',
          father: '猪爸爸',
          sister: '乔治'
        },
        age: 6
      }
    ]
    const [{ name, family: { mother, father, sister } }] = person
    console.log(name)
    console.log(mother)
    console.log(father)
    console.log(sister)
  </script>
~~~

# 四，综合案例

## * forEach遍历数组

forEach() 方法用于调用数组的每个元素，并将元素传递给回调函数

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230913164029566.png" alt="image-20230913164029566" style="zoom:50%;" />

>注意：  
>
>1.forEach 主要是遍历数组
>
>2.参数==当前数组====元素==是必须要写的， 索引号可选。

~~~html
<body>
  <script>
    // forEach 就是遍历  加强版的for循环  适合于遍历数组对象
    const arr = ['red', 'green', 'pink']
    const result = arr.forEach(function (item, index) {
      console.log(item)  // 数组元素 red  green pink
      console.log(index) // 索引号
    })
    // console.log(result) //不返回值 
  </script>
</body>
~~~

## * filter筛选数组

filter() 方法创建一个新的数组，新数组中的元素是通过检查指定数组中符合条件的所有元素

主要使用场景： 筛选数组符合条件的元素，并返回筛选之后元素的新数组

~~~html
<body>
  <script>
    const arr = [10, 20, 30]
    // const newArr = arr.filter(function (item, index) {
    //   // console.log(item)
    //   // console.log(index)
    //   return item >= 20
    // })
    // 返回的符合条件的新数组

    const newArr = arr.filter(item => item >= 20)
    console.log(newArr) //[20,30]
  </script>
</body>
~~~





