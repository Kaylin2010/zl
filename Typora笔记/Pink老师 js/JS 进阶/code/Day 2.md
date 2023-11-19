# 综合案例： 购物车展示

- 需求：

根据后台提供的数据，渲染购物车页面

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230915160353731.png" alt="image-20230915160353731" style="zoom:50%;" />

- 分析业务模块：

①：渲染图片、标题、颜色、价格、赠品等数据

②：单价和小计模块

③：总价模块

- 分析业务模块：
  - 两个盒子： 渲染信息 list，综合 total

①：把整体的结构直接生成然后渲染到大盒子.list 里面

②：那个方法可以遍历的同时还有返回值 map 方法

③：最后计算总价模块，那个方法可以求和？ reduce 方法

- 分析业务模块：

①：先利用map来遍历，有多少条数据，渲染多少相同商品

\- 可以先写死的数据

\- 注意map返回值是 数组，我们需要用 join 转换为字符串

\- 把返回的字符串 赋值 给 list 大盒子的 innerHTML

~~~html
<script>
// 1. 根据数据渲染页面
    document.querySelector('.list').innerHTML = goodsList.map(item => { //map创建4个空对象，然后return给结构）
      // console.log(item)  // 每一条对象
      // 对象解构(固定数据)  = item.price item.count，解构对象要写名字一样
      const { picture, name, count, price, spec, gift } = item
      2.2// 规格文字模块处理
      const text = Object.values(spec).join('/')
      2.4 // 计算小计模块 单价 * 数量  保留两位小数 
      // 注意精度问题，因为保留两位小数，所以乘以 100  最后除以100
      const subTotal = ((price * 100 * count) / 100).toFixed(2)
      2.3// 处理赠品模块 '50g茶叶,清洗球'
      // const str = gift ? XX : '' (gif他有吗？有 ： 没有给空字符串)
      const str = gift ? gift.split(',').map(item => `<span class="tag">【赠品】${item}</span> `).join('') : ''
      return `
        <div class="item">
          <img src=${picture} alt="">
          <p class="name">${name} ${str} </p>
          <p class="spec">${text} </p>
//.toFixed(2)保留2位小数
          <p class="price">${price.toFixed(2)}</p> 
          <p class="count">x${count}</p>
          <p class="sub-total">${subTotal}</p>
        </div>
      `
    }).join('') //map以后是数组， 要转换为字符串

~~~



②：里面更换各种数据，注意使用对象解构赋值

- 先更换不需要处理的数据，图片，商品名称，单价，数量

- 2.2 - 更换数据 - 处理 规格文字 模块

​	\+ 获取 每个对象里面的 spec , 上面对象解构添加 spec

​	+ 获得所有属性值是： Object.values() 返回的是数组

​	+ 拼接数组是 join(‘’) 这样就可以转换为字符串了

​	\+ 采取对象解构的方式

注意 单价要保留2位小数， 489.00 toFixed(2)

- 2.3 - 更换数据 - 处理 赠品 模块
+ 获取 每个对象里面的 gift , 上面对象解构添加 gift
  + 注意要判断是否有gif属性，==没有的话不需要渲染==
+ 利用变成的字符串然后写到 p.name里面

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230915164148801.png" alt="image-20230915164148801" style="zoom:50%;" />

![image-20230915164757825](/Users/guoguo/Library/Application Support/typora-user-images/image-20230915164757825.png)

bug：报错因为有的没有gif --- 需要判断

- 2.4 - 更换数据 - 处理 小计 模块

  \- 小计 = 单价 * 数量

  \- 小计名可以为: subTotal = price * count 

  \- 注意保留2位小数

  关于小数的计算精度问题:

  0.1 + 0.2 = ? 

  解决方案： 我们经常转换为整数

  （0.1*100 + 0.2*100）/ 100 === 0.3

  这里是给大家拓展思路和处理方案

③：利用reduce计算总价

\- 求和用到数组 reduce 方法 累计器

\- 根据数据里面的数量和单价累加和即可

\- 注意 reduce方法有2个参数，第一个是回调函数，第二个是 初始值，这里写 0

~~~html
 <script>
   // 3. 合计模块
   //goodsList里面的price，count
    const total = goodsList.reduce((prev, item) => prev + (item.price * 100 * item.count) / 100, 0)
    // console.log(total)
    document.querySelector('.amount').innerHTML = total.toFixed(2)
 </script>
~~~



