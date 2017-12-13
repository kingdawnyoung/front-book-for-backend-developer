# 计算属性和观察者

## 计算属性

Vue模版内可以写一些表达式进行简单的技术，但放入太多逻辑时往往回事模版过重难以维护。对于复杂逻辑，我们应当使用**计算属性**

```html
<div id="example">
  我是条下划线{{ line }}
</div>
```

```js
new Vue({
  el: "#example",
  data(){
    return {
      arr: new Array(12)
    }
  },
  computed: {
    line: function(){
      return this.arr.fill('-').join('')
    }
  }
})
```

这里我们生命而来一个计算属性`line`，用于将属性arr转化为下划线。我们像绑定普通属性一样绑定计算属性，Vue知道`line`依赖于`arr`，因此当`arr`发生改变时，所有依赖`arr`的绑定也会更新。我们使用声明的形式确定了这种依赖关系，而且没有任何副作用。

我们同样可以通过表达式是想同样的效果：

```html
<div id="example">
  我是条下划线{{ lineRender }}
</div>
```

```js
methods: {
  lineRender: function(){
    return this.arr.fill('-').join('')
  }
}
```

这两种最终结果忘却相同。然而，不同的是**计算属性是基于它们的依赖进行缓存的**。计算属性只有它的相关依赖发生改变才会重新计算。这就意味着`arr`还没有发生改变，多次访问'line'计算属性都是返回之前的计算结果，不必再执行函数。

```js
computed: {
  //无论使用多少次, now值都不会变
  now: function() {
    return Date.now()
  }
}
```

默认的计算属性只有获取值`getter`，不过需要时也可以设置值`setter`

```js
computed:{
  line: {
    //默认getter
    get: function(){
      return this.arr.fill('-').join('')
    },
    //setter
    set: function(newValue){
      this.arr = new Array(newValue)
    }
  }
}
```

## 侦听器

虽然计算属性能够满足大多数情况，但有时我们还是需要自定义一个侦听器。Vue通过`watch`选项提供了一个更通用的方法，来相应数据的变化。

```html
<div id="example">
  <div>{{ line }}-></div>
  <button @click="changeArrLength">change arr length</button>
</div>
```

```js
new Vue({
  el: "#example",
  data(){
    return {
      line: '',
      arr: new Array(12)
    }
  },
  watch: {
    arr: function(){
      this.line = this.arr.fill('-').join('')
    }
  },
  methods:{
    changeArrLength(){
      this.arr = new Array(parseInt(Math.random() * 100));
    }
  }
})
```
