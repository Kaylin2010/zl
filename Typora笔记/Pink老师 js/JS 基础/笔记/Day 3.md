# 一，循环 - for

## 1. for 循环基本使用

- 作用：重复执行代码

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230817093341942.png" alt="image-20230817093341942" style="zoom:50%;" />

- continue：退出本次循环 （用来排除或者跳过某一个选项的时候）
- break：退出整个for循环（解雇哦已经达到，后续的循环不需要了）
- 了解：
  - while(true) 来构造“无限”循环 （不结束），需要使用break退出循环
  - for(;;) 也可以来构造“无限”循环，同样需要使用break退出循环

### *练习

1. 利用for循环输出1~100岁

~~~html
 <script>
    // 利用for循环输出1~100岁
    for (let i = 1; i <= 100; i++) {
      document.write(`${i}岁 <br>`)
    }
  </script>
~~~

2. 求1-100之间所有的偶数和

~~~html
<script>
    // 求1-100之间所有的偶数和
    let sum = 0
    for (let i = 1; i <= 100; i++) {
      sum = sum + i
    }
    document.write(sum)
  </script>
~~~

3. 页面中打印5个小星星

![image-20230824160627815](/Users/guoguo/Library/Application Support/typora-user-images/image-20230824160627815.png)

~~~html
<script>
   for (let i = 1; i <= 5; i++) {
      document.write('*')
    }
  </script>
~~~

**4. for循环的最大价值： 循环数组**

~~~html
	<script>
   let name = ['A', 'B', 'C', 'D']
    for (let i = 0; i < name.length; i++) {
      // 要打印那么里面的i
      document.write(name[i])
    }
    // 打印name （错）
    document.write(name)
  </script>
~~~



## 2. 循环嵌套

- 外面的for执行一次 --》 里面的for全部执行

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230817100220496.png" alt="image-20230817100220496" style="zoom:50%;" />

- 

### *练习1：

计算： 假如每天记住5个单词，3天后一共能记住多少单词？

~~~html
	<script>
    for (i = 1; i <= 3; i++) {
      document.write(`第${i}天<br>`)
      for (j = 1; j <= 5; j++) {
        document.write(`第${j}个生词 <br>`)
      }
    }
  </script>
~~~

### ? 练习2：打印倒三角形星星 

需求： 如图

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230817101908276.png" alt="image-20230817101908276" style="zoom:50%;" />

~~~html
s
~~~

### ? 练习3：九九乘法表

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230817101923410.png" alt="image-20230817101923410" style="zoom:50%;" />



# 二，数组

## 1. 数组是什么

- Array 是一种可以按顺序保存数据的数据类型
- 场景：如果有很多数据可以用数组保存起来，然后放到一个变量中，管理非常方便

## 2. 数组的基本使用

### 2.1 声明语法

![image-20230817102529036](/Users/guoguo/Library/Application Support/typora-user-images/image-20230817102529036.png)

- 计算机中的编号从0开始
- 数据的编号也叫 索引号 或 下标号
- 数组可以存储任何类型的数据 （数字，字符串，布尔。。）

### 2.2 取值语法

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230817102715117.png" alt="image-20230817102715117" style="zoom:50%;" />

- 通过[下标]取数据
- 取出什么类型，就根据这种类型特点访问

### ==2.3 遍历数组==

- 用循环把==数据中每个元素都访问到==，一般用for循环遍历
- 语法

![image-20230817103129543](/Users/guoguo/Library/Application Support/typora-user-images/image-20230817103129543.png)

#### ? 练习1：

需求：求数组 [2,6,1,7, 4] 里面所有元素的和以及平均值

~~~javascript
	<script>
    let sum = 0
    let arr = [2, 3, 1, 7, 4]
    for (let i = 0; i < arr.length; i++) {
      sum = sum + arr[i]
    }
    document.write(sum / arr.length)
  </script>
~~~



#### ? 练习2：

需求：求数组 [2,6,1,77,52,25,7] 中的最大值

