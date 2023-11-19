# Web APIs - 第6天笔记

> 目标：能够利用正则表达式完成小兔鲜注册页面的表单验证，具备常见的表单验证能力

- 正则表达式
- 综合案例
- 阶段案例


# 一，正则表达式

**正则表达式**（Regular Expression）是一种字符串匹配的模式（规则）

**使用场景：**

- 验证表单：手机号表单要求用户只能输入11位的数字 (匹配)
- 过滤掉页面内容中的一些敏感词(替换)，或从字符串中获取我们想要的特定部分(提取)等 

## 1 - 正则基本使用

1. 定义规则

   ~~~JavaScript
   const reg =  /表达式/ （表达式： 需要检查）
   ~~~

   - 其中` /   / `是正则表达式字面量
   - 正则表达式也是对象

2. 检测查找是否匹配

   - `test()方法`   用来查看正则表达式与指定的字符串是否匹配
   
   <img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230906142918739.png" alt="image-20230906142918739" style="zoom:50%;" />
   
   - 如果正则表达式与指定的字符串匹配 ，返回`true`，否则`false`

~~~html
<body>
  <script>
    // 正则表达式的基本使用
    const str = 'web前端开发'
    // 1. 定义规则
    const reg = /web/

    // 2. 使用正则  test()
    console.log(reg.test(str))  // true  如果符合规则匹配上则返回true
    console.log(reg.test('java开发'))  // false  如果不符合规则匹配上则返回 false
  </script>
</body>
~~~

3. 检索（查找）符合规则的字符串：

   - **exec()** 方法 在一个指定字符串中执行一个搜索匹配

   <img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230906142943162.png" alt="image-20230906142943162" style="zoom:50%;" />

   - 如果匹配成功，exec() 方法返回一个数组，否则返回null
   
   ~~~javascript
   //结果
   ['web', index: 0, input: 'web', groups: undefined]
   ~~~
   
   

## 2 - 元字符

1. **普通字符:**

- 大多数的字符仅能够描述它们本身，这些字符称作普通字符，例如所有的字母和数字。
- 普通字符只能够匹配字符串中与它们相同的字符。    
- 比如，规定用户只能输入英文26个英文字母，普通字符的话  /[abcdefghijklmnopqrstuvwxyz]/

