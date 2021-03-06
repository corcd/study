### ['1', '2', '3'].map(parseInt)返回的内容

`[1, NaN, NaN]`

因为map方法会传递 `item, index, array` 3个内容给回调函数，而`parseInt`会接收两个参数，第二个参数是表示进制数`2-36`之间，所以一次会调用到`parseInt('1', 0); parseInt('2', 1); parseInt('3', 2);`，后面两个返回的是NAN

### 什么是防抖和节流？有什么区别？如何实现？

防抖是指在多次点击的时候只会调用一次

```javascript

function debounce(fn, _this = null, time) {
    let timer = null;
    return function() {
        clearTimeout(timer);
        timer = setTimeout(() => {
            fn.apply(_this, arguments);
        }, time)
    }
}

```

节流是在同一时间内多次点击只会调用最后一次

```javascript

function throttle(fn, _this, time) {
    let timer = null;
    return function() {
        if (timer) {
            return ;
        }
        timer = setTimeout(() => {
            fn.apply(_this, arguments);
            clearTimeout(timer);
        }, time)
    }
}
```

### 介绍下 Set、Map、WeakSet 和 WeakMap 的区别？

Set：键值对形式，唯一，只接受一对一
Map：键值对形式，不唯一，不同的key可能会对应相同的内容
WaekSet：键值对形式，唯一，值只能是复合类型，不能是基础类型
WarkMap：键值对形式，非唯一，只接受对象为键名

### 介绍下深度优先遍历和广度优先遍历，如何实现？

基于树的概念，深度优先是优先遍历当期的分支，当到了叶子节点的时候在回溯，广度优先是优先遍历兄弟节点，等兄弟节点遍历完成在遍历兄弟节点的子节点

### ES5/ES6 的继承除了写法以外还有什么区别？

    - class 声明会提升
    - class 内部是有严格模式
    - class 的所有属性跟方法是不可枚举的
    - class 的所有方法是没有原型的所以不可以使用new
    - class 必须使用new来进行初始化
    - class 无法重写类名

### setTimeout、Promise、Async/Await 的区别

setTimeout是宏任务
promise，async/await是微任务

async/await返回的是一个promise，遇到await的时候会跳出本次方法，直接执行方法之后的内容，等下一个微任务执行的时候会await后面的内容继续执行

一个执行栈是一个宏任务+微任务，执行栈是一个队列先进先出

### 异步笔试题

``` javascript

async function async1() {
    console.log('async1 start');
    await async2();
    console.log('async1 end');
    await async3();
    console.log('async3 end');
}
async function async2() {
    console.log('async2');
}
async function async3() {
    console.log('async3');
}
console.log('script start');
setTimeout(function() {
    console.log('setTimeout');
}, 0)
async1();
new Promise(function(resolve) {
    console.log('promise1');
    resolve();
}).then(function() {
    console.log('promise2');
});
console.log('script end');

```

```javascript

script start
async1 start
async2
promise1
script end
async1 end
async3
promise2
async3 end
setTimeout

```

### JS 异步解决方案的发展历程以及优缺点

    - ajax
        - 容易造成回调地狱
    - promise
        - 能够解决回调地狱的问题但是依旧会有很长的then连接
    - async/await
        - 将异步代码写的跟同步一样

### Promise 构造函数是同步执行还是异步执行，那么 then 方法呢

构造函数是同步的，then方法是异步的

### 简单讲解一下http2的多路复用

http2基于http1.1的请求方式进行了升级，在同一时刻可以针对同一个tcp连接进行多次http请求

### 三次握手以及四次挥手

三次握手是客户端发送请求到服务端，服务端返回，然后客户端进行连接
四次挥手是客户端要断开连接，发送信息给服务端，服务端判断当前还有一个包在传输然后等待，等包传输完成，发送已经完成，然后客户端发送关闭

### 介绍下 npm 模块安装机制，为什么输入 npm install 就可以自动安装对应的模块？

