# 对象

## 1.什么是对象

- 对象（object）无序的数据集合（数组是有序）
- 可以详细描述 某个事物

## 2.对象使用

### 2.1 对象声明

> 使用花括号（ngoặc nhọn {})

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230822161545584.png" alt="image-20230822161545584" style="zoom:50%;" />

### 2.2 对象有属性跟方法组成

- 属性： 信息或者特征（名词）。例如：颜色，尺寸。。。
- 方法： 功能或者行为。例如：打电话，发短信。。。

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230822162043692.png" alt="image-20230822162043692" style="zoom:50%;" />

### 2.3 属性（信息： 名词）

- 属性（名词性）： 数据描述性的信息, 例如： 年龄， 身高。。。
- `属性名（变量） ： 值（赋值）`  用英语的冒号（:）分隔
- 多个属性之间用英语的逗号（,）分隔
- 对象本质是无序的数据集合，操作数据就是：  增 删 改 查

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230822163203483.png" alt="image-20230822163203483" style="zoom:50%;" />

#### *属性 - 查

- 就是获取页面的属性值
  - ==对象名.属性==

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230822163753426.png" alt="image-20230822163753426" style="zoom:50%;" />

> 查的另外一种写法：
>
> - 语法：==对象[‘属性’]== (记得加引号)

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230822164937519.png" alt="image-20230822164937519" style="zoom:50%;" />

#### *属性 - 改

- 语法： ==对象名.属性 = 新值==

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230822164123079.png" alt="image-20230822164123079" style="zoom:50%;" />

#### *属性 - 增

- 语法：==对象名.新属性 = 新值==

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230822164329124.png" alt="image-20230822164329124" style="zoom:50%;" />

#### *属性 - 删 （了解）

- 语法：delete 对象名.属性

> 1. 改跟增如果属性已经有 ---》 一样（改值）
> 2. 所有属性的操作都写在对象外面

### 2.3 对象中方法 （行为： 动词）

- 对象的方法（动词）： 数据行为性的信息，如跑步，唱歌
-  ==方法名:函数== 
  - 多方法用逗号隔开
  - 方法调用（实参）：对象名.方法名（）

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230822211211397.png" alt="image-20230822211211397" style="zoom:50%;" />

## 3. 遍历对象

- 为什么不能用for遍历对象
  - 属性没有length属性，所以无法确定长度
  - 对象里面的是无序，没有规律的下标
- 遍历对象
  - for in 语法中的k是一个变量，在循环的过程中一次代表对象的属性名
  - 由于k是变量所以必须使用[]语法解析
  - k获取对象的属性名（打印出来的值 = 字符串）

> 记住：k是获得对象的属性名 `obj[k]`（打印出来属性名， 也可以改成a b c）

~~~javascript
<script>
    let obj = {
      uname: 'halo',
      age: 12,
      gender: 'male'
    }
    // 遍历对象
    for (let k in obj) {
      // 打印出来属性名 ('name' 'age' 'gender')
      // console.log(k)  
      //打印出来属性值 ('halo' '12' 'male')
      // console.log(obj[k])
      //打印出来每个属性的值 ('halo')
      console.log(obj['uname'])
    }
  </script>
~~~



### *练习：遍历数字对象

需求：请把下面数据中的对象打印出来：

 // 定义一个存储了若干学生信息的数组 

let students =[

 {name: '小明', age: 18, gender: '男', hometown: '河北省'},

 {name: '小红', age: 19, gender: '女', hometown: '河南省'}, 

{name: '小刚', age: 17, gender: '男', hometown: '山西省'},

 {name: '小丽', age: 18, gender: '女', hometown: '山东省'} ]

~~~javascript
<script>
    let students = [
      { name: '小明', age: 18, gender: '男', hometown: '河北省' },
      { name: '小红', age: 19, gender: '女', hometown: '河南省' },
      { name: '小刚', age: 17, gender: '男', hometown: '山西省' },
      { name: '小丽', age: 18, gender: '女', hometown: '山东省' }]
    // 是一个数组所以使用以前的for
    for (let i = 0; i < students.length; i++) {
      // 下表索引号 (0,1,2,3)
      // console.log(i)

      // 打印每个对象
      // console.log(students[i])

      // 打印对象指定的属性的值 (所有name的值)
      console.log(students[i].name)
    }
  </script>
