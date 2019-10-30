## computed and watch
computed和methods
> 我们可以将同一函数定义为一个方法而不是一个计算属性。两种方式的最终结果确实是完全相同的。然而，不同的是计算属性是基于它们的依赖进行缓存的。只在相关依赖发生改变时它们才会重新求值。这就意味着只要 message 还没有发生改变，多次访问 reversedMessage 计算属性会立即返回之前的计算结果，而不必再次执行函数。

总而言之：  
 　　　　　methods方法每次调用都要重新计算；  
      　　　　computed基于相关依赖是否发生改变进行计算
          
 -----
 ## conditions and loops
 ### v-if  
 It’s easy to toggle the presence of an element, too:  
 ```
 <div id="app-3">
  <span v-if="seen">Now you see me</span>
</div>
```
```
var app3 = new Vue({
  el: '#app-3',
  data: {
    seen: true
  }
})
```
### v-for
There are quite a few other directives, each with its own special functionality. For example, the v-for directive can be used for displaying a list of items using the data from an Array:
```
<div id="app-4">
  <ol>
    <li v-for="todo in todos">
      {{ todo.text }}
    </li>
  </ol>
</div>
```
```
var app4 = new Vue({
  el: '#app-4',
  data: {
    todos: [
      { text: 'Learn JavaScript' },
      { text: 'Learn Vue' },
      { text: 'Build something awesome' }
    ]
  }
})
```
## v-show
Another option for conditionally displaying an element is the v-show directive. The usage is largely the same:
```
<h1 v-show="ok">Hello!</h1>
```
The difference is that an element with v-show will always be rendered and remain in the DOM; v-show only toggles the display CSS property of the element.
>Note that v-show doesn’t support the <\template> element, nor does it work with v-else.  

## v-if vs v-show
**v-if is “real” conditional rendering because it ensures that event listeners and child components inside the conditional block are properly destroyed and re-created during toggles.  **

**v-if is also lazy: if the condition is false on initial render, it will not do anything - the conditional block won’t be rendered until the condition becomes true for the first time. ** 
**In comparison, v-show is much simpler - the element is always rendered regardless of initial condition, with CSS-based toggling.**

**Generally speaking, v-if has higher toggle costs while v-show has higher initial render costs. So prefer v-show if you need to toggle something very often, and prefer v-if if the condition is unlikely to change at runtime.**

##  v-on
To let users interact with your app, we can use the v-on directive to attach event listeners that invoke methods on our Vue instances:
```
<div id="app-5">
  <p>{{ message }}</p>
  <button v-on:click="reverseMessage">Reverse Message</button>
</div>
```
```
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
## components
![](https://vuejs.bootcss.com/images/components.png)
```
Vue.component('button-counter', {
  data: function () {
    return {
      count: 0
    }
  },
  template: '<button v-on:click="count++">You clicked me {{ count }} times.</button>'
})
```
```
<div id="components-demo">
  <button-counter></button-counter>
