# JavaScript 进阶 - 第4天

# 一， 深浅拷贝

## 1 - 浅拷贝

- 首先浅拷贝和深拷贝只针对引用类型


- 浅拷贝：拷贝的是地址


- 常见方法：


1. 拷贝对象：Object.assgin() / 展开运算符 {...obj} 拷贝对象
2. 拷贝数组：Array.prototype.concat() 或者 [...arr]

~~~html
<script>
    const obj = {
      uname: 'pink',  //属性存的是值
      age: 18,
      family: {
        baby: '小pink' //对象存的是地址 -- 同地址指向同对象
      }
    }
    // 浅拷贝 - 展开运算符
    const o = { ...obj }
    console.log(o)
    o.age = 20
    console.log(o)
    console.log(obj)

    //浅拷贝 - Object
    const o = {}
    Object.assign(o, obj)
    o.age = 20
    o.family.baby = '老pink'
    console.log(o)
    console.log(obj)
  </script>
</body>
~~~



>如果是简单数据类型拷贝值，引用数据类型拷贝的是地址 (简单理解： 如果是单层对象，没问题，如果有多层就有问题)

总结：

1. 直接复制跟浅拷贝有什么区别？

-  直接赋值的方法，只要是对象，都会相互影响，因为是直接拷贝对

象栈里面的地址 (多层对象会影响)

- 浅拷贝如果是一层对象，不相互影响，如果出现多层对象拷贝还会

相互影响

~~~html
<script>
    const obj = {
      uname: 'pink',
      age: 18
    }
    const o = obj
    console.log(o)
    o.age = 20
    console.log(o) // age = 20
    console.log(obj) // age = 20 
  </script>
~~~

2. 浅拷贝怎么理解？

- 拷贝对象之后，里面的属性值是简单数据类型直接拷贝值
- 如果属性值是引用数据类型则拷贝的是地址

### 2 - 深拷贝

- 首先浅拷贝和深拷贝只针对引用类型


- 深拷贝：==拷贝的是对象，不是地址==


常见方法：

1. 通过递归实现深拷贝
2. lodash/cloneDeep
3. 通过JSON.stringify()实现

### 2.1 递归实现深拷贝

#### *函数递归

如果一个函数在内部可以调用其本身，那么这个函数就是递归函数

- 简单理解:函数内部自己调用自己, 这个函数就是递归函数
- 递归函数的作用和循环效果类似
- 由于递归很容易发生“栈溢出”错误（stack overflow），所以必须要加退出条件 return

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230916095024528.png" alt="image-20230916095024528" style="zoom:50%;" />

#### * 练习：利用递归函数实现 setTimeout 模拟 setInterval效果

需求：

①：页面每隔一秒输出当前的时间

②：输出当前时间可以使用：new Date().toLocaleString() 

~~~html
<body>
  <div></div>
  <script>
    //调用两次 外面的 getTime 跟 setTimeout的 getTime
    function getTime() {
      document.querySelector('div').innerHTML = new Date().toLocaleString()
      setTimeout(getTime, 1000)
    }
    getTime()
  </script>
</body>
~~~

#### * **通过递归函数实现深拷贝（简版）**

- 怎么实现深拷贝
  - 要用到函数递归
  - 如果遇到数组或者对象再次调用这个递归函数就可以
  - 数组先写， 对象后面写

~~~html
<body>
  <script>
    const obj = {
      uname: 'pink',
      age: 18,
      hobby: ['乒乓球', '足球'],
      family: {
        baby: '小pink'
      }
    }
    const o = {}
    // 拷贝函数
    function deepCopy(newObj, oldObj) {
      debugger
      for (let k in oldObj) {
        // 处理数组的问题  一定先写数组 在写 对象 不能颠倒
        if (oldObj[k] instanceof Array) {  //属不属于数组
          newObj[k] = []
          //  newObj[k] 接收 []  hobby
          //  oldObj[k]   ['乒乓球', '足球']
          deepCopy(newObj[k], oldObj[k])
        } else if (oldObj[k] instanceof Object) {
          newObj[k] = {}
          deepCopy(newObj[k], oldObj[k])
        }
        else {
          //  k  属性名 uname age    oldObj[k]  属性值  18
          // newObj[k]  === o.uname  给新对象添加属性
          newObj[k] = oldObj[k] // newObj[k]: 属性名， oldObj[k]： 属性值（给赋值）
        }
      }
    }
    deepCopy(o, obj) // 函数调用  两个参数 o 新对象  obj 旧对象
    console.log(o)
    o.age = 20
    o.hobby[0] = '篮球'
    o.family.baby = '老pink'
    console.log(obj)
    console.log([1, 23] instanceof Object)
