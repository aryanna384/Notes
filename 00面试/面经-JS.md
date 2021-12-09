## 数据类型

基本数据类型：string、number、boolean、undefined、null、symbol

引用类型：Object

>对象的值是保存在**堆内存**中的，而对象的引用（即变量）是保存在**栈内存**中的
>
>对象的复制是浅拷贝；基本数据类型的复制是深拷贝

## 确定数据类型的方法？typeof & instanceof 

### **typeof** 

操作符返回一个字符串，表示未经计算的操作数的类型

原理：在 javascript 的最初版本中，使用的 32 位系统，为了性能考虑使用低位存储了变量的类型信息：

- 000：对象
- 010：浮点数
- 100：字符串
- 110：布尔
- 1：整数

`null`：对应机器码的 NULL 指针，一般是全零。

`undefined`：用 −2^30 整数来表示!

所以typeof 在判断 null 的时候会当成 object

缺点：typeof在判断object类型的数据的时候，不能准确的告知我们具体是哪一种Object，而且在判断null的时候,也会上述的附加信息。对于判断是哪一种object的时候，我们需要用到instanceof这个操作符来。

### **instanceof**

用于检测构造函数的 `prototype` 属性是否出现在某个实例对象的原型链上。

```
object instanceof constructor // Object.getPrototypeOf(object) === constructor.prototype
```

原理：`instanceof`在查询的过程中会遍历左边变量的原型链,直到找到右边变量的prototype,如果查找失败的话,返回false,告诉我们左边的变量并非是右边变量的实例。

## 事件循环Event loop

> Link：浏览器核心进程

### 浏览器执行线程

浏览器是多进程的，浏览器每一个 tab 标签都代表一个独立的进程，包含

- Brower进程
- 第三方插件进程

- GPU进程

- **浏览器渲染进程（浏览器内核）**属于浏览器多进程中的一种，主要负责页面渲染，脚本执行，事件处理等

**浏览器渲染进程**包含线程：

- GUI 渲染线程（负责渲染页面，解析 HTML，CSS 构成 DOM 树）

- JS 引擎线程

- 事件触发线程

- 定时器触发线程

- http 请求线程

**主线程**：也就是 js 引擎执行的线程，这个线程只有一个，页面渲染、函数处理都在这个主线程上执行。
**工作线程**：也称幕后线程，这个线程可能存在于浏览器或js引擎内，与主线程是分开的，处理文件读取、网络请求等异步事件。

**同步任务**，立即执行的任务，同步任务一般会直接进入到主线程中执行，形成一个`执行栈 `

**异步任务**，异步执行的任务，异步任务会通过任务队列的机制(先进先出的机制)来进行协调。只要异步任务有了运行结果，就在任务队列之中放置一个事件。
异步任务的类型：

- 普通事件：click、resize 等

- 资源加载：load、error 等

- 定时器：setInterVal、setTimeout 等

  `setTimeout`待加入队列的消息和一个时间值（可选，默认为 0）。这个时间值代表了消息被实际加入到队列的最小延迟时间。如果队列中没有其它消息并且栈为空，在这段延迟时间过去之后，消息会被马上处理。但是，如果有其它消息，`setTimeout` 消息必须等待其它消息处理完。因此第二个参数仅仅表示最少延迟时间，而非确切的等待时间

<img alt='MDN稍作修改的图' src="/Users/ynchen/Library/Mobile Documents/iCloud~md~obsidian/Documents/Vault/01 前端/images/截屏2021-06-22 20.12.05-4364779.png" style="zoom:50%;" />

同步和异步任务分别进入不同的执行环境，**同步的进入主线程，即主执行栈，异步的进入任务队列（先进先出）**。主线程内的任务执行完毕为空，会去任务队列读取对应的任务，推入主线程执行。

上述过程的不断重复就是 `Event Loop (事件循环)`。

### 事件循环

>参考 https://segmentfault.com/a/1190000012925872#comment-area

每个线程都会有它自己的事件循环，所以都能独立运行。然而所有同源窗口会共享一个事件循环以同步通信。事件循环会一直运行，来执行进入队列的`宏任务`。一个事件循环有多种的`宏任务源`，这些宏任务源保证了在本任务源内的顺序。
但是浏览器每次都会选择一个源中的一个宏任务去执行。这保证了浏览器给与一些宏任务（如用户输入）以更高的优先级。

在事件循环中，每进行一次循环操作称为 tick，每一次 tick 的任务处理模型是比较复杂的，但关键步骤如下：

- 在此次 tick 中选择最先进入队列的任务(oldest task)，如果有则执行(一次)
- 检查是否存在 Microtasks，如果存在则不停地执行，直至清空 Microtasks Queue
- 更新 render
- 主线程重复执行上述步骤

```html
<script>
      console.log('script start');

      setTimeout(function () {
        console.log('setTimeout');
      }, 0);

      Promise.resolve().then(function () {
        console.log('promise1');
      }).then(function () {
        console.log('promise2');
      });

      console.log('script end');
</script>
```

打印顺序：script start, script end, promise1, promise2, setTimeout

#### 宏任务、微任务

在挂起任务时，JS 引擎会将所有任务按照类别分到`宏任务macrotask`和`微任务microtask`这两个队列中，首先在宏任务队列（task queue）中取出第一个任务，执行完毕后取出微任务队列中的所有任务顺序执行；之后再取宏任务任务，周而复始，直至两个队列的任务都取完。

##### 宏任务（task）