</div>
```
```
new Vue({ el: '#components-demo' })
```
### 组件的data必须是一个函数
当我们定义这个 <button-counter> 组件时，你可能会发现它的 data 并不是像这样直接提供一个对象：
```
data: {
  count: 0
}
```
取而代之的是，一个组件的 data 选项必须是一个函数，因此每个实例可以维护一份被返回对象的独立的拷贝：
```
data: function () {
  return {
    count: 0
  }
}
```
为了能在模板中使用，这些组件必须先注册以便 Vue 能够识别。这里有两种组件的注册类型：全局注册和局部注册。至此，我们的组件都只是通过 Vue.component 全局注册的：
```
Vue.component('my-component-name', {
  // ... options ...
})
```
全局注册的组件可以用在其被注册之后的任何 (通过 new Vue) 新创建的 Vue 根实例，也包括其组件树中的所有子组件的模板中。
### prop
#### 通过prop向子组件传递数据
早些时候，我们提到了创建一个博文组件的事情。问题是如果你不能向这个组件传递某一篇博文的标题或内容之类的我们想展示的数据的话，它是没有办法使用的。这也正是 prop 的由来。

Prop 是你可以在组件上注册的一些自定义特性。当一个值传递给一个 prop 特性的时候，它就变成了那个组件实例的一个属性。为了给博文组件传递一个标题，我们可以用一个 props 选项将其包含在该组件可接受的 prop 列表中：
```
Vue.component('blog-post', {
  props: ['title'],
  template: '<h3>{{ title }}</h3>'
})
```
一个组件默认可以拥有任意数量的 prop，任何值都可以传递给任何 prop。在上述模板中，你会发现我们能够在组件实例中访问这个值，就像访问 data 中的值一样。

一个 prop 被注册之后，你就可以像这样把数据作为一个自定义特性传递进来：
```
<blog-post title="My journey with Vue"></blog-post>
<blog-post title="Blogging with Vue"></blog-post>
<blog-post title="Why Vue is so fun"></blog-post>
```
>My journey with Vue  
>Blogging with Vue  
>Why Vue is so fun
然而在一个典型的应用中，你可能在 data 里有一个博文的数组：
```
new Vue({
  el: '#blog-post-demo',
  data: {
    posts: [
      { id: 1, title: 'My journey with Vue' },
      { id: 2, title: 'Blogging with Vue' },
      { id: 3, title: 'Why Vue is so fun' }
    ]
  }
})
```
并想要为每篇博文渲染一个组件：
```
<blog-post
  v-for="post in posts"
  v-bind:key="post.id"
  v-bind:title="post.title"
></blog-post>
```  

如上所示，你会发现我们可以使用 v-bind 来动态传递 prop。这在你一开始不清楚要渲染的具体内容，比如从一个 API 获取博文列表的时候，是非常有用的。
***
## component
```
Vue.component('todo-item',//组件的名称
//对象
{
  props: ['todo'],//为接收到的数据命名
  template: '<li>{{ todo.text }}</li>'//模板
})

var app7 = new Vue({
  el: '#app-7',
  data: {
    groceryList: [
      { id: 0, text: '蔬菜' },
      { id: 1, text: '奶酪' },
      { id: 2, text: '随便其它什么人吃的东西' }
    ]
  }
})

<div id="app-7">//绑定vue对象
  <ol>
    <todo-item
      v-for="item in groceryList"//绑定操作
      v-bind:todo="item"//绑定操作
      v-bind:key="item.id"//绑定操作
    ></todo-item>
  </ol>
</div>
```
---
### 组件和Vue函数对象的联系
** vue组件和vue函数对象之间的联系就是组件模板中的绑定操作**
---
### 组件和Vue函数对象的顺序
**组件必须放在Vue函数对象的上面**
---
## Data and Methods
When a Vue instance is created, it adds all the **properties found in its data object** to Vue’s **reactivity system**. When the values of those properties change, the view will “react”, updating to match the new values.
```
// Our data object
var data = { a: 1 }

// The object is added to a Vue instance
var vm = new Vue({
  data: data
})

// Getting the property on the instance
// returns the one from the original data
vm.a == data.a // => true

// Setting the property on the instance
// also affects the original data
vm.a = 2
data.a // => 2

// ... and vice-versa
data.a = 3
vm.a // => 3
```
---
In addition to data properties, Vue instances expose a number of useful **instance properties and methods**. These are **prefixed with $** to differentiate them from user-defined properties. For example:
```
var data = { a: 1 }
var vm = new Vue({
  el: '#example',
  data: data
})

vm.$data === data // => true
vm.$el === document.getElementById('example') // => true

