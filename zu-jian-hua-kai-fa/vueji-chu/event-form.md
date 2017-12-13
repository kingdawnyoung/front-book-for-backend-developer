# 事件处理和表单输入

## 事件处理

我们可以用`v-on`指令监听DOM事件

```html
<button v-on:click="clickHandler">click</button>
<!--简写为-->
<button @click="clickHandler">click</button>
```

这里`clickHandler`对监听的事件`click`进行处理

除了绑定一个方法，也可以直接在内联语句中调用方法：

```html
<button @click="clickHandler(`Don't click me!`)">click</button>
```

有时候在内联语句处理器中访问原始的DOM事件，这是用特殊变量`$event`将它出啊如方法

```html
<button @click="clickHandler(`Don't click me!`, $event)">click</button>
```

如果我们需要阻止事件的冒泡(`event.stopPropagation()`)或默认方法(`event.preventDefault()`)执行，可以在方法里直接执行相应方法，或者直接使用Vue为`v-on`提供的**事件修饰符**

```html
<!-- 阻止单击事件继续传播 -->
<button v-on:click.stop="clickHandler"></button>

<!-- 提交事件不再重载页面 -->
<form v-on:submit.prevent="onSubmit"></form>

<!-- 修饰符可以串联 -->
<a v-on:click.stop.prevent="doThat"></a>

<!-- 只有修饰符 -->
<form v-on:submit.prevent></form>

<!-- 添加事件监听器时使用事件捕获模式 -->
<!-- 即内部元素触发的事件先在此处处理，然后才交由内部元素自身进行处理 -->
<button v-on:click.capture="clickHandler(1)">
  <span v-on:click="clickHandler(2)"></span>
</button>

<!-- 只当在 event.target 是当前元素自身时触发处理函数 -->
<!-- 即事件不是从内部元素触发的 -->
<button v-on:click.self="clickHandler(1)">
  <span v-on:click="clickHandler(2)"></span>
</button>
<!-- 点击事件将只会触发一次 -->
<button v-on:click.once="clickHandler"></button>
```
