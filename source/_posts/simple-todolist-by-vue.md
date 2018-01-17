---
title: simple todolist by vue
date: 2018-01-17 23:22:02
tags: vue
---
## Vue实践 【二】todolist开发

### 项目目录结构
todolist-vue
```
/ 
  -build  // webpack 打包配置文件
  -config // 项目配置文件
  -node_modules // packages包
  -src  // 源代码文件目录
  -static 
  -test // 测试单元的目录
  -package.json // 项目相关描述文件

```

程序代码编码的目录都是在```src```目录下的,src目录下文件：
```
src
  -assets // 静态文件存储目录
  -components// 页面组件存放目录
  -router // 路由控制目录
  -App.vue // vue页面，在main.js中作为组件被引用
  -main.js // 入口，在build/webpack.base.conf.js 中定义

```

编写完成后，可以通过```npm run dev``` 实时看到修改的效果，通过 ```npm run build``` 来运行webpack打包源码生成静态文件供正式环境访问。

### Vue基本语法
#### vue基本结构
```
new Vue({
  name: '',
  data () {return {}}, // 属性
  props: [], // 组件传递的值
  components: [],// 使用组件
  created (){}, // 创建
  mounted (){}, // 挂载
  computed: {}, // 计算属性
  watch: {}, // 侦听器
  methods: {} // 方法
})
```

#### vue常用命令
##### v-model
```
# 双向数据绑定
<input type="text" v-model="name" />

<select v-model="selected">
    <option value="1"></option>
</select>
```

##### v-is v-else
```
# 条件判断
<div v-if="is_show">
    <span>show</span>
</div>
<div v-else>
    <span>hide</span>
</div>
```
##### v-bind (简写为 :)
```
# 绑定属性
<a v-bind:href="item_url">链接</a>

<img :src="item_photo">图片</img>

<li :class="[state == 'effective' ? 'active' : '']">
```
##### v-on (简写为 @)
```
<a v-on:click="addItem">链接</a>

<a @click="addItem">链接</a>
```

#### vue生命周期
Vue的生命周期
##### beforeCreate 
在实例初始化之后，数据观测(data observer) 和 event/watcher 事件配置之前被调用。
##### created  
实例已经创建完成之后被调用。在这一步，实例已完成以下的配置：数据观测(data observer)，属性和方法的运算， watch/event 事件回调。然而，挂载阶段还没开始，$el 属性目前不可见。
##### beforeMount
在挂载开始之前被调用：相关的 render 函数首次被调用。
##### mounted
el 被新创建的 vm.$el 替换，并挂载到实例上去之后调用该钩子。如果 root 实例挂载了一个文档内元素，当 mounted 被调用时 vm.$el 也在文档内。
##### beforeUpdate
数据更新时调用，发生在虚拟 DOM 重新渲染和打补丁之前。
你可以在这个钩子中进一步地更改状态，这不会触发附加的重渲染过程。
该钩子在服务器端渲染期间不被调用。
##### updated
由于数据更改导致的虚拟 DOM 重新渲染和打补丁，在这之后会调用该钩子。
当这个钩子被调用时，组件 DOM 已经更新，所以你现在可以执行依赖于 DOM 的操作。然而在大多数情况下，你应该避免在此期间更改状态，因为这可能会导致更新无限循环。
##### activated
keep-alive 组件激活时调用。
该钩子在服务器端渲染期间不被调用。
##### deactivated
keep-alive 组件停用时调用。
该钩子在服务器端渲染期间不被调用。
##### beforeDestroy
实例销毁之前调用。在这一步，实例仍然完全可用。
该钩子在服务器端渲染期间不被调用。
##### destroyed
Vue 实例销毁后调用。调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。
该钩子在服务器端渲染期间不被调用。


### Todolist实现
#### vue devtools推荐
chrome浏览器插件 vue-devtools。可以查看当前运行状态和组件内的值。

##### todolist实现

1.新建 todolist.vue,初步编写如下
```
<template>
  <div>todolist</div>
</template>

<script>
export default {
  name: 'todolist'
}
</script>

<style scoped>

</style>
```

2.在router/index.js修改如下,增加todolist的页面路由
```
import Vue from 'vue'
import Router from 'vue-router'
import HelloWorld from '@/components/HelloWorld'
import Todolist from '@/components/Todolist'

Vue.use(Router)

export default new Router({
  routes: [
    {
      path: '/',
      name: 'HelloWorld',
      component: HelloWorld
    },
    {
      path: '/todolist',
      name: 'todolist',
      component: Todolist
    }
  ]
})

```
3.浏览器中输入localhost:8080/#/todolist, 能看到页面上会出现todolist，但是也会出现一个vue的logo，去除这logo只需要在app.vue中去除```id='app'```下的image标签即可
4.完善todolist页面功能
```
<template>
  <div>
    <div>
      <input type="text" name="new-todo" class='add-new-todo' v-model='name'>
      <a href='javascript:void(0)' @click='add' class='btn-add'>add</a>
    </div>
    <ul>
      <li v-for='(item, index) in lists' :key='index'>
        <span class='list-info'>{{item.name}}</span>
        <a href='javascript:void(0)' class='btn-del' @click='del(index)'>del</a>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'todolist',
  data () {
    return {
      name: '',
      lists: []
    }
  },
  created () {
    document.title = 'todolist'
  },
  methods: {
    add () {
      this.lists.push({
        name: this.name,
        state: 'todo'
      })
      this.name = ''
    },
    del (index) {
      this.lists.splice(index, 1)
    }
  }
}
</script>

<style scoped>
.add-new-todo {
  width:90%;
  height: 40px;
}

.btn-add {
  text-decoration: none;
  padding: 10px;
  background: #eee;
  border-radius: 5px;
}

ul {
  list-style-type: square;
}
ul li {
  height: 50px;
  line-height: 50px;
}

ul li .list-info{
  display: inline-block;
  width: 90%;
  text-align: left;
}

.btn-del {
  text-decoration: none;
  padding: 10px;
  background: orange;
  border-radius: 5px;
}
</style>
```

至此，一个简单的todolist就完成了。