// $watch is an instance method
vm.$watch('a', function (newValue, oldValue) {
  // This callback will be called when `vm.a` changes
})
```
## Instance Lifecycle Hooks
Each Vue instance goes through a series of initialization steps when it’s created - for example, it needs to** set up data observation, compile the template, mount the instance to the DOM, and update the DOM when data changes.** Along the way, it also **runs functions called lifecycle hooks,** giving users the opportunity to add their own code at specific stages.
For example, the created hook can be used to run code after an instance is created:
```
new Vue({
  data: {
    a: 1
  },
  created: function () {
    // `this` points to the vm instance
    console.log('a is: ' + this.a)
  }
})
// => "a is: 1"
```
There are also other hooks which will be called at different stages of the instance’s lifecycle, such as **mounted, updated, and destroyed**. All lifecycle hooks are called with their **this** context pointing to the Vue instance invoking it.

**Don't Use => Function***
Don’t use arrow functions on an options property or callback, such as created: () => console.log(this.a) or vm.$watch('a', newValue => this.myMethod()). Since an arrow function doesn’t have a this, this will be treated as any other variable and lexically looked up through parent scopes until found, often resulting in errors such as Uncaught TypeError: Cannot read property of undefined or Uncaught TypeError: this.myMethod is not a function.
***
## Lifecycle Diagram
Below is a diagram for the instance lifecycle. You don’t need to fully understand everything going on right now, but as you learn and build more, it will be a useful reference.
![](https://vuejs.org/images/lifecycle.png)
***
## Template Syntax
Vue.js uses an **HTML-based template syntax** that allows you to declaratively **bind the rendered DOM to the underlying Vue instance’s data.** All Vue.js templates are valid HTML that can be parsed by spec-compliant browsers and HTML parsers.

Under the hood, Vue compiles the templates into Virtual DOM render functions. Combined with the reactivity system, Vue is able to intelligently figure out the minimal number of components to re-render and apply the minimal amount of DOM manipulations when the app state changes.
***

## Interpolations
### Text
The most basic form of data binding is text interpolation using the “Mustache” syntax (double curly braces):
```
<span>Message: {{ msg }}</span>
```
The mustache tag will be replaced with the value of the msg property on the corresponding data object. It will also be updated whenever the data object’s msg property changes.

You can also perform one-time interpolations that do not update on data change by using the** v-once **directive, but keep in mind this will also affect any other bindings on the same node:
```
<span v-once>This will never change: {{ msg }}</span>
```
### Raw Html
The double mustaches interprets the data as plain text, not HTML. In order to output real HTML, you will need to use the v-html directive:
```
<p>Using mustaches: {{ rawHtml }}</p>
<p>Using v-html directive: <span v-html="rawHtml"></span></p>
```
### Attributes
Mustaches **cannot** be used inside HTML attributes. Instead, **use a v-bind directive:**
```
<div v-bind:id="dynamicId"></div>
```
***
## Using JavaScript Expressions
So far we’ve only been binding to simple property keys in our templates. But Vue.js actually supports the full power of JavaScript expressions inside all data bindings:
```
{{ number + 1 }}

{{ ok ? 'YES' : 'NO' }}

{{ message.split('').reverse().join('') }}

<div v-bind:id="'list-' + id"></div>
```
These expressions will be evaluated as JavaScript in the data scope of the owner Vue instance. One restriction is that each binding can only contain one **single expression,** so the following will NOT work:
```
<!-- this is a statement, not an expression: -->
{{ var a = 1 }}

<!-- flow control won't work either, use ternary expressions -->
{{ if (ok) { return message } }}
```
***Template expressions are sandboxed and only have access to a whitelist of globals such as Math and Date. You should not attempt to access user defined globals in template expressions.***
***
##  Directives
Directives are special attributes with the v- prefix. Directive attribute values are expected to be a **single JavaScript expression** (with the **exception of v-for**, which will be discussed later). A directive’s job is to reactively apply side effects to the DOM when the value of its expression changes. Let’s review the example we saw in the introduction:
***
## Arguments
Some directives can take an “argument”, denoted by a colon after the directive name. For example, the v-bind directive is used to reactively update an HTML attribute:
```
<a v-bind:href="url"> ... </a>
```
Here href is the argument, which tells the v-bind directive to bind the element’s href attribute to the value of the expression url.

Another example is the v-on directive, which listens to DOM events:
***

## v-model vs v-bind
From here - Remember:
```
<input v-model="searchText">
```
does the same thing as:
```
<input
  v-bind:value="searchText"
  v-on:input="searchText = $event.target.value"
