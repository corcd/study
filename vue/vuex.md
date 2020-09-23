# vuex

这次来聊一聊vuex的内容，由于公司最近用的项目一直都是vue，所以研究了一会vuex

## 为什么要用vuex

首先在使用之前，肯定会有一定的需求会使用到vuex，基本我公司的现状，我们使用vuex的理由一共只有以下两点

1. 组件层级比较深，使用vuex做数据层
2. 公用数据的维护以及处理

确实，正常应用，只要我们设计到这两个方向上，总会去选择一款状态管理的库，而在vue里面，最常用的就是vuex

那么，vuex具体做了什么事情

## vuex做了什么

### 数据集中管理

vuex申请了一个store，将所有数据维护在一个store上面，同时，将这个store挂载在了vue上，这就说明了一件事情，只要我们的应用启动，那么我们的数据中一定会带上store的数据，不管我们是否需要

### 纯函数修改状态

vuex使用了mutation来做一种修改状态的方法，但是，基于vue observe的原因，直接修改store中的数据依旧能够起到修改数据的作用，所以难以起到一种强制规范走mutation的过程，同样的action也存在同样的问题，在vuex中，action是用来处理有副作用的方法，但是在实际使用过程中，并没有完全用上action，因为使用的数据是跟全局数据无关，直接使用的请求然后修改数据

那么，mutation跟action，唯一起到的作用就是只有一个聚合，一个mutation或者一个action对应的一个事件，一个响应

例如，我们获取用户信息，可以设置成一个action，但是除此之外，并没有体现出了action跟mutation的好处

### 模块

vuex可以申请多种模块，来将数据分割开来，但是所有的数据依旧还是挂载在vue上面，从本质上来讲，就是一个Object，将不同的module解析成了不同的key来塞到vue的Object里面，并实现了observe的监听

### 插件

在vuex里面，插件类似于一个中间层的概念，没触发一个mutation将会触发所有的plugins，并在在插件当中是不能直接修改store中的数值，毕竟不是直接存在于vue上面，vue的defineObjectPrototype并不能作用于plugins中，所以直接修改并不能导致我们页面进行更新

## 横向对比下redux

### 相同点

1. 数据存储方式相同，都是将完整的数据存放在store中
2. 都提供了一个纯函数的方式修改store中的数据
3. 都提供了action，并且触发的方式都是通过dispatch

### 不同点

1. redux的action是一个type类型，不提供任何操作，只触发reducer，而vuex中的action是一个操作副作用的方法
2. redux的数据流向只能是发起一个action来进行数据变更，而vuex却不仅仅是只有mutation，action方法去修改，还可以直接进行赋值修改
3. redux采用compose组合的方式提供中间件的能力，vuex是采用observe的方法添加插件，监听修改
4. redux在全局挂载了provider之后还需要使用connect连接组件才能使用store中的数据，vuex直接挂载在vue上，全局影响
5. redux可以使用在任何地方， vuex只能使用在vue里面
6. redux存在时间回溯，即他只能通过reducer修改，所以能够回溯到几次修改之前的操作，而vuex没有这个能力

## 回过头来看vuex

根据上面的异同点，不难发现，vuex本身只是在vue中使用的一个小小的库，设计的并不是特别的强大，提供了基础的全局使用的能力以外，就是插件监听mutation的功能，无法在这之上做操作，这个可能也是由于是一个observe的应用引起的难以操作的地方

那么如果一个应用中并没有使用到vuex中的plugins的时候，那么vuex存在的必要性在哪里，他仅仅只提供了一个全局数据存储的能力，而并没有其他的任何实际用处，就本质而言，直接在Vue上面挂载一个我们想要的全局数据，并且封装完成所有的跟数据相关的方法，我们依旧能够实现当前vuex所提供的能力，并且，还可以做到按需引用的操作，只有在需要使用的页面，我们才进行数据的引用，挂载在vue上面，当当前页面退出的时候，直接将挂载在vue上的数据清除（这个数据并不是全局使用的数据），那么会不会减少vue的消耗呢？

## 继续来看vue3.0

在vue3.0里面，提供了一个