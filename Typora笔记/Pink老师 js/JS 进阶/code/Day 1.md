# 案例：渲染商品列表

* 请根据数据渲染以下效果

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230913164256498.png" alt="image-20230913164256498" style="zoom:50%;" />

- 核心思路：有多少条数据，就渲染多少模块，然后 生成对应的 html结构标签， 赋值给 list标签即可 

①：利用forEach 遍历数据里面的 数据 

②：拿到数据，利用**字符串拼接**生成结构添加到页面中 

③：注意：传递参数的时候，可以使用对象解构

~~~html
<script>
   // 1. 声明一个字符串变量
    let str = ''
    // 2. 遍历数据 
    // goodsList.forEach(function(item){})
    // item：当前数组元素（只有一个数组）
    goodsList.forEach(item => {
      // console.log(item)  // 可以得到每一个数组元素  对象 {id: '4001172'}
      // const {id} =  item  对象解构
      const { name, price, picture } = item //对象结构 下面不要写item.name
      str += `
      <div class="item">
        <img src=${picture} alt=""> // 结构后不要写item
        <p class="name">${name}</p>
        <p class="price">${price}</p>
      </div>
      `
    })
    // 3.生成的 字符串 添加给 list 
    document.querySelector('.list').innerHTML = str //渲染页面
 </script>
~~~

# 综合案例：商品列表价格筛选

需求： 

①：渲染数据列表 

步骤： 

- (1) 初始化需要渲染页面，同时，点击不同的需求，还会重新渲染页面，所以渲染做成一个函数 

- (2) 做法基本跟前面案例雷同，就是封装到了一个函数里面

~~~html
* 用css写 （逗号： 同时包括两个）
		.filter a:active,
    .filter a:focus {
      background: #05943c;
      color: #fff;
~~~

②：根据选择不同条件显示不同商品

- (1) 点击采取事件委托方式 .filter 

- (2) 利用过滤函数 filter 筛选出符合条件的数据，因为生成的是一个数组，传递给渲染函数即可 

- (3) 筛选条件是根据点击的 data-index 来判断 

- (4) 可以使用对象解构，把 事件对象 解构 

- (5) 因为 全部区间不需要筛选，直接 把goodList渲染即可

<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230913203721275.png" alt="image-20230913203721275" style="zoom:50%;" />

~~~html
<script>
  // 1. 渲染函数  封装 （用函数包起来）
  // arr:形参， goodsList： 实参
    function render(arr) {
      // 声明空字符串
      let str = ''
      // 遍历数组 
      arr.forEach(item => {
        // 解构
        const { name, picture, price } = item
        str += `
         <div class="item">
          <img src=${picture} alt="">
          <p class="name">${name}</p>
          <p class="price">${price}</p>
        </div> 
        `
      })
      // 追加给list 
      document.querySelector('.list').innerHTML = str
    }
    render(goodsList)  // 页面一打开就需要渲染

    // 2. 过滤筛选  - 事件委托 ：判断点了哪个
 document.querySelector('.filter').addEventListener('click', e => {
      // e.target.dataset.index   e.target.tagName
   		// e.target 是包括 tagName, dataset 的数组， 结构出来
      const { tagName, dataset } = e.target
      // 判断 
       // e.target.tagName === 'A' (以前的写法)
      if (tagName === 'A') { 
        // console.log(11)  检测点击拿到11
        // arr 返回的新数组，filter的
        let arr = goodsList //（可以先给空数组）
        //判断点了哪个a
        if (dataset.index === '1') {
          arr = goodsList.filter(item => item.price > 0 && item.price <= 100)
        } else if (dataset.index === '2') {
          //filter 返回新数组
          arr = goodsList.filter(item => item.price >= 100 && item.price <= 300)
        } else if (dataset.index === '3') {
          arr = goodsList.filter(item => item.price >= 300)
        }
        // 渲染函数
        render(arr)
      }
    })
</script>
~~~

~~~html
 let arr = goodsList 
=》不用空数组是因为全选，不是点123 剩下的是渲染全部（ goodsList ），给空数组就没什么了
=》点1， arr重新赋值
~~~



<img src="/Users/guoguo/Library/Application Support/typora-user-images/image-20230913213341432.png" alt="image-20230913213341432" style="zoom:50%;" />