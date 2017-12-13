# Vue模板，条件和列表渲染

## 模版

Vue使用了基于HTML的模版语法，允许开发者声明地的将DOM绑定到Vue实例的底层数据上。所有Vue的模版都是合法的HTML，所以可以被遵循规范的浏览器和HTML解析器直接解析。

底层上，Vue将模版编译成DOM渲染函数。结合响应系统，在应用状态改变时，Vue能够花费最小的代价重新计算渲染组件到DOM上。

你也可以直接写渲染行数或者使用JSX来定义模版。

### 插入一个值

数据绑定最常见的形式就是使用"Mustache"语法(\{\{\}\})。

```html
<span>Hello，{{ name }}</span>
```

上面例子中`{{ name }}`将被替换成 `name` 属性的值，而且无论何时，绑定的数据 `name` 属性值发生了改变时，插值处的内容都会更新

Mustache语法会被解释为普通文本，有时我们可能需要插入一段HTML代码，这是我们需要`v-html`指令

```html
<span v-html="rawHtml"></span>
```

Mustache语法不能作用在HTML特性上，这时我们会使用`v-bind`指令

```html
<div v-bind:id="dynamicId"></div>
```

在布尔特性的情况下，特性的存在即意味着`true`，这时`v-bind`渲染时如果值为`null``undefined`或者`false`，改特性将被会被渲染出来

```html
<button v-bind:disabled="abled">Button</button>
```

在模版中我们不仅可以绑定简单属性值，还支持JavaScript表达式

```html
{{ 1 + 1 }}
{{ ok ? 'YES' : 'NO'}}
{{ new Array(12).fill('_').join('') }}
<div v-bind:id="'list-' + id"></div>
```

注意，这些表达式只会在所属Vue实例的作用域下进行解析。还有每个绑定都只能包含**单个表达式**。

### 指令(Directives)

指令是带有`v-`前缀的特殊属性。指令的预期值是**单个JavaScript表达式**。指令的作用是，当表达式的值改变时，指令会产生连带影响，响应式的作用于DOM。

```html
<p v-if="show">你看到了什么</p>
```

上面的例子`v-if`指令将根据表达式`show`的值的真假来控制`<p>`元素的插入和移除。

一些指令能够接受一个参数，在指令名称以后已冒号表示。比如上面的`v-bind`指令，接受一个`id`参数表示`html`上属性`id`的值和`id`绑定

另一个例子是`v-on`,它用于监听DOM事件:

```html
<button v-on:click="clickHandler">Don't click me</button>
```

这里的参数表示监听的事件名

指令也可以添加修饰符(Modifiers)用于指出一个指令应该已特殊的形式绑定。修饰符已半角句号`.`指明特殊后缀。

```html
<button v-on:click.prevent="clickHandler">Don't click me</button>
```

上面例子中`.prevent`修饰符告诉`v-on`指令对于出发事件调用时执行`event.preventDefault()`语句

### 缩写

`Vue`中`v-`前缀是一种视觉提示，用来识别模版中Vue特有的特性。当我们为Vue.js现有便签添加动态行为时，`v-`前缀很有帮助，但时对频繁使用的指令来说，就会感觉到繁琐。因此，Vue.js为`v-bind`和`v-on`这两个常:用指令提供了缩写:

```html
<div v-bind:id="dynamicId"></div>
<div :id="dynamicId"></div>
```

```html
<button v-on:click="clickHandler">Don't click me</button>
<button :click="clickHandler">Don't click me</button>
```

## 条件渲染

### `v-if`

前面的例子，`v-if`控制`<p>`渲染与否

```html
<p v-if="show">你看到了什么</p>
```

我们也可以在`v-if`添加一个`v-else`表示一个`else代码块`或者`v-else-if`

```html
<p v-if="show">你看到了什么</p>
<p v-else-if="seen">我什么也没看到</p>
<p v-else>我看到你了</p>
```

有时需要控制一组元素的渲染，此时需要把一个`<template>`元素包裹在这组元素上，并在上面使用`v-if`。

```html
<template v-if="show">
  <section>我是谁?</section>
  <section>我来自哪儿?</section>
  <section>我要去哪里?</section>
</template>
```

Vue为了尽可能高效的渲染元素，通常会复用已有元素，而不是从头渲染。

```html
<template v-if="show">
    吃:<input />
</template>
<template v-else>
    喝:<input />
</template>

<button v-on:click="show = !show">
    toggle
</button>
```

上面的例子中我们切换`show`时`<input/>`的值仍然保留，这是因为Vue复用了`<input />`元素。

有时我们需要两个元素完全独立，不需要复用，只需要添加一个拥有唯一值的属性`key`即可

```html
<template v-if="show">
    吃:<input key="eat"/>
</template>
<template v-else>
    喝:<input key="drink"/>
</template>

<button v-on:click="show = !show">
    toggle
</button>
```