~~~

### *练习：

需求：根据以上数据渲染生成表格

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230823060027008.png" alt="image-20230823060027008" style="zoom:50%;" />

~~~javascript
<body>
  <h2>学生信息</h2>
  <p>将数据渲染到页面中...</p>
  <table>
    <caption>学生列表</caption>
    <tr>
      <th>序号</th>
      <th>姓名</th>
      <th>年龄</th>
      <th>性别</th>
      <th>家乡</th>
    </tr>

    <script>
      let students = [
        { name: '小明', age: 18, gender: '男', hometown: '河北省' },
        { name: '小红', age: 19, gender: '女', hometown: '河南省' },
        { name: '小刚', age: 17, gender: '男', hometown: '山西省' },
        { name: '小丽', age: 18, gender: '女', hometown: '山东省' }]
      // 是一个数组所以使用以前的for
      for (let i = 0; i < students.length; i++) {
        document.write(`
        <tr>
          <td>${i + 1}</td>
          <td>${students[i].name}</td>
          <td>${students[i].age}</td>
          <td>${students[i].gender}</td>
          <td>${students[i].hometown}</td>
        </tr>
        `)
      }
    </script>
  </table>

</body>
~~~

> 1. 上面是一个数组所以使用以前的for
>
> 2.用 document.write 打印除页面并且用模版字符串（拼接字符和变量） `document.write(``）`
>
> 3.序号是下标号 + 1   `<td>${i + 1}</td>` 
>
> 4. <tr>是列，<th> 行 <td> 行

## 4. 内置对象

### 4.1 内置对象是什么

- JavaScript 内部提供的对象，包含各种属性和方法给开发调用

### 4.2 内置对象Math

- 是一个数学对象，提供了一系列做数学运算的方法

  ~~~javascript
  // 属性
      console.log(Math.PI)
  ~~~

  - random：生成0-1之间的随机数（包含0不包括1）

  - abs：绝对值

    ~~~javascript
     console.log(Math.abs(-1))  //1
    ~~~

    

  - max：找最大数, min：找最小数

    ~~~javascript
    	console.log(Math.max(1, 2, 3, 4, 5)) // 5
      console.log(Math.min(1, 2, 3, 4, 5)) //1
    ~~~

  - floor：向下取整

    ~~~javascript
     // floor 地板  向下取整
    	console.log(Math.floor(1.9))  // 1
    	console.log(Math.floor('12px'))  // NaN
    ~~~

  - ceil：向上取整

    ~~~javascript
    // ceil 天花板  向上取整
        console.log(Math.ceil(1.1)) // 2 
        console.log(Math.ceil(1.5)) // 2 
        console.log(Math.ceil(1.9)) // 2 
    ~~~

### 4.3 生成任意随机数

- Math.radom 随机数函数，返回一个0 -1 之间，并且包含0 不包含1的随机小数 [0,1)

  ~~~javascript
  	<script>
      // 0-10 之间整数 跟 Math.floor 向下取整
      console.log(Math.floor(Math.random() * 11));
  
      // 随机颜色数组, 给radom声明
      let arr = ['red', 'green', 'blue']
      let radom = (Math.floor(Math.random() * arr.length))
      console.log(arr[radom]);
    </script>
  ~~~

  - 取到 N ~ M 的随机整数 （有 N，M） --- 直接赋值用

  ~~~javascript
  	function getRandom(N, M) {
        return Math.floor(Math.random() * (M - N + 1)) + N
      }
      console.log(getRandom(4, 8))
  解释： (M - N + 1) = 5 （ 0-5： 取到4 [0-5 = 4 （0+4 - 4+5= （4 - 9]
  ~~~

  - 生成N-M之间的随机数 

  ~~~javascript
  Math.floor(Math.random() * (M - N + 1)) + N
  ***用于已经声明的数组
  let radom = (Math.floor(Math.random() * arr.length))
  ~~~

  ### *练习：随机点名案例
  
  