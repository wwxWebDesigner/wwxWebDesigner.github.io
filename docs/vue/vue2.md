# Vue2
> [官网](https://v2.cn.vuejs.org/)

## Vue2 简介

Vue2 是一个渐进式 JavaScript 前端框架，适合从小型页面增强到中大型单页应用（SPA）开发。  
“渐进式”意味着你可以按需引入能力：既可以只在页面某一块使用 Vue，也可以配合路由和状态管理构建完整应用。

## 核心特点

- **响应式数据绑定**：数据变化会自动更新视图，减少手动操作 DOM 的工作量。
- **组件化开发**：将页面拆分为可复用组件，提升代码可维护性。
- **模板语法直观**：通过 `v-if`、`v-for`、`v-bind`、`v-model` 等指令高效描述 UI。
- **生态完善**：常见配套包括 `vue-router`（路由）与 `vuex`（状态管理）。

## 适用场景

- 传统项目中逐步引入前端框架能力
- 中后台系统与管理平台
- 需要快速开发和迭代的业务页面

## 基础示例

```html
<div id="app">
  <h2>{{ title }}</h2>
  <input v-model="title" />
  <p v-if="showTip">你正在学习 Vue2</p>
</div>

<script src="https://cdn.jsdelivr.net/npm/vue@2/dist/vue.js"></script>
<script>
  new Vue({
    el: '#app',
    data: {
      title: 'Hello Vue2',
      showTip: true
    }
  })
</script>
```

上面示例演示了 Vue2 的几个基本能力：插值表达式、双向绑定和条件渲染。