宏任务可以理解是每次执行栈执行的代码就是一个宏任务（包括每次从事件队列中获取一个事件回调并放到执行栈中执行）。
浏览器为了能够使得JS内部宏任务与DOM任务能够有序的执行，会在一个宏任务执行结束后，在下一个宏任务执行开始前，对页面进行重新渲染 （宏任务->渲染->宏任务->...）鼠标点击会触发一个事件回调，需要执行一个宏任务，然后解析HTMl。
`setTimeout`的作用是等待给定的时间后为它的回调产生一个新的宏任务。这就是为什么打印‘setTimeout’在‘script end’之后。因为打印‘script end’是第一个宏任务里面的事情，而‘setTimeout’是另一个独立的任务里面打印的。

##### 微任务（Microtasks / jobs）

微任务：在当前宏任务执行结束后立即执行，只要执行栈中没有其他的JS代码正在执行且每个宏任务执行完，微任务队列会立即执行。如果在微任务执行期间微任务队列加入了新的微任务，会将新的微任务加入队列尾部，之后也会被执行。

结论：

- <u>对一个已经有了结果的promise（fullfill/reject）调用.then()会立即产生一个微任务</u>。这就是为什么‘promise1’,'promise2'会打印在‘script end’之后，因为所有微任务执行的时候，当前执行栈的代码必须已经执行完毕。
- <u>所有微任务总会在下一个宏任务之前全部执行完毕</u>

| 宏任务                | 微任务           |
| --------------------- | ---------------- |
| script(整体代码)      | Promise.then     |
| SetTimeout            | Object.observe   |
| setInterval           | MutationObserver |
| setImmediate          | Process.nextTick |
| Ajax 请求             | Async/await      |
| requestAnimationFrame |                  |
| I/O                   |                  |
| UI 交互事件           |                  |
| postMessage           |                  |
| MessageChannel        |                  |

### JS执行机制

在事件循环中，每进行一次循环操作称为 `tick`，每一次 tick 的任务处理模型是比较复杂的，但关键步骤如下：

- 执行一个宏任务（栈中没有就从事件队列中获取）
- 执行过程中如果遇到微任务，就将它添加到微任务的任务队列中
- 宏任务执行完毕后，立即执行当前微任务队列中的所有微任务（依次执行）
- 当前宏任务执行完毕，开始检查渲染，然后GUI线程接管渲染
- 渲染完毕后，JS线程继续接管，开始下一个宏任务（从事件队列中获取）



### 读代码

```javascript
setTimeout(() => {//宏任务
  //执行后 回调一个宏事件
  console.log('内层宏事件3')
}, 0)
console.log('外层宏事件1');

new Promise((resolve) => {
  console.log('外层宏事件2');
  resolve()
}).then(() => {
  console.log('微事件1');
}).then(() => {
  console.log('微事件2')
})
```

打印顺序：外层宏事件1、外层宏事件2、微事件1、微事件2、内层宏事件3

解释：

- 首先浏览器执行JS进入第一个宏任务进入主线程, 遇到` setTimeout` 分发到`宏任务`Event Queue中

- 遇到 console.log() 直接执行:输出外层宏事件1

- 遇到 Promise， new Promise 直接执行：输出外层宏事件2

- 执行 then 被分发到`微任务`Event Queue中

- 第一轮宏任务执行结束，开始执行微任务 打印 '微事件1' '微事件2

- 第一轮微任务执行完毕，执行第二轮宏事件，打印setTimeout里面内容'内层宏事件3'

  

