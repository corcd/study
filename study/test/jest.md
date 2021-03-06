## jest

jest的测试方式

### 安装

`npm insall jest -D`

或

`npm install jest -g`

建议本地还是安装一个jest，因为可以通过 `jest --init` 来初始化jest的config文件

### 使用方式

1. test

``` javascript

test('test a + b ', () => {
    expect(2 + 2).toBe(4);
})

```

test内部有各种方法可以供自己去做对比

- toBe
- toEqual
- toBeNull
- toBeUndefined
- toBeDefined
- toBeTruthy
- toBeFalsy

数字的对比

- toBeGreaterThan
- toBeGreaterThanOrEqual
- toBeLessThan
- toBeLessThanOrEqual

浮点数对比

- toBeCloseTo

正则对比

- toMatch

数组比较其中是否含有某项

- toContain

特定报错对比

- toThrow

[完整的对比列表](https://jestjs.io/docs/zh-Hans/expect)

### 测试异步内容

可使用promise，也可以是用done的参数传入，还有就是await和async

### 共同的内容分离

可以使用beforeEach和afterEach进行统一的内容出来

也可以全局写beforeAll和afterAll

### 作用域

使用 `describe` 可以形成测试作用域，独立测试内容

顶级的钩子是可以也是会执行的

``` javascript

beforeAll(() => console.log('1 - beforeAll'));
afterAll(() => console.log('1 - afterAll'));
beforeEach(() => console.log('1 - beforeEach'));
afterEach(() => console.log('1 - afterEach'));
test('', () => console.log('1 - test'));
describe('Scoped / Nested block', () => {
  beforeAll(() => console.log('2 - beforeAll'));
  afterAll(() => console.log('2 - afterAll'));
  beforeEach(() => console.log('2 - beforeEach'));
  afterEach(() => console.log('2 - afterEach'));
  test('', () => console.log('2 - test'));
});

// 1 - beforeAll
// 1 - beforeEach
// 1 - test
// 1 - afterEach
// 2 - beforeAll
// 1 - beforeEach
// 2 - beforeEach
// 2 - test
// 2 - afterEach
// 1 - afterEach
// 2 - afterAll
// 1 - afterAll

```