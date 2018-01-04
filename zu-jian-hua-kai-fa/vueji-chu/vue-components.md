{% raw %}

# Vue组件

组件可认为是HTML元素的扩展，封装可重用的代码。在加高层面上，组件就是自定义元素，Vue.js编译器为它添加特殊功能。

所有Vue组件同时也是Vue的实例，所以可接受相同的选项对象、提供相同的生命周期钩子。 

## 使用组件

注册一个全局组件

```js
Vue.component('custom-component', {
  template: '<div>A custom component!</div>'
});
```

使用全局组件

```html
<div id="example">
  <custom-component></custom-component>
</div>
```

渲染为：

```html
<div id="example">
  <div>A custom component!</div>
</div>
```

局部注册

```js
var child = {
  template: '<div>A custom component!</div>'
}

new Vue({
  el: "#example",
  components:{
    "custom-component": child
  }
})
```

## data必须是函数

如果data是一个对象，data将被共享，轻重一个组件组件中的属性被修改，会被同步到其他组件。

```js
Vue.component('custom-component', {
  template: '<div> {{message}} </div>',
  data: function(){
    return {
      message: "A custom component!"
    }
  }
});
```

## 组件组合

组件涉及的初衷就是要配合使用，最常见的形式就是父子关系：组件A中在它的模版中使用了组件B。配合使用必然涉及到组件间的通信，定义良好的接口尽可能将父子组件件解耦就是非常重要，这就保证每个组件在相对隔离的环境中书写和理解，从而保证组件的可维护性和复用性。

在Vue中，父组件功过**prop**给子组件下发数据，子组件通过**事件**给父组件发送消息。