2. **元字符(特殊字符）**

- 是一些具有特殊含义的字符，可以极大提高了灵活性和强大的匹配功能。
- 比如，规定用户只能输入英文26个英文字母，换成元字符写法： /[a-z]/  

### 2.1 边界符

正则表达式中的边界符（位置符）用来提示字符所处的位置，主要有两个字符

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230906145350092.png" alt="image-20230906145350092" style="zoom:50%;" />

>:octopus:注意点：
>
>如果 ^ 和 $ 在一起，表示必须是精确匹配

~~~html
<body>
  <script>
    // 元字符之边界符
    // 1. 匹配开头的位置 ^
    const reg = /^web/
    console.log(reg.test('web前端'))  // true
    console.log(reg.test('前端web'))  // false
    console.log(reg.test('前端web学习'))  // false
    console.log(reg.test('we'))  // false

    // 2. 匹配结束的位置 $
    const reg1 = /web$/
    console.log(reg1.test('web前端'))  //  false
    console.log(reg1.test('前端web'))  // true
    console.log(reg1.test('前端web学习'))  // false
    console.log(reg1.test('we'))  // false  

    // 3. 精确匹配 ^ $ --- 所以只有一个结果是true
    const reg2 = /^web$/
    console.log(reg2.test('web前端'))  //  false
    console.log(reg2.test('前端web'))  // false
    console.log(reg2.test('前端web学习'))  // false
    console.log(reg2.test('we'))  // false 
    console.log(reg2.test('web'))  // true
    console.log(reg2.test('webweb'))  // flase 
  </script>
</body>
~~~

### 2.2 - 量词

量词用来设定某个模式重复次数

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230906153033366.png" alt="image-20230906153033366" style="zoom:50%;" />

> 注意： 逗号左右两侧千万不要出现空格

~~~html
<body>
  <script>
    // 元字符之量词
    // 1. * 重复次数 >= 0 次
    const reg1 = /^w*$/
    console.log(reg1.test(''))  // true
    console.log(reg1.test('w'))  // true
    console.log(reg1.test('we334w'))  // false （必须要w开头结尾， 不能出现任何其他的）
    console.log('-----------------------')

    // 2. + 重复次数 >= 1 次
    const reg2 = /^w+$/
    console.log(reg2.test(''))  // false
    console.log(reg2.test('w'))  // true
    console.log(reg2.test('ww'))  // true
    console.log('-----------------------')

    // 3. ? 重复次数  0 || 1 
    const reg3 = /^w?$/
    console.log(reg3.test(''))  // true
    console.log(reg3.test('w'))  // true
    console.log(reg3.test('ww'))  // false
    console.log('-----------------------')


    // 4. {n} 重复 n 次
    const reg4 = /^w{3}$/
    console.log(reg4.test(''))  // false
    console.log(reg4.test('w'))  // flase
    console.log(reg4.test('ww'))  // false
    console.log(reg4.test('www'))  // true
    console.log(reg4.test('wwww'))  // false
    console.log('-----------------------')

    // 5. {n,} 重复次数 >= n 
    const reg5 = /^w{2,}$/
    console.log(reg5.test(''))  // false
    console.log(reg5.test('w'))  // false
    console.log(reg5.test('ww'))  // true
    console.log(reg5.test('www'))  // true
    console.log('-----------------------')

    // 6. {n,m}   n =< 重复次数 <= m
    const reg6 = /^w{2,4}$/
    console.log(reg6.test('w'))  // false
    console.log(reg6.test('ww'))  // true
    console.log(reg6.test('www'))  // true
    console.log(reg6.test('wwww'))  // true
    console.log(reg6.test('wwwww'))  // false

    // 7. 注意事项： 逗号两侧千万不要加空格否则会匹配失败

  </script>
~~~

### 2.3 - 范围

- 表示字符的范围，定义的规则限定在某个范围，比如只能是英文字母，或者数字等等，用表示范围


<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230906153254602.png" alt="image-20230906153254602" style="zoom:50%;" />

~~~html
<body>
  <script>
    // 元字符之范围  []  
    // 1. [abc] 匹配包含的单个字符， 多选1
    const reg1 = /^[abc]$/
    console.log(reg1.test('a'))  // true
    console.log(reg1.test('b'))  // true
    console.log(reg1.test('c'))  // true
    console.log(reg1.test('d'))  // false
    console.log(reg1.test('ab'))  // false // 只能匹配一个，精致匹配
    
    //选两个
    console.log(/^[abc]{2}$/.test('ab'))  // true 可以选2个

    // 2. [a-z] 连字符 单个
    const reg2 = /^[a-z]$/
    console.log(reg2.test('a'))  // true
    console.log(reg2.test('p'))  // true
    console.log(reg2.test('0'))  // false
    console.log(reg2.test('A'))  // false
    // 想要包含小写字母，大写字母 ，数字
    const reg3 = /^[a-zA-Z0-9]$/
    console.log(reg3.test('B'))  // true
    console.log(reg3.test('b'))  // true
    console.log(reg3.test(9))  // true
    console.log(reg3.test(','))  // flase

    // 用户名可以输入英文字母，数字，可以加下划线，要求 6~16位
    const reg4 = /^[a-zA-Z0-9_]{6,16}$/
    console.log(reg4.test('abcd1'))  // false 
    console.log(reg4.test('abcd12'))  // true
    console.log(reg4.test('ABcd12'))  // true
    console.log(reg4.test('ABcd12_'))  // true

    // 3. [^a-z] 取反符
    const reg5 = /^[^a-z]$/
    console.log(reg5.test('a'))  // false 
    console.log(reg5.test('A'))  // true
    console.log(reg5.test(8))  // true

  </script>
</body>
~~~

- 字符类 . (点)代表匹配除换行符之外的任何单个字符

![image-20230906160320001](/Users/guoguo/Library/Application Support/typora-user-images/image-20230906160320001.png)

### 2.4 - 字符类

某些常见模式的简写方式，区分字母和数字

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230906164128140.png" alt="image-20230906164128140" style="zoom:50%;" />

 <img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230906164201269.png" alt="image-20230906164201269" style="zoom:50%;" />

# 二，替换和修饰符

- replace 替换方法，可以完成字符的替换 （替换什么 替换成什么）


<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230906164240341.png" alt="image-20230906164240341" style="zoom:50%;" />

~~~html
<body>
  <script>
    // 替换和修饰符
    const str = '欢迎大家学习前端，相信大家一定能学好前端，都成为前端大神'
    // 1. 替换  replace  需求：把前端替换为 web
    // 1.1 replace 返回值是替换完毕的字符串
    // const strEnd = str.replace(/前端/, 'web') 只能替换一个
  </script>
</body>
~~~

- 修饰符约束正则执行的某些细节行为，如是否区分大小写、是否支持多行匹配等

  - i 是单词 ignore 的缩写，正则匹配时字母不区分大小写

  - g 是单词 global 的缩写，匹配所有满足正则表达式的结果


~~~html
<body>
  <script>
    // 替换和修饰符
    const str = '欢迎大家学习前端，相信大家一定能学好前端，都成为前端大神'
    // 1. 替换  replace  需求：把前端替换为 web
    // 1.1 replace 返回值是替换完毕的字符串
    // const strEnd = str.replace(/前端/, 'web') 只能替换一个

    // 2. 修饰符 g 全部替换
    const strEnd = str.replace(/前端/g, 'web')
    // 全局替换部分大小
    const strEnd = str.replace(/前端/ig, 'web')
    console.log(strEnd) 
  </script>
</body>
~~~

## * change 事件 

（ DAY 6 - video144 - 2p）

给input注册 change 事件，值被==修改并且失去焦点==后触发 （代替blur)

输入框， 输入错之后没有更改内容只焦点失去不要再验证

~~~javascript
<body>
  <!-- <input type="text"> -->
  <input type="checkbox" name="" id="">
  <script>
    // change 事件 内容发生了变化
    const input = document.querySelector('input')
    input.addEventListener('change', function () {
      console.log(111)
    })
  </script>
</body>
~~~



## * 判断是否有类

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230906164618357.png" alt="image-20230906164618357" style="zoom:50%;" />

元素.classList.contains() 看看有没有包含某个类，如果有则返回true，么有则返回false