~~~

### * js库lodash里面cloneDeep内部实现了深拷贝

- 引用代码

~~~html
<body>
  <!-- 先引用 -->
  <script src="./lodash.min.js"></script>
  <script>
    const obj = {
      uname: 'pink',
      age: 18,
      hobby: ['乒乓球', '足球'],
      family: {
        baby: '小pink'
      }
    }
    const o = _.cloneDeep(obj)
    console.log(o)  //新的变
    o.family.baby = '老pink'
    console.log(obj) //旧的不变
  </script>
</body>
~~~

### * JSON序列化 （JSON.stringify，JSON.parse）

~~~html
<body>
  <script>
    const obj = {
      uname: 'pink',
      age: 18,
      hobby: ['乒乓球', '足球'],
      family: {
        baby: '小pink'
      }
    }
    // 把对象转换为 JSON 字符串
    // console.log(JSON.stringify(obj))
    //然后JSON.parse 转换成 对象
    const o = JSON.parse(JSON.stringify(obj))
    console.log(o)
    o.family.baby = '123'
    console.log(obj)
  </script>
</body>
~~~

> 总结
>
> - 实现深拷贝三种方式？
>
>   -  自己利用递归函数书写深拷贝
>
>   - 利用js库 lodash里面的 _.cloneDeep() 
>
>   - 利用JSON字符串转换

# 二，异常处理

> 了解 JavaScript 中程序异常处理的方法，提升代码运行的健壮性。

## 2.1 - throw

- 异常处理是指预估代码执行过程中可能发生的错误，然后最大程度的避免错误的发生导致整个程序无法继续运行


总结：

1. throw 抛出异常信息，程序也会终止执行
2. throw 后面跟的是错误提示信息
3. ==Error 对象==配合 throw 使用，能够设置更详细的错误信息

```html
<script>
  function counter(x, y) {

    if(!x || !y) { //没有参数执行下面代码
      // throw '参数不能为空!';
      throw new Error('参数不能为空!') 
    }

    return x + y
  }

  counter()
</script>
```

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230916114551114.png" alt="image-20230916114551114" style="zoom:50%;" />

## 2.2 - try ... catch

- 我们可以通过try / catch 捕获错误信息（浏览器提供的错误信息） try 试试 catch 拦住 finally 最后

```html
<body>
  <p>123</p>
  <script>
    function fn() {
      try {
        // 可能发送错误的代码 要写到 try
        const p = document.querySelector('.p')
        p.style.color = 'red'
      } catch (err) {
        // 拦截错误，提示浏览器提供的错误信息，但是不中断程序的执行
        console.log(err.message)
        throw new Error('你看看，选择器错误了吧') //自动中断
        // 不自动中断 需要加return 中断程序
        // return
      }
      finally {
        // 不管你程序对不对，一定会执行的代码 （用于必须要执行的代码）
        alert('弹出对话框')
      }
      console.log(11)
    }
    fn()
  </script>
```

总结：

1. `try...catch` 用于捕获错误信息
2. 将预估可能发生错误的代码写在 `try` 代码段中
3. 如果 `try` 代码段中出现错误后，会执行 `catch` 代码段，并截获到错误信息


## 2.3 - debugger

- 我们可以通过try / catch 捕获错误信息（浏览器提供的错误信息）（打断点一样， 例如： 有一点代买在修复中就用debugger标志）

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230916120445240.png" alt="image-20230916120445240" style="zoom:50%;" />

