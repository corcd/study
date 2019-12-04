## prototype

原型链，在js中是一个很常见的也是一个很常用的方法，用于继承
但是在js中这个也是一个比较晦涩难懂的，因为他实际上会是比较饶的一个原理

### prototype，\_proto\_，constructor三者的关系

> prototype

是一个指针，指向的是原型的constructor方法

> proto

是一个实例对象的属性，指向该实例的原型

> constructor

是一个原型的构造函数，含有prototype

由上可知，prototyp指向的是一个原型的constructor方法，二constructor方法中含有prototype，那么这就是一个无限的死循环

``` javascript

// 原型
function superType() {
    this.super = '123'
}

// 输出的是一个对象，内部含有原型的constructor方法
// {constructor: f}
console.log(superType.prototype)
// 指回原型方法
console.log(superType.prototype.constructor)
// {constructor: f} 指回构造函数
console.log(superType.prototype.constructor.prototype)

const test = new superType();

// prototype 只存在于原型对象上，不存在实例对象上面，
// 所以不能够直接使用prototype访问，只能通过_proto_访问到原型的prototype
console.log(test._proto_);

```

### 继承

原型链的继承其实相对而言很简单，只需要将要继承的原型的prototype指向继承的实例即可

``` javascript

// 原型链继承
// 方法1
function superFunc() {
    this.test = '123';
}
// 方法1上的prototype添加getValue方法
superFunc.prototype.getValue = function () {
    return this.test;
}
// 方法2
function subFunc() {
    this.subTest = '555';
}
// 实例化方法1
const superType = new superFunc();
// 将方法2的prototype指向方法1的实例
subFunc.prototype = superType;
// 实例化方法2
const interface = new subFunc();
// 方法2继承了方法1的内容
console.log(interface.getValue());  // 123
console.log(interface.subTest);     // 555

```

代码可能看的会有点饶，看图可能会更加清晰一些

![prototype流程图](../public/image/3.png)

### 总结

1. Object是所有的对象的终结点，就是所有的对象的爸爸，根据原型链查询最后都会走到Object的方法上
2. 实例化后的对象是不能获取prototype属性的只能通过__proto__
3. 每个__proto__属性指向的是原型的prototype