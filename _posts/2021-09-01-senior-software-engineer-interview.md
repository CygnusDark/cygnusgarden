---
layout: post
title: Senior Software Engineer Interview
author: 细雪
header-style: text
lang: en
published: true
categories: Interview
tags:
  - Interview
---


# JavaScript 题库

##  ES5/ES6 的继承有什么区别？
ES5 的继承，实质是先创造子类的实例对象this，然后再将父类的方法添加到this上面（Parent.apply(this)）。ES6 的继承机制完全不同，实质是先将父类实例对象的属性和方法，加到this上面（所以必须先调用super方法），然后再用子类的构造函数修改this。

## 异步解决方案及优缺点
1. 回调函数（callback） 如setTimeout
优点：实现了异步
缺点：
	1. 可能会导致回调地狱
	2. 不能用 try catch 捕获错误
	3. 不能 return
2. Promise 链式调用
优点：解决了回调地狱的问题
缺点：
	1. 无法取消 Promise
	2. 错误需要通过回调函数来捕获
3. Async / await
优点：
	1. 代码比Promise清晰
	2. 解决了回调地狱的问题
缺点：await 将异步代码改造成同步代码，如果多个异步操作没有依赖性而使用 await 会导致性能上的降低。

## 如何解决回调地狱
1. promise 
2. 拆解 function：将各步拆解为单个的 function  
3. 通过 Generator 函数暂停执行的效果方式
4. async / await

## Promise构造函数及then方法是同步执行还是异步执行？
promise构造函数是同步执行的，then方法是异步执行的

## ES6新增特性
1. let关键字
2. const定义常量
3. 箭头函数
4. 字符串模板
5. 第七种数据类型Symbol
6. set集合和Map集合
7. Promise规范
8. 类的支持

## 对象浅拷贝
1.
```javascript
var obj1 = {a: 1, b: 2}
var obj2 = Object.assign({}, obj1)
```
2.
```javascript
var obj1 = {a: 1, b: 2};
var obj2 = {...obj1};
```

## 对象深拷贝
1. jQuery的$.extend方法
2. JSON.parse(JSON.stringify())
3. 
```javascript
function deepClone(obj) {
  let objClone = Array.isArray(obj) ? [] : {};
  if (obj && typeof obj === 'object') {
    for (key in obj) {
      if (obj.hasOwnProperty(key)) {
        if (obj[key] && typeof obj[key] === 'object') {
          objClone[key] = deepClone(obj[key])
        } else {
          objClone[key] = obj[key]
        }
      }
    }
  }
  return objClone;
}
let a = [20,8,6,19],
b = deepClone(a);
a[1] = 2;
console.log(a, b)
```

## 原型链
1. 所有原型链的终点都是 Object 函数的 prototype 属性
2. 每一个构造函数都拥有一个 prototype 属性，此属性指向一个对象，也就是原型对象
3. 原型对象默认拥有一个 constructor 属性，指向它的构造函数
4. 每个对象都拥有一个隐藏的属性 proto ，指向它的原型对象

## JavaScript基本类型
1. boolean
2. string
3. number
4. null
5. undefined
6. symbol
7. object

## 设计模式
1. 单例模式
2. 适配器模式
3. 代理模式
4. 发布-订阅模式
5. 策略模式
6. 迭代器模式

## 操作DOM节点的常用API
查找节点的API：
1. document.getElementById ：根据ID查找元素，大小写敏感，如果有多个结果，只返回第一个
2. document.getElementsByClassName ：根据类名查找元素，多个类名用空格分隔，返回一个 HTMLCollection
3. document.getElementsByTagName ：根据标签查找元素， * 表示查询所有标签，返回一个 HTMLCollection
4. document.getElementsByName ：根据元素的name属性查找，返回一个 NodeList
5. document.querySelector ：返回单个Node，如果匹配到多个结果，只返回第一个
6. document.querySelectorAll ：返回一个 NodeList
7. document.forms ：获取当前页面所有form，返回一个 HTMLCollection