# 三，处理this

> 了解函数中 this 在不同场景下的默认值，知道动态指定函数 this 值的方法。

`this` 是 JavaScript 最具“魅惑”的知识点，不同的应用场合 `this` 的取值可能会有意想不到的结果，在此我们对以往学习过的关于【 `this` 默认的取值】情况进行归纳和总结。

## 3.1 this指向

### * 普通函数

**普通函数**的调用方式决定了 `this` 的值，即【谁调用 `this` 的值指向谁】，如下代码所示：

```html
<body>
  <button>点击</button>
  <script>
    // 普通函数：  谁调用我，this就指向谁
    console.log(this)  // window
    function fn() {
      console.log(this)  // window    
    }
    window.fn()
    window.setTimeout(function () {
      console.log(this) // window 
    }, 1000)
    
    document.querySelector('button').addEventListener('click', function () {
      console.log(this)  // 指向 button
    })
    const obj = {
      sayHi: function () {
        console.log(this)  // 指向 obj
      }
    }
    obj.sayHi()
  </script>
</body>
```

注： 普通函数没有明确调用者时 `this` 值为 `window`，严格模式下没有调用者时 `this` 的值为 `undefined`。

### * 箭头函数

- **箭头函数**中的 `this` 与普通函数完全不同，也不受调用方式的影响，事实上箭头函数中并不存在 `this` ！
  - 箭头函数会默认帮我们绑定外层 this 的值，所以在箭头函数中 this 的值和外层的 this 是一样的
  - 箭头函数中的this引用的就是最近作用域中的this
  - 向外层作用域中，一层一层查找this，直到有this的定义

注意： 对象没有this，只有函数有this

```html
<script> 
  console.log(this) // 此处为 window
  // 箭头函数
  const sayHi = function() {
    console.log(this) // 该箭头函数中的 this 为函数声明环境中 this 一致
  }
  // 普通对象
  const user = {
    name: '小明',
    // 该箭头函数中的 this 为函数声明环境中 this 一致
    walk: () => {
      console.log(this)
    },
    
    sleep: function () {
      let str = 'hello'
      console.log(this)
      let fn = () => {
        console.log(str)
        console.log(this) // 该箭头函数中的 this 与 sleep 中的 this 一致
      }
      // 调用箭头函数
      fn();
    }
  }

  // 动态添加方法
  user.sayHi = sayHi
  
  // 函数调用
  user.sayHi()
  user.sleep()
  user.walk()
</script>
```

- 注意情况1 ：

在开发中【使用箭头函数前需要考虑函数中 `this` 的值】，**事件回调函数**使用箭头函数时，`this` 为全局的 `window`，因此DOM事件回调函数不推荐使用箭头函数，如下代码所示：

```html
<script>
  // DOM 节点
  const btn = document.querySelector('.btn')
  // 箭头函数 此时 this 指向了 window
  btn.addEventListener('click', () => {
    console.log(this)
  })
  // 普通函数 此时 this 指向了 DOM 对象 btn
  btn.addEventListener('click', function () {
    console.log(this)
  })
</script>
```

- 注意情况2 （原形的面向对象不建议使用箭头函数）

同样由于箭头函数 `this` 的原因，**基于原型的面向对象也不推荐采用箭头函数**，如下代码所示：

```html
<script>
  function Person() {
  }
  // 原型对像上添加了箭头函数
  Person.prototype.walk = () => {
    console.log('人都要走路...')
    console.log(this); // window
  }
  const p1 = new Person()
  p1.walk()
</script>
```

> 总结
>
> 1.函数内不存在this，沿用上一级的
>
> 2.不适用
>
> -  构造函数，原型函数，dom事件函数等等
>
> 3. 适用
>
> -  需要使用上层this的地方
>
> 4. 使用正确的话，它会在很多地方带来方便，后面我们会大量使用慢慢体会

## 3.2 - 改变this指向