```
When used on a component, v-model instead does this:

```
<custom-input
  v-bind:value="searchText"
  v-on:input="searchText = $event"
></custom-input>
```
So v-model is a two-way binding for form inputs. It combines v-bind, which brings a js value into the markup, and v-on:input to update the js value.
***
## Dynamic Arguments
>New in 2.6.0+

Starting in version 2.6.0, it is also possible to use a JavaScript expression in a directive argument by wrapping it with square brackets:
```
<!--
Note that there are some constraints to the argument expression, as explained
in the "Dynamic Argument Expression Constraints" section below.
-->
<a v-bind:[attributeName]="url"> ... </a>
```
Here attributeName will be dynamically evaluated as a JavaScript expression, and its evaluated value will be used as the final value for the argument. For example, if your Vue instance has a data property, attributeName, whose value is "href", then this binding will be equivalent to v-bind:href.

Similarly, you can use dynamic arguments to bind a handler to a dynamic event name:
```
<a v-on:[eventName]="doSomething"> ... </a>
```
In this example, when eventName‘s value is "focus", v-on:[eventName] will be equivalent to v-on:focus.

**Dynamic Argument Value Constraints**

Dynamic arguments are expected to evaluate to a string, with the exception of null. The special value null can be used to explicitly remove the binding. Any other non-string value will trigger a warning.

**Dynamic Argument Expression Constraints**
Dynamic argument expressions have some syntax constraints because certain characters, such as spaces and quotes, are invalid inside HTML attribute names. For example, the following is invalid:
```
<!-- This will trigger a compiler warning. -->
<a v-bind:['foo' + bar]="value"> ... </a>
```
The workaround is to either use expressions without spaces or quotes, or replace the complex expression with a computed property.

When using in-DOM templates (templates directly written in an HTML file), you should also avoid naming keys with uppercase characters, as browsers will coerce attribute names into lowercase:

When using in-DOM templates (templates directly written in an HTML file), you should also avoid naming keys with uppercase characters, as browsers will coerce attribute names into lowercase:
```
<!--
This will be converted to v-bind:[someattr] in in-DOM templates.
Unless you have a "someattr" property in your instance, your code won't work.
-->
<a v-bind:[someAttr]="value"> ... </a>
```
## Modifiers
Modifiers are special postfixes denoted by a dot, which indicate that a directive should be bound in some special way. For example, the .prevent modifier tells the v-on directive to call event.preventDefault() on the triggered event:
```
<form v-on:submit.prevent="onSubmit"> ... </form>
```
You’ll see other examples of modifiers later, for v-on and for v-model, when we explore those features.
## Shorthands
The v- prefix serves as a visual cue for identifying Vue-specific attributes in your templates. This is useful when you are using Vue.js to apply dynamic behavior to some existing markup, but can feel verbose for some frequently used directives. At the same time, the need for the v- prefix becomes less important when you are building a SPA, where Vue manages every template. Therefore, Vue provides special shorthands for two of the most often used directives, v-bind and v-on:

## v-bind Shorthand
```
<!-- full syntax -->
<a v-bind:href="url"> ... </a>

<!-- shorthand -->
<a :href="url"> ... </a>

<!-- shorthand with dynamic argument (2.6.0+) -->
<a :[key]="url"> ... </a>
```
## v-on Shorthand
```
<!-- full syntax -->
<a v-on:click="doSomething"> ... </a>

<!-- shorthand -->
<a @click="doSomething"> ... </a>

