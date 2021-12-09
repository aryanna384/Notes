[TOC]

##  同步&异步操作

同步：对应内存中顺序执行的处理器指令。每条指令都会严格按照出现的顺序执行

异步：

- 类似系统终端，当前进程外部的实体可以触发代码的执行

- 非阻塞，异步逻辑和主逻辑相互独立，主逻辑不需要等待异步逻辑完成，可以立即继续执行

```javascript
console.log("a");
console.log("b");
console.log("c");
//没有阻塞主程序的运行
setTimeout(()=>{console.log("异步操作")},2000);
console.log("d");
console.log("e");

//打印
a
b
c
d
e
异步操作
```

[[零散#箭%%%%头函数]]

[[零散[[setTimeout]]]]



## 回调函数callback function

[参考](https://blog.csdn.net/hu_belif/article/details/80284140)

字面上的理解，回调函数就是传递一个参数化的函数，就是将这个函数作为一个参数传到另一个主函数里面，当那一个主函数执行完之后，再执行传进去的作为参数的函数。走这个过程的参数化的函数 就叫做回调函数。换个说法也就是被作为参数传递到另一个函数（主函数）的那个函数就叫做回调函数。

[[零散[[setTimeout]]]]用来模拟无法立即执行的函数

```javascript
function title(value){//这是回调函数
		alert(value);
}

function main(title, value){//这个主函数:在参数列表中，title作为一个参数传递进来，也就是上文说的 参数化函数；然后value这个值正是title（）函数中所需要的。
		alert("我是主函数");
		title(value);//结果为：'我是回调函数'。——————然后在这行这个title()，它就是回调函数咯。
}
//主函数（回调函数，回调函数的参数）
main(title,"我是回调函数");

//PS:看清楚，调用的是main()函数，意味着先执行main()，这时已经执行了主函数，title()被main()在函数体中执行了一次，因此title()是回调函数
```

[[回调函数]]



## 回调地狱

回调函数套回调函数，难以维护，理论上可以一直无限地向下走

```javascript
function doSth(t){
    return function(){
        if(--t ===0){
            logSth(function(){
                logSth2(function(){
                    logSth3()
                })
            })
        }
    }
}
```















