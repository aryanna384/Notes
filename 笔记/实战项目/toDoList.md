[TOC]



### 功能

- 计划：可以已完成或者删除

- 已完成：已完成列表可以更改状态，但是不可修改

- 删除（不可行的计划）

  

### 步骤

- Html 结构

- css 样式

- JS 功能
  - 输入框输入功能
  - 列表显示功能
  - 修改列表功能

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title>TodoList</title>
    <link rel="stylesheet" href="css.css">
</head>

<body>
    <div class="main">
        <div class="header">
            <div class="logo">ynTodo</div>
            <input class="userInput" placeholder="今日计划">
        </div>
        <div class="todo">
            <h3>
                <span class="title">toDo</span>
                <span class="count">0</span>
            </h3>
            <div class="list">
                <div class="todoItem">
                    <input type="checkbox" >
                    <div class="content">今日计划：完成今日计划</div>
                    <button class="delete">del</button>
                </div>
            </div>

        </div>
        <div class="done">
            <h3>
                <span class="title">done</span>
                <span class="count">0</span>
            </h3>
            <div class="list">
                <div class="todoItem">
                    <input type="checkbox" checked="checked">
                    <div class="content">今日计划：完成今日计划</div>
                    <button class="delete">del</button>
                </div>
            </div>
        </div>
    </div>
    </div>
    <script>
        if(localStorage.todoList == undefined){
            var todoList =[];
        }else{
            //json 格式字符串转为对象
            var todoList = JSON.parse(localStorage.todoList);
        }
        
        var todoDiv = document.querySelector(".todo .list");
        var doneDiv = document.querySelector(".done .list")//.querySelector(".list");
        var inputDoc = document.querySelector(".userInput");
        var mainDiv = document.querySelector(".main");
        render(todoList);
       
        inputDoc.onkeydown = function (event) {
            // console.log(event.key,event.keyCode);//keyCode 是 ASCII 码
            if(event.key === "Enter"){
                var value = inputDoc.value;
                console.log(value);
                var objItem = {
                    content:value,
                    isDone:false,
                }
                todoList.push(objItem);
                render(todoList);
                inputDoc.value = "";
            }            
        }
        function render(todoList){
            //localStorage 只能存储字符串，直接复制显示的是[object,object]
            localStorage.todoList = JSON.stringify(todoList);
            todoDiv.innerHTML = "";
            doneDiv.innerHTML = "";
            todoList.forEach(function(item,index){
                var todo_item = document.createElement("div");
                todo_item.className = "todoItem";
                if(item.isDone){
                    todo_item.innerHTML = `
                    <input type="checkbox" checked="checked" data-index=${index}>
                    <div class="content">${item.content}</div>
                    <button class="delete" data-index=${index}>del</button>`;
                    doneDiv.appendChild(todo_item);
                }else{
                    todo_item.innerHTML = `
                    <input type="checkbox" data-index=${index}>
                    <div class="content">${item.content}</div>
                    <button class="delete" data-index=${index}>del</button>`;
                    todoDiv.appendChild(todo_item);
                }                
            });
        }
        //删除
        function deleteItem(candiList,pos){
            candiList.forEach(function(item,index){
                if(item.isDone){
                    candiList.splice(pos,1);
                    console.log(candiList);
                }
            });
        };
        todoDiv.onchange = function(e){
            //通过 data-index 获取勾选的是第几个
            var index = parseInt(e.target.dataset.index);
            todoList[index].isDone = true;
            render(todoList);        
        }
        doneDiv.onchange = function(e){
            var index = parseInt(e.target.dataset.index);
            todoList[index].isDone = false;
            render(todoList);        
        }
        mainDiv.onclick = function(e){
            if(e.target.className == "delete"){
                var index = parseInt(e.target.dataset.index);
                todoList.splice(index,1);
                render(todoList);
            }            
        } 
    </script>
</body>
</html>
```



`JSON.stringify()`对象转成 json 字符串

`JSON.parse()`:json 字符串转成对象

### 接口