## BOM对象模型
1. screen
2. window （窗口对象，重要，需要知道常用属性及函数）
3. navigator
4. location
5. document（文档对象，重要，需要知道常用属性及函数）
6. history

## 事件循环（Event loop）机制
```javascript
console.log('1');

setTimeout(function() {
    console.log('2');
    process.nextTick(function() {
        console.log('3');
    })
    new Promise(function(resolve) {
        console.log('4');
        resolve();
    }).then(function() {
        console.log('5')
    })
})
process.nextTick(function() {
    console.log('6');
})
new Promise(function(resolve) {
    console.log('7');
    resolve();
}).then(function() {
    console.log('8')
})

setTimeout(function() {
    console.log('9');
    process.nextTick(function() {
        console.log('10');
    })
    new Promise(function(resolve) {
        console.log('11');
        resolve();
    }).then(function() {
        console.log('12')
    })
})
```

解析：
整体以script1作为宏任务进入主线程，输出1
遇到setTimeout，回调函数分到宏任务队列中，记为s1
遇到process.nextTick()，回调函数分到微任务队列中，记为p1
遇到promise，直接输出7，then回调函数放置到微任务队列，记为t1
遇到setTimeout，回调函数分配到宏任务队列，记为s2
结束第一轮事件循环

由于微任务先于宏任务执行，此时输出 1 7 6 8，然后开启第二轮事件循环。从setTimeout1开始。

遇到同步任务，打印 2
遇到process，放到微任务队列，记为p2
遇到promise，打印同步任务输出 4
遇到then任务，放到微任务队列，记为t2

第二轮宏任务结束，执行微任务p2，t2。输出 3 、5。然后开启第三轮事件循环，从setTimeout2开始。

遇到同步任务，打印 9
遇到process，放到微任务队列，记为 p3
遇到promise，打印同步任务输出 11
遇到then任务，放到微任务队列，记为 t3

第三轮宏任务结束，执行微任务p3，t3，输出 10 、12。到此，三轮事件循环结束。输出顺序为：
// 1，7，6，8，2，4，3，5，9，11，10，12

## 闭包的特性及优缺点
闭包有3大特性：
1. 函数嵌套函数
2. 函数内部可以引用函数外部的参数和变量
3. 参数和变量不会被垃圾回收机制回收

闭包优点：
1. 可读取函数内部的变量
2. 局部变量可以保存在内存中，实现数据共享
3. 执行过程所有变量都匿名在函数内部

闭包缺点：
1. 使函数内部变量存在内存中，内存消耗大
2. 滥用闭包可能会导致内存泄漏
3. 闭包可以在父函数外部改变父函数内部的值，慎操作

## this关键字的用法
1. 纯粹的函数调用 代表全局对象
2. 作为对象方法的调用 this就指这个上级对象
3. 作为构造函数调用  this就指这个新对象
4. apply调用  apply()是函数的一个方法，作用是改变函数的调用对象。它的第一个参数就表示改变后的调用这个函数的对象。因此，这时this指的就是这第一个参数。apply()的参数为空时，默认调用全局对象

# TypeScript 题库

## TypeScript新增了哪些类型？
1. number
2. string
3. boolean
4. Symbol
5. Array
6. Tuple
7. enum
8. object
9. never
表示那些永不存在的值类型。如总是抛出异常或者根本不会有返回值的函数的返回值类型。
10. void
与any相反表示没有任何类型。函数没有返回值时用void。
11. null和undefined
它们是所有类型的子类型。当你指定structNullChecks时，它们只能赋值给void或者它们自己本身。
12. any

## 类、接口的基础和使用场景
类（Class）是面向对象程序设计（OOP）实现信息封装的基础。类是一种用户定义的引用数据类型，也称类类型。每个类包含数据说明和一组操作数据或传递消息的函数。类的实例称为对象。
类的实质是一种引用数据类型

