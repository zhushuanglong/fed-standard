<!--
 * @Author: yunfan
 * @Date: 2019-11-21 15:23:52
 * @LastEditTime: 2019-11-22 09:52:57
 * @LastEditors: Please set LastEditors
 * @Description: 参考https://juejin.im/post/5ada9b586fb9a07aaf34c746
 * @FilePath: /Tsign/fed-standard/docs/vue.md
 -->
# Vue 编码规范

Vue前端开发规范，基于Vue官方风格指南整理


## 组件数据
组件的 data 必须是一个函数。

当在组件中使用 data 属性的时候 (除了 new Vue 外的任何地方)，它的值必须是返回一个对象的函数。

```js
// bad 
export default {
  name: 'Todo',
  // ...
}
// good
// In a .vue file
export default {
  data () {
    return {
      foo: 'bar'
    }
  }
}
// 在一个 Vue 的根实例上直接使用对象是可以的，
// 因为只存在一个这样的实例。
new Vue({
  data: {
    foo: 'bar'
  }
})
```
## Prop定义
Prop 定义应该尽量详细。
在你提交的代码中，prop 的定义应该尽量详细，至少需要指定其类型。


```js
// bad 
props: ['status']

// good
props: {
  status: {
    type: String,
    required: true,
    ...
  }
}
```

## 为v-for设置键值
总是用 key 配合 v-for。
在组件上_总是_必须用 key 配合 v-for，以便维护内部组件及其子树的状态。甚至在元素上维护可预测的行为，比如动画中的对象固化 (object constancy)，也是一种好的做法。


// bad 
<ul>
  <li v-for="todo in todos">
    {{ todo.text }}
  </li>
</ul>

// good
<ul>
  <li v-for="todo in todos" :key="todo.id">
    {{ todo.text }}
  </li>
</ul>


## 避免 v-if 和 v-for 用在一起
永远不要把 v-if 和 v-for 同时用在同一个元素上。
一般我们在两种常见的情况下会倾向于这样做：

为了过滤一个列表中的项目 (比如 v-for="user in users" v-if="user.isActive")。在这种情形下，请将 users 替换为一个计算属性 (比如 activeUsers)，让其返回过滤后的列表。
为了避免渲染本应该被隐藏的列表 (比如 v-for="user in users" v-if="shouldShowUsers")。这种情形下，请将 v-if 移动至容器元素上 (比如 ul, ol)。


// bad 
<ul>
  <li
    v-for="user in users"
    v-if="shouldShowUsers"
    :key="user.id"
  >
    {{ user.name }}
  </li>
</ul>

// good
<ul v-if="shouldShowUsers">
  <li
    v-for="user in users"
    :key="user.id"
  >
    {{ user.name }}
  </li>
</ul>


## 为组件样式设置作用域
对于应用来说，顶级 App 组件和布局组件中的样式可以是全局的，但是其它所有组件都应该是有作用域的。
这条规则只和单文件组件有关。你不一定要使用 scoped 特性。设置作用域也可以通过 CSS Modules，那是一个基于 class 的类似 BEM 的策略，当然你也可以使用其它的库或约定。

不管怎样，对于组件库，我们应该更倾向于选用基于 class 的策略而不是 scoped 特性。
这让覆写内部样式更容易：使用了常人可理解的 class 名称且没有太高的选择器优先级，而且不太会导致冲突。


## 单文件组件文件的大小写
单文件组件的文件名应该要么始终是单词大写开头 (PascalCase)

```js
// bad
components/
|- myComponent.vue
|- mycomponent.vue

// good
components/
|- MyComponent.vue

```

## 基础组件名
应用特定样式和约定的基础组件 (也就是展示类的、无逻辑的或无状态的组件) 应该全部以一个特定的前缀开头，比如 Base、App 或 V。

```js
// bad
components/
|- MyButton.vue

// good
components/
|- BaseButton.vue
```

## 单例组件名
只应该拥有单个活跃实例的组件应该以 The 前缀命名，以示其唯一性。
这不意味着组件只可用于一个单页面，而是每个页面只使用一次。这些组件永远不接受任何 prop，因为它们是为你的应用定制的，而不是它们在你的应用中的上下文。如果你发现有必要添加 prop，那就表明这实际上是一个可复用的组件，只是目前在每个页面里只使用一次。

```js
// bad
components/
|- Heading.vue

// good
components/
|- TheHeading.vue
```