npm 是命令行方法，输入npm install的时候会查找本地的package.lock.json文件，如果存在直接通过lock文件去下载对应路径下的包内容，如果lock文件不存在就会通过packagejson中的包内容，去根据设置的本地镜像源下载对应的包内容

### 有以下 3 个判断数组的方法，请分别介绍它们之间的区别和优劣

`Object.prototype.toString.call() 、 instanceof 以及 Array.isArray()`

Array.isArray() 方法是判断数据最合适的内容，但是他仅仅只能判断方法
instanceof 能够简单的判断数组，但是不可靠，因为他是根据原型链来查找对应的是否存在，所有的原型链尽头都是Object
Object.prototype.toString.call() 能够判断所有的内容

### 说说浏览器和 Node 事件循环的区别

浏览器的事件循环（event loop）是先执行宏任务，然后在宏任务中如果遇到`promise`或者`async/await`将会把内容放到下一个微任务中，如果遇到`setTimeout`或`setInterval`将会将内容放入下一个宏任务，先执行完宏任务再执行微任务，当微任务结束之后会回头看是否还有宏任务需要去执行，不断的循环

node的事件循环，在node11以下的版本会有部分的差异，针对宏任务以及微任务，一定先将宏任务处理完成在去处理微任务，而不是将宏任务与微任务组成一个任务去执行

### 全局作用域中，用 const 和 let 声明的变量不在 window 上，那到底在哪里？如何去获取？。

`const`和`let` 申明的变量是不会直接存在windows的全局作用域上的，他有一个私有的作用域，所以如果定义的变量`const a = '1'`在全局范围直接用`window.a`是无法查找的，可以直接使用`a`而不加window，这样在js中会默认去寻找

### cookie 和 token 都存放在 header 中，为什么不会劫持 token？

cookie是存放在浏览器里面的，而token不一定是存放在哪里，而且基本上是jwt生成的token，后端发放给前端进行校验使用，即使劫持了也不一定能够有效，主要还是防止csrf

### 改造下面的代码，使之输出0 - 9，写出你能想到的所有解法。

```javascript

for (var i = 0; i< 10; i++){
    setTimeout(() => {
        console.log(i);
    }, 1000)
}

// 闭包
for (var i = 0; i < 10; i++) {
    (function(i) {
        setTimeout(() => {
            console.log(i);
        }, 1000)
    })(i)
}

// setTimeout立即执行函数
for (var i = 0; i< 10; i++){
    setTimeout((() => {
        console.log(i);
    })(), 1000)
}

// setTimeout第三个参数
for (var i = 0; i< 10; i++){
    setTimeout((i) => {
        console.log(i);
    }, 1000, i)
}

// let, 局部作用域
for (let i = 0; i < 10; i++) {
    setTimeout(() => {
        console.log(i);
    }, 1000)
}

// promise
for (var i = 0; i < 10; i++) {
    new Promise((resolve, reject) => {
        resolve(i);
    }).then(res => {
        setTimeout(() => {
            console.log(res);
        })
    })
}

```

### 下面的代码打印什么内容，为什么？

``` javascript

var b = 10;
(function b(){
    b = 20;
    console.log(b);
})();

// ƒ b(){
//     b = 20;
//     console.log(b); 
// }

```

1. 函数表达式与函数申明不同，函数名只在内部有效，且是常量，不可修改
2. 对常量进行赋值会默认失败，严格模式下会报错

### 如何使上述内容可输出10跟20

``` javascript

var b = 10;
console.log(b);
(function b(){
    window.b = 20;
    console.log(window.b);
})();

```

### 浏览器缓存规则

