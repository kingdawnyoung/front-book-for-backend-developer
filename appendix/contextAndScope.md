## 理解JavaScript的作用域和上下文

Js的作用域和上下文是是该语言的一大特色, 这种特色某种程度上上来说是得益于它的灵活性. 函数被应用于不同的上下文和作用域的隐藏和封装. 这些概念被应用于js里一些最为强大的设计模式的实现. 然而这也是开发者之间困惑的主要来源,和很好理由. 接下来的文章,我们将对作用域和上下文和作用域进行全面的解释, 他们的区别, 以及如何使用它们实现各种设计模式.

### 上下文和作用域的比较
首先要澄清的是上下文和作用域是不一样的. 我发现很多有多年经验的开发者混淆这两个词产生, 不能正确的描述他们.实际上, 术语也混淆了好多年.

每一个函数的调用都会和作用域和上下文相关. 根本来说, 作用域是基于函数的, 而上下文是基于对象的. 换句话说, 作用域适用于函数被调用和执行时变量的访问; 上下文通常是指谁当前执行代码中`this`关键字说指代的对象的值.

### 变量的作用域
一个变量可以定义为局部和全局作用域, 在其运行时根据不同作用域构建起不同的可访问性. 任何被定义为全局的变量, 意思是说实在函数主体外面声明, 生命周期将贯穿整个运行期间, 可以在任何作用域被访问和修改. 局部变量仅存在说定义的函数体的内部, 函数每次被调用拥有不一样的作用域. 这样变量的赋值, 检索, 操作只能在内部, 并且不能在作用域外面被访问.

JavaScript目前还不支持块级作用域(一种通过if语句, switch, for循环, while循环等确定变量作用域的能力). 这就意味大括号并不能限制变量的可访问性, 任何在代码块内部定义的变量也可以在代码块外也能被访问. 
这个已经已经在`ES6`发生改变, 关键字`let`被加进了标准.

### 什么是this上下文
很多时候上下文由函数的调用确定的. 当一个函数作为一个对象的的方法被访问时, `this`就被设为这个调用的对象:

```js
var obj = {
    foo: function(){
        alert(this === obj);    
    }
};

obj.foo(); // true
```

同样的规则也适用于`new`操作符创建一个对象实例的方式调用一个函数.当以这种方式调用时, 这个作用域下的`this` 的值将被设置为刚创立的那个实例


```js
function foo(){
    alert(this);
}

foo() // window
new foo() // foo 
```

### 执行上下文

JavaScript是单线程的语言, 意味着在同一时间仅有一个任务被执行. 当JavaScript解释器开始执行代码时, 他首先默认执行全局上下文. 然后每个函数的执行都会创建一个新的执行上下文. 

这就往往会产生混淆, 所谓的"执行上下文", 实际如前面所讨论的意图和目的更多涉及的是作用域而非上下文. 这个一个不幸的命名约定, 但它是由ECMAScript规范中定义的术语, 所以我们还是坚持了下来.

每一个新的执行上下文创建时都会被附加到执行堆栈的顶部. 浏览器总是执行当前执行堆栈里顶部上的执行上下文. 一旦完成, 执行上下文会被从当前堆堆栈的顶部移除, 然后移动到下面的执行下文.

一个执行上下文可分为创建期和执行期. 在创建期, 解释器会首先创建一个对象变量(也称激活一个对象), 所有的变量, 声明的函数和定义的参数被包含进执行上下文里. 接下来作用域链将被初始化, `this`的值也将确定下来. 再然后便进入了执行阶段, 代码将被解释和执行.

### 作用域链

每个执行上下文都会伴随着执行作用域链. 执行堆栈中每个执行上下文的变量对象都包含其中. 它被用来确定变量的访问和标识符解析. 比如:


```js
function first(){
    second();
    function second(){
        third();
        function third(){
            fourth();
            function fourth(){
                // do something
            }
        }
    }   
}
first();
```

运行前面的代码, 嵌套函数将被一路执行下去, 直到`fourth`函数. 此时, 作用域链也将自上而下, fourth, third, second, first, global. `fourth`函数也可以访问全局变量, 以及`first`, `second`, 和`third`函数内部定义的变量以及函数本身.

不同执行上下文的变量名间的冲突也将被解决, 通过在作用域上的爬升, 局部移到了全局. 这就意味着, 越往上作用域链里拥有相同名称的局部变量优先级越高.

简而言之, 在同一个函数的执行上下文里, 每次访问变量里, 检索变动总是以你的变量对象里的优先. 如果没有在当前变量对象里没有发现这个标识, 将会在作用域链里继续搜索. 它会攀爬每个执行上下文的每个作用域链来查询匹配的变量名.

### 闭包
在即时作用域外访问变量将创建一个闭包. 换句话说,  一个嵌套函数定义在另一个函数内部形成一个闭包, 这样就允许访问到外面函数的的变量. 返回内嵌函数可以允许你访问局部变量, 参数, 以及其外部函数所定义的内部函数. 这种封装允许我们隐藏和保护外部函数的执行上下文, 同时暴露出一个公共接口进行进一个操作. 如下, 给出了一个类似的例子:


