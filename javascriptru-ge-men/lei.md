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
new Clazz(1, 2).toString();//1,2
```

```js
//事实上，类的所有方法都定义在类的prototype属性上面。
Clazz.prototype.toString.toString()

//我们可以通过高Object.assign给Clazz添加更多方法
Object.assign(Clazz.prototype, {
  say(msg = "德玛西亚"){
    console.log(msg)
  },
  sing(msg = "最炫民族风"){
    console.log(msg)
  }
});

new Clazz(1, 2).say();
new Clazz(1, 2).say();//output，德玛西亚
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

clazz1.__proto__ === clazz2.__proto__ //true
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

```js
new Clazz();//报错 Clazz 未定义
class Clazz{...}
```

#### this的指向

类的方法内部如果含有this，它默认指向类的实例。

#### Class 的取值行数(getter)和取值函数(setter)

“类”的内部可以使用get和set关键字，对某个属性设置存值函数和取值函数，拦截该属性的存取行为

```js
class Clazz{
  get prop(){
    return "123";
  }
  set prop(value){
    console.log("setter:" + value);
  }
}

let inst = new Clazz();

inst.prop = 456
// setter:456

inst.prop;
// 123

//春曲子函数实际上这自在属性的Descriptor对象上
let descriptor = Object.getOwnPropertyDescriptor(Clazz.prototype, "prop");
console.log(descriptor)
```

#### Class 的静态方法

如果在一个方法前，加上`static`关键字，就表示该方法不会被实例继承，而是直接通过类来调用，这就称为“静态方法”。

```js
class Clazz {
  static method() {
    return 'hello';
  }
}
Clazz.method()// 'hello'
```

如果静态方法包含`this`关键字，这个`this`指的是类，而不是实例。

```js
class Clazz {
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

Clazz.bar() // hello
```

#### new.target
`new`是从构造函数生成实例对象的命令. `new.target`属性一般用于构造函数中返回`new`命令作用于的那个构造函数. 如果这个构造函数不是用`new`命令调用的, `new.target`会返回`undefined`.

```js
class Clazz{
  constructor(){
    console.log(new.target)
  }
}
```

### 继承

Class 可以通过extends关键字实现继承

```js
class Parent {
  say(){
    console.log("I am parent")
  }

  watch(){
    console.log("watch TV");
  }
}

class Child extends Parent {
  say(){
    console.log("I am child")
  }
}

let child = new Child();
child.say();
child.watch();
```

`constructor`方法为类的构造方法, 当显示使用构造方法时需要在构造方法里优先调用`super`方法执父类的构造方法. 这是因为子类没有自己的`this`对象，而是继承父类的`this`对象, 然后对其进行加工。如果不调用`super`方法，子类就得不到`this`对象。

```js
class Child extends Parent {
  constructor(){
    super();
    this.foo = 123
  }
};

//如果不显示使用默认指定constructor
class Child extends Parent {
  constructor(...args){
    super(args);
  }
};
```

```js
child instanceof Child;//true
child instanceof Parent;//true
```

父类的静态方法，也可以被子类继承。

```js
class Parent {
  static sing(){
    return '大王叫我来巡山';
  }
  static say() {
    return '好的, 大王';
  }
}

class Child extends Parent {
  static say() {
    return '请叫我女王大人';
  }
}
Child.sing();// "大王叫我来巡山"
Child.say(); // "请叫我女王大人"
```

#### Object.getPrototypeOf()

`Object.getPrototypeOf`方法可以用来从子类上获取父类。

```js
Object.getPrototypeOf(Child) === Parent;
```

#### super关键字

当`super`在作为方法时, 它代表的父类的构造函数, 但是返回的是子类的实例, 即`super`内部的`this`指的的是子类

```js
class Parent{}

class Child extends Parent{
  constructor(){
    super()
  }
}
//相当于
Parent.prototype.constructor.call(this)
```

当`super`作为对象时, 它指向父类的原型对象`prototype`; 在静态方法中, 指向父类本身

```js
class Parent {
  static arrange(){
    console.log('大王叫我来巡山');
  }

  static say() {
    console.log('- 好的, 大王');
  }

  reply(){
    console.log('- 是的, 大王');
  }
}

class Child extends Parent {
  static say() {
    super.say();
    console.log('- 请叫我女王大人');
  }

  reply(){
    super.reply();
    console.log('- (o_ _)ﾉ');
  }
}
Child.arrange();
Child.say();

new Child().reply();
```

由于super指向父类的原型对象，所以定义在父类实例上的方法或属性，是无法通过super调用的。

```js
class Parent{
  constructor(){
    this.name = "大王";
  }
}

class Child extends Parent{
  say(){
    console.log(super.name)
  }
}

let child = new Child();
child.say();//undefined
Parent.prototype.name = "大大王";
child.say();//"大大王"
```

通过`super`调用父类的方法时，方法内部的this指向子类

```js
class Parent {
  say() {
    console.log('- 好的, 大王');
    this.reply();
  }

  reply() {
    console.log('- 是的, 大王');
  }
}

class Child extends Parent {
  say() {
    console.log('- 请叫我女王大人')
    super.say();
  }

  reply() {
    console.log('(o_ _)ﾉ');
  }
}

new Child().say();

//Child中super.say()等价于
super.say.call(this)
```

由于`this`指向子类，所以如果通过`super`对某个属性赋值，这时`super`就是`this`，赋值的属性会变成子类实例的属性。

```js
class Parent {
  constructor() {
    this.name = "大王"
  }
}

class Child extends Parent{
  constructor(){
    super()
    this.name = "小妖";
    super.name = "小小妖";
    console.log(super.name);//undefined
    console.log(this.name);//"小小妖"
  }
}
```

注意，使用super的时候，必须显式指定是作为函数、还是作为对象使用.

```js
class Child{
  method(){
    console.log(super)
  }
} 
```

最后, 由于对象总是继承与其他对象, 所以可以在任意一个对象中使用`super`关键字

```js
let obj = {
  toString(){
    return `I am ${super.toString()}`
  }
}

console.log(obj.toString())
```

#### 原生构造函数的继承
ES6 允许继承原生构造函数定义子类，因为 ES6 是先新建父类的实例对象this，然后再用子类的构造函数修饰this，使得父类的所有行为都可以继承。
```bash
Boolean()
Number()
String()
Array()
Date()
Function()
RegExp()
Error()
Object()
```

```js
class CustomArray extends Array{
  constructor(...args){
    super(...args);
  }
}
let arr = new CustomArray();
arr.push(123);
console.log(arr)
```
