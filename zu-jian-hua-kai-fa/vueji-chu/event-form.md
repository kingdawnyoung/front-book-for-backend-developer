
{% raw %}
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

Vue同样监听键盘事件，使用`v-on`在监听时添加按键修饰符

```html
<input v-on:keyup.13="alert">
```

记住`keyCode`比较困难，Vue为最常用的按键提供了别名

```bash
.enter
.tab
.delete (捕获“删除”和“退格”键)
.esc
.space
.up
.down
.left
.right
```

你也可以通过全局`config.leyCodes`对象自定义按键修饰符名:

```js
//v-on:keyup.f1
Vue.config.keyCodes.f1 = 112
```

可以用如下修饰符来实现仅在按下相应按键时才触发鼠标或键盘事件的监听器。

```bash
.ctrl
.alt
.shift
.meta(Mac对应command, Windows对应Windows微标键)
```

```html
<input @keyup.ctrl.enter='doSomething' >

<button @click.alt="clickHandler"></button>
```

`.exact` 修饰符应与其他系统修饰符组合使用，以指示处理程序只在精确匹配该按键组合时触发。

```html
<!-- ctrl或者shift一起按着也会触发 -->
<button @click.alt="clickHandler"></button>

<!-- 仅在只有alt被按下的时候触发 -->
<button @click.alt.exact="clickHandler"></button>
```

下面修饰符会限制处理函数仅响应特定的鼠标按钮。

```bash
.left
.right
.middle
```

## 表单输入

我们使用`v-model`指令在表单元素上创建双向数据绑定。它会根据控件类型自动选取正确的方法来更新元素。尽管神奇，当`v-model`本质上不过是语法糖，它负责监听用户的输入事件以更新数据，并特别处理一些极端的例子。

```html
<input v-model="name" placeholder="输入名字">
<p>Hello, {{ name || "某某"}}</p>
```

```html
<textarea v-model="message" placeholder="输入点什么吧"></textarea>
<p>{{ message }}</p>
```

单个复选框，绑定到布尔值

```html
<input type="checkbox" id="checkbox" v-model="checked">
<label for="checkbox">{{ checked }}</label>
```

多个复选框，绑定到同一个数组：

```html
<div id='example'>
  <input type="checkbox" id="KingZ" value="KingZ" v-model="checkedNames">
  <label for="KingZ">KingZ</label>
  <input type="checkbox" id="Dawn" value="Dawn" v-model="checkedNames">
  <label for="Dawn">Dawn</label>
  <input type="checkbox" id="Young" value="Young" v-model="checkedNames">
  <label for="Young">Young</label>
  <br>
  <span>Checked names: {{ checkedNames }}</span>
</div>
```

单选按钮

```html
<div id="example">
  <input type="radio" id="one" value="One" v-model="picked">
  <label for="one">One</label>
  <br>
  <input type="radio" id="two" value="Two" v-model="picked">
  <label for="two">Two</label>
  <br>
  <span>Picked: {{ picked }}</span>
</div>
```

选择框单选时

```html
<div id="example">
  <select v-model="selected">
    <option disabled value="">请选择</option>
    <option>A</option>
    <option>B</option>
    <option>C</option>
  </select>
  <span>Selected: {{ selected }}</span>
</div>
```

多选时，绑定到一个数组

```html
<div id="example">
  <select v-model="selected" multiple style="width: 50px;">
    <option>A</option>
    <option>B</option>
    <option>C</option>
  </select>
  <br>
  <span>Selected: {{ selected }}</span>
</div>
```

对于单选按钮，复选框及选择框的选项，`v-model` 绑定的值通常是静态字符串。但是有时我们可能想把值绑定到 Vue 实例的一个动态属性上，这时可以用 v-bind 实现，并且这个属性的值可以不是字符串。

```html
<input type="checkbox" v-model="checked" v-bind:true-value="'为了部落'" v-bind:false-value="'为了联盟'">
```

```html
<input type="radio" v-model="picked" v-bind:value="'为了部落'">
```

```html
<select v-model="selected">
    <!-- 内联对象字面量 -->
  <option v-bind:value="{ number: 123 }">123</option>
</select>
```

在默认情况下，`v-model` 在每次 `input` 事件触发后将输入框的值与数据进行同步 (除了输入法组合文字时)。你可以添加 `lazy` 修饰符，从而转变为使用 `change` 事件进行同步

```html
<!-- 在“change”时而非“input”时更新 -->
<input v-model.lazy="name" placeholder="输入名字">
<p>Hello, {{ name || "某某"}}</p>
```

如果想自动将用户的输入值转为数值类型，可以给 `v-model` 添加 `number` 修饰符：

```html
<input v-model.number="age" type="number">
<p>我 {{ age || 8}} 岁，{{ typeof age }}类型的{{ age }}</p>
```

如果要自动过滤用户输入的首尾空白字符，可以给 `v-model` 添加 `trim` 修饰符：

```html
<input v-model.trim="name" placeholder="输入名字">
<p>Hello, {{ name || "某某"}}</p>
```


{% endraw %}