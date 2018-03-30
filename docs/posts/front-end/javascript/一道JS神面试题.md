
!!! info "面试"
    最近在面试前端同学，问到一题神题，基本上大家都不能很好地回答

![](02-juewang.png)

## 以下JS代码输出什么？

```js
for (var i = 0; i < 10; i++) {
  setTimeout(function () {
    console.log(i);
  })
}
```

## 大家的答案

- 答案1：输出0-9 -> 你只是用过JS而已
- 答案2：输出10个9 -> 你的理解不透彻
- 奇葩答案3：输出一个10 -> 为什么？ -> 因为这个循环执行得很快 -> ???
- 其他答案4：输出10个0 -> ???

## 正确答案

输出10个10。

## 为什么输出10个10？

基本上很少同学解释得清楚。我们来看执行顺序。

1. 执行10次 setTimeout(callback) 这个函数，执行完后 `i` 的值为10
2. 第一次注册在setTimeout的callback执行
  - callback函数中没有`i`这个变量，于是往外层作用域寻找 
  - 在上层作用域找到`i`这个变量，并且值为10
  - 执行console.log，输出10
3. 与2一样执行剩下的callback

## 如何改写才能让他输出 0-10，且不能去掉setTimeout?

基本上同学们冥思苦想，绞尽脑汁。

### 方法1：用一个立即执行的函数

用一个立即执行函数包裹，使得setTimeout注册的callback执行的时候，找到的上级作用域中，是已经保存的变量的值。

```js
for (var i = 0; i < 10; i++) {
  (function (i) {
    setTimeout(function () {
      console.log(i);
    })
  })(i);
}
```

或者

```js
for (var i = 0; i < 10; i++) {
  (function () {
    var j = i;
    setTimeout(function () {
      console.log(j);
    })
  })();
}
```

或者

```js
function _loop(i) {
  setTimeout(function () {
    console.log(i);
  })
}

for (var i = 0; i < 10; i++) {
  _loop(i);
}

```

### 方法2：使用es6的let，创建块级作用域

```js

for (let i = 0; i < 10; i++) {
  console.log(i);
}

```

## 为什么考察这题？

本题目考察JavaScript编程中经典的坑，如下例子：

```js
var buttons = $('.my-buttons')

for (var i = 0; i < buttons.length; i++) {
  buttons[i].click = function () {
    console.log(i);
  }
}

```

这是很多初学者经常犯的错误


本题目考察的知识：

1. 作用域。是否知道var是函数作用域，大部分初学者会认为存在块级作用域
2. 闭包。什么是闭包，如何通过闭包原理，正确地处理异步循环
3. es6的let，回答到这里还会继续深入问下去，比如临时死区等问题

主要考察面试者对这些基础知识的深度，可谓是相当的严厉。

目前效果看来，这是一道神题目，很多面试无法回答解释。