```javascript
//主线程直接执行
console.log('1');
//丢到宏事件队列中
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
//微事件1
process.nextTick(function() {
    console.log('6');
})
//主线程直接执行
new Promise(function(resolve) {
    console.log('7');
    resolve();
}).then(function() {
    //微事件2
    console.log('8')
})
//丢到宏事件队列中
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

- 执行js进入主线程, `1`
- 遇到` setTimeout `加到宏任务task queue中（2、4、【3、5】）
- 遇到 `process.nextTick`加到微任务jobs queue中 【6】
- 遇到 Promise， `new Promise ` 直接执行 `7`

--------第一轮事件循环结束---------

- 执行 `Promise.then `被分发到微任务jobs queue中 【6,8】
- 遇到`setTimeOut`，加到宏任务队列(（2、4、【3、5】），（9、11、【10、12】）)
- 开始执行微任务`6,8`

--------第二轮事件循环结束---------

（（2、4、【3、5】），（9、11、【10、12】））【】将第一个宏任务取出来执行

- 执行第一个`setTimeout`先执行主线程宏任务`2,4`，再执行微任务`3,5`

--------第三轮事件循环结束---------

（（9、11、【10、12】））【】

- 执行第二个`setTimeout`,同理打印` 9,11,10,12`

整段代码，共进行了三次事件循环，完整的输出为1，7，6，8，2，4，3，5，9，11，10，12







## this指向

- 以函普通数的形式调用时，this永远都是window。比如`fun();`相当于`window.fun();`

- 以方法(对象中函数)的形式调用时，this是调用方法的那个对象

- 以构造函数的形式调用时，this是新创建的那个对象

- 使用call和apply调用时，this是指定的那个对象

## == 和 ===

- ==  也叫`loose equality`，仅比较两者的值。对于不同类型的对象，会先做类型转换
  - `NaN == NaN` false NaN和任何数都不相等，包括NaN本身 。` [] == []` false `{} == {} `false 引用数据类型比较的是地址。

  - `undefined == null` true 但是 undefined === true false (因为数据类型不一样)。

  - `对象 == 字符串` 将对象转换成字符串.

  - 剩下的其他情况如果两边数据类型不一样，都需要转换成数字类型。

- === 也叫strict equality，会先比较两个对象的类型，类型相同才比较值。
  - 如果操作数的类型不同，则返回 `false`。
  - 如果两个操作数都是对象，只有当它们指向同一个对象时才返回 `true`。
  - 如果两个操作数都为 `null`，或者两个操作数都为 `undefined`，返回 `true`。
  - 如果两个操作数有任意一个为 `NaN`，返回 `false`。
  - 否则，比较两个操作数的值：
    - 数字类型必须拥有相同的数值。`+0` 和 `-0` 会被认为是相同的值。
    - 字符串类型必须拥有相同顺序的相同字符。
    - 布尔运算符必须同时为 `true` 或同时为 `false`。

## 变量声明和声明提升

- var
  - 可声明任意数据类型
  - 作用域：函数作用域
  - 存在声明提升（将所有变量声明都拉到函数作用域的顶部，ex：先输出变量在定义并不会报错）
  - **可以**重复声明
  - 在全局作用域中声明的变量**会**成为 windows 对象的属性

- let
  - 可声明任意数据类型
  - 作用域：块作用域-函数作用域的子集
  - 不存在**声明提升**
  - **不可以**重复声明
  - 在全局作用域中声明的变量**不会**成为 **windows 对象的属性**（使用 window.变量 不可访问）但是是全局变量

- const 
  - 与 let 一样，不同的是：
    - 声明时必须赋值
    - 不可修改，除非声明的对象是 object（可修改）
    - 不能用 const 声明自增的迭代变量，但是如果在迭代中需要变量不变可用 const 声明

读代码

```javascript
const a = {};
a.test = 1;
function aa(flag) {
  if(flag) {
    var test = 'hello man';
  } else {
    console.log(test);
  }
}
aa(false);
function bb(flag) {
  if(flag) {
    let test = 'hello man';
  } else {
    console.log(test);
  }
}
bb(false);
// 答案：undefined 和报错 ReferenceError: test is not defined
```

## 原型和继承

### 1、原型链

原理：让一个引用类型继承另一个引用类型的属性和方法。

缺点：原型中包含的引用值会在所有引用值间共享，子类型实例化时不能给父类型构造函数传参

```javascript
function A() {

}
//在A的原型上绑定sayA()方法
A.prototype.sayA = function(){
  console.log("from A")
}
function B(){

}

// 让B的原型对象指向A的一个实例
// 本来B的原型中的constructor指向B，现在指向A
B.prototype = new A();
```

`B.prototype = new A();`函数对象B的prototype指针指向一个A的实例，**B的原型对象里面不再有constructor属性**，其实B本来有一个真正的原型对象，原本可以通过B.prototype访问，但是我们现在改写了这个指针，使它指向了另一个对象，所以B真正的原型对象现在没法被访问了，取而代之的这个新的原型对象是A的一个实例，自然就没有`constructor`属性了

### 2、盗用构造函数

原理：在子类构造函数中用`call(this)/apply(this)`调用父类构造函数，不需要共享的引用值为父类的属性。创建子类实例，对子类实例的属性进行修改。

优点：可以在子类构造函数中向父类构造函数传参

缺点：必须在构造函数中定义方法，函数不能复用；子类不能访问父类原型上定义的方法。

```javascript
function SuperType() { 
 this.colors = ["red", "blue", "green"]; 
} 
function SubType() { 
  // 子类借用父类的构造函数，实现继承
	SuperType.call(this); 
} 
let instance1 = new SubType(); 
instance1.colors.push("black"); 
console.log(instance1.colors); // "red,blue,green,black" 
let instance2 = new SubType(); 
console.log(instance2.colors); // "red,blue,green"
```



### 3、组合式继承(使用最多)

原理：使用原型链继承原型上的属性和方法，而通过盗用构造函数继承实例属性

优点：实例都有各自的属性，同时方法都定义在两个原型对象上，**属性独立，方法复用**

```javascript
function A(name){ 
 this.name = name; 
 this.colors = ["red", "blue", "green"]; 
} 
A.prototype.sayName = function() { 
 console.log(this.name); 
}; 

function B(name, age){ 
   // 借用构造函数继承  不同属性
   A.call(this, name);  
   this.age = age; 
} 