## 抽象类与接口的区别
1. 抽象类是类（事物）的抽象，抽象类用来捕捉子类的通用特性，接口是行为的抽象
2. 接口可以多实现，而抽象类只能单一继承
3. 接口不具备继承的任何具体特点，仅仅承诺了能够调用的方法
4. 抽象类更多的定义是在一系列紧密相关的类之间，而接口大多数是定义在关系疏松但都实现某一功能的类中
5. 综合来说抽象类更多的是实现业务上的严谨性；接口更多的是制定各种规范，而此规范又分为很多类规范，就像官方文档在介绍接口这一节的时候所说的，例如函数型规范、类类型规范、混合规范、索引规范等。

## TS对JS的改进
1. 静态类型检查
2. interface接口规范 / abstract抽象类
3. 编译期类型检查
4. 开发环境能提供丰富的信息
5. 完备的系统设计能力

# React 题库

## 写 React / Vue 项目时为什么要在列表组件中写 key，其作用是什么？
1. key的作用就是给每一个VNode一个唯一的key，通过key可以更准确更快的拿到VNode。
2. vue和react都是采用diff算法来对比新旧虚拟节点，从而更新节点。当新节点跟旧节点头尾交叉对比没有结果时，会根据新节点的key去对比旧节点数组中的key，从而找到相应的旧节点。如果没找到就认为是一个新增节点。而如果没有key，那么就会采用遍历查找的方式去找到对应的旧节点。
3. 在不带key的情况下，节点可以进行复用，省去了操作DOM的开销，只适用于简单的无状态组件的渲染。虽然带上唯一的key会增加开销，但是能保证组件的状态正确，而且用户基本感受不到差距。

## setState 什么时候是同步的，什么时候是异步的？
在React中，如果是由React引发的事件处理（比如通过onClick引发的事件处理），调用setState不会同步更新this.state，除此之外的setState调用会同步执行this.state 。所谓“除此之外”，指的是绕过React通过addEventListener直接添加的事件处理函数，还有通过setTimeout/setInterval产生的异步调用。

## React fiber的实现原理
解决主线程长时间被 JS 运算占用这一问题的基本思路，是将运算切割为多个步骤，分批完成。也就是说在完成一部分任务之后，将控制权交回给浏览器，让浏览器有时间进行页面的渲染。等浏览器忙完之后，再继续之前未完成的任务。

旧版 React 通过递归的方式进行渲染，使用的是 JS 引擎自身的函数调用栈，它会一直执行到栈空为止。而Fiber实现了自己的组件调用栈，它以链表的形式遍历组件树，可以灵活的暂停、继续和丢弃执行的任务。实现方式是使用了浏览器的requestIdleCallback这一 API。官方的解释是这样的：

window.requestIdleCallback()会在浏览器空闲时期依次调用函数，这就可以让开发者在主事件循环中执行后台或低优先级的任务，而且不会对像动画和用户交互这些延迟触发但关键的事件产生影响。函数一般会按先进先调用的顺序执行，除非函数在浏览器调用它之前就到了它的超时时间。

## React 组件的生命周期及对应的钩子函数
*初始渲染阶段：*这是组件即将开始其生命之旅并进入 DOM 的阶段。
*更新阶段：*一旦组件被添加到 DOM，它只有在 prop 或状态发生变化时才可能更新和重新渲染。这些只发生在这个阶段。
*卸载阶段：*这是组件生命周期的最后阶段，组件被销毁并从 DOM 中删除。

componentWillMount**()** – 在渲染之前执行，在客户端和服务器端都会执行。
componentDidMount**()** – 仅在第一次渲染后在客户端执行。
componentWillReceiveProps**()** – 当从父类接收到 props 并且在调用另一个渲染器之前调用。
shouldComponentUpdate**()** – 根据特定条件返回 true 或 false。如果你希望更新组件，请返回true 否则返回 false。默认情况下，它返回 false。
componentWillUpdate**()** – 在 DOM 中进行渲染之前调用。
componentDidUpdate**()** – 在渲染发生后立即调用。
componentWillUnmount**()** – 从 DOM 卸载组件后调用。用于清理内存空间。


