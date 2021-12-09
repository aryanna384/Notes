### 代理

代理是**目标对象的抽象**

从很多方面看,代理类似 C++指针,因为它可以用作目标对象的替身, 但又完全独立于目标对象。 目标对象既可以直接被操作, 也可以通过代理来操作。但直接操作会绕过代理施予的行为。

### 新建代理

```javascript
const target = {    
  id: 'target' 
};  
const handler = {};

const proxy = new Proxy(target, handler);
```

```html
<script>
  const usr = {
    name:"zoro",
    age:20,
    wife:{
      name:"sanji",
      age:20
    }
  }
  //target
  //handler
  const proxyUser = new Proxy(usr,{
    get(){ },
    set(){ },
    deleteProperty(){}
  })
</script>
```

```html
<script>
    const proxyUser = new Proxy(usr,{
      get(target,prop){
        console.log("get 方法");
        return Reflect.get(target,prop)
      },
      set(target,prop,val){},
      deleteProperty(){}
    })
    console.log(proxyUser.name);
    proxyUser.name = "luffe"
  	console.log(proxyUser);//并没有改
</script>
```

更新 set之后有更改

```html
<script>
    const usr = {
      name:"zoro",
      age:20,
      wife:{
        name:"sanji",
        age:20
      }
    }
    const proxyUser = new Proxy(usr,{
      get(target,prop){
        console.log("get 方法");
        return Reflect.get(target,prop)
      },
      set(target,prop,val){
        console.log("Set 方法");
        return Reflect.set(target,prop,val)
      },
      deleteProperty(){}
    })
    console.log(proxyUser.name);
    proxyUser.name = "luffe"
    console.log(proxyUser);//自定义了 set 方法之后更改
</script>
```



```html
//通过代理对象获取目标对象中的某个属性值
console.log(proxyUser.name);
//通过代理对象更新目标对象上的某个属性值
proxyUser.name = "luffe"
//通过代理对象添加目标对象上的某个属性值
proxyUser.gender = "male"
```



