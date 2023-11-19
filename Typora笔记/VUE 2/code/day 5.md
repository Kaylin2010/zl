# 综合案例：商品列表

- App.vue

~~~html
<template>
<!-- 11.4 使用 MyTable-->
<!-- 12.1 获取数据 数组给子元素-->
  <div class="table-case">
    <MyTable :data="goods">
      <!-- 13.2 表头复制过来复用 -->
      <template #head>
        <th>编号</th>
        <th>名称</th>
        <th>图片</th>
        <th width="100px">标签</th>
      </template>
    <!-- 13.2 主题复用，然后用插槽的name 接数据过来  #body #body="{ obj },可以直接写{ item, index }-->
      <template #body="{ item, index }">
        <td>{{ index + 1 }}</td>
        <td>{{ item.name }}</td>
        <td>
          <img
            :src="item.picture"
          />
        </td>
        <td>
          <!-- 8.2 绑定 model 获取 v-model="tempText" -->
           <!-- 15.换成数组对应的数据 "item.tag"-->
          <MyTag v-model="item.tag"></MyTag>
        </td>
      </template>
    </MyTable>
  </div>
</template>

<script>

// 3.导入组件
import MyTag from './components/MyTag.vue'
// 11.2 导入
import MyTable from './components/MyTable.vue'
export default {
  name: 'TableCase',
  // 4.注册局部组件
  // 11.3 注册局部组件
  components: {
    MyTag,
    MyTable
  },
  data () {
    return {
      // 8.1 测试组件功能的临时数据，怎么传给子并且可以修改
      tempText: '水杯',
      tempText2: '钢笔',
      goods: [
        { id: 101, picture: 'https://yanxuan-item.nosdn.127.net/f8c37ffa41ab1eb84bff499e1f6acfc7.jpg', name: '梨皮朱泥三绝清代小品壶经典款紫砂壶', tag: '茶具' },
        { id: 102, picture: 'https://yanxuan-item.nosdn.127.net/221317c85274a188174352474b859d7b.jpg', name: '全防水HABU旋钮牛皮户外徒步鞋山宁泰抗菌', tag: '男鞋' },
        { id: 103, picture: 'https://yanxuan-item.nosdn.127.net/cd4b840751ef4f7505c85004f0bebcb5.png', name: '毛茸茸小熊出没，儿童羊羔绒背心73-90cm', tag: '儿童服饰' },
        { id: 104, picture: 'https://yanxuan-item.nosdn.127.net/56eb25a38d7a630e76a608a9360eec6b.jpg', name: '基础百搭，儿童套头针织毛衣1-9岁', tag: '儿童服饰' },
      ]
    }
  }
}
</script>

<style lang="less" scoped>
.table-case {
  width: 1000px;
  margin: 50px auto;
  img {
    width: 100px;
    height: 100px;
    object-fit: contain;
    vertical-align: middle;
  }
}

</style>
~~~



- MyTag.vue

~~~html
<template>
  <!-- 1. 赋值对应的结构 -->
  <div class="my-tag">
    <!-- 5.1 是否显示 用if -->
    <!-- 6.自动获取焦点 -->
    <!-- 6.4 添加 v-focus 指令-->
    <!-- 7.绑定失去焦点事件，输入框隐藏 @blur="isEdit = false"-->
    <!-- 8.4 子不能修改父传子的数据 :value="value"-->
    <!-- 9. 绑定回车事件 @keyup.enter="handleEnter" -->
    <input
      v-if="isEdit"
      v-focus
      ref="inp"
      class="input"
      type="text"
      placeholder="输入标签"
      :value="value"
      @blur="isEdit = false"
      @keyup.enter="handleEnter"
    />
    <!-- 5.2是否显示 用else -->
    <!-- 5.3 双击显示事件 @dblclick="handleClick"-->
    <!-- 8.4 渲染 {{ value }} -->
    <div v-else @dblclick="handleClick" class="text">
      {{ value }}
    </div>
  </div>
</template>

<script>
// 8.3 接受父传子
export default {
  props: {
    value: String,
  },
  // 5.3 写对应的方法， 如果是true :输入框显示
  data() {
    return {
      isEdit: false,
    };
  },
  // 5.3 对应方法，this.isEdit = true 双击时候显示
  methods: {
    handleClick() {
      // 双击后，切换到显示状态 (Vue是异步dom更新)
      this.isEdit = true;

      // // 6.2 等dom更新完了，再获取焦点
      // this.$nextTick(() => {
      //  6.1  立刻获取焦点，因为Vue是异步dom更新， 所以立刻获取不行
      //   this.$refs.inp.focus()
      // }) 但是不能复用， 所以去main.js 给它封装
    },
    // 9.2 对应语法
    handleEnter(e) {
      // 11. 非空处理 - 内容为空
      if (e.target.value.trim() === "") return alert("标签内容不能为空");

      // 子传父，将回车时，[输入框的内容] 提交给父组件更新
      // 由于父组件是v-model，触发事件，需要触发 input 事件
      // 9.3 测试是否拿到输入框 input
      // console.log(e.target)
      // 9.4 e.target.value 拿到实时的数据， $emit传给父
      this.$emit("input", e.target.value);
      // 10. 提交完成，关闭输入状态
      this.isEdit = false;
    },
  },
};
</script>

<style lang="less" scoped>
//2.赋值对应的样式，加上 lang="less" scoped
.my-tag {
  cursor: pointer;
  .input {
    appearance: none;
    outline: none;
    border: 1px solid #ccc;
    width: 100px;
    height: 40px;
    box-sizing: border-box;
    padding: 10px;
    color: #666;
    &::placeholder {
      color: #666;
    }
  }
}
</style>
~~~



- MyTable.vue

~~~html
<template>
<!-- 11.把结构 跟 样式 赋值过来 -->
  <table class="my-table">
    <thead>
      <!-- 13.1表头不能写死， 给 name="head"-->
      <tr>
        <slot name="head"></slot>
      </tr>
    </thead>
    <tbody>
      <!-- 12.3 渲染数据 -->
      <tr v-for="(item, index) in data" :key="item.id">
        <!-- 13.1 标题主题 不能写死 -->
        <!-- 13.3 插槽 要传给父什么数据:item="item" :index="index" -->
        <slot name="body" :item="item" :index="index" ></slot>
      </tr>
    </tbody>
  </table>
</template>

<script>
// 12.2 用props 接受数据
export default {
  props: {
    data: {
      type: Array,
      required: true
    }
  }
};
</script>

<style lang="less" scoped>

.my-table {
  width: 100%;
  border-spacing: 0;
  img {
    width: 100px;
    height: 100px;
    object-fit: contain;
    vertical-align: middle;
  }
  th {
    background: #f5f5f5;
    border-bottom: 2px solid #069;
  }
  td {
    border-bottom: 1px dashed #ccc;
  }
  td,
  th {
    text-align: center;
    padding: 10px;
    transition: all .5s;
    &.red {
      color: red;
    }
  }
  .none {
    height: 100px;
    line-height: 100px;
    color: #999;
  }
}

</style>
~~~