~~~javascript
 <script>
    let arr = [2, 6, 1, 77, 52, 25, 7]
    let max = arr[0] //用一个固定的值对比
    for (let i = 0; i < arr.length; i++) {
      i < max ? max : max = arr[i]
    }
    document.write(max)
  </script>
~~~



## 3. 操作数组

- 数组本质是数据集合, 操作数据无非就是 增 删 改 查 语法：

![image-20230817103446089](/Users/guoguo/Library/Application Support/typora-user-images/image-20230817103446089.png)

### 3.1 操作数据 - 改

- 给所有的数组元素后面加个老师  （修改）

~~~html
<script>
    // let arr = []
    // console.log(arr)
    // // console.log(arr[0])  // undefined
    // arr[0] = 1
    // arr[1] = 5
    // console.log(arr)
    let arr = ['pink', 'red', 'green']
    // 修改
    // arr[0] = 'hotpink'
    // console.log(arr)
    // 给所有的数组元素后面加个老师  修改
    for (let i = 0; i < arr.length; i++) {
      // console.log(arr[i])
      arr[i] = arr[i] + '老师'
    }
    console.log(arr)
  
  ------------------------------------------------------
 *** 自己写***
    let arr = ['A', 'B', 'C', 'D']
    for (let i = 0; i < arr.length; i++) {
      // document.write(`${arr[i]}ok`)
      arr[i] = arr[i] + '老师'
      // 这样是打印每个i， 没有括号
      // document.write(`${arr[i]}ok`)
    }
    // 打印arr带有括号
    document.write(arr)
 
  </script>
~~~



### ==3.2 操作数组 - 新增==

- 数组 ==.push(新增内容)==  方法将一个或多个元素添加到<u>**数组的末尾**</u>，并返回该数组的<u>**新长度**</u>
- 语法

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230817104940694.png" alt="image-20230817104940694" style="zoom:50%;" />

- 例如：

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230817105028590.png" alt="image-20230817105028590" style="zoom:50%;" />

- 数组 ==arr.unshift (新增的内容)==  方法将一个或多个元素添加到**<u>数组的开头</u>**，并返回该数组的**<u>新长度</u>**

- 语法：

  <img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230817105404185.png" alt="image-20230817105404185" style="zoom:50%;" />

  - 例如

  <img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230817105437084.png" alt="image-20230817105437084" style="zoom:50%;" />

#### ？练习1 ：

需求：将数组 [2, 0, 6, 1, 77, 0, 52, 0, 25, 7] 中大于等于 10 的元素选出来，放入新数组

#### ？练习2 ：

需求：将数组 [2, 0, 6, 1, 77, 0, 52, 0, 25, 7] 中的 0 去掉后，形成一个不包含 0 的新数组

### 3.3 操作数组 - 删除

- ==数组. shift()== 方法从数组中删除<u>**第一个元素**</u>，并返回<u>**该元素的值**</u>

- 语法：

  <img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230817110114418.png" alt="image-20230817110114418" style="zoom:50%;" />

  - 例如

  <img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230817110130471.png" alt="image-20230817110130471" style="zoom:50%;" />

- 数组==. pop()== 方法从数组中删除**<u>最后一个元素</u>**，并返回**<u>该元素的值</u>**

  - 语法

  <img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230817110230273.png" alt="image-20230817110230273" style="zoom:50%;" />

  - 例如

  <img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230817110243585.png" alt="image-20230817110243585" style="zoom:50%;" />

- 数组==. splice()== 方法 **<u>删除指定元素</u>**

  - 语法

  <img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230817110347365.png" alt="image-20230817110347365" style="zoom:50%;" />

  - 解释：
    - start 起始位置: 指定修改的开始位置（从0计数）
    - deleteCount: 
      -  表示要移除的数组元素的个数 
      - 可选的。 如果省略则默认从指定的起始位置删 除到最后

## 4. 数组案例



# 三，综合案例