## 列举常用的React Hooks，并简要说明使用场景
常用hooks：
1. useState
2. useEffect
3. useContext
4. useReducer
5. useCallback 
6. useRef
7. useMemo

## React与其他主流前端框架的比较
1. 它使用Virtual DOM而不是Real DOM
2. 它可以用服务器端渲染
3. 它遵循单向数据流或数据绑定

## Virtual DOM 的工作原理
Virtual DOM 是一个轻量级的 JavaScript 对象，它最初只是 real DOM 的副本。它是一个节点树，它将元素、它们的属性和内容作为对象及其属性。 React 的渲染函数从 React 组件中创建一个节点树。然后它响应数据模型中的变化来更新该树，该变化是由用户或系统完成的各种动作引起的。
Virtual DOM 工作过程有三个简单的步骤:
1. 每当底层数据发生改变时，整个 UI 都将在 Virtual DOM 描述中重新渲染
2. 然后计算之前 DOM 表示与新表示的之间的差异
3. 完成计算后，将只用实际更改的内容更新 real DOM

## React中的refs作用是什么
Refs 是 React 提供给我们的安全访问 DOM 元素或者某个组件实例的句柄。
我们可以为元素添加 ref 属性然后在回调函数中接受该元素在 DOM 树中的句柄，该值会作为回调函数的第一个参数返回。

## 常用的React组件库
1. Ant Design UI库
2. Bootstrap UI库
3. Material UI库
4. Semantic UI库

## React中的高阶组件（HOC / higher-order component）
定义：高阶组件是一个函数，接收一个组件参数，然后返回一个新组件。
A higher-order component is a function that takes a component and returns a new component.
示例：const EnhancedComponent = higherOrderComponent(WrappedComponent);
说明：高阶组件是react应用中很重要的一部分，最大的特点就是重用组件逻辑。它并不是由React API定义出来的功能，而是由React的组合特性衍生出来的一种设计模式。如果你用过redux，那你就一定接触过高阶组件，因为react-redux中的connect就是一个高阶组件。
作用：对复用UI、数据逻辑等进行封装，对参数组件进行制式处理，从而让参数组建具备特定的ui或功能

# 解决方案 题库

## 跨域的基本概念和解决方案
解决方案有多种，常见的如使用axios二次封装携带的请求头解决跨域问题

## 防抖的实现思路
通过SetTimeout设置毫秒时间，在触发事件后，n秒内没有再次触发事件，处理函数才会执行，如果在这一段时间来到之前，又一次触发了事件，那就重新计时
```javascript
<script>
		var num = 0;
 
        function Fun() {
            console.log(num++)
        };
 
        var timer = null;
 
        function thow(delay, fun) {
            clearTimeout(timer)
            timer = setTimeout(() => {
                fun();
            }, delay);
        }
        scry.onmousemove = function () {
            thow(2000, Fun)
        }
</script>
```

## 节流的实现思路
高频事件触发，但在n秒内只会执行一次，节流会减少函数的执行频率
```javascript
    /* 节流 */
    function throttle(func, wait) {
        var previous = 0;
        return function () {
            var now = +new Date();
            if (now - previous > wait) {
                func.apply(this, arguments);
                previous = now;
            }
        }
    }
    function getUserAction() {
        console.log(`每秒1秒内打印一次`)
    }
    document.querySelector('div').addEventListener('click', throttle(getUserAction, 1000))
```

## 图片懒加载的解决方案
1. 基于 offsetTop + clientHeight + scrollTop 实现懒加载
2. 基于 getBoundingClientRect 实现懒加载
3. 基于 Intersection Observer API 实现懒加载
4. 基于 img 标签的 loading 属性实现懒加载

## 怎么实现单点登录
解决方案有多种，例如使用一个统一的登录中心

# Vue 题库

## Vue实例的生命周期及对应的钩子函数
beforeCreate
created
beforeMount
mounted
beforeUpdate
updated
beforeDestroy
destroyed
activated
deactivated
erroeCaptured