// 原型链继承  共用方法
B.prototype = new A(); 
B.prototype.sayAge = function() { 
 console.log(this.age); 
}; 
let instance1 = new B("Nicholas", 29); 
instance1.colors.push("black"); 
console.log(instance1.colors); // "red,blue,green,black" 
instance1.sayName(); // "Nicholas"; 
instance1.sayAge(); // 29 
let instance2 = new B("Greg", 27); 
console.log(instance2.colors); // "red,blue,green" 
instance2.sayName(); // "Greg"; 
instance2.sayAge(); // 27
```



### 4、原型式继承

场景：原型上既有原始值属性又有引用值属性`Object.create()`

```javascript
let person = { 
 name: "Nicholas", 
 friends: ["Shelby", "Court", "Van"] 
}; 
let anotherPerson = Object.create(person); 
anotherPerson.name = "Greg"; 
anotherPerson.friends.push("Rob"); 
let yetAnotherPerson = Object.create(person); 
yetAnotherPerson.name = "Linda"; 
yetAnotherPerson.friends.push("Barbie"); 
console.log(person.friends); // "Shelby,Court,Van,Rob,Barbie"
```

为什么有的继承了有的没继承？

相当于对对象进行浅拷贝，数据类型为基本类型的浅拷贝就直接拷贝了值，数据类型为对象的，浅拷贝只拷贝了引用，指向的地址都是同一个所以是共享的

### 5、寄生式继承

```javascript
function createAnother(original){ 
   let clone = object(original); // 通过调用函数创建一个新对象
   clone.sayHi = function() { // 以某种方式增强这个对象
   console.log("hi"); 
   }; 
   return clone; // 返回这个对象
}
let person = { 
   name: "Nicholas", 
   friends: ["Shelby", "Court", "Van"] 
}; 
let anotherPerson = createAnother(person); 
anotherPerson.sayHi(); // "hi"
```

### isPrototypeOf()

`函数A.prototype.isPrototypeOf(函数实例B)`判断函数实例B是否在函数A的原型链上

```javascript
let instance=new SubType()
let superInstance = new SuperType()
// instance 是否在原型链上
console.log(SubType.prototype.isPrototypeOf(instance))  // true
console.log(SuperType.prototype.isPrototypeOf(instance)) // true
console.log(Object.prototype.isPrototypeOf(instance)) // true
// superInstance不在subType原型链上
console.log(SubType.prototype.isPrototypeOf(superInstance)) // false
console.log(SuperType.prototype.isPrototypeOf(superInstance)) // true
console.log(Object.prototype.isPrototypeOf(superInstance)) // true
```

`B instanceOf A`判断函数实例B是否在函数A的原型链上

```javascript
console.log(instance instanceof SubType) // true
console.log(superInstance instanceof SubType) //false
```

### Object.defineProperty()

`Object.defineProperty()` 方法会直接在一个对象上定义一个新属性，或者修改一个对象的现有属性，并返回此对象。 

Object 构造函数上的静态方法（不被继承）

```javascript
Object.defineProperty(obj, prop, descriptor)
```

注意：

默认情况下，使用 `Object.defineProperty()` 添加的属性值是不可修改的、不可遍历。

- 数据描述符

| 数据描述符              | 描述                   | 默认值    |
| ----------------------- | ---------------------- | --------- |
| value(数值、对象、函数) |                        | undefined |
| `configurable: true`    | 属性描述符是否可被修改 | false     |
| `enumerable: true`      | 对象是否可遍历         | false     |
| writable:true           | value 值是否可修改     | false     |

- 存储描述符

| 存取描述符           | 描述                   | 默认值    |
| -------------------- | ---------------------- | --------- |
| get                  |                        | undefined |
| set                  |                        | undefined |
| `configurable: true` | 属性描述符是否可被修改 | false     |
| `enumerable: true`   | 对象是否可遍历         |           |



## 作用域链

每个函数调用都有自己的上下文。 当代码执行流进入函数时, 函数的上下文被推到一个**上下文栈上**。在函数执行完之后,上下文栈会弹出该函数上下文,将控制权返还给之前的执行上下文。
ECMAScript **程序的执行流**就是通过这个上下文栈进行控制的。
上下文中的代码在执行的时候,会创建变量对象的一个**作用域链(scope chain)** 。这个作用域链决定了各级上下文中的代码在访问变量和函数时的顺序。 代码正在执行的上下文的变量对象始终位于作用域链的最前端。如果上下文是函数，则其活动对象(activation object)用作变量对象。活动对象最初只有一个定义变量: `arguments`。 (全局上下文中没有这个变量 ) 作用域链中的下一个变量对象来自包含上下文,再下一个对象来自再下一个包含上下文。以此类推直至全局上下文;全局上下文的变量对象始终是作用域链的最后一个变量对象。(显示得来看就是最里面的函数可以调用之前所有函数的变量)
代码执行时的**标识符解析**是通过沿作用域链逐级搜索标识符名称完成的。 搜索过程始终从作用域链的最前端开始,然后逐级往后,直到找到标识符。 (如果没有找到标识符,那么通常会报错 )

## ES6新特性

> [阮一峰ES6新特性](https://es6.ruanyifeng.com/)


数组方法

扩展运算符
Array.from()
Array.of()
数组实例的 copyWithin()
数组实例的 find() 和 findIndex()
数组实例的 fill()
数组实例的 entries()，keys() 和 values()
数组实例的 includes()
数组实例的 flat()，flatMap()
数组的空位
Array.prototype.sort() 的排序稳定性

## 闭包

理解：

- 闭包指的是那些引用了另一个函数作用域中变量的函数，通常是在嵌套函数中实现的。闭包是将函数内部和函数外部连接起来的桥梁。
- 一个函数和它周围状态的引用捆绑在一起的组合

一般形式：

1. 函数作为另一个函数的返回值

2. 函数作为另一个函数的参数

闭包作用/特点：

-   读取函数内部的变量，这些变量的值始终保持在内存中，不会在外层函数调换后被自动清除。
-   变量或参数不会被垃圾回收机制回收GC。
-   闭包会保留它们包含函数的作用域，所以比其他函数更占用内存，因此处理不好会导致内存泄露

优缺点

## 垃圾回收的方式

## 跨域

### 跨域

跨域：为浏览器的**同源策略**导致，当一个请求 url 的协议、域名、端口三者之间任意一个与当前页面 url 不同

受浏览器同源策略的影响，不是同源的脚本不能操作其他源下面的对象。想要操作另一个源下的对象就需要跨域。

#### 同源策略

是一个重要的**安全策略**，它用于限制一个origin的文档或者它加载的脚本如何能与另一个源的资源进行交互。它能帮助阻隔恶意文档，减少可能被攻击的媒介。

**同源**：协议、域名、端口一致

#### html 特殊标签

link、script、img、frame 等这些标签具有**跨域特性**，可以直接访问

### 跨域分类

cookie跨域

localStorage跨域

**Ajax跨域**：Ajax只能同源使用、浏览器同源策略

### 解决方案：

- CORS（服务器端解决）
- Node 正向代理
- Ngnix 反向代理
- JSONP
- Proxy代理
- Websocket
- postMessage
- ....

[参考](https://juejin.cn/post/6844904126246027278#heading-39)

#### CORS 资源跨域共享

跨域资源共享(CORS) 是一种机制，它使用额外的 HTTP 头来告诉浏览器 让运行在一个 origin (domain) 上的 Web 应用被准许访问来自不同源服务器上的指定的资源。当一个资源从与该资源本身所在的服务器「不同的域、协议或端口」请求一个资源时，资源会发起一个「跨域 HTTP 请求」。

CORS简单请求、非简单请求

#### Jsonp

Jsonp(JSON with Padding)是一个简单高效的跨域方式，

原理： `script` 标签没有跨域限制，HTML中的script标签可以加载并执行其他域的javascript。

例如我要从域A的页面pageA加载域B的数据，那么在域B的页面pageB中我以JavaScript的形式声明pageA需要的数据，然后在 pageA中用script标签把pageB加载进来，那么pageB中的脚本就会得以执行。JSONP在此基础上加入了回调函数，pageB加载完之后会执行pageA中定义的函数，所需要的数据会以参数的形式传递给该函数。

特点：JSONP易于实现，但是也会存在一些安全隐患，如果第三方的脚本随意地执行，那么它就可以篡改页面内容，截获敏感数据。但是在受信任的双方传递数据，JSONP是非常合适的选择。



## Promise

- **异步程序执行机制**，比传统解决方案（回调函数和事件）更合理和强大

- ES6 的一个引用类，是一个有状态的对象

- 解决**回调地狱**问题，因为 resolve 和 reject 可以进行异步处理并且得知任务进度

  ### 三种状态

- 待定 pending-->尚未开始或者正在执行中

- 兑现 fulfilled/resolved-->成功完成

- 拒绝 rejected-->没有成功完成

不可逆


```javascript
var promise = new Promise(执行器函数)
var promise = new Promise(function(resolve,reject){
  if(异步操作成功){
    	resolve(value);
  }else{
    	reject(reason);
  }
});
```

执行器函数是必须的，传入的为非函数时会被默认忽略

当 Promise 状态改变，调用 resolve/reject 会自动触发 then()响应函数

### then()

```javascript
var promise = new Promise(function(resolve,reject){
  if(false){
    	resolve("success");
  }else{
    	reject("fail");
  }
});
//自动触发
promise.then(res =>{
  	console.log(res);//“success”
}).catch(err => {
  	console.log(err);//“fail”
})
```

> [参考](https://www.liaoxuefeng.com/wiki/1022910821149312/1023024413276544)这个讲的还不错但还是不太懂



Promise 是同步执行

```javascript
let promise = new Promise((resolve,reject)=>{
    console.log("1");
});
console.log('2');
//顺序打印 1，2
```

resolve 时打印对应的 then 方法

```javascript
let promise = new Promise((resolve,reject)=>{
    // resolve("承诺实现"); 
    reject("承诺拒绝");
});
promise.then((res)=>{
    console.log(res);
},(err)=>{
  	  console.log(err);
})
```

then 是异步调用

```javascript
let promise = new Promise((resolve,reject)=>{
    // resolve("承诺实现"); 
    reject("承诺拒绝");
});
promise.then((res)=>{
    console.log(res);
},(err)=>{
  	  console.log(err);
})
console.log("Global");
//顺序打印：Global、承诺拒绝
```

可以 then 里面再套 then

```javascript
let promise = new Promise((resolve,reject)=>{
   	 resolve("承诺实现");
});
promise.then((res)=>{
      console.log(res);
      return new Promise((resolve,reject)=>resolve("成功"));
}).then((res)=>{
    	console.log(res);
})
console.log("Global");
//顺序打印：Global、承诺实现、成功


