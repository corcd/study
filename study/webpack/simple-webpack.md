## webpack的简易实现

### ast

> 用处

模板引擎解析

编程语言相互转换

> ast解析引擎

https://esprima.org/

### 需要做的事情

1. es6转换成es5
    - babylon 生成AST
    - babel-core 将AST转换成源码
2. 分析模块之前的依赖关系
    - babel-traverse 的 importDeclaration 方法获取依赖属性
3. 生成js文件

### 实现

https://github.com/HuskyToMa/simple_webpack