除了`v-if`指令控制元素的渲染与否，另一个指令`v-show`可以根据条件控制元素的显示和隐藏

```html
<p v-show="show">你看到了什么</p>
```

`v-show`只是控制样式的显示和否，这也意味着，元素无论是隐藏还是显示都会渲染。

`v-if`是真正的条件渲染，在切换过程中对应条件内的事件组件都会被销毁和重建。如果一开始条件就是`false`，则什么也不会做，知道条件变为`true`，才会开始渲染。

## 列表渲染

我们使用指令`v-for`将数组渲染为一组元素。`v-for`指令需要使用`item in items`形式的语法，`items`表示一组数据，`item`为数组元素迭代时的别名。

```html
<ul id="list">
  <li v-for="item in items">
      {{item}}
  </li>
</ul>
```

```js
new Vue({
  el: '#list',
  data: function(){
    return {
      items:[
        '我是谁?',
        '我来自哪儿?',
        '我要去哪里?'
      ]
    }
  }
});
```

输出结果

```html
<ul id="list">
  <li>我是谁?</li>
  <li>我来自哪儿?</li>
  <li>我要去哪里?</li>
</ul>
```

`v-for`还支持一个可选的第二个参数表示当前像索引

```html
<ul id="list">
  <li v-for="(item, index) in items">
      {{index}}.{{item}}
  </li>
</ul>
```

你也可以用`of`替代`in`作为分隔符，这在形式上更接近于JavaScript迭代器语法。

我们也可以使用`v-for`对对象进行遍历渲染

```html
<ul id="obj">
  <li v-for="value in person">
      {{value}}
  </li>
</ul>
```

```js
new Vue({
  el: '#obj',
  data: function(){
    return {
      person: {
        name: 'KingZ',
        age: 18
      }
    }
  }
});
```

输出结果

```html
<ul id="obj">
  <li>KingZ</li>
  <li>18</li>
</ul>
```

对于对象的遍历，`v-for`提供另外提供了两个参数，一个表示键名，一个表示索引

```html
<ul id="obj">
  <li v-for="(value, key, index) in person">
      {{index}}.{{key}}:{{value}}
  </li>
</ul>
```

为了充分复用和跟踪每个节点，你需要为没想提供一个唯一的`key`属性。

```html
<li v-for="(item, index) in items" :key="index">
  {{item}}
</li>
```

`key`与`v-for`特别关联，它是Vue识别节点的一个通用机制。

数组更新时会触发视图更新，Vue监测的数组更新的方法如下

```bash
push, pop, shift, unshift, splice, sort, reverse
```

由于JavaScript的限制，数组的变动并不会被Vue监测到:
1. 替换元素 `items[index] = item`
2. 改变数组长度 `items.length = newLength`

为了解决这类问题我们可以使用如下方法模拟

```js
//替换元素
items.splice(index, 1, item)
//改变数组长度
items.splice(newLength)
```

由于JavaScript的限制，对象属性的添加或删除Vue并不能监测到。但我们可以使用`Vue.set(object, key, value)`

```html
<ul id="obj">
  <li v-for="value in person">
      {{value}}
  </li>
  <li>
    <button @click="changePerson">click</button>
  </li>
</ul>
```

```js
new Vue({
  el: '#obj',
  data: function(){
    return {
      person: {
        name: 'KingZ',
        age: 18
      }
    }
  },
  methods:{
    changePerson(){
      //this.person.handsome = false;
      this.$set(this.person, 'sex', 'male')
    }
  }
});
```

如果要为一个对象富裕多个新属性

```js
this.person = Object.assign({}, this.person, {
  handsome: true,
  sex: 'male'
})
```

`v-for`也可以取一段整数

```html
<div>
  <span v-for='num in 5'>{{ num }}</span>
</div>
```

输出

```html
<div>
  <span>1</span>
  <span>2</span>
  <span>3</span>
  <span>4</span>
  <span>5</span>
</div>
```

`v-for`也可以使用在`<template>`，用于遍历渲染多个元素

```html
<ul id="list">
  <template v-for="item in items">
    <li>
      {{item}}
    </li>
    <li>{{new Array(15).join('-')}}</li>
  </template>
</ul>
```

`v-for`和`v-if`可以组合使用，当他们处于统一节点时，`v-for`的优先级`v-if`更高，这意味着`v-if`将分别重复运行域每个`v-for`循环中。

```html
<ul id="list">
  <li v-for="(item, index) in items" v-if='index >= 1'>
      {{index}}.{{item}}
  </li>
</ul>
```

在自定义组件里，你可以像普通元素一样使用`v-for`

```html
<el-button-group>
  <el-button v-for="item in items">{{item}}</el-button>
</el-button-group>
```