```

### catch()

Promise 对象的错误有冒泡性质，会一直向后传递直到被处理，但是 catch 和（err）只会有一个生效

```javascript
let promise = new Promise((resolve,reject)=>{
    reject("承诺拒绝");
});
promise.then((res)=>{
    console.log(1)
}).then(()=>{

}).then(()=>{

},catch(err)=>{
    console.log(err);
})

console.log("Global");
//Global,承诺拒绝
```

### finally()

### throw

可以和 catch/then().(err)一起用

```javascript
let promise = new Promise((resolve,reject)=>{
      throw new Error("throw:承诺拒绝");
});
promise.then((res)=>{
    	console.log(1)
}).then(()=>{

}).then(()=>{

}).catch(err=>{
    	console.log("catch",err);
})

console.log("Global");
//Global、catch Error: throw:承诺拒绝
```

> [参考这个](https://www.bilibili.com/video/BV1sv411i7v8?t=23&p=4)，讲得不错，但是例子使用 require 目前不会用

### 

### Promise.all()方法

Promise.all()静态方法创建的期约会在一组期约全部解决之后再解决。

```
Promise.all(iterable)
```

不一定传入 Promise对象，可以字符串

iterable 内部元素传递的是 promise对象集合，如果不是则直接 resolve，只要一个是 rejected 状态，直接返回 rejected

```javascript
const fs = require("fs");

