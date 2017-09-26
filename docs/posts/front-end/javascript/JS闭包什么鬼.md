# JS原型链什么鬼


## 前言

闭包问题是js中比较困扰人的一个问题，现在我们来探究这个到底是什么鬼

## 问题起源

### 问题：循环异步调用

典型的JS初学者犯下错误

```javascript

for (var i = 0; i < 10; i++) {
  setTimeout(function () {
    console.log(i)
  })
}
```

这个函数看上去应该输出0 , 1 , 2, 3, 4, 5, 6, 7, 8, 9，但是实际上执行会发现输出了10个10

为什么？

## 闭包

js闭包最简单的解释：函数x返回函数y，y中所有的外层变量（包括x的变量）形成闭包。在y中可以使用外层所有的变量，并且y引用的x中的变量将会与函数y共存亡，即使x已经运行完毕被销毁。

Example:

```js
function inner
```


## 词法作用域

> In languages with lexical scope (also called static scope), name resolution depends on the location in the source code and the lexical context, which is defined by where the named variable or function is defined
> 在使用词法作用域（静态作用域）的语言中，名字解析依赖于名字在代码中出现的位置以及词法上下文，而这是在命名的变量或函数处定义的。

Example:

```
function p () {
  print x
}

function q () {
  variable x = 1
  p()
}
```

以上函数示例演示的是动态作用域的闭包。这个在词法作用域的语言中将无法使用，因为在词法作用域中，名字解析是依赖于名字所在代码的位置以及词法上下文的。在函数p定义的时候，显然在他的词法上下文中并不能找到变量`x`的解析结果，因此这种情形在词法作用域中无效。

现代编程语言大多数使用词法作用域，其中包括我们日常使用的Python、Javascript。

初学者对词法作用域的"闭包"会非常疑惑，需要理解的是“词法作用域中的闭包，取决于闭包**定义**时候的上下文，而不是调用他的时候的上下文”。

Example: 有效闭包

```js
function outer () {
  var value = 0
  return function inner () {
    return value++
  }
}

var count = outer()

count() // 0
count() // 1
count() // 2
```

Example：无效闭包
```js

function inner () {
  return value++
}

function outer () {
  var value = 0
  return inner
}

var count = outer()

count() // Uncaught ReferenceError: value is not defined
```

## 函数作用域

## 应用

## 内存泄漏问题

****
## Further Reading

- [wikipedia: Scope](https://www.google.com.hk/url?sa=t&rct=j&q=&esrc=s&source=web&cd=2&ved=0ahUKEwi_ibfb8KTWAhUNPrwKHafbBfQQFggsMAE&url=https%3a%2f%2fen%2ewikipedia%2eorg%2fwiki%2fScope_%28computer_science%29&usg=AFQjCNHUMAHHqsb5wZiYjLfQC2aR__lgbA)
- [what is javascript lexial scope?](https://stackoverflow.com/questions/1047454/what-is-lexical-scope)