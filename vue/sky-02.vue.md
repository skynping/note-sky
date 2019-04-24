<h1><center style="color:blue">Vue入门</center></h1>

[TOC]

## 1.过滤器

#### 1.1 全局过滤器

- 过滤器调用的格式 **{{name|过滤器名称 | ...}}**

```xml
<span>{{item.id}}--{{item.name}}---{{item.date | dateFormat("yyyy-mm-dd")}}</span>
```

```js
// 全局过滤器,可以有多个
// 第一个参数过滤器名称,回调函数第一个参数默认是需要过滤的内容
// 字符处理padStart，第一个参数保留多少个字符，第二个参数为不够时以什么字符填充
Vue.filter("dateFormat",(data,pattern="")=>{
    var y = data.getFullYear();
    var m = (data.getMonth() + 1).toString().padStart(2,"0");
    var d = data.getDate().toString().padStart(2,"0");

    if(pattern.toLowerCase()=="yyyy-mm-dd"){
        return `${y}-${m}-${d}`;
    }else{
        var H = data.getHours().toString().padStart(2,"0");
        var M = data.getMinutes().toString().padStart(2,"0");
        var S = data.getSeconds().toString().padStart(2,"0");
        return `${y}-${m}-${d}  ${H}:${M}:${S}`;
    }

})
```



#### 1.2 局部过滤器

```js
var vm2 = new Vue({
    el:'#app2',
    data:{
        name:new Date()
    },
    methods:{},
    filters:{//私有过滤器，当和全局过滤器冲突时，以私有为准
        dateFormat2:function(data,pattern=""){
            var y = data.getFullYear();
            var m = data.getMonth() + 1;
            var d = data.getDate();


            if(pattern.toLowerCase()=="yyyy-mm-dd"){
                return `${y}-${m}-${d}`;
            }else{
                var H = data.getHours();
                var M = data.getMinutes();
                var S = data.getSeconds();
                return `${y}-${m}-${d}  ${H}:${M}:${S}`;
            }
        }
    }
})
```



## 2. 自定义按键修饰符

- vue提供以下按键：**.enter .tab .delete .esc .space .up .down .left .down**
- [键盘码表](www.cnblogs.com/wuhua1/p/6686237.html) , 113对应F2

```js
// 自定义全局按键修饰符
Vue.config.keyCodes.F2 = 113;
```

```xml
<!-- keyup键盘抬起事件 -->
姓名：<input type="text" v-model="name" @keyup.F2="add">
```



## 3. 字符串处理padStart

- 字符处理padStart，第一个参数保留多少个字符，第二个参数为不够时以什么字符填充

```js
"".padStart(2,"0")
```



## 4. 指令

#### 4.1 全局指令

```js
// 使用自定义全局指令
// 参数1，指令的名称，定义时不需要加v-前缀，使用的时候需要
// 参数2,有些特定的指令函数
Vue.directive("focus",{
    bind:(el)=>{
        /*  el参数是原生的js对象

                */
    },
    inserted:(el)=>{
        el.focus();
    },
    updated:(el)=>{

    }
})
```

```xml
<input type="text" v-model="keyword" placeholder="查询关键字" v-focus>
```



#### 4.2 私有指令

```js
var vm2 = new Vue({
    el:'#app2',
    data:{

    },
    methods:{},
    filters:{
    },
    directives:{// 私有指令

    }
```

