# Promise

## 异步编程
1. 同步模式：后一个任务等待前一个任务结束，然后再执行，程序的执行顺序与任务的排列顺序是一致的、同步的

2. 异步模式：每一个任务分成两个部分，第一部部分对外部数据发起请求，第二部分会写成一个回调函数，包含了对外部数据的处理。第一部分代码执行完，不是会立刻执行第二部分代码，而是将程序的执行权交给第二个任务。等到外部数据返回了，再由系统通知执行第二部分代码。


## 回调函数

## Promise 概念

1. Promise 是一个对象，可以获取异步操作的消息，各个异步操作都可以使用相同方法进行处理。
2. 对象封装了三种状态，不受外界影响：pending(进行中)、fulfilled(已成功)、rejected(已失败)。
3. 一旦状态改变，就不会再变，任何时候都可以得到这个结果。

## Promise基本用法

1. resolve和reject
2. 执行顺序

## Promise.prototype.then()

1. 接收resolve和reject执行结果
2. 返回一个新的Promise对象

## Promise.prototype.catch()
1. 捕获Promise对象执行过程抛出的异常、接受reject执行结果
2. 内部异常不影响外部任务执行，且会被“吞掉”

## Promise.all()

1. const p = Promise.all([p1, p2, p3]);
2. 将多个Promise包装成一个Promise对象，最终将返回多个Promise结果的数组
3. 当所有状态都变成fulfilled，最终状态才为fulfilled
4. 其中一个状态为rejected, 最终状态即为rejected

## Promise.race()

1. const p = Promise.race([p1, p2, p3]);
2. 将多个Promise包装成一个Promise对象，最终将返回最先执行完的Promise结果

## Promise.resolve()

1. 将现有对象转化为Promise对象
2. 参数是Promise对象，保持改Promise对象
3. 参数thenable对象，立即执行对象then方法，且状态变为已完成
4. 普通参数，返回一个状态为已完成的Promise对象
5. 没有参数，返回一个状态为已完成的Promise对象

## Promise.reject()

1. 返回一个新的Promise对象，状态为rejected
2. 它的参数会作为后续方法的参数



