# vue

## 生命周期

### 创建阶段

- 初始化事件和生命周期
- beforeCreate
- 数据观测、属性、侦听器配置等
- created
- 模板编译到render
- beforeMount
- render
- mounted

	- 异步请求、操作DOM、定时器等

### 更新阶段

- 依赖数据改变或$forceUpdate强制刷新
- beforeUpdate

	- 移除已添加的事件监听器等 万万不可更改依赖数据

- render
- updated

	- 操作DOM添加事件监听器等 万万不可更改依赖数据

### 销毁阶段

- beforeDestroy

	- 移除已经添加的事件监听器、计时器等

- destroyed

## 函数式组件

### functional: true 无状态、无实例、没有this上下文、无生命周期

## vue-router

### 使用流程

- 使用插件

	- Vue.use(VueRouter)

- 生成路由实例

	- 路由记录routes
	- 路由实例 new VueRoute({ routes })

- 挂载至Vue实例

	- new Vue({ router })

### 特性

- 动态路由匹配
- 嵌套路由
- 嵌套视图
- 导航守卫
- 过渡动效
- 路由懒加载

## 分支主题 2

## 分支主题 3

*XMind: ZEN - Trial Version*
