# sandbox

沙盒， 在javascript中指代的是js环境隔离， 即在当前沙盒中无法访问到对应的window下的变量

## 使用地方

1. 需要隔离当前应用环境的， 线上的ide
2. 微前端， 各项目独自隔离

## 实现原理

理解一下， 沙盒是隔离当前的环境， 那么就是说在我们当前的环境下， 他的作用域是被隔离的，在js里面，能够实现作用域独立出来的方法一共有以下两种

1. eval
2. with

### eval

eval的话，是直接执行我们的当前的一个字符串， 但是所有的作用域链还是能够访问， 所以不适合做我们的沙箱环境

### with

我们知道， with是可以传入一个变量的，所有的访问window的会先访问到with传入的变量，所以可以通过劫持当前变量，设置`get`和`set`来拦截当前的内容，阻止访问到我们的全局变量