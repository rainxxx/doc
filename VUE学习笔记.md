# VUE学习笔记

**VUE.JS介绍**

VUE是一款JS框架，用于解决DOM操作的问题，传统的页面内容改变是通过javascript操作DOM来实现的，比如某一个DIV的内容发生了变更，那么我们可能需要清楚DIV里的内容然后加入新的内容，如果DIV里的内容结构复杂，那浏览器重新生成DOM将会耗用一定的性能，而VUE的出现则解决了这个问题，我们将根据他提供的方法来做页面内容的动态更换，而他底层去帮助我们处理DOM，所以他的出现更多是为了实现高性能的HTML与JS的交互。

## 1. HELLO WORD

```html
<div id="app">
  {{ message }}
</div>
```

```javascript
var app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue!'
  }
})
```

### 1.1 v-指令简写

`v-` 前缀作为一种视觉提示，用来识别模板中 Vue 特定的特性。当你在使用 Vue.js 为现有标签添加动态行为 (dynamic behavior) 时，`v-` 前缀很有帮助，然而，对于一些频繁用到的指令来说，就会感到使用繁琐。同时，在构建由 Vue.js 管理所有模板的[单页面应用程序 (SPA - single page application) 时，`v-` 前缀也变得没那么重要了。因此，Vue.js 为 `v-bind` 和 `v-on` 这两个最常用的指令，提供了特定简写： 

### v-bind缩写

```
<!-- 完整语法 -->
<a v-bind:href="url">...</a>

<!-- 缩写 -->
<a :href="url">...</a>
```

### v-on缩写

```
<!-- 完整语法 -->
<a v-on:click="doSomething">...</a>

<!-- 缩写 -->
<a @click="doSomething">...</a>
```

它们看起来可能与普通的 HTML 略有不同，但 `:` 与 `@` 对于特性名来说都是合法字符，在所有支持 Vue.js 的浏览器都能被正确地解析。而且，它们不会出现在最终渲染的标记中。缩写语法是完全可选的，但随着你更深入地了解它们的作用，你会庆幸拥有它们。

## 2. 基础

#### 2.1 绑定

```php+HTML
<div id="app-2">
  <span v-bind:title="message">
    鼠标悬停几秒钟查看此处动态绑定的提示信息！
  </span>
</div>

var app2 = new Vue({
  el: '#app-2',
  data: {
    message: '页面加载于 ' + new Date().toLocaleString()
  }
})
```

 `v-bind` 特性被称为**指令**。指令带有前缀 `v-`，以表示它们是 Vue 提供的特殊特性 

#### 2.2 条件判断

```javascript
v-if=""
```

#### 2.3 循环

```html
<div id="app-4">
  <ol>
    <li v-for="todo in todos">
      {{ todo.text }}
    </li>
  </ol>
</div>

var app4 = new Vue({
  el: '#app-4',
  data: {
    todos: [
      { text: '学习 JavaScript' },
      { text: '学习 Vue' },
      { text: '整个牛项目' }
    ]
  }
})
```

在控制台里，输入 `app4.todos.push({ text: '新项目' })`，你会发现列表最后添加了一个新项目。 



#### 2.4创建监听

`v-on:click="xxx"`

```javascript
<div id="app-5">
  <p>{{ message }}</p>
  <button v-on:click="reverseMessage">逆转消息</button>
</div>

var app5 = new Vue({
  el: '#app-5',
  data: {
    message: 'Hello Vue.js!'
  },
  methods: {
    reverseMessage: function () {
      this.message = this.message.split('').reverse().join('')
    }
  }
})
```

#### 2.5 生命周期

每个 Vue 实例在被创建时都要经过一系列的初始化过程——例如，需要设置数据监听、编译模板、将实例挂载到 DOM 并在数据变化时更新 DOM 等。同时在这个过程中也会运行一些叫做**生命周期钩子**的函数，这给了用户在不同阶段添加自己的代码的机会。

比如 `created`钩子可以用来在一个实例被创建之后执行代码

```javascript
new Vue({
  data: {
    a: 1
  },
  created: function () {
    // `this` 指向 vm 实例
    console.log('a is: ' + this.a)
  }
})
// => "a is: 1"
```

也有一些其它的钩子，在实例生命周期的不同阶段被调用，如 [`mounted`](https://cn.vuejs.org/v2/api/#mounted)、[`updated`](https://cn.vuejs.org/v2/api/#updated) 和 [`destroyed`](https://cn.vuejs.org/v2/api/#destroyed)。生命周期钩子的 `this` 上下文指向调用它的 Vue 实例。 

#### 2.6 输出HTML代码

双大括号会将数据解释为普通文本，而非 HTML 代码。为了输出真正的 HTML，你需要使用 `v-html` 指令：

 ```html
<p>Using mustaches: {{ rawHtml }}</p>
<p>Using v-html directive: <span v-html="rawHtml"></span></p>
 ```

#### 2.7 使用javascript表达式

迄今为止，在我们的模板中，我们一直都只绑定简单的属性键值。但实际上，对于所有的数据绑定，Vue.js 都提供了完全的 JavaScript 表达式支持。 

```html
{{ number + 1 }}

{{ ok ? 'YES' : 'NO' }}

{{ message.split('').reverse().join('') }}

<div v-bind:id="'list-' + id"></div>
```



## 3. 常用语法

#### 3.1 阻止响应绑定

```javascript
Object.freeze(obj)
```

#### 3.2 控制插入移除

```html
<p v-if="seen">现在你看到我了</p>
```

这里，v-if 指令将根据表达式 seen 的值的真假来插入/移除 <p> 元素。

**Demo**

```html
<div id="app-1">
    <span v-if="seen">{{ message }}</span>
    <button v-on:click="disp">{{seen ? '隐藏' : '显示'}}</button>
</div>
```

```javascript
var app1 = new Vue({
    el: '#app-1',
    data: {
        message: '现在你能看到我啦',
        seen: true
    },
    methods:{
        disp: function(){
            this.seen = this.seen == true ? false : true;
        }
    }
});
```



#### 3.2 绑定回调

```javascript
// $watch 是一个实例方法
vm.$watch('a', function (newValue, oldValue) {
  // 这个回调将在 `vm.a` 改变后调用
})
```

#### 3.3 参数绑定

```html
<div id="app-2">
    <a v-bind:href="url">{{ url }}</a>
</div>
```

在这里 `href` 是参数，告知 `v-bind` 指令将该元素的 `href` 特性与表达式 `url` 的值绑定。 

```javascript
var app2 = new Vue({
    el: '#app-2',
    data: {
        url: 'https://www.baidu.com'
    }
});
```

#### 3.4 绑定时间

绑定onclick时间示例：

```html
<div id="app-3">
    <button v-on:click="doSomething">绑定onLick事件</button>
</div>
```

```javascript
var app3 = new Vue({
    el: '#app-3',
    methods: {
        doSomething: function(){
            alert('我被点击了');
        }
    }
});
```



## 4. 值得注意的

4.1 必须在创建Vue实前定义好需要响应的变量

## 5. 常用指令

## 6. 常用参数

## 7. 常用修饰符

## 8. 疑难解惑

#### 8.1 什么是NPM

https://blog.csdn.net/qq_37696120/article/details/80507178

#### 8.2 vue.js和node.js的关系

https://www.cnblogs.com/lianxuan1768/p/8365245.html

#### 8.3 webpack是什么

#### 8.4 文档中经常出现的箭头函数=>

#### 8.5 5.export default

https://www.cnblogs.com/xiaotanke/p/7448383.html

#### 8.6 vue-cli

http://www.cnblogs.com/momozjm/p/6297455.html

#### 8.7 安装nodejs

https://www.cnblogs.com/yominhi/p/7039795.html