
# Know this,use this!

摘录自：[link](https://mp.weixin.qq.com/s?__biz=MzI4MzQxMzY5MQ==&mid=2247483781&idx=1&sn=f4b664e2d0e7db327d0f0cdd60e43e93&mpshare=1&scene=1&srcid=0905AtkpBcgaqKV11awXnx1p#rd)

> this应该是一个讨论了很久的话题了。其中，关于this的文章，在很多的博客当中也有很多介绍，但是，以前我都是一知半解的去了解它，就是看博客当中，只介绍了一些情况下的 this 的使用方式，但是也并没有自己去做过总结。刚好是在掘金当中有看到一篇关于this的一些详细文章，文末会附上链接以及英文原文，这里纯粹是自己进行一个总结，以后方便自己进行回顾以及加深印象。希望这篇文章对于你了解this有一定的帮助，文末还有一些练习题噢~希望真的对你们有帮助。(因为写项目过程中，一直被 this 坑过，却找了很久的 bug ，我真是 乐了狗)

在了解this之前，相信大家都应该会知道作用域这个知识点的存在，函数在创建之后，会构建自己的执行环境以及作用域，这是一开始就确定了。但是实际的上下文(context)环境，也可以理解为就是this，它是动态确定的，即在函数运行时才确定this所指向的对象，而非声明时所指向的对象。

关于this，总结起来，主要有以下几个途径能够被运用到。

## 对象方法中调用this
如果函数被当中对象的一个方法进行调用，则this值指向该对象。
```
var person = {
    name: 'Alice',
    sayName: function() {
        alert('welcome ' + this.name);
    }
}

person.sayName();    // this == person, alert: 'welcome Alice'
```
在这里，函数的this指向该对象(即 person)；但是有一点需要注意，就是当对象的方法被赋予给一个变量时，其则变为了函数触发，此时的this为 window 或者 undefined(严格模式下)，如下：
```
var name = 'Bob';
var person;    // 即上面的定义，此不拓展详细，直接使用

var say = person.sayName;    // this == window || undefined
say();    // 'welcome Bob' || throw an error: Cannot read property 'name' of undefined(...)
```

##  函数内部使用

在函数内部当中使用了 this，即函数被当做方法使用，不同于 1 当中作为对象的方法使用，此时调用，是在全局作用域下进行调用，即在window下进行调用，由定义可以知道，在全局作用域下声明一个函数，其自动加为window的一个属性。this此时名正言顺的会指向window，严格模式下为 undefined
```
function sayThis() {
    alert(this == window);// true
}
```
结合第一点，函数作为对象的一个方法使用，这里存在一个小坑，即闭包，啥是闭包，这个在这里就不扯开了，最简单的理解就是 Function that returns function，如果不理解什么是闭包的话，可以去翻翻 《JavaScript 高级程序设计》第七章关于闭包的相关内容。第一点当中存在一个小坑，就是将对象的方法赋予给一个变量的时候，其变为函数触发，此时的 this 实际上是指向 window(非严格模式)。

那么，当函数中返回一个函数，此时在对象当中调用该方法，其就相当于是函数触发，此时的 this，在不做任何上下文绑定的前提之下，其指向 window(非严格模式)。
```
var name = 'Bob',
    person = {
        name: 'Alice',
        sayName: function() {
            console.log(this === person);    // true
            return function() {
                console.log(this === person);    // false
                console.log(this === window);    // true
                console.log(this.name);          // Bob
            };
        }
    };

person.sayName()();
```
当然，要解决这个问题的方法，很简单，就是给他绑定一个上下文。
```
var name = 'Bob',
    person = {
        name: 'Alice',
        sayName: function() {
            console.log(this === person);// true
            return function() {
                console.log(this === person);    // true
                console.log(this === window);    // false
                console.log(this.name);          // Alice
            }.bind(this);
        }
    };

person.sayName()();
```

### new 当中进行使用

我们知道在使用 new 方法创建对象的时候，会经过如下这些个过程：
创建对象，将 this 值赋予新的对象
调用构造函数，为 this 添加属性和方法
返回 this 给当前的对象
```
function Person(name, age) {
    this.name = name;
    this.age = age;
}

var person1 = new Person('Alice', 29);
console.log(person1.name);    // Alice
```
这里要记得使用 new 运算符，否则，其只能算是普通的调用，而不是创建一个新的实例对象。而当做普通函数调用的话，实际上即 第 2 种情况下，对函数普通调用，此时的 this 指向 window
```
function Person(name, age) {
    this.name = name;
    this.age = age;
    return this;
}
    
var person1 = Person('Alice', 29);
console.log(person1.name);    // Alice
console.log(window.name);     // Alice
console.log(person1 === window);    // true
```
这是正常情况下，this 会正确返回并且指向该对象，但是在构造函数当中，如果返回了一个对象，那么 this 会指向返回的那个对象。
```
function Person(name, age) {
    this.name = name;
    this.age = age;
    return {
        name: 'Bob'
    };
}
        
var person1 = new Person('Alice');
console.log(person1.name);    // Bobconsole.log(person1.age);// undefined
```
题外话，类似的，联想到 var a = new Person()，则 a instanceof Person一定返回 true吗？留给你们想一想咯。

## 使用 call、apply 或 bind 改变 this

在引用类型 Function当中，函数存在两个方法属性，call 和 apply，在 ECMAScript5当中，加入了 bind 方法。题外话，他们三者区别，应该都知道了吧，不知道的加紧补习呀。
```
var name = 'Bob';var person = {
    name: 'Alice',
    age: 29
}

function sayName() {
    console.log(this.name);
}

sayName.call(person);    // Alice
```
这里是使用了 call 方法来改变了 this的执行环境，至于使用 apply，效果一样，只是二者差别在于传入参数的不同。
```
func.call(context, arg1, arg2, ...)
func.apply(context, [arg1, arg2, ...])
```
使用 bind 方法进行上下文的改变，bind 方法与 call 和 apply有着本质的不同，其不同点是，bind()函数返回的是一个新的函数，即方法，而后两者则都是立即执行函数，使用的时候即调用了该函数，返回方法操作的结果。

并且，使用 bind()方法创建的 上下文，其为永久的上下文环境，不可修改，即使是使用 call 或者 apply方法，也无法修改 this 所指向的值。
```
var name = 'Bob';
var person = {
    name: 'Alice',
    age: 29
}
function sayName() {
    console.log(this.name);
}
    
var say = sayName.bind(person);
say();        // Alice
sayName();    // Bob
```

## 箭头函数

箭头函数并不创建其自身的上下文，其上下文 this，取决于其在定义时的外部函数。

并且，箭头函数拥有静态的上下文，即一次绑定之后，便不可再修改，即使是用了 第 4 种用途当中的改变上下文的方法，也不为之动容。
```
var num = [1, 2, 3];

(function() {
    var showNumber = () => {
        console.log(this === num);    // true
        console.log(this);            // [1, 2, 3]
    }
    console.log(this === num);        // true
    showNumber();                     // true && [1, 2, 3]
    showNumber.call([1, 2]);          // true && [1, 2, 3]
    showNumber.apply([1, 2]);         // true && [1, 2, 3]
    showNumber.bind([1, 2])();        // true && [1, 2, 3]
}).call(num);
```
由于箭头函数的外部决定上下文以及静态上下文等的特性，不太建议使用箭头函数在全局环境下来定义方法，因为不能通过其他方法改变其上下文。这很蛋疼。
```
function Period (hours, minutes) {    this.hours = hours;
    this.minutes = minutes;
}
Period.prototype.format = () => {
    console.log(this === window);    // => true
    return this.hours + ' hours and ' + this.minutes + ' minutes';
};
var walkPeriod = new Period(2, 30);
console.log(walkPeriod.hours);
walkPeriod.format();    // => 'undefined hours and undefined minutes'
```
此时的 this 实际上是指向了 window，所以 this.hours 和 this.minutes实际上没有声明的，故为 undefined。

在全局环境下，还是选用 函数表达式 来进行函数的定义，可以保证正确的上下文环境
```
function Period (hours, minutes) {
    this.hours = hours;
    this.minutes = minutes;
}
Period.prototype.format = function() {
    console.log(this === walkPeriod);    // => true
    return this.hours + ' hours and ' + this.minutes + ' minutes';
};
var walkPeriod = new Period(2, 30);
walkPeriod.format(); // '2 hours and 30 minutes'
```

## 总结

this可以说是javascript设计给大家的坑，他的指向令人迷惑，饱受意义。作为前端开发，需要充分理解this的含义，以及他在不同情形下的指向，才能真正避免踩坑。总结一下最常见的坑：

### this丢失

```javascript

person = {
  name: 'gzyejiaqi',
  sayName: function () {
    setTimeout(function() {
      console.log('My name is ' + this.name)
    })
  }
}
```

此时发生了this丢失的问题，

解决方案1: that

```
person = {
  name: 'gzyejiaqi',
  sayName: function () {
	var that = this  // 保存this
    setTimeout(function() {
      console.log('My name is ' + that.name)
    })
  }
}

person.sayName()  // "my name is gzyejiaqi"
```

解决办法2: 箭头函数

```
person = {
  name: 'gzyejiaqi',
  sayName: function () {
    setTimeout(() => {
      console.log('My name is ' + this.name)
    })
  }
}

person.sayName()  // "my name is gzyejiaqi"
```


### 全局作用域污染
```
function Person(name, age) {
    this.name = name;
    this.age = age;
}
```
上述函数应该通过new进行实例化，但是注意的是，这个函数同样可以使用普通函数的执行方式进行执行。**这样执行的后果是，this指向的是全局命名空间(browser下是window, node下是global)，会产生全局命名污染**。这也是javascript饱受批评的一点。

```
Person('gzyejiaqi'. '18')
console.log(window.name)  // gzyejiaqi
console.log(window.age)  // 18
```
	
## 练习
```
// 练习1
var func = (function(a) {
    this.a = a;
    return function(a) {
        a += this.a;
        return a;
    }
})(function(a, b) {
    return a;
}(1, 2))

func(4) // ?
// 练习2
var x = 10,
    foo = {
        x: 20,
        bar: function() {
            var x = 30;
            return this.x;
        }
    }

console.log(foo.bar());
console.log((foo.bar)());
console.log((foo.bar = foo.bar)());
console.log((foo.bar, foo.bar)());
```
