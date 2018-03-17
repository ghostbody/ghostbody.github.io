
1. 原型链
    - typeof(null)
2. this指向懵逼
    - 构造函数错误调用，非new调用
3. promise
3. 布尔值和类型转换
    - "" == 0
    - "", {}, [], null, undefined, NaN
    - [0] == false, Boolean([0]), [''], ['0'], [null], [undefined]
    - var b = 10 + 20 + 'abc' + 'cd'
    - console.log(false == '0');
    - console.log(null == undefined);
    - console.log(" \t\r\n" == 0);
    - console.log('' == 0);
    - Number和parseInt
        - Number(false) 0
        - Number(true) 1
        - Number(undefined) NaN
        - Number(null) 0
        - Number("5.5 ") 5.5
        - Number("56 ") 56
        - Number("5.6.7 ") NaN
        - Number(new Object()) NaN
        - Number(100) 100
5. 数字精度
    - Math.pow(2,53) +1 === Math.pow(2,53) // true
    - 0.1+0.2!=0.3和9999999999999999 == 10000000000000000;
6. 字符串
    - 3..toString;
    - replace 只替换一个
5. 默认全局声明
6. var 变量提升
7. 函数作用域
8. 分号问题
9. es6
    - 运行时支持问题
        - Chrome旧版本与严格模式限制
        - node旧版本不支持
        - babel is not es6
    - var let = 1
    - let 块级作用域、临时死区、不存在变量提升、不允许重复声明
    - 箭头函数返回字面量的object
    - 箭头函数不能作为构造函数
10. lodash & angular
    - 副作用函数，和非副作用的函数
    - 字符串解析
    - isObject 不一样逻辑
11. Date类使用的时间戳和unix的差别

## 原型链

### typeof(null)

```js
typeof(null) === 'Object' // true!
```

错误的判断对象

```js
var a = {}
if (typeof a === 'Object') {
    // ...
}
```


所以使用typeof判断对象的正确使用方法

```js
var a = {}
if (a != null && (typeof a === 'object' || typeof a === 'function')) { // 注意函数也是object...
    // ...
}
```

### 忽略原型链属性

```js
let sumUp = {}
let content = "providing constructor for your classes. A class contains constructors that are invoked to create objects from the class blueprint"

for (let word of content.split(' ')) {
    if (!sumUp[word]) {
        sumUp[word] = 1
    } else {
        sumUp[word] += 1
    }
}

console.log(sumUp)
```

```
{ providing: 1,
  constructor: 'function Object() { [native code] }1',
  for: 1,
  your: 1,
  'classes.': 1,
  A: 1,
  class: 2,
  contains: 1,
  constructors: 1,
  that: 1,
  are: 1,
  invoked: 1,
  to: 1,
  create: 1,
  objects: 1,
  from: 1,
  the: 1,
  blueprint: 1 }
```

```
let sumUp = Object.create(null)
```

### for...in 操作

运维开发写python写的比较多。但是在python中的for in 和js中的for in 不是同一个含义。


!!! info "for...in循环定义"
    for...in 循环只遍历可枚举属性。像 Array和 Object使用内置构造函数所创建的对象都会继承自Object.prototype和String.prototype的不可枚举属性，例如 String 的 indexOf()  方法或 Object的toString()方法。循环将遍历对象本身的所有可枚举属性，以及对象从其构造函数原型中继承的属性（更接近原型链中对象的属性覆盖原型属性）。

!!! info "可枚举属性"
    可枚举属性是指那些内部 “可枚举” 标志设置为 true 的属性
    默认标记为true情况：
        - 字面量方式定义的属性
        - 直接的赋值的属性

#### Example: 忽视原型链属性导致的错误

```js
let arr = [1, 2, 3, 4, 5]

for (let x in arr) {
    let ele = arr[x]
    console.log(ele)
}
// 1
// 2
// 3
// 4
// 5
```

```js

Array.prototype.test = function () {}

let arr = [1, 2, 3, 4, 5]

for (let x in arr) {
    let ele = arr[x]
    console.log(ele)
}
// 1
// 2
// 3
// 4
// 5
// test <-- wrong!
```

数组遍历推荐使用 for...of...

!!! note "语法"

    ```js
        for (variable of iterable) {
            //statements
        }
    ```

#### Example: for...of用法

可以理解为真正的js类似python的for each循环

```js

Array.prototype.vlaue = 'test'

let arr = [1, 2, 3, 4, 5]

for (let x of arr) {
    console.log(x)
}
```


### 实力懵逼的原型链关系

- `typeof null // Object ???`
- `Object instanceof Object // true`
- `Function instanceof Object // true ???`
- `Object instanceof Function // true ???`
- `Function instanceof Function // true ???`
- `Function.prototype instanceof Object // true`
- `Function.prototype instanceof Function // ture ???`
- `Object.__proto__.__proto__.constructor == Object // true ???`
- `Function.constructor === Function // true ???`

还是判断一个变量是不是对象，使用instanceof
