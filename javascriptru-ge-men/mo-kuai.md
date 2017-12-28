### 模块

JavaScript在历史上是一直完整的模块体系的，这对大型的、复杂的项目开发非常的不方便。所以社区制定了一些模块加载的方案，最主要的有 CommonJS 和 AMD 两种。前者用于服务端，后者用于浏览器。ES6在语言标准的层面实现了模块功能，成为了服务端和浏览器通用的模块解决方案。

ES6 模块的设计思想是尽量的静态话，使得在编译时就能确定模块的依赖关系，已经输入输出的变量。CommonJS 和 AMD 只能在运行是确定这些东西。

```js
//CommonJs 模块
let { stat, exists, readFile } = require('fs');
```

上面例子是整体加载`fs`模块，然后通过对象结构取值，这就导致只有运行时才能取得上面三个值这种接在称之为`运行时加载`。

ES6 模块不是对象，二十通过`export`命令显示置顶输出的代码，在通过`import`命令输入。

```js
//ES6模块
import {stat, exists, readFile} from 'fs'
```

上面的例子实际上只加载fs模块的3个方法， 其他方法不加载，这种加载称之`编译时加载` 为 `静态加载`，即在编译时就完成了模块的加载。这种加载的效率要比CommonJS模块的高，但也导致没法应用ES6模块本身，因为它不是对象。

由于 ES6 模块是编译时加载，使得静态分析陈巍可能。有了它，就能基尼一步拓宽JavaScript的预发，比如引入宏和类型检验这些只能靠静态分析才能实现的功能。

1. 不需要`UMD`(统一模块定义)模块格式，服务端和浏览器都通过ES6模块格式定义。
2. 将来浏览器的新 API 就能用模块格式提供
3. 不再需要对象作为命名空间

#### export 命令

模块的功能主要由两个命令构成：`export`和`import`。`export`命令用于规定模块的对外接口，`import`用于输入其他模块定义的功能。

一个模块就是一个独一的文件，文件内的所有变量无法对外暴露，除非你用`export`输出改变量。

```js
//module.js
export let foo = 1;
export let bar = 2;

export function fn(){}

let var1 = 3;
let var2 = 4
export {
    var1,
    var2
}
```

```js
//我们可以使用关键字as给输出重命名
let var1 = 3;
let var2 = 4
export {
    var1 as num3,
    var2 as num4
    var2 as digital4
}
```

`export`命令规定的是对外的接口，必须与模块内部的变量建立一一对应关系。

```js
//错误示例
export 1;

var m = 1;
export m;
```

```js
//正确写法
// 写法一
export var m = 1;

// 写法二
var m = 1;
export {m};

// 写法三
var n = 1;
export {n as m};
```

`export`语句输出的接口，与其对应的值是动态绑定关系，即通过该接口，可以取到模块内部实时的值。

```js
export var foo = 'bar';
setTimeout(() => foo = 'baz', 500);
```

`export`命令可以出现在模块的任何位置，只要处于模块顶层就可以。如果处于块级作用域内，就会报错，这违背了ES6静态加载的设计初衷。

```js
function foo() {
  export default 'bar' // SyntaxError
}
foo()
```

#### import命令

使用`export`命令定义了模块的对外接口以后，其他 JS 文件就可以通过`import`命令加载这个模块。

```js
//main.js，导入前面定义的moudle.js
import {foo, bar} from "./moudle" 

console.log(foo, bar);
```

`import` 同样可以使用as关键字为变量重新命名

```js
import {foo as num1, bar as num2} from "./moudle" 
```

`import` 命令有提升效果，会提升到模块的头部，首先执行

```js
//
console.log(foo);
import{foo} form "./moudle"
```

`import`是静态执行， 所以不能使用表达式和变量

```js
//错误示例
import { 'f' + 'oo' } from './moudle';

let module = './moudle';
import { foo } from module;


if (true) {
  import { foo } from './moudle';
}
```

`import`语句会执行所加载的模块。

```js
import "echart"
```

如果多次重复执行同一句import语句，那么只会执行一次，而不会执行多次。

```js
import "echart"
import "echart"
```

#### 模块的整体加载

除了指定加载某个输出值，还可以使用整体加载，即用星号（*）指定一个对象，所有输出值都加载在这个对象上面。

```js
import * as moudle from "./moudle"
console.log(moudle.foo, moudle.bar)//output 1 2
```

#### export default 命令

```js
//moudle.js
export default function () {
  console.log('foo');
}
```

```js
//main.js
import customName from "./moudle"

customName()//output，foo
```

`export default`命令用于指定模块的默认输出。显然，一个模块只能有一个默认输出，因此`export default`命令只能使用一次。

 本质上，`export default`就是输出一个叫做default的变量或方法，然后系统允许你为它取任意名字。

 ```js
 // modules.js
function add(x, y) {
  return x * y;
}
export {add as default};
// 等同于
// export default add;

// app.js
import { default as foo } from 'modules';
// 等同于
// import foo from 'modules';
 ```

 正是因为`export default`命令其实只是输出一个叫做`default`的变量，所以它后面不能跟变量声明语句。

 #### `export` 与 `import` 的复合写法
 ```js
 export { foo, bar } from './module';

// 等同于
import { foo, bar } from 'my_module';
export { foo, bar };

//变量重命名
export { foo as num1 } from './module';

//整体输出
export * from './module';

//默认导出
export {foo as default } from './moudle';

//默认导出重命名
export {export as foo} from './moudle'
 ```