# 数组

至今为止，2020年初，数组的方法已经非常的多了

在`Array`上的方法一共只有三个`from, of, isArray`

在原型链上的方法有非常之多，一共可以分为以下几类

## Array 静态方法

### from

from能够将一个具有类似数组的数据结构转换成数组

> 类数组结构

1. 存在length
2. 存在0,1,2,3,4的key

典型的内容就是`arguments`, function的参数都是可以当做是一个类数组`Array.from(arguments)`;

同时还可能会出现以下问题

```javascript

const obj = {
    1: '1',
    2: '2',
    length: 2
}

console.log(Array.from(obj)); // [undefined, '1']

// 因为，from的时候会拿length来当做数组的长度，然后拿对象的键值当做key，数组实际上是的key都是0，1，2，3，4等等
```

### of

of 方法是创建一个数组所用，跟正常的`constructor`方法来说，存在一定的区别

``` javascript

Array.of(2)     // [2]
new Array(2)    // [,] 不是undefined，而是空

```

### isArray

判断当前内容是否是数组

``` javascript

Array.isArray([]) // true
Array.isArray({}) // false

```

## 数组原型链上的方法

针对几个不常用的拿出来说明一下

1. 循环操作
    - map
    - reduce
    - reduceRight
    - filter
    - every
    - some
    - keys
    - values
    - entries
    - forEach
    - find
    - findLast
2. 查询（访问）
    - indexOf
    - lastIndexOf
    - slice
    - join
    - toString
    - toLocalString
    - includes
3. 数组赋值（修改器）
    - sort
    - copywithin
    - fill
    - pop
    - push
    - shilt
    - unshilt
    - splice
    - reverse


### entries

返回的是一个`iterator`，里面存在着`key,value`

只有在需要一步一步的调试当前数组的时候，才可能会用到这个函数，他返回的迭代器，可以跟generator一样，使用next不断下移

``` javascript

const arr = [1,2,3,4,5];
const iter = arr.entries(); // [0, 1]

// 可用for...of循环使用

```

### find

获取第一个满足条件的元素， 参数是一个`function`，返回元素内容

### copywithin

复制当前数组的第几个元素到对应位置上去

``` javascript

const arr = ['a', 'b', 'c', 'd', 'e', 'f'];
arr.copywithin(0, 1, 3) // ['b', 'c', 'c', 'd', 'e', 'f']
arr.copywithin(1, 2, 3) // ['a', 'c', 'c', 'd', 'e', 'f']
```

### fill

使用固定的元素覆盖对应位置的元素

```javascript

const arr = ['a', 'b', 'c', 'd', 'e'];
arr.fill(1, 1, 2); // ['a', 1, 'c', 'd', 'e']
arr.fill(1); // [1, 1, 1, 1, 1]
arr.fill(1, 1); // ['a', 1, 1, 1, 1]

```