function readFile(path,isSetError) {
    return new Promise((resolve,reject)=>{
        fs.readFile(path,"utf-8",function(err,data){
            if(err||isSetError){
                reject(err);
            }
            const resData = JSON.parse(data);
            resolve(resData);
        })
    })
}
Promise.all([
    readFile('./user.json'),
    readFile('./course.json',true),
    readFile('./userCourse.json')
])
.then((res)=>{
    console.log(res);
}),(err)=>{
   console.log(err) ;
}

```

```javascript
let  promise = new Promise((resolve,reject)=>{
    resolve("承诺成功");
})
.then((res)=>{
    console.log(res);
  //想知道这个 res的状态可以加上👇这句
  Promise.resolve("成功！");
})
.then((res)=>{
    
})
```



### Promise.race()

返回最先完成的 Promise 的结果，无论是完成还是拒绝

```javascript
const fs = require("fs");
function readFile(path,isSetError){
    return new Promise((resolve,reject)=>{
        fs.readFile(path,"utf-8",function(err,data){
            if(err||isSetError){
                reject("Failed");
            }
            const resData = JSON.parse(data);
            resolve(resData);
        })
    })
}
Promise.race([
    readFile('./user.json'),
    readFile('./course.json',true),
    readFile('./userCourse.json')
])
.then((res)=>{
    console.log(res);
})
```

应用场景：测试资源或者接口的响应速度

```html
<script>
        function getImg() {
            return new Promise((resolve, reject) => {
                const oImg = new Image();
                oImg.onload = function () {
                    resolve(oImg);
                }
                oImg.src = "./1.jpeg";
            })
        }
        function timeout() {
            return new Promise((resolve, reject) => {
                setTimeout(() => {
                    reject("图片请求超时");
                }, 1000);
            })
        }
        Promise.race([
            getImg(),
            timeout()
        ])
            .then((res) => {
                console.log(res);
            }).catch((err) => {
                console.log(err);
            })
    </script>
```

## async/await 异步函数

`async/await` 是Promise 的应用，使得 JS 用同步方式写的代码可以异步执行

- `await `操作符，等待一个 Promise 对象产出结果的操作手段

- `async`用于声明异步函数，必须在异步函数中使用
- 其中真正起作用的是 `await`,JS运行遇到await 时会记录在哪里暂停执行，等到 await 右边的值可以用了 JS 运行时会向消息队列中推送一个任务，这个任务会恢复异步函数的执行。

### async 返回值

返回 `Promise`对象，通过一个隐式的 Promise返回 pending 状态，返回值意义不大主要是需要 await

`async` 如果没有指定 return 值则会返回 undefined



```javascript
function getData(){
    return new Promise((resolve,reject)=>{
            resolve("成功");
  }).then((res)=>{
        console.log(res);
  }).catch((err)=>{
        console.log(err);
  })
}
async function loadData(){
  const data = await getData();
}
console.log("async return",loadData());
//输出：
//async return Promise { <pending> }
//成功
```

### async 和 promise 和 global 执行顺序

`async` 意思是当前这个异步函数与同一作用域下的程序（以下为global）是异步关系

`awit`可以暂停异步函数代码的执行等待期约解决

EX1

```javascript
function getData(){
    return new Promise((resolve,reject)=>{
            resolve("成功");
  }).then((res)=>{
        console.log(res);
  }).catch((err)=>{
        console.log(err);
  })
}
async function loadData(){
  	console.log("before await");
  	const data = await getData();
  	console.log("async,",data);
}

loadData();
console.log("global");

//输出
//before await
//global
//成功
//async, undefined
```

EX2

```javascript
async function foo(){
      console.log(2);
      console.log(await Promise.resolve(8));
      console.log(9);
}
async function bar(){
      console.log(4);
      console.log(await 6);
      console.log(7);
}
console.log(1);
foo();
console.log(3);
bar();
console.log(5)
//打印：1、2、3、4、5、8、9、6、7
```

遇到 await 之后，暂停执行，将后面的内容添加到消息**队列**，退出执行 Global 内容，Global 执行完毕再从消息队列先进先出

[[JS执行机制]]

### 比较

async await 方式比用 Promise.then 的方式好，结构较清晰

并不是 async await 代替 Promise，而是在处理 Promise 结果时可以使用 async await 

```javascript
async function loadData(){
  	const data1 = await getData();  
  	const data2 = await getData(data1);
   	 const data3 = await getData(data2);
  	console.log("async,",data3);
}
```





## URL解析

```
function getReques(url) {
            //查询拆分
            if(url.indexOf("?")!=-1){
                var str = url.substr(url.indexOf("?")+1);
                console.log('str',str);
                var theRequest = []
                strs = str.split('&');
                for(var i=0;i<strs.length;i++){
                    //unescape 和 escape 是一对解-编码字符串
                    theRequest[strs[i].split('=')[0]]=unescape(strs[i].split('=')[1])
                }
                console.log('theRequest',theRequest);
//theRequest 
//[fr: "iks", word: "slice", ie: "gbk"]
//fr: "iks"
//ie: "gbk"
//word: "slice"
//length: 0
//__proto__: Array(0)
//               return theRequest;
       }
}
var url = "https: //zhidao.baidu.com/question/1768422895052400180.html?fr=iks&word=slice&ie=gbk";
getReques(url);
//当前网址地址
var curr = window.location;
console.log('curr',curr);
console.log('协议',curr.protocol);//http:
console.log('主机名',curr.hostname);//主机名
console.log('端口',curr.port);//端口
console.log('主机',curr.host);//主机
console.log('来源',curr.origin);//来源
console.log('从域名最后一个/到？',curr.pathname);//从域名的最后一个/到？
console.log('锚点',curr.hash);//锚点
console.log('查询参数',curr.search);//查询参数
```

## ⭐️==手写代码==

### Promise.all()

```javascript
function all(arr){
  //返回一个promise
  return new Promise((res,rej) => {
    let length = arr.length  //传入的promise的个数
    let count = 0  //进入fullfilled的promise个数
    const result = []  //创建一个等长的数组,放置结果
    // 当传递是一个空数组，返回一个为fulfilled状态的promise
    if(arr.length === 0 ) {
      return new Promise.resolve(arr)
    }
    for(let i = 0; i < arr.length; i++){
      arr[i].then(resolve => {
        result.push(resolve) //将每次结果保存在result数组中
        count ++  //个数加1
        //是否所有的promise都进入fullfilled状态
        if(count === length){
          res(result)  //返回结果
        }
      }).catch(e => {
        rej(e)  //如果有错误则直接结束循环，并返回错误
      })
    }
  })
}