<!-- shorthand with dynamic argument (2.6.0+) -->
<a @[event]="doSomething"> ... </a>
```
They may look a bit different from normal HTML, but : and @ are valid characters for attribute names and all Vue-supported browsers can parse it correctly. In addition, they do not appear in the final rendered markup. The shorthand syntax is totally optional, but you will likely appreciate it when you learn more about its usage later.
# Computed Properties and Watchers
## Computed Caching vs Methods
**In cases where you do not want caching, use a method instead.**
## Computed vs Watched Property

## computed vs watch
### computed
A computed property sample:
```
computed: {
   val () {
     return this.someDataProperty * someOtherVariable
   }
}
```
1. It creates a property named val for the component .
2. It has a dependency tree which consists of reactive properties (data properties, other computed properties) in this case : this.someDataProperty, which means the moment the dependencies change, the computed property will be recalculated.
3. Although debated, can't have arguments passed to it. So something like
```
computed: {
  val (flag) {
    return (flag === 1) 
      ? this.someDataProperty * someOtherVariable 
      : this.someDataProperty * 5
    }
}
```
can't be done

4. To summarise：

  1. computed创造一个新的property
  2. computed的依赖树由响应式属性组成（包括data的properties和其他的computed的properties），当依赖树发生变化时，computed重新计算。  
  ps：**computed** properties are cached based on their dependencis.A computed property will only re-evaluate when some of it's reactive dependencies have changed.
  If it's dependencies not changed,even a re-render happens,computed will not re-evaluate.   
  In comparison,a **method** invocation will always run the function whenever a re-render happens.
  3. computed properties can't have arguments passed to it.  
###  watcher
A watcher sample:
```
watch: {
   val (n, o) {
     console.log(n, o)
   }
}
```


1. It does not create any new property, but it watches the changes over a reactive property.
2. Watches only one specific property, unlike computed where any dependent property change can cause recalculation.
3. Has arguments of new and old value.

## computed vs methods
1. 每当computed的依赖树中的**数据**发生**改变**时，**computed**中的相关**property**就会被**自动**调用。
2. 当且仅当methods中的函数在DOM中**被显示触发**时，**methods**的相关函数才会被**调用**。
   
template中绑定函数时可以传入参数，例如：
`````
<li\  v-for='todo in todos'>
 <button class="destroy" @click="removeTodo(todo)"></button>
</li>  
`````
# **！！只有被绑定到DOM中的computed的property才会被自动执行！！**

# computed 与数据的关系
## 场景设置
现在 页面A 引用了 computed B，computed B 依赖了 data C

像是这样，A->B->C 的依赖顺序
其实真正的流程是，data C 开始变化后.......1通知 computed B watcher 更新，其实只会重置 脏数据标志位 dirty =true，不会计算值2通知 页面 A watcher 进行更新渲染，进而重新读取 computed B ，然后 computed B 开始重新计算
![](https://pic1.zhimg.com/v2-9b1b7fd20fbf6a057bc54ffffa47392c_b.jpg)
## ?为什么 data C 能通知 页面 A
data C 的依赖收集器会同时收集到 computed B 和 页面 A 的 watcher
## ?为什么 data C 能收集到 页面A 的watcher
就是 Vue 设计的巧妙之处了，也就是我开始讲的，computed 其实是一个 月老在 页面 A 在读取 computed B 的时候，趁机把 页面A 介绍给 data C ，于是  页面A watcher 和 data C 间接牵在了一起，于是 data C 就会收集到 页面A watcher
## ?所以computed 如何更新
被依赖通知更新后，重置 脏数据标志位 ，页面读取 computed 时再更新值

# 可以在函数中声明并初始化一个vue属性
比如：
`````
var app = new Vue({
  // app initial state
  data: {
    todos: todoStorage.fetch(),
    newTodo: '',
    editedTodo: null,
    visibility: 'all'
  },
methods: {
 editTodo: function (todo) {
      this.beforeEditCache = todo.title//这一行声明了一个vue  property
      this.editedTodo = todo
    },
 }
}）
 ```











