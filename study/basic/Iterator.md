## 遍历器（iterator）

在es6之前想要遍历只能通过js原生提供的方法进行数据遍历，对于一些不可迭代的属性进行遍历是比较麻烦的事情，例如object属性，他本身是不可迭代的，想要进行迭代只能先获取当且的keys属性，然后通过keys属性进行迭代

在es6里面添加一个新的迭代方法 `for...of` ，他通过使用iterator接口自定义的遍历的将要遍历的内容，在es6里，默认的iterator接口指的是`Symbol.iterator` ，也就是说当一个对象中存在 `Symbol.iterator` 的时候那么就认为这个对象是可遍历的

但是对于一些属性而言他们本身就存在iterator可迭代的属性，例如数组

``` javascript

const arr = [1, 2, 3];
const iter = arr[Symbol.iterator]();

iter.next() // {value: 1, done: false}
iter.next() // {value: 2, done: false}
iter.next() // {value: 3, done: false}
iter.next() // {value: underfined, done: true}
iter.next() // {value: underfined, done: true}

```

当next方法被调用超过了当前数组的长度的时候返回的就是undefined和done为true的内容

如果直接对object对象进行遍历的话，那么就会直接进行报错，因为对于object本身来说他是不可遍历的

![遍历](../public/image/6.png)

当然也是有办法将不可遍历的内容变成为可遍历的属性

```javascript

const obj = {
    '1': 1,
    '2': 2,
    '3': 3
}

obj.__proto__[Symbol.iterator] = function() {

    const keys = Object.keys(this);
    let currentIndex = 0;

    this.next = () => {
        if (currentIndex >= keys.length) {
            return { done: true }
        } else {
            return { value: obj[keys[currentIndex++]], done: false }
        }
    }

    return this;
}

for(let value of obj) {
    console.log(value)
}

// 1
// 2
// 3

```

为object对象添加了 `Symbol.iterator` 方法，并内部实现了next的迭代方案，这样就可以通过 `for...of`来进行遍历

当然我们也可以通过借用Array的迭代器接口去对obj进行改造

``` javascript

const obj = {
    0: 1,
    1: 2,
    2: 3,
    length: 3,
    [Symbol.iterator]: Array.prototype[Symbol.iterator]
}

for (let v of obj) {
    console.log(v);
}

// 1
// 2
// 3
```

当然这个方法有很大的局限性

- key值必须是跟数组一样为0，1，2可连续的，一但中间断过则无法输出，直接为undefined
- 必须在obj对象中添加length属性，迭代会出现错误
- Symbol.iterator也必须写入

对于迭代器而言，只要将所需的内容上加上 `Symbol.iterator` 那么就默认这个对象是可迭代的，不管当前这个是class还是function都是可以实现可迭代的属性

## 遍历器生成函数（generator）

`generator` 是es6新增的一个异步解决方案，通过使用 `yield` 来等待执行结果返回并获取的一个实现方案，将异步执行转换为同步执行的一个表达式

``` javascript

function * generator(x, y) {
    const a = yield x + y;
    return a;
}

const gen = generator(1, 2);

gen.next(); // 3

```

实际上 `generator` 与 `iterator` 的关系非常接近，本质上就是将函数变为一个可迭代的内容，所以用 `for...of` 也是可以对其进行迭代的

```javascript

function * generator(x, y) {
    yield x + y;
    yield 2*x + y;
}

const gen = generator(1, 2);

console.log(gen === gen[Symbol.iterator]()) // true

for(let v of gen) {
    console.log(v);
}

// 3
// 4

```

在实际运用中，会发现当你在把`yield`返回的值运用到下一个`yield`里的时候，会发现前一个值为`undefined`，因为`yield`方法本身是没有返回值的，他只会返回 `undefined` ，所以你并不能直接将运算结果带到下一步的执行内容里面，但是next函数帮我们提供了这么一个可以修改的办法

``` javascript

function * generator(x, y) {
    const a = yield x + y;
    yield 1 + a;
}

const gen = generator(1, 2);

const a = gen.next();   // { value: 3, done: false }
// gen.next()           // { value: NaN, done: true }
gen.next(a.value);      // { value: 4, done: true }

```

当然在这个里面，next的参数，只会默认代替前一个 `yield` 需要带上的参数，如果两个`yield`中间还有一个的话，那么这个传入的参数就不会起作用

``` javascript

function * generator(x, y) {
    const a = yield x + y;
    yield 1 + 2;
    yield 1 + a;
}

const gen = generator(1, 2);

const a = gen.next();   // { value: 3, done: false }
gen.next()              // { value: 3, done: false }
gen.next(a.value);      // { value: NaN, done: true }

```