```

### Promise.race()

```javascript
function race(arr){
  return new Promise((res,rej) => {
    for(let i = 0; i < arr.length; i++){
      arr[i].then(resolve => {
        res(resolve)  //某一promise完成后直接返回其值
      }).catch(e => {
        rej(e)  //如果有错误则直接结束循环，并返回错误
      })
    }
  })
}

```



### 数组去重

1. Array.from(new Set(arr))
2. 嵌套循环
```javascript
function unique(arr){            
    for(var i=0; i<arr.length; i++){
        for(var j=i+1; j<arr.length; j++){
            if(arr[i]==arr[j]){         //第一个等同于第二个，splice方法删除第二个
                arr.splice(j,1);
                j--;
            }
        }
    }
  return arr;
}
```
3. indexof 去重
```javascript
function unique(arr) {
    if (!Array.isArray(arr)) {
        console.log('type error!')
        return
    }
    var array = [];
    for (var i = 0; i < arr.length; i++) {
        if (array .indexOf(arr[i]) === -1) {
            array .push(arr[i])
        }
    }
    return array;
}
```
4. sort()
```javascript
function unique(arr) {
    if (!Array.isArray(arr)) {
        console.log('type error!')
        return;
    }
    arr = arr.sort()
    var arrry= [arr[0]];
    for (var i = 1; i < arr.length; i++) {
        if (arr[i] !== arr[i-1]) {
            arrry.push(arr[i]);
        }
    }
    return arrry;
}
```
5. includes()
```javascript
function unique(arr) {
    if (!Array.isArray(arr)) {
        console.log('type error!')
        return
    }
    var array =[];
    for(var i = 0; i < arr.length; i++) {
            if( !array.includes( arr[i]) ) {//includes 检测数组是否有某个值
                    array.push(arr[i]);
              }
    }
    return array
}
```
6. hasOwnProperty
```javascript
function unique(arr) {
    var obj = {};
    return arr.filter(function(item, index, arr){
        return obj.hasOwnProperty(typeof item + item) ? false : (obj[typeof item + item] = true)
    })
}
```
### 防抖debounce
针对响应跟不上触发频率的问题

为什么需要防抖节流

前端开发过程中，有一些事件，常见的例如，**onresize**，**scroll**，**mousemove** ,**mousehover** 等，会被频繁触发（短时间内多次触发），不做限制的话，有可能一秒之内执行几十次、几百次，如果在这些函数内部执行了其他函数，尤其是执行了操作 DOM 的函数（浏览器操作 DOM 是很耗费性能的），那不仅会浪费计算机资源，还会降低程序运行速度，甚至造成浏览器卡死、崩溃。这种问题显然是致命的。

[参考🔗](https://blog.csdn.net/txfyteen/article/details/104457688)


当事件被触发时，设定一个周期延迟执行动作，若期间又被触发，则重新设定周期，直到周期结束，执行动作

**当事件快速连续不断触发时，动作只会执行一次**


```javascript
function func() {
            console.log(1);
            console.log(this)
        }
        
        function debounce(fn, delay) {
            // 点击-》清除延时-》设置定时器-》规定时间内点击-》清除延时。。。
            console.log(this)
            let timer;
            return function () {
                let context = this;
                let args = arguments;// 保留传递给fn的参数
                clearTimeout(timer)
                timer = setTimeout(function () {
                    fn.call(context,arguments);
                }, delay);
            }
        }
        var btn = document.querySelector('button');
        btn.addEventListener('click', debounce(func, 1000))