![](https://cn.vuejs.org/images/props-events.png)

## 使用Prop传递数据

组件实例的作用域是孤立的。这就意味着不能在植株间的模版内直接引用父组件的数据。父组件的数据需要通过prop才能下发到子组件。

子组件也要显示的用`props`声明它预期的数据：

```js
Vue.component('child', {
  props: ['name'],
  template: '<div>Hello，{{name}} </div>'
})
```

父组件可以向它传入一个普通字符串

```html
<child name='World'></child>
```

与绑定任何普通组件的HTML特性类似，我们也可以用`v-bind`来动态的将prop绑定到父组件的数据。每当副主见的数据变化时，也将改变化传递给子组件

```html
<input v-model='name'>
<br>
<child v-bind:name='name'></child>
```

如果把一个对象的所有属性作为prop进行传递，可以使用不带任何参数的`v-bind`

```js
//data
person: {
  name: "Kingz"
}
```

```html
<child v-bind='person'></child>

<!--等价于-->
<child v-bind:name='person.name'></child>
```

Prop是单向数据绑定，父组件属性变化将传导给子组件，但是反过来不会。这是为了防止植株间无意间修改了父组件状态，来避免应用数据流变得难以理解。

例外，每次父组件更新时，子组件的所有prop都会更新为最新值，这也意味着不应该在子组件内部改变prop。

有时我们很容易忍不住想去修改prop中数据

1. Prop作为初始值传入后，子组件想把他作为局部数据来用
2. Prop作为原始数据传入，由组件处理成其它数据输出使用

针对上面两种情况，正确的应对方式是：

1. 定义一个局部变量，用prop的值初始话

```js
props: ['initialCounter'],
data: function () {
  return { counter: this.initialCounter }
}
```

2. 定义一个计算属性，处理 prop 的值并返回

```js
props: ['size'],
computed: {
  normalizedSize: function () {
    return this.size.trim().toLowerCase()
  }
}
```

>> 在JavaScript中对象和数组是引用数据类型，指向同一个内存空间，如果prop是一个对象或数组，子组件内部改变它的会影响副主见的状态

Prop可以指定验证规则，如果传入的数据不满足要求，Vue会翻出警告

```js
props: {
  // 基础数据类型(传null指任何类型)
  propA: Number,
  // 可能是多种类型
  propB: [String, Number],
  // 必传且是字符串
  propC: {
    type: String,
    required: true
  },
  // 数值且有默认值
  propD: {
    type: Number,
    default: 100
  },
  // 数组/对象的默认值应当由一个工厂函数返回
  propE: {
    type: Object,
    default: function () {
      return { message: 'hello' }
    }
  },
  // 自定义验证函数
  propF: {
    validator: function (value) {
      return value > 10
    }
  }

}
```

为组件定义明确的prop是推荐的传参方式，但组件的作者并不能总能遇见组件被使用的场景。所以组件可以接受任意传入的特性，这些特性都会被添加到组件的根元素上。

```html
<!--gender="male"会被自动添加到child的根元素上-->
<child v-bind="person" gender="male"></child>
```

对于多数特性来说，传递的值难免会覆盖组件自身的值，比如`type="text"`将会覆盖`type="date"`。Vue针对`class`和`style`特性进行特殊处理，将两个值进行合并。

子组件

```js
Vue.component('child', {
  template: '<div class="classA">Hello</div>'
})
```

父组件

```html
<child v-bind='person' class='classB' ></child>
```

输出

```html
<div class="classA classB" name="">Hello</div>
```

## 自定义事件

父组件使用prop传递数据给子组件，子组件通过自定义事件传递消息给父组件。

每个Vue实例都实现了事件接口，即：
* 使用 `$on(eventName)` 监听事件
* 使用 `$emit(eventName)` 触发事件

父组件可以在子组件的地方直接用`v-on`来监听植株间触发的事件

```html
<div id="example">
  <child v-on:say="sayHandler"></child>
</div>
```

```js
Vue.component('child', {
  template: `<div>
    <button v-on:click='click'>click</button>
  </div>`,
  methods: {
    click(){
      this.$emit("say");
    }
  }
});

new Vue({
  el: "#example",
  methods:{
    sayHandler(){
      alert("don't click me");
    }
  }
})
```

有时我们需要建个某个组件的根元素的原生事件，可以使用`v-on`的修饰符`.native`

```html
<child v-on:say="sayHandler" v-on:click.native="clickHandler"></child>
```

有时我们可能需要对一个prop进行“双向绑定”，这是我们需要为改prop加上`.sync`装饰器。

```html
<child v-bind:name.sync='name'></child>

<!--等价于-->
<child :name="name" @update:name="val => name = value"></child>
```

当子组件需要更新`name`时，他需要显式地出发一个更新事件：

```js
this.$emit('update:name', newValue);
```

自定义的事件也可以创建自定义的表单输入组件，使用`v-model`进行数据的双向绑定：

```html
<input v-model="input">

<!--等价于-->
<input
  v-bind:value="input"
  v-on:input="input = $event.target.value">
```

默认的`v-model`是有那个`value`prop和`input`事件。但是诸如单选框复选框之类输入类型`value`可能做了别的用途。使用`model`选项可以避免这样冲突：

```js
Vue.component('my-checkbox', {
  model: {
    prop: 'checked',
    event: 'change'
  },
  props: {
    checked: Boolean,
    // 这样就允许拿 `value` 这个 prop 做其它事了
    value: String
  },
  // ...
})
```

```html
<my-checkbox v-model="foo" value="some value"></my-checkbox>
```

有时候，非父子关系组件也要通信，这一定义一个空的Vue实例作为事件总线

```js
var bus = new Vue();

//组件A种种触发事件
bus.$emit("trigger", 1);

//组件B中创建钩子监听事件
bus.$on("trigger", function(param){
  //do something
});
```

## 插槽

我们在使用组件时有时需要这样组合它

```html
<!--一篇文章往往会有页头, 正文, 页脚组成-->
<cc-article>
  <cc-title></cc-title>
  <cc-content></cc-content>
  <cc-footer></cc-footer>
</cc-article>
```
上面的组合中有两个问题

1. `cc-article`不知道它会收到什么内容。这是由它的父组件决定
2. `cc-article`可能有自己的模版

为了让组件可以组合，我们需要一种方式来混合父组件的内容与子组件自己的模板。这个过程被称为内容分发，Vue实现了内容分发API，使用`<slot>`元素作为原始内容的插槽。


子组件需要提供一个`<slot>`插口，来接受父组件的内容，否则父组件的内容将会被**丢弃**。当子组件模版只有一个没有属性的插槽时，父组件传入的整个内容片段将插入到插槽所在的DOM位置，并替换插槽本身。

最初在`<slot>`便签总的内容被视为`备用内容`。备用内容在子组件的作用域内进行编译，并且只有在宿主元素为空，切没有插入内容的时候才会显示。

假定我们的`cc-article`组件有如下模版：
```html
<article>
  <header>title</header>
  <slot>content</slot>
  <footer>footer</footer>
</article>
```

父组件模版：

```html
<div id="example">
  <cc-article>
    我是父组件的content
  </cc-article>
</div>
```

输出结果

```html
<div id="example">
  <article>
    <header>title</header>
    我是父组件的content
    <footer>footer</footer>
  </article>
</div>
```

`<slot>`元素可以用一个特殊的特性`name`来进一步配置如何分发内容，称之为**具名插槽**。多个插槽可以有不同的名字，具名插槽将匹配内容片段中对应`slot`特性的元素。

仍然可以有一个匿名插槽，它就是**默认插槽**，作为找不到匹配内容片段的备用插槽。如果没有默认插槽，这些找不到匹配的内容片段将抛弃。

`cc-article`组件模版
```html
<article>
  <slot name="header">
    <header>title</header>
  </slot>
  <slot>content</slot>
  <slot name="footer">
    <footer>footer</footer>
  </slot>
</article>
```

父组件模版：

```html
<cc-article>
  <header slot="header">
    我是父组件的header
  </header>
  我是父组件的content
  <header slot="footer">
    我是父组件的footer
  </header>
</cc-article>
```

输出：

```html
<article>
  <header>
    我是父组件的header
  </header>
  我是父组件的content
  <header>
    我是父组件的footer
  </header>
</article>
```

在子组件中，有时需要将数据传递给插槽

```html
<article>
  <header>title</header>
  <slot text="这是子组件的content">content</slot>
  <footer>footer</footer>
</article>
```

父组件模版中通过特殊特性`slot-scope`接受子组件传递过来的对象

```html
<div slot-scope="scope">
  {{scope.text}}, 我是父组件的content
</div>

<!--可以直接通过解构获取text值-->
<div slot-scope="{ text }">
  {{text}}, 我是父组件的content
</div>
```
{% endraw %}