以上归纳了普通函数和箭头函数中关于 `this` 默认值的情形，不仅如此 JavaScript 中还允许指定函数中 `this` 的指向，有 3 个方法可以动态指定普通函数中 `this` 的指向：

### * call （了解）

- 使用 `call` 方法调用函数，同时指定函数中 `this` 的值，使用方法如下代码所示
- 语法
  - thisArg：在fun函数运行时指定的 this 值
  - argsArray：传递的值，必须包含在数组里面
  - 返回值就是函数的返回值，因为它就是调用函数
  - 因此 apply 主要跟数组有关系，比如使用 Math.max() 求数组的最大值

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230916132930998.png" alt="image-20230916132930998" style="zoom:50%;" />



```html
<script>
    const obj = {
      uname: 'pink'
    }
    function fn(x, y) {
      console.log(this) // window
      console.log(x + y) // 3
    }
    // 1. 调用函数  
    // 2. 改变 this 指向
    fn.call(obj, 1, 2)
  </script>
```

总结：

1. `call` 方法能够在调用函数的同时指定 `this` 的值
2. 使用 `call` 方法调用函数时，第1个参数为 `this` 指定的值
3. `call` 方法的其余参数会依次自动传入函数做为函数的参数

### * apply (重要)

- 使用 `call` 方法**调用函数**，同时指定函数中 `this` 的值，使用方法如下代码所示
- 语法
  - thisArg：在fun函数运行时指定的 this 值
  - argsArray：传递的值，必须包含在数组里面
  - 返回值就是函数的返回值，因为它就是调用函数
  - 因此 apply 主要跟数组有关系，比如使用 Math.max() 求数组的最大值

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230916133436547.png" alt="image-20230916133436547" style="zoom:50%;" />

```html
<body>
  <script>
    const obj = {
      age: 18
    }
    function fn(x, y) {
      console.log(this) // {age: 18}
      console.log(x + y)
    }
    // 1. 调用函数
    // 2. 改变this指向 
    //  fn.apply(this指向谁, 数组参数)
    fn.apply(obj, [1, 2])
    // 3. 返回值   本身就是在调用函数，所以返回值就是函数的返回值

    // 使用场景： 求数组最大值
    // const max = Math.max(1, 2, 3)
    // console.log(max)
    const arr = [100, 44, 77]
    const max = Math.max.apply(Math, arr) //max()是方法（函数）
    const min = Math.min.apply(null, arr) //null, math 不指向谁
    console.log(max, min)
    // 使用场景： 求数组最大值
    console.log(Math.max(...arr))
  </script>
</body>
```

总结：

1. `apply` 方法能够在调用函数的同时指定 `this` 的值

2. 使用 `apply` 方法调用函数时，第1个参数为 `this` 指定的值

3. `apply` 方法第2个参数为数组，数组的单元值依次自动传入函数做为函数的参数

4. call和apply的区别是？

   -  都是调用函数，都能改变this指向

   - 参数不一样，apply传递的必须是数组

### * bind

- `bind` 方法并**不会调用函数**，而是创建一个指定了 `this` 值的新函数，使用方法如下代码所示

- 语法

  - thisArg：在 fun 函数运行时指定的 this 值 

  - arg1，arg2：传递的其他参数

  - 返回由指定的 this 值和初始化参数改造的 原函数拷贝 （新函数）

  - 因此当我们只是想改变 this 指向，并且不想调用这个函数的时候，可以使用 bind，比如改变定时器内部的

    this指向

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230916140420913.png" alt="image-20230916140420913" style="zoom:50%;" />

```html
<script>
    const obj = {
      age: 18
    }
    function fn() {
      console.log(this)
    }

    // 1. bind 不会调用函数 
    // 2. 能改变this指向
    // 3. 返回值是个函数，  但是这个函数里面的this是更改过的obj
    const fun = fn.bind(obj)
    // console.log(fun) 
    fun()  //返回值是函数 --- fun也是函数
</script>
```

- 需求: 有一个按钮，点击里面就禁用，2秒钟之后开启

