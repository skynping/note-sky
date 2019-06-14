<h1><center style="color:blue">Vue入门</center></h1>

[TOC]

## 1. vue基础指令

- v-cloak: 解决闪烁问题
- v-text: 默认没有闪烁问题，会替换标签内的内容
- v-html:可以解析标签
- v-bind:绑定属性的指令 缩写是 :
- v-on: 事件绑定机制  缩写是  @

实例：
```xml
<div id="app">
    <input type="button" value="浪起来" @click="lang" :title="mytitle">
    <input type="button" value="低调" v-on:click="stop" v-bind:title="mytitle">
    <div v-cloak>{{ msg }}</div>
    <div v-text="msg">啦啦啦</div>
</div>
```
```JavaScript
var vm = new Vue({
    el:"#app",
    data:{
        msg:"猥琐发育，别浪~~!  ",
        intervalId:null,
        mytitle: "点击！！"
    },
    methods:{
        lang(){
            if(this.intervalId) return;
            this.intervalId = setInterval(()=>{
                var start = this.msg.substring(0,1);
                var end = this.msg.substring(1);
                this.msg = end + start;
            },100)
        },

        stop(){
            clearInterval(this.intervalId);
            this.intervalId = null;
        }
    }
})
```

## 2. 事件修饰符
-  **<span style="color:blue">.stop</span>** 阻止冒泡
-  **<span style="color:blue">.prevent</span>** 阻止默认行为
-  **<span style="color:blue">.self</span>** 只有点击当前元素的时候才触发事件处理函数
-  **<span style="color:blue">.once</span>** 只触发一次

实例：
```xml
<!-- 阻止跳转 -->
<a href="http://www.baidu.com" @click.prevent="linkclick">百度一下</a>
<!-- 阻止第一次跳转 -->
<a href="http://www.baidu.com" @click.prevent.once="linkclick">百度一下</a>
```

## 3. 数据双向绑定
- **v-model**指令可以实现表单元素(**input select checkbox**)和**Model**实现数据双向绑定
```xml
<div id="app">
    <h4>{{msg}}</h4>
    <input type="text" v-model="msg" style="width:100%">
</div>
```
```js
var vm = new Vue({
        el:"#app",
        data:{
            msg:"blabla"
        }
    })
```

## 4. 通过属性绑定元素设置class
css代码：
```css
.mycolor{
    color: blue;
}

.thin{
    font-size: 30px;
}

.fontfamily{
    font-family:'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    }
```
```xml
<div id="app">
    <p :class="{mycolor:flag,thin:!flag,fontfamily:true}">这是一个普通测试！！！</p>
</div>
```
```javascript
var vm = new Vue({
    el:"#app",
    data:{
        flag:false
    }
})

```
## 5. v-for指令的使用
```xml
<div id="app">
    <!-- 循环数组 -->
    <p v-for="(item, index) in arr">{{index}}---{{item}}</p>
    <hr>
    <p v-for="item,index in arrs">{{item.name}}---{{item.id}}--{{index}}</p>
    <hr>
    <!-- 遍历对象 -->
    <p v-for="val,key,index in dict">{{key}}---{{val}}---{{index}}</p>
    <hr>
    <p v-for="val in 10">{{val}}</p>
</div>
```
```js
var vm = new Vue({
    el:"#app",
    data:{
        arr:[1,2,3,4,5,6,7,8,9,0],
        arrs:[
            {"name":"a1", "id":1},
            {"name":"a2", "id":2},
            {"name":"a3", "id":3},
            {"name":"a4", "id":4},
            {"name":"a5", "id":5}
        ],
        dict:{
            name:"jack",
            age:12,
            phone:"1289284039"
        }
    }

})
```
#### 5.1 v-forz中的key
- key 保证了每一个唯一性--

```xml
<div id="app">
    <label>
        <input type="text" v-model="id" placeholder="id" autocomplete="false" required>
    </label>
    <label>
        <input type="text" v-model="name" placeholder="name" autocomplete="false" required>
    </label>
    <input type="button" @click="add" value="添加">
    <br>
    <!-- :key 保证了每一个唯一性-->
    <label v-for="item,index in lists"  :key="item.id">
        <input type="checkbox">{{item.id}}--{{item.name}}
        <br>
    </label>
</div>
```
```js
var vm = new Vue({
    el:"#app",
    data:{
        id:null,
        name:null,
        lists:[
            {id:1,name:"嬴政"},
            {id:2,name:"庄周"}
        ]
    },
    methods:{
        add(){
            this.lists.unshift({id:this.id,name:this.name});
            this.id = null;
            this.name = null;
        }
    }
})
```
## 6.v-if 和v-show的使用
- **v-if**可以进行条件判断
- **v-show**根据条件判断是否显示，底层未 display属性的设置

```xml
<div id="app">
    <input type="button" value="v-if" @click="ifToggle">
    <input type="button" value="v-for" @click="forToggle">
    <p v-if="ifFlag">这是v-if</p>
    <P v-show="forFlag">这是v-show</P>
</div>
```
```js
var vm = new Vue({
    el:"#app",
    data:{
        ifFlag:true,
        forFlag:true
    },
    methods:{
        ifToggle(){
            this.ifFlag = !this.ifFlag;
        },
        forToggle(){
            this.forFlag = !this.forFlag; 
        }
    }
})
```

