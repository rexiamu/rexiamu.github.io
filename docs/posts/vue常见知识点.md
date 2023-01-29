---
title: vue常见知识点
date: 2022-03-24 19:57:15
permalink: /pages/2092ef/
categories:
  - vue
tags:
  - 
---

## 生命周期及其应用

vue2 的声明周期分为实例创建前后、实例挂载前后、数据更新前后、实例销毁前后。

| **生命周期** | **描述** |
| --- | --- |
| beforeCreate | 执行时组件实例还未创建，通常用于插件开发中执行一些初始化任务 |
| <span style="color: red">created</span> | 组件初始化完毕，各种数据可以使用，常用于异步数据获取 |
| beforeMount | 未执行渲染、更新，DOM未创建 |
| <span style="color: red">mounted</span> | 初始化结束，DOM已创建，可用于获取访问数据和DOM元素 |
| beforeUpdate | 更新前，可用于获取更新前的各种状态 |
| updated | 更新后，所有状态已是最新状态 |
| <span style="color: red">beforeDestroy</span> | 销毁前，可用于一些定时器或订阅的取消 |
| destroyed | 组件已销毁，作用同上 |

## $set

当数据改变，但视图未更新时，使用 $set 进行更新。

```javascript
this.$set(原对象, 属性值, 需要赋的值)
```

常用场景
- 当你利用索引直接设置一个数组项时
- 当你修改数组的长度时
- 当你给一个对象添加属性时
  
## $nextTick

将回调延迟到下次DOM更新循环之后执行。在修改数据之后立即使用它，然后等待DOM更新。

```javascript
const vm = new Vue({
  el: '#app',
  data: function () {
    return {
      title: '标题',
  },
  mounted() {
    this.title = '更新了';
    console.log(this.$refs.title.innerHTML); // 标题
    this.$nextTick(() => {
      console.log(this.$refs.title.innerHTML); // 更新了
    })
  }
})
```

## v-if 和 v-show

v-if 和 v-show 两个命令都用来控制元素的显示和隐藏，但是他们又有区别。

| | **v-if** | **v-show** |
| --- | --- | --- |
| 表现效果 | 值为真时显示元素 <br/> 值为假时隐藏元素 | 值为真时显示元素 <br/> 值为假时隐藏元素 |
| 实现方式 | 插入/移除 dom | 设置元素 display 的属性值 |
| 应用场景 | 状态切换较少 | 状态切换频繁 |

## computed 和 watch

computed 和 watch 可以对 vue 中的数据进行扩展使用，两者有其各自的特点和使用方式。

### computed

- computed 是计算属性，也就是计算值，它更多用于计算值的场景
- computed 具有缓存性，computed 的值在 getter 执行后是会缓存的，只有在它依赖的属性值改变之后，下一次获取 computed 的值时才会重新调用对应的 getter 来计算
- computed 适用于计算比较消耗性能的计算场景
  
```vue
<template>
  <span>商品总价为：{{goodsTotalPrice}}</span>
</template>

<script>
  export default{
    data(){
      return {
        // 商品列表
        goodsList: [
          {
            id: 1,
            name: 'apple',
            price: 7999
          },
          {
            id: 2,
            name: 'huawei',
            price: 5999
          },
          {
            id: 3,
            name: 'xiaomi',
            price: 2999
          }
          ...
        ]
      }
    },
    computed: {
      // 使用计算属性计算所有商品的总价格
      goodsTotalPrice(){
        return this.goodsList.reduce((prev, curr) => prev + curr.price, 0);
      }
    }
  }
</script>
```

### watch

- 更多的观察的作用，类似于某些数据的监听回调，用于观察 props 或者本组件的值，当数据变化时来执行回调进行后续操作
- 无缓存性，页面重新渲染时值不变化也会执行
  
```vue
<script>
  export default{
  	props: {
      showDialog: {
        type: Boolean,
        default: false
      }
    },
    watch: {
      showDialog(val){
        if(val){
          // do something
        }else{
          // do something
        }
      }
    }
  }
</script>
```
## style 加 scoped 属性的用途和原理

- **用途**：防止全局同名 CSS 污染
- **原理**：在标签加上 v-data-something 属性，再在选择器时加上对应 v-data-something，即CSS属性选择器，以此完成类似作用域的选择方式