~~~html
<body>
  <button>发送短信</button>
  <script>    document.querySelector('button').addEventListener('click', function () {
      // 禁用按钮 disabled
      // btn.disabled = true (或者改成箭头函数)
      this.disabled = true
      window.setTimeout(function () {
        // 在这个普通函数里面，我们要this由原来的window 改为 btn
        this.disabled = false
      }.bind(this), 2000)   // 这里的this 和 btn 一样， 因为bind写在函数外面所以 this = btn
    })
  </script>
</body>
~~~

注：`bind` 方法创建新的函数，与原函数的唯一的变化是改变了 `this` 的值

> * call apply bind 总结
>
> -  **相同点:** 
>   -  都可以改变函数内部的this指向. 
>
> -  **区别点:** 
>
>   -  call 和 apply 会调用函数, 并且改变函数内部this指向. 
>
>   - call 和 apply 传递的参数不一样, call 传递参数 aru1, aru2..形式 apply 必须数组形式[arg] 
>
>   - bind 不会调用函数, 可以改变函数内部this指向. 
>
> - **主要应用场景:** 
>
>   -  call 调用函数并且可以传递参数
>
>   -  apply 经常跟数组有关系. 比如借助于数学对象实现数组最大值最小值
>
>   -  bind 不调用函数,但是还想改变this指向. 比如改变定时器内部的this指向.

# 三，性能优化

## 1 -防抖（debounce）

- 防抖： 单位事件内，频繁触发事件，只执行一次
- 例如：
- 北京买房政策：需要连续5年的社保，如果中间有一年断了社保，则需要从新开始计算

  比如，我 2020年开始计算，连续交5年，也就是到2024年可以买房了，包含2020年

  但是我 2024年断社保了，整年没交，则需要从2025年开始算第一年往后推5年… 

- 使用场景：
  - 搜搜框搜索输入，只要用户最后一次输入完，再发送请求
  - 手机号，邮箱输入检测 （意思是输入完了才进行验证）
  - 假设输入就可以发送请求，但是不能每次输入都去发送请求，输入比较快发送请求会比较多
  
    我们设定一个时间，假如300ms， 当输入第一个字符时候，300ms后发送请求，但是在200ms的时候又输入了一个字符，
  
    则需要再等300ms 后发送请求
  
- 所谓防抖，就是指触发事件后在 n 秒内函数只能执行一次，如果在 n 秒内又触发了事件，则会重新计算函数执行时间

### *练习：利用防抖来处理-鼠标滑过盒子显示文字

要求：

1. 鼠标在盒子上移动，里面的数字就会变化+1

2. 鼠标在盒子移动，鼠标停止500ms之后，俩面的数字才会变化+1

   - 实现方式：

     - lodash 提供的防抖来处理

     <img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230916150202807.png" alt="image-20230916150202807" style="zoom:50%;" />

     - 手写一个防抖函数来处理

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230916145228744.png" alt="image-20230916145228744" style="zoom:50%;" />

- 思路

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230916150358858.png" alt="image-20230916150358858" style="zoom:50%;" />

- 核心思路：

利用定时器实现，当鼠标滑过，判断有没有定时器，还有就清除，以最后一次滑动为准开启定时器

①： 写一个防抖函数debounce ，来控制这个操作函数(mouseMove) 

②: 防抖函数传递2个参数， 第一个参数 mouseMove函数，第二个参数 指定时间500ms

③： 鼠标移动事件，里面写的是防抖函数

④： 声明定时器变量 timeId

⑤： 但是节流函数因为里面写的函数名 debounce(mouseMove, 500), 是调用函数，无法再次调用执行，

所以需要在节流函数里面写return 函数 这样可以多次执行

⑥： 如果有定时器，则清除定时器

⑦: 否则开启定时器， 在设定时间内，调用函数