[浏览器缓存](https://www.jianshu.com/p/54cc04190252)

### 下面代码中 a 在什么情况下会打印 1？

``` javascript

var a = ?;
if(a == 1 && a == 2 && a == 3){
    console.log(1);
}

a = {
    i: 1,
    toString() {
        return a.i++;
    }
}

```

### 下面代码输出什么

``` javascript

var a = 10;
(function () {
    console.log(a)
    a = 5
    console.log(window.a)
    var a = 20;
    console.log(a)
})()

// undefined , 10, 20
// 解析
// 1. var会有变量提升，所以在函数作用域中，a一开始就定义了，所以第一个输出undefined
// 2. a = 5找的是就近的a变量，所以改变的是函数作用域中的a，window下没有发生变化
// 3. 重新赋值了a=20

```

### 使用 sort() 对数组 [3, 15, 8, 29, 102, 22] 进行排序，输出结果

`[102, 15, 22, 29, 3, 8]`

sort排序是会转换成string来对比

### 输出以下代码执行的结果并解释为什么

``` javascript

var obj = {
    '2': 3,
    '3': 4,
    'length': 2,
    'splice': Array.prototype.splice,
    'push': Array.prototype.push
}
obj.push(1)
obj.push(2)
console.log(obj)

// [,,1,2]

// push方法会通过length的长度上增加一个对应数字量的key，并添加对应的值，对length+1，所以push的时候，第一次key是2，第二次是3，length变为4

```

### call 和 apply 的区别是什么，哪个性能更好一些

call 跟函数一样传多个参数，apply传一个数组

call性能会好一些，因为call的使用除了第一个参数，其他参数都是对应函数的参数，不需要做什么转换，而apply还需要将数组转换成对应的参数

### 为什么通常在发送数据埋点请求的时候使用的是 1x1 像素的透明 gif 图片？

1. 图片不存在跨域
2. 1x1的gif是图片中大小最小的
3. 能够完成一次http请求
4. 执行过程无阻塞

### 要求设计 LazyMan 类，实现以下功能。

```javascript

LazyMan('Tony');
// Hi I am Tony

LazyMan('Tony').sleep(10).eat('lunch');
// Hi I am Tony
// 等待了10秒...
// I am eating lunch

LazyMan('Tony').eat('lunch').sleep(10).eat('dinner');
// Hi I am Tony
// I am eating lunch
// 等待了10秒...
// I am eating diner

LazyMan('Tony').eat('lunch').eat('dinner').sleepFirst(5).sleep(10).eat('junk food');
// Hi I am Tony
// 等待了5秒...
// I am eating lunch
// I am eating dinner
// 等待了10秒...
// I am eating junk food


// 简单实现
function LazyMan(name) {
    console.log(name);
    this.sleep = (time) => {
        setTimeout(() => {
            console.log(`等待了${time}秒`);
        })
        return this;
    }

    this.eat = (food) => {
        setTimeout(() => {
            console.log(`I am eating ${food}`);
        })
        return this;
    }

    this.sleepFirst = (time) => {
        new Promise((resolve, reject) => {
            resolve(time);
        }).then(res => {
            console.log(`等待了${res}秒`);
        })
        return this;
    }
    return this;
}

```

### 箭头函数与普通函数（function）的区别是什么？构造函数（function）可以使用 new 生成实例，那么箭头函数可以吗？为什么？

箭头函数是普通函数的简写方式，但是箭头函数有跟普通函数有几点区别

1. 箭头函数没有申明提升
2. 箭头函数的this是生命的时候的this
3. 箭头函数无法使用new操作符（因为没有this对象，也没有prototype）
4. 没有arguments参数，可使用...reset代替
5. 不可以使用 yield 命令，因此箭头函数不能用作 Generator 函数

### 给定两个数组，写一个方法来计算它们的交集

> 给定 nums1 = [1, 2, 2, 1]，nums2 = [2, 2]，返回 [2, 2]

1. 循环死判断
2. fliter(也是循环判断)

### a.b.c.d 和 a['b']['c']['d']，哪个性能更高？

a.b.c.d性能更好一点，毕竟不需要查找当前是不是一个数组，直接就是对对象的操作

### ES6 代码转成 ES5 代码的实现思路是什么

通过babel将es6代码转换成ast，然后转换成es5的ast在变换源代码

### 介绍下 webpack 热更新原理，是如何做到在不刷新浏览器的前提下更新页面的

使用的是websocket，通过node监听到文件的变化然后服务端发送请求到客户端，客户端重新编译刷新