追问：
created 和 beforeCreate的区别
A 可以操作数据 B 数据没有初始化

追问：
mounted 和 beforeMount的区别
A 可以操作DOM B 还未生成DOM

## 双向绑定的实现原理
监听器 Observer ，用来劫持并监听所有属性（转变成setter/getter形式），如果属性发生变化，就通知订阅者
订阅器 Dep，用来收集订阅者，对监听器 Observer 和 订阅者 Watcher 进行统一管理
订阅者 Watcher，可以收到属性的变化通知并执行相应的方法，从而更新视图
解析器 Compile，可以解析每个节点的相关指令，对模板数据和订阅器进行初始化

## Vue不能检测哪些属性变化
1. 数组
	1. 使用下标更新数组元素
	2. 使用赋值方式改变数组长度
	3. 使用下标增删数组元素
官方应对方法：
	1. Vue.set(target, key, value)
	2. vm.items.splice(indexOfItem, 1, newValue)

2. 对象
	1. 增删元素
官方应对方法：
	1. Vue.set(target, propertyName, value);
	2. Vue.delete( target, propertyName/index )

## Vue组件的通信方式
1. props / $emit 适用 父子组件通信
2. ref 与 $parent / $children 适用 父子组件通信
3. EventBus （$emit / $on） 适用于 父子、隔代、兄弟组件通信
4. $attrs/$listeners 适用于 隔代组件通信
5. provide / inject 适用于 隔代组件通信
6. Vuex 适用于 父子、隔代、兄弟组件通信

## Vue常用的指令
1. v-if / v-else：判断是否隐藏
2. v-for: 数据循环
3. v-bind: 给元素的属性赋值 简写方式 :
4. v-model: 双向数据流 （页面改变影响内存 内存改变影响页面）
5. v-on 处理自定义原生事件 简写方式 @

追问：v-if和v-show的区别
共同点：都是通过判断绑定数据的true/false来展示的
不同点：v-if只有在判断为true的时候才会对数据进行渲染，如果为false代码删除。除非再次进行数据渲染，v-if才会重新判断。可以说是用法比较倾向于对数据一次操作。
v-show是无论判断是什么都会先对数据进行渲染，只是false的时候对节点进行display:none;的操作。所以再不重新渲染数据的情况下，改变数据的值可以使数据展示或隐藏

## keep-alive组件 及router-view组件的作用及用法
1. keep-alive 是 Vue 内置的一个组件，可以使被包含的组件保留状态，或避免重新渲染
```javascript
<keep-alive>
  <component>
  </component>
</keep-alive>
```

2. router-view 也是一个组件，如果直接被包在 keep-alive 里面，所有路径匹配到的视图组件都会被缓存
```javascript
<keep-alive>
    <router-view>
    </router-view>
</keep-alive>
```

追问：如果只想 router-view 里面某个组件被缓存，怎么办？
1. 使用 include/exclude
2. 增加 router.meta 属性


# 算法 题库
## 反转链表
### 题目
```
/**
 * Definition for singly-linked list.
 * function ListNode(val, next) {
 *     this.val = (val===undefined ? 0 : val)
 *     this.next = (next===undefined ? null : next)
 * }
 */
/**
 * @param {ListNode} head
 * @return {ListNode}
 */
var reverseList = function(head) {

};
```
### 题解
```
var reverseList = function(head) {
    let prev = null;
    let curr = head;
    while (curr) {
        const next = curr.next;
        curr.next = prev;
        prev = curr;
        curr = next;
    }
    return prev;
};
```
## 回文链表
### 题目
```
## 回文链表
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode} head
 * @return {boolean}
 */
var isPalindrome = function(head) {
};
```

### 题解
```
var isPalindrome = function(head) {
    const vals = [];
    while (head !== null) {
        vals.push(head.val);
        head = head.next;
    }
    for (let i = 0, j = vals.length - 1; i < j; ++i, --j) {
        if (vals[i] !== vals[j]) {
            return false;
        }
    }
    return true;
};
```