~~~html
<body>
  <div class="box"></div>
  <script>
    const box = document.querySelector('.box')
    let i = 1  // 让这个变量++
    // 鼠标移动函数
    function mouseMove() {
      box.innerHTML = ++i
      // 如果里面存在大量操作 dom 的情况，可能会卡顿
    }
    // 防抖函数
    function debounce(fn, t) { //形参  fn = mouseMove 函数
      let timeId
      //return返回一个匿名函数
      return function () { //返回值给 debounce(mouseMove, 200)
        // 如果有定时器就清除
        if (timeId) clearTimeout(timeId)
        // 开启定时器 200
        timeId = setTimeout(function () {
          fn()
        }, t) // t = 500
      }
    }
    // box.addEventListener('mousemove', mouseMove)
    box.addEventListener('mousemove', debounce(mouseMove, 200)) //实参， debounce(mouseMove, 200) = 匿名函数
debounce(mouseMove, 200)
  </script>
</body>
~~~



## 1- 节流（throttle）

- 所谓节流，就是指连续触发事件但是在 n 秒中只执行一次函数（要生上一次完成才开始下一个）
- 使用场景：高频事件：鼠标移动mousemove，页面尺寸缩放resize，滚动条scroll 等等
- 例如： 只有等到了上一个人做完核酸，整个动作完成了， 第二个人才能排队跟

### *练习：利用节流来处理-鼠标滑过盒子显示文字

要求： 鼠标在盒子上移动，里面的数字就会变化+

- lodash 提供的防抖来处理
- 手写一个防抖函数来处理

核心思路：

利用时间相减：移动后的时间 - 刚开始移动的时间 >= 500ms 我才去执行 mouseMove函数

①： 写一个节流函数throttle ，来控制这个操作函数(mouseMove)， 500ms之后才去执行这个函数

②: 节流函数传递2个参数， 第一个参数 mouseMove函数，第二个参数 指定时间500ms

③： 鼠标移动事件，里面写的是节流函数

④： 声明一个起始时间 startTime = 0

⑤： 但是节流函数因为里面写的函数名 throttle(mouseMove, 500), 是调用函数，无法再次调用执行

，所以需要在节流函数里面写return 函数 这样可以多次执行

⑥：记录当前时间 now = Date.now()

⑦：进行判断 如果大于等于 500ms，则执行函数， 但是千万不要忘记 让 起始时间 = 现在时间

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230916154732884.png" alt="image-20230916154732884" style="zoom:50%;" />

*注意： 要使用 timer = null 清楚定时器 （开启定时器里面清楚定时器）

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230916154620002.png" alt="image-20230916154620002" style="zoom:50%;" />

### *防抖 和 节流 总结

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230916154908282.png" alt="image-20230916154908282" style="zoom:50%;" />

# 四，综合案例：页面打开，可以记录上一次的视频播放位置

- 分析

两个事件:

①：ontimeupdate 事件在视频/音频（audio/video）当前的播放位置发送改变时触发

②：onloadeddata 事件在当前帧的数据加载完成且还没有足够的数据播放视频/音频（audio/video）的下一帧时触发

- 思路：

1. 在ontimeupdate事件触发的时候，每隔1秒钟，就记录当前时间到本地存储
2. 下次打开页面， onloadeddata 事件触发，就可以从本地存储取出时间，让视频从取出的时间播放，如

果没有就默认为0s

3. 获得当前时间 video.currentTime

- 谁需要节流？

ontimeupdate ， 触发频次太高了，我们可以设定 1秒钟触发一次

~~~html
<script src="https://cdn.jsdelivr.net/npm/lodash@4.17.21/lodash.min.js"></script>
  <script>
    // 1. 获取元素  要对视频进行操作
    const video = document.querySelector('video') //video标签
    //不能用定时器， 因为到这个时间就触发
    video.ontimeupdate = _.throttle(() => {
      // console.log(video.currentTime) 获得当前的视频时间
      // 把当前的时间存储到本地存储 localStorage
      //当前时间 currentTime
      localStorage.setItem('currentTime', video.currentTime)
    }, 1000)

    // 打开页面触发事件，就从本地存储里面取出记录的时间， 赋值给  video.currentTime
    video.onloadeddata = () => {
      // console.log(111)
      video.currentTime = localStorage.getItem('currentTime') || 0 //如果没有执行后面的0
    }

  </script>
~~~