## 紧密耦合的组件名
和父组件紧密耦合的子组件应该以父组件名作为前缀命名。
如果一个组件只在某个父组件的场景下有意义，这层关系应该体现在其名字上。因为编辑器通常会按字母顺序组织文件，所以这样做可以把相关联的文件排在一起。

```js
// bad
components/
|- SearchSidebar.vue
|- NavigationForSearchSidebar.vue

// good
components/
|- SearchSidebar.vue
|- SearchSidebarNavigation.vue
```


## 组件名中的单词顺序
组件名应该以高级别的 (通常是一般化描述的) 单词开头，以描述性的修饰词结尾。

```js
// bad
components/
|- ClearSearchButton.vue
|- ExcludeFromSearchInput.vue
|- LaunchOnStartupCheckbox.vue
|- RunSearchButton.vue
|- SearchInput.vue
|- TermsCheckbox.vue
// good
components/
|- SearchButtonClear.vue
|- SearchButtonRun.vue
|- SearchInputQuery.vue
|- SearchInputExcludeGlob.vue
|- SettingsCheckboxTerms.vue
|- SettingsCheckboxLaunchOnStartup.vue
```

## 模板中的组件名大小写
总是 PascalCase 的

```js
// bad
<mycomponent/>

// good
<MyComponent/>
```

## 完整单词的组件名
组件名应该倾向于完整单词而不是缩写。

```
// bad
components/
|- SdSettings.vue
|- UProfOpts.vue

// good
components/
|- StudentDashboardSettings.vue
|- UserProfileOptions.vue
```

## 多个特性的元素
多个特性的元素应该分多行撰写，每个特性一行。
```
// bad
<img src="https://vuejs.org/images/logo.png" alt="Vue Logo">

// good
<img
  src="https://vuejs.org/images/logo.png"
  alt="Vue Logo"
>
```

## 模板中简单的表达式
组件模板应该只包含简单的表达式，复杂的表达式则应该重构为计算属性或方法。
复杂表达式会让你的模板变得不那么声明式。我们应该尽量描述应该出现的是什么，而非如何计算那个值。而且计算属性和方法使得代码可以重用。


## 带引号的特性值
非空 HTML 特性值应该始终带引号 (单引号或双引号，选你 JS 里不用的那个)。
在 HTML 中不带空格的特性值是可以没有引号的，但这样做常常导致带空格的特征值被回避，导致其可读性变差。

```
// bad
<AppSidebar :style={width:sidebarWidth+'px'}>

// good
<AppSidebar :style="{ width: sidebarWidth + 'px' }">
```

## 指令缩写
都用指令缩写 (用 : 表示 v-bind: 和用 @ 表示 v-on:)

```
// bad
<input
  v-bind:value="newTodoText"
  :placeholder="newTodoInstructions"
>

// good
<input
  @input="onInput"
  @focus="onFocus"
>
```

## 单文件组件的顶级元素的顺序
单文件组件应该总是让<script>、<template> 和 <style> 标签的顺序保持一致。且 <style> 要放在最后，因为另外两个标签至少要有一个。

```
<!-- ComponentA.vue -->
<template>...</template>
<script>/* ... */</script>
<style>/* ... */</style>
```

## 慎使用 (有潜在危险的模式)
1. 没有在 v-if/v-if-else/v-else 中使用 key
如果一组 v-if + v-else 的元素类型相同，最好使用 key (比如两个 <div> 元素)。

```
// bad
<div v-if="error">
  错误：{{ error }}
</div>
<div v-else>
  {{ results }}
</div>

// good
<div
  v-if="error"
  key="search-status"
>
  错误：{{ error }}
</div>
<div
  v-else
  key="search-results"
>
  {{ results }}
</div>
```

## scoped 中的元素选择器
元素选择器应该避免在 scoped 中出现。
在 scoped 样式中，类选择器比元素选择器更好，因为大量使用元素选择器是很慢的。

```
// bad
<template>
  <button>X</button>
</template>

<style scoped>
button {
  background-color: red;
}
</style>

// good
<template>
  <button class="btn btn-close">X</button>
</template>

<style scoped>
.btn-close {
  background-color: red;
}
</style>
```

## 全局状态管理
应该优先通过 Vuex 管理全局状态，而不是通过 this.$root 或一个全局事件总线。


**[⬆ 回到顶部](#内容目录)**