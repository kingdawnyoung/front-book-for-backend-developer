# Vue模板，条件和列表渲染

## 模版

Vue使用了基于HTML的模版语法，允许开发者声明地的将DOM绑定到Vue实例的底层数据上。所有Vue的模版都是合法的HTML，所以可以被遵循规范的浏览器和HTML解析器直接解析。

底层上，Vue将模版编译成DOM渲染函数。结合响应系统，在应用状态改变时，Vue能够花费最小的代价重新计算渲染组件到DOM上。

你也可以直接写渲染行数或者使用JSX来定义模版。

### 插入一个值

数据绑定最常见的形式就是使用"Mustache"语法({{}})。

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

···html
<button v-on:click.prevent="clickHandler">Don't click me</button>
···

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