```js
function foo(){
    var localVariable = 'private variable';
    return function bar(){
        return localVariable;
    }
}

var getLocalVariable = foo();
getLocalVariable() // private variable
```

其中最为普遍的闭包的使用是我们周知的模块貌似; 它可以让你模拟公共, 私有和特殊成员:

```js
var Module = (function(){
    var privateProperty = 'foo';

    function privateMethod(args){
        // do something
    }

    return {

        publicProperty: '',

        publicMethod: function(args){
            // do something
        },

        privilegedMethod: function(args){
            return privateMethod(args);
        }
    };
})();
```

这个模块就像一个单体, 在编译解析时执行, 然后又在函数结束处闭幕. 唯一在外面允许的闭包里的执行上下文成员是你在返回的对象里公开的方法和属性. 然而, 所有的私有属性和方法还会贯穿整个程序的生命周期就像执行上下文被保留了下来, 也意味着变量还可以公共的方法进一步交互.

另一种形式的闭包被称之为即时函数(IIFE), 这只是一种自调用的匿名函数在window的上下文里执行而已.

```js
(function(window){
    var foo, bar;

    function private(){
        // do something
    }

    window.Module = {

        public: function(){
            // do something 
        }
    };

})(this);
```

 这种表达式在你试图保存任何函数体定义的变量到全局命名空间以贯穿整个声明周期来说仍然是最有用的. 这是封装程序和框架的源代码, 暴露单一的公共交互接口最为流行的方式.

### Call 和 Apply

所有函数都拥有两个这样方法, 它们允许你使用想要的上下文里执行函数. 这可以实现难以置信的强大功能. `call`函数需要的参数是明确陈列的, 而`apply`函数需要提供的参数确实个数组:

```js
function user(firstName, lastName, age){
    // do something 
}

user.call(window, 'John', 'Doe', 30);
user.apply(window, ['John', 'Doe', 30]);
```

上面两个执行结果是完全一项的, `user`函数在window的上下文里被执行, 同时还被传递了三个相同的参数.

ES5也介绍`Function.prototype.bind`方法用来操作上下文. 它会返回一个新的函数, 这个函数被永久地绑定上`bind`的第一个参数. 它工作是会用一个闭包负责重定向调用和访问上下文. 不受支持的浏览器可以看下面替代方法:


```js
if(!('bind' in Function.prototype)){
    Function.prototype.bind = function(){
        var fn = this, 
        context = arguments[0], 
        args = Array.prototype.slice.call(arguments, 1);
        return function(){
            return fn.apply(context, args.concat([].slice.call(arguments)));
        }
    }
}
```

它通常被用于上下文的丢失, 面向对象和事件的绑定. 这是很有必要, 因为节点的`addEventListener`方法始终在节点所绑定的上下文里执行, 这也是它的应由有的执行方式. 但是如果你用的是面向对象技术或者你的回调是一个实例的方法, 你就需要手动调整上下文. 这是`bind`就派上用场了:

```js
function MyClass(){
    this.element = document.createElement('div');
    this.element.addEventListener('click', this.onClick.bind(this), false);
}

MyClass.prototype.onClick = function(e){
    // do something
};
```

当你回顾`Function.prototype.bind`函数时, 你也许注意到了两个调用中都涉及到`Array`的`slice`方法:

```js
Array.prototype.slice.call(arguments, 1);
[].slice.call(arguments);
```

有趣的提示这个`arguments`对系那个实际上根本不是一个数组, 因此它经常被描述成"类数组"对象, 非常相似一个nodelist(element.childNodes). 它们有长度属性和索引值, 但他们依然不是一个数组, 然后他们也不支持任何数组的本地方法, 比如`slice`和`push`. 尽管如此, 由于它们有相似的行为, 数组的方法也能被接受和控制, 假如你愿意, 它也能像上面例子那样执行在类数组对象上下文里.

使用另一个对象方法的技术适用于JavaScript模拟的经典的面向对象的继承:


```js
MyClass.prototype.init = function(){
    // call the superclass init method in the context of the "MyClass" instance
    MySuperClass.prototype.init.apply(this, arguments);
}
```

通过在另一个子类实例的上下文里调用父类的方法, 我们可以模拟 通过调用方法的父级去充分挖掘强力的设计模式的能力.

### 总结

在你开始接触的设计模式之前了解这些概念是非常重要的, 作用域和上下文在现代JavaScript扮演着非常基础的角色. 我们讨论的闭包, 面向对象和继承, 或者原生的各种实现, 无论哪一样, 上下文和作用域在他们之间都扮演着非常重要的角色. 假如你的目标是精通JavaScript语言或者更好地理解它所包涵的一切, 作用域和上下文将是你的出发点之一.

 
 
 








