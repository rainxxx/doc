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



#### 2.4 创建监听

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

#### 2.8 过滤器

```html
{{date | formatDate}}

var app = new Vue({
	//省略el data等内容...
    filters:{
    formateDate: function(value){
        //这里的value就是date的值
        //可以在这里面进行逻辑判断然后return最终想要显示的值
        return '';
    }
    }
})

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

#### 3.4 绑定事件

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

#### 3.5 绑定事件传递DOM参数

```html
<div id="app">
    <a href="https://www.baidu.com" @click="handleClose('不要打开',$event)">点击</a>
</div>
```

```javascript
var app = new Vue({
    el: '#app',
    methods: {
        // 这里event就是事件
        handleClose(text,event){
            event.preventDefault();
            alert(text);
        }
    }
})
```

#### 3.6 更新数组

```javascript
1. push()	//在数组末尾处新增元素
2. pop()	//删除数组中最后一个值,并且将最后一个值作为返回结果返回,如果数组为null将返回undefied
3. shift()	//删除数组中第一个值，并且将删除的值返回
4. unshift()//在数组首位添加元素,即array[0]添加一个元素
5. splice()	//插入、删除或替换数组的元素。这种方法会改变原始数组
6. sort()	//排序,默认升序排序(1,2,3)
7. reverse()//反转数组元素,比如array == ['A','B','C'] array.reverse() == ['C','B','A']
```

##### 3.6.1 splice()方法详解

```javascript
/* splice使用示例 */
var texts = [1,2,3];

/*替换指定下标内容*/
texts.splice(2,1,'A'); /* 等价于 */  text[2] = 'A';
//输出的结果都为：texts = [1,2,A]

/*删除指定下标内容*/
texts.splice(2,1);
//输出结果为：texts = [1,2]; 索引为2的被删除了

/*在指定下标添加内容*/
texts.splice(2,0,'spliceAdd');
//输出结果为：texts = [1,2,'spliceAdd',3],在索引2的位置增加了一个字符串'spliceAdd'

/*在指定下标添加多个内容*/
texts.splice(2,0,'spliceAdd1','spliceAdd2');
//输出结果为：texts = [1,2,'spliceAdd1','spliceAdd2']
```

##### 3.6.2 sort()方法详解

```javascript
/* splice使用示例 */
var texts = [2,1,3];

// 对数组升序
this.texts.sort(function(a,b){
    return a<b;
});
//最终结果为：var texts = [3,2,1]

// 对数组降序
this.texts.sort();
//最终结果为：var texts = [1,2,3]
```



## 4. 值得注意的

#### 4.1 响应变量定义

必须在创建Vue实前定义好需要响应的变量

#### 4.2 组件Prop

组件参数时v-bind:属性代表动态绑定如果是字符串不要加v-bind:

```html
var MyTitle = {
    props:['title'],
    template: '<span >标题是：{{title}} </span>'
}

<!-- 动态绑定的值可以是Number，Boolean，Methods中的一个方法名,但不能是一个字符串 -->
<my-title v-bind:title="1"></my-title>
<my-title v-bind:title="true"></my-title>
<my-title v-bind:title="methodName"></my-title>

<!-- 不加v-bind那么里面的值会被解析为字符串 -->
<my-title title="1"></my-title>
```

#### 4.3 不要用关键字命名

不能使用常用关键字来给组件命名，比如main否则会报错

#### 4.4 method中调用其它method

```javascript
var methods = {
    handleClose: function(){
        // 可以通过this来调用其他method
        this.close();
    },
    close: function(){
        this.show = false;
    }
}
```





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