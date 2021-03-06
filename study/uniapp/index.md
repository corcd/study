# uniapp项目搭建

## 项目搭建

1. vue-cli 直接生成ts+uniapp的项目
2. 添加eslint，删除原先的tslint配置以及添加eslint对应的插件配置
3. 添加pre-commit，检测commit之前的变动，主要还是eslint的检测
4. jest自动化组件测试（在components文件夹中的组件都需要添加jest测试）（待添加）

## 项目框架

```

- public            // 公共出口文件夹
- src               // 项目代码
    - entry         // 入口依赖文件
    - components    // 组件
    - consts        // 静态变量
    - interface     // 公用的ts接口
    - pages         // 页面
    - sass          // 公共sass目录
    - services      // 接口
    - static        // 静态文件
    - store         // vuex store
    - utils         // 公用方法

```

## eslint的问题

1. class中的方法没有使用this的时候使用static
2. 函数针对传入的参数是否支持直接在参数上做修改

## eslint的配置

在原先的uniapp直接下载下来的框架中是不存在eslint的，他的typescript是依赖于tslint做的一层检查

现在手动为其添加eslint配置

### 添加eslint

vue add eslint

### 配置eslint的插件

根据喜好配置对应的eslint内容，我这边现在用的airbnb的配置

### 配置precommit插件

> npm i pre-commit - D

添加eslint检测，在上传之前检测eslint是否存在错误

`eslint --ext .vue,.ts,.tsx --quiet src`

## ts + vue 的问题

1. computed的类型问题

``` javascript

// computed 默认写法
computed: {
    test(): object {
        return this.testData
    }
}

// this.test 的默认返回值是 () => string 而不是string
// 会导致this.test.a的时候会报错
//  Property 'a' does not exist on type '() => object'

```

## 基于问题选择使用

针对上述的一系列问题，可以选择使用class模式的vue开发流程

添加jsx，即可将vue与react的变的及其相似，可以说开发模式都类似了
