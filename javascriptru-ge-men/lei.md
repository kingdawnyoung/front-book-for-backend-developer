### 类

```js
//意义一个类
class Clazz{
    constructor(x, y) {
        this.x = x;
        this.y = y;
    }

    toString(){
        return `${this.x},${this.y}`;
    }
}
```

```js
//实际上class只是构造函数的语法糖
typeof Clazz //"function"
Clazz === Clazz.prototype.constructor // true 
```

```js
//使用时直接new既可以
(new Clazz(1, 2)).toString()//1,2
```

```js
//事实上，类的所有方法都定义在类的prototype属性上面。
Clazz.prototype.toString.toString()

//我们可以通过高Object.assign给Clazz添加更多方法
Object.assign(Clazz.prototype,{
    say(msg = "德玛西亚"){console.log(msg)},
    sing(msg = "最炫民族风"){console.log(msg)}
});

(new Clazz(1, 2)).say();
(new Clazz(1, 2)).say();//output，德玛西亚
```

```js
//类的内部定义都是不可枚举的
Object.keys(Clazz.prototype) //[ 'say', 'sing' ]
Object.getOwnPropertyNames(Clazz.prototype) //[ 'constructor', 'toString', 'say', 'sing' ]
```

#### constructor方法

`constructor`方法是类的默认方法，通过new命令生成对象实例时，自动调用该方法。一个类必须有`constructor`方法，如果没有显式定义，一个空的`constructor`方法会被默认添加。

类必须使用`new`调用，否则会报错。这是它跟普通构造函数的一个主要区别，后者不用`new`也可以执行。

#### 类的实例

实例的属性除非显式定义在其本身（即定义在this对象上），否则都是定义在原型上（即定义在class上）。

```js
let clazz = new Clazz(1, 2);

clazz.hasOwnProperty('x') // true
clazz.hasOwnProperty('y') // true
clazz.hasOwnProperty('toString') // false
clazz.__proto__.hasOwnProperty('toString') // true

//类实例都共享一个原型对象
let clazz1 = new Clazz(1, 2);
let clazz2 = new Clazz(1, 2);

clazz1 === clazz2 //true
```

#### Class表达式
与函数一样，类也可以使用表达式的形式定义。

```js
const MyClazz = class Clazz {
  getClassName() {
    return Clazz.name;
  }
};

 let ins = new MyClazz();
 ins.getClassName() //Clazz
 Clazz.name //error, Clazz is not defined
```

```js
//如果内部没有到Clazz, 可以省略类名
const MyClazz = class{...}
```

```js
//Class表达式可以写出立即执行的Class
let ins = new class{...}()
```

#### 不存在变量提升

#### this的指向

类的方法内部如果含有this，它默认指向类的实例。

#### Class 的静态方法

如果在一个方法前，加上static关键字，就表示该方法不会被实例继承，而是直接通过类来调用，这就称为“静态方法”。

```js
class Foo {
  static method() {
    return 'hello';
  }
}
Foo.method()// 'hello'
```

如果静态方法包含this关键字，这个this指的是类，而不是实例。

```js
class Foo {
  static bar () {
    this.baz();
  }
  static baz () {
    console.log('hello');
  }
  baz () {
    console.log('world');
  }
}

Foo.bar() // hello
```

父类的静态方法，可以被子类继承。

```js
class Foo {
  static classMethod() {
    return 'hello';
  }
}

class Bar extends Foo {
  static classMethod() {
    return super.classMethod() + ', too';
  }
}

Bar.classMethod() // "hello, too"
```