```

### 节流 throttling

固定周期内，只执行一次动作，若有新事件触发，不执行。周期结束后，又有事件触发，开始新的周期

**连续高频触发事件时，动作会被定期执行，响应平滑**

```javascript
function coloring(){
            let r = Math.floor(Math.random()*255)
            let g = Math.floor(Math.random()*255)
            let b = Math.floor(Math.random()*255)
            document.body.style.background = `rgb(${r},${g},${b})`;
        }
        function throttle(fn,delay){
            let timer;
            // 判断触发事件是否在时间间隔内
            // 触发，设定时间间隔，时间间隔内的任何操作都会被忽略，
            return function(){
                let context = this;
                let args = arguments;
                if(timer){
                    return 
                }
                timer = setTimeout(function(){
                    fn.call(context,args);
                    timer = null;
                },delay)
            }
        }
        window.addEventListener('resize',throttle(coloring,2000));
```
### 展平数组的方法

```javascript
//第一种方法：
const arr1 = arr.flat(Infinity)

// 第二种方案：利用join展平
const arr2 = arr.join().split(',').map(Number)

// 第三种方案：利用toString直接展平
const arr3 = arr.toString().split(',').map(Number)

// 第四种方案：原生循环递归实现
function flat(arr) {
    const result = []
    arr.forEach((item) => {
        if (Array.isArray(item)) {
            result.push(...flat(item))
        } else {
            result.push(item)
        }
    })
    return result
}
```

### 对象深拷贝

浅拷贝

```javascript
  //浅拷贝：Object.assign
var obj={
  	stu:{name:'小明',age:12},
};
var obj2={};
Object.assign(obj2,obj);
obj2.stu.name='hong';
```

浅拷贝只是拷贝变量的引用，如果改变新对象的值原对象的属性也会改变

1. 深拷贝

```javascript
JSON.parse(JSON.stringify(obj));
```

缺点：

1. 性能问题，stringify再解析其实需要耗费较多时间，特别是数据量大的时候。

2. 一些类型无法拷贝，例如函数(不输出)，正则(输出空对象)，时间对象(输出时间字符串)，Undefiend(不输出)等等问题

3. 深拷贝递归

```javascript
function deepCopy(obj = {}) {
    if (typeof obj != 'object' || obj == null) {
        return obj;
    }
    let result;
    if (obj instanceof Array) {
        result = [];//数组
    } else {//对象
        result = {};
    }
    for (let key in obj) {
        if (obj.hasOwnProperty(key)) {
            result[key] = deepCopy(obj[key]);//递归拷贝
        }
    }
    return result;
}
```

### 千分位分隔符

```javascript
function numberModify(number) {
    var numberStr = Array.from(number.toString());
    var len = numberStr.length;
    var arr = [];
    var res = '';
    for (let i = 1; i <= len / 3; i++) {
        var spliced = numberStr.splice((len - i * 3), (len - i * 3) + 3).toString();
        var replaced = spliced.replace(/,/g,'');
        arr.unshift(replaced);
    }
    arr.unshift([numberStr.splice(0, len - len / 3)].toString().replace(',',''));

    res = arr.join(',');
    return res;
}
```

### bind()函数

```javascript
function test(a,b,c){
    console.log(a,b,c);
    return 'bind函数'
}

Function.prototype.myBind = function(){
    const self = this;
    const args = Array.prototype.slice.call(arguments);
    const thisValue = args.shift(); //取头
    return function(){
        return self.apply(thisValue,self);
    }
}

const result = test(1,10,100);
const bindTest = test.myBind({name:'yn'},7,77,100);
```

### call()函数

```javascript
Function.prototype.myCall = function (context) {
      const ctx = context || window;	//obj
        console.log('ctx', ctx);
        ctx.func = this;
        console.log(this);//function: a 被调用的函数
        const args = Array.from(arguments).slice(1);
        console.log(arguments,...args);
      
        const res = arguments.length > 1 ? ctx.func(...args) : ctx.func();
  	  //1. arguments.length > 1  ctx.func（1，2）
        //ctx.func（1，2）=> a(1,2) 1,2
        //2. arguments.length <= 1  ctx.func（）;
        //ctx.func（1，2）=> a(x,y) undefined undefined
        delete ctx.func;
        return res;
}
const obj = {c: 2};
function a(x, y) {
    console.log(this, x, y);
}
a.call(obj, 1, 2);
a.myCall(obj, 1, 2);
```

### Apply()函数

```javascript
Function.prototype.myApply = function (context) {
      const ctx = context || window;	//obj
      console.log('ctx', ctx);
      ctx.func = this;
      console.log(this);	//function: a 被调用的函数
      //arguments.length > 1  ctx.func（1，2）
      //ctx.func（[1，2]）=> a(1,2) 1,2
      //arguments.length <= 1  ctx.func（）;
      //ctx.func（1，2）=> a(x,y) undefined undefined
      const res = arguments[1].length > 1 ? ctx.func(...arguments[1]) : ctx.func();
      delete ctx.func;
      return res;
}
```

### new构造的实现

```javascript
function newFunction() {
    let res = {};
    console.log(arguments);
    let construct = Array.prototype.shift.call(arguments);
    console.log(arguments, construct);
    //对象的原型 赋值
    res.__proto__ = construct.prototype;
    let turn = construct.apply(res, arguments);
    console.log(res, turn)
    //返回对象
    return turn instanceof Object ? turn : res;
}

function Person(name, age) {
    this.name = name;
    this.age = age;
}

Person.prototype.sayHi = function () {
    console.log(this.name);
}
var person1 = newFunction(Person, 'xm', 18);
console.log(person1.age);
person1.sayHi();

```

### Object.create()实现

```javascript
function createObject(object){
      function Fn(){};
      Fn.prototype = object;
      return new Fn();
}
const person= {
    	name:'willim',
}
const p1 = createObject(person);
console.log(p1.name);
```
