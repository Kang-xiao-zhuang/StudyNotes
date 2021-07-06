*学习地址：[2019年coderwhy vue-vuejs从入门到精通教程_哔哩哔哩 (゜-゜)つロ 干杯~-bilibili](https://www.bilibili.com/video/BV17j411f74d)**

**写在前面：学习的版本不同，有些语法在新版本已经废弃，具体详解参考官方文档！**

**官方文档：[https://cn.vuejs.org/v2/guide/](https://cn.vuejs.org/v2/guide/)**

**最后感谢coderwhy老师的讲解视频！十分感谢！**

# 1，什么是Vue

- Vue.js（读音 /vjuː/, 类似于 view） 是一套构建用户界面的渐进式框架。

- Vue 只关注视图层， 采用自底向上增量开发的设计。

- Vue 的目标是通过尽可能简单的 API 实现响应的数据绑定和组合的视图组件。

**学习前需要有前端三大件的基础 html，css，JavaScript**

## 1.1 Vue第一个程序

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
</head>
<body>
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<div id="app">{{message}}</div>
<script>
  // let(变量）/const（常量）
  // 编程范式：声明式编程

  // 创建Vue对象的时候，传入了一些option:{}
  // {}中包含了el属性：该属性决定了这个Vue对象挂载到哪一个元素上，
  // 很明显，这里是挂载到了id为app的元素上
  // {}中包含了data属性：该属性中通常会存储一些数据
  // 这些数据可以是我们直接定义出来的，比如像上面这样
  // 也可能是来自网络，从服务器加载的
  const app = new Vue({
    el: "#app",//用于挂载管理的元素
    data:{//定义数据
      message:"Hello zk I am Vue !"
    }
  })
  //传统js的做法（编程范式：命令式编程）
  //1.创建div元素，设置id属性
  //2.定义一个变量叫message
  //3.message变量放在前面的div元素中显示
</script>
</body>
</html>
```

![](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Snipaste_2021-04-20_21-33-57.png)

## 1.2 列表的案例

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
</head>
<body>
<!--引入vue-->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<div id="app">

  <ul>
    <li v-for="item in movies">{{item}}</li>
  </ul>
</div>
<script>
  const app = new Vue({
    el: "#app",
    data:{
      message:"Hello zk I am Vue !",
      movies:["速度与激情","海贼王","盗墓笔记","成龙历险记"]
    }
  })
</script>
</body>
</html>
```

![](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Snipaste_2021-04-20_21-34-24.png)

## 1.3 计算器案例

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
</head>
<body>
<!--引入vue-->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<div id="app">
  <h2>当前计数:{{counter}}</h2>

  <button v-on:click="add">+</button>
  <button v-on:click="sub">-</button>
</div>
<script>
  const app = new Vue({
    el: "#app",
    data:{
      counter:0
    },
    methods:{
      add:function (){
        console.log("add被执行");
        this.counter++
      },
      //减法
      sub:function () {
        console.log("sub被执行");
        this.counter--
      }
    }
  })
</script>
</body>
</html>
```

![](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Snipaste_2021-04-20_21-34-47.png)

## 1.5 Vue中MVMM

计数器的MVVM

- 我们的计数器中就有严格的MVVM思想

- View依然是我们的DOM

- Model就是我们我们抽离出来的obj

- ViewModel就是我们创建的Vue对象实例

它们之间如何工作呢？

- 首先ViewModel通过Data Binding让obj中的数据实时的在DOM中显示。

- 其次ViewModel通过DOM Listener来监听DOM事件，并且通过methods中的操作，来改变obj中的数据。

- 有了Vue帮助我们完成VueModel层的任务，在后续的开发，我们就可以专注于数据的处理，以及DOM的编写工作了。

**创建Vue实例传入的options**

创建vue实例的时候，传入一个对象options

**el:** 

- 类型：string | HTMLElement

- 作用：决定之后Vue实例会管理哪一个DOM。

**data:** 

- 类型：Object | Function （组件当中data必须是一个函数）

- 作用：Vue实例对应的数据对象。

**methods:** 

- 类型：{ [key: string]: Function }

- 作用：定义属于Vue的一些方法，可以在其他地方调用，也可以在指令中使用。



## 1.6 Vue的生命周期

![](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/图片1.png)

![图片3](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/图片3.png)

# 2，插值的操作

## 2.1 Mustache语法

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
</head>
<body>
<!--引入vue-->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<div id="app">
  <h2>{{message}}}</h2>
  <h2>{{firstName}}</h2>
  <h2>{{lastName}}</h2>
  <h2>{{firstName}}{{lastName}}</h2>
  <h2>{{age+"--"+pet}}</h2>
</div>
<script>
  const app = new Vue({
    el: "#app",
    data:{
      message:"Hello zk I am Vue !",
      firstName:"zhuang",
      lastName:"kang",
      age:"20",
      pet:"dog"
    }
  })
</script>
</body>
</html>
```

![](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Snipaste_2021-04-20_21-35-33.png)

## 2.2 V-once指令

- 该指令后面不需要跟任何表达式
- 该指令表示元素和组件只渲染一次，不会随着数据的改变而改变

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
</head>
<body>
<!--引入vue-->
<!--该指令后面不需要跟任何表达式
该指令表示元素和组件只渲染一次，不会随着数据的改变而改变-->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<div id="app">
  <h2>{{message}}</h2>
  <h2 v-once>
    {{message}}
  </h2>
</div>
<script>
  const app = new Vue({
    el: "#app",
    data:{
      message:"Hello zk I am Vue !"
    }
  })
</script>
</body>
</html>
```

![](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Snipaste_2021-04-20_21-36-58.png)

## 2.3 V-html指令

- 1.某些情况下，我们从服务器请求到的数据本身就是一个HTML代码
  - 如果我们直接通过{{}}来输出，会将HTML代码也一起输出
  - 但是我们可能希望的是按照HTML格式进行解析，并且显示对应的内容
- 2.如果我们希望解析出HTML展示，可以使用v-html指令
  - 该指令后面往往会跟上一个string类型
  - 会将string的html解析出来并且进行渲染

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
</head>
<body>
<!--引入vue-->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<div id="app">
  <h2>{{url}}</h2>
  <h2 v-html="url"></h2>
</div>
<script>
  const app = new Vue({
    el: "#app",
    data:{
      message:"Hello zk I am Vue !",
      url:'<a href="www.itkxz.cn">康小庄的博客</a>'
    }
  })
</script>
</body>
</html>
```

![](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Snipaste_2021-04-20_21-37-31.png)

## 2.4 V-text指令

- v-text作用和Mustache比较相似：都是用于数据显示在界面中
- v-text通常情况下，接受一个string类型
- v-text不够灵活，一般开发中，就是用Mustache语法插值

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
</head>
<body>
<!--引入vue-->
<!--v-text作用和Mustache比较相似：都是用于数据显示在界面中-->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<div id="app">
  <h2>{{message}}</h2>
  <h2 v-text="message"></h2>
</div>
<script>
  const app = new Vue({
    el: "#app",
    data:{
      message:"Hello zk I am Vue !"
    }
  })
</script>
</body>
</html>
```

![](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Snipaste_2021-04-20_21-38-20.png)

## 2.5 V-pre指令

- v-pre用于跳过这个元素和它子元素的编译过程，用于显示原来的Mustache语法

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
</head>
<body>
<!--引入vue-->
<!--v-pre用于跳过这个元素和它子元素的编译过程，用于显示原来的Mustache语法-->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<div id="app">
  <h2>{{message}}</h2>
  <h2 v-pre>{{message}}</h2>
</div>
<script>
  const app = new Vue({
    el: "#app",
    data:{
      message:"Hello zk I am Vue !"
    }
  })
</script>
</body>
</html>
```

![](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Snipaste_2021-04-20_21-38-45.png)

## 2.6 V-cloak指令

- 某些情况下，浏览器可能会直接显示出未编译的Mustache标签

```html
<!DOCTYPE html>
<html lang="en">
 <head> 
  <meta charset="UTF-8" /> 
  <title>Title</title>
 </head>
 <body> 
  <!--引入vue-->
  <!--某些情况下，浏览器可能会直接显示出未编译的Mustache标签--> 
  <script src="https://cdn.jsdelivr.net/npm/vue"></script> 
  <div id="app" v-cloak=""> 
   <h2>{{message}}</h2>
  </div> 
  <script>  const app = new Vue({    el: "#app",    data:{      message:"Hello zk I am Vue !"    }  })</script>  
 </body>
</html>
```

![](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Snipaste_2021-04-20_21-39-11.png)

# 3，Vue基础语法

## 3.1 V-bind指令

**1.除了内容需要动态来决定外，某些属性我们也希望动态来绑定**

- 比如动态绑定a元素的href属性

- 比如动态绑定img元素的src属性
  2.这个时候，我们可以使用v-bind指令：

**作用：动态绑定属性**

- 缩写：语法糖 ：（简写）
- 预期：any(with argument) | Object（without argument）
- 参数：attrOrProp（optional）

```html
<!DOCTYPE html>
<html lang="en">
 <head> 
  <meta charset="UTF-8" /> 
  <title>Title</title>
 </head>
 <body>
  <!--引入vue-->
  <script src="https://cdn.jsdelivr.net/npm/vue"></script>
  <div id="app"> 
   <img v-bind:src="imgURL" alt="" /> 
   <a v-bind:href="aHref">v-bind的基本使用</a>
   <!--  语法糖语法--> 
   <img :src="imgURL" alt="" /> 
   <a :href="aHref">v-bind的基本使用</a>
  </div>
  <script>  const app = new Vue({    el: "#app",    data:{      message:"Hello zk I am Vue !",      imgURL:"https://gitee.com/zhuang-kang/note-picture/raw/master/RabbitMQ%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/RabbitMQSnipaste_2021-04-15_21-37-31.png",      aHref:"https://gitee.com/zhuang-kang/note-picture/raw/master/RabbitMQ%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/RabbitMQSnipaste_2021-04-15_21-37-31.png"    }  })</script>
 </body>
</html>
```

![](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Snipaste_2021-04-20_22-53-24.png)



## 3.2 V-bind绑定class

- **直接通过{ }绑定一个类**

```javascript
<h2 :class="{'active':isActive}">Hello zk!</h2>
```

- **通过判断，传入多个值**

```javascript
<h2 :class="{'active':isActive,'family':isFamily}">Hello zk!</h2>
```

- **和普通的类同时存在，并不冲突**

```javascript
<h2 class="size" :class="{'active':isActive,'family':isFamily}">Hello zk ！</h2>
```

- **如果过于复杂，可以放在一个methods或者computed中**

```javascript
<h2 class="size" :class="classes">>Hello zk ！</h2>
```



```html
<!DOCTYPE html>
<html lang="en">
 <head> 
  <meta charset="UTF-8" /> 
  <title>Title</title>
  <style>  .active{    color:red;  }  .size{    font-size: 30px;  }  .family{    font-family: 宋体;  }</style>
 </head>
 <body>
  <!--引入vue-->
  <script src="https://cdn.jsdelivr.net/npm/vue"></script>
  <div id="app"> 
   <h2 class="size" v-bind:class="getClasses()">{{message}}</h2> 
   <button v-on:click="btnClickActive">切换颜色</button> 
   <button v-on:click="btnClickSize">切换字体风格</button>
  </div>
  <script>  const app = new Vue({    el: "#app",    data:{      message:"Hello zk I am Vue !",      isActive:true,      isFamily:false    },    methods: {      btnClickActive:function () {        this.isActive=!this.isActive      },      btnClickSize:function () {        this.isFamily=!this.isFamily      },      getClasses:function () {        return{active:this.isActive,family:this.isFamily}      }    }  })</script>
 </body>
</html>
```

![](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Snipaste_2021-04-20_23-09-40.png)

## 3.3 点击列表更改颜色

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
</head>
<style>
  .active{
    color:red
  }
</style>
<body>
<!--引入vue-->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<div id="app">

  <ul>
    <li v-for="(item,index) in movies"
    v-on:click="liClick(index)"
    v-bind:class="{active: activeIndex===index}">{{item}}

    </li>
  </ul>
</div>
<script>
  const app = new Vue({
    el: "#app",
    data:{
      message:"Hello zk I am Vue !",
      movies:["速度与激情","海贼王","盗墓笔记","成龙历险记"],
      activeIndex:-1
    },
    methods:{
      liClick:function (index) {
        this.activeIndex=index;
      }
    }
  })
</script>
</body>
</html>
```

![](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Snipaste_2021-04-20_23-18-45.png)

## 3.4 V-bind动态绑定style

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
</head>
<body>
<!--引入vue-->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<div id="app">
<!--    <h2 v-bind:style="{key(属性名):value(属性名)}">{{message}}</h2>-->
<!--    <h2 :style="{fontSize:'50px'}">{{message}}</h2>-->
<!--    <h2 :style="{fontSize:finalSize+'px',color:finalColor,background:finalBackgroundColor}">{{message}}</h2>-->
  {{message}}
<h2 :style="getStyles()">{{message}}</h2>
</div>
<script>
  const app = new Vue({
    el: "#app",
    data:{
      message:"Hello zk I am Vue !",
      finalSize:100,
      finalColor:"red",
      finalBackgroundColor:"blue"
    },
    methods:{
      getStyles(){
        return {fontsize:this.finalSize+'px',color:this.finalColor,background:this.finalBackgroundColor}
      }
    }
  })
</script>
</body>
</html>
```

![](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Snipaste_2021-04-20_23-24-58.png)



## 3.5 计算属性的基本使用

1.在模板中可以直接通过插值语语法显示一些data中的数据

2.但是在某些情况下，我们可能需要对数据进行一些转化后在显示，或者需要将多个数据结合起来进行显示

- 比如我们有firstName和lastName两个变量，我们需要显示完整的名称
- 但是如果多个地方都需要显示完整的名称，我们就需要写多个{{firstName}} {{lastName}}

3.我们可以将上面代码换成计算属性

计算属性是写在实例的computed选项中的

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
</head>
<body>
<!--引入vue-->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<div id="app">
  <h2>{{firstName}} {{lastName}}</h2>
  <h2>{{firstName+" "+lastName}}</h2>
  <h2>{{getFullName()}}</h2>
  <h2>{{fullName}}</h2>
</div>
<script>
  const app = new Vue({
    el: "#app",
    data: {
      message: "Hello zk I am Vue !",
      firstName: 'zhuang',
      lastName: 'kang'
    },
    computed: {

      fullName(){
        return this.firstName+" "+this.lastName
      }
    },
    methods:{
      getFullName(){
        return this.firstName+" "+this.lastName
      }
    }
  })
</script>
</body>
</html>
```

![](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Snipaste_2021-04-21_12-49-23.png)

## 3.6 计算属性的高级操作

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
</head>
<body>
<!--引入vue-->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<div id="app">
  <h2>总价格:{{totalPrice}}</h2>
</div>
<script>
  const app = new Vue({
    el: "#app",
    data:{
      message:"Hello zk I am Vue !",
      books:[
        {id:1,name: '书1',price:10},
        {id:2,name: '书2',price:15},
        {id:3,name: '书3',price:20},
        {id:4,name: '书4',price:30},
      ]
    },
    computed:{
      totalPrice(){
        //高级用法 filter/map/reduce
        let result=0
        for (let i=0;i<this.books.length;i++){
          result+=this.books[i].price
        }
        return result
      }
    }
  })
</script>
</body>
</html>
```

![](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Snipaste_2021-04-21_13-04-06.png)

## 3.7 计算属性中的getter和setter方法

- 每个计算属性都包括一个getter和一个setter
- 语法糖情况下，表示getter，取数据
- setter一般不用

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
</head>
<body>
<!--引入vue-->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<div id="app">
  <h2>{{fullName}}</h2>
</div>
<script>
  const app = new Vue({
    el: "#app",
    data:{
      message:"Hello zk I am Vue !",
      firstName:'zhuang',
      lastName:'kang'
    },
    computed:{
      //不加动词，计算属性，以属性命名
      //语法糖，简写
      /*fullName() {
        return this.firstName+" "+this.lastName;
      }*/
      fullName:{
        //属性中get方法不适用，只是一个只读属性
        set(newValue){
          console.log('-->',newValue);
          const names=newValue.split('');
          this.firstName=names[0];
          this.firstName=names[1];
        },
        get(){
          return this.firstName+" "+this.lastName
        }
      }
    }
  })
</script>
</body>
</html>
```

![](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Snipaste_2021-04-21_13-13-11.png)

## 3.8 计算属性和methods方法对比

包含一个知识点

- 计算属性的缓存

- methods和computed看起来都可以实现我们的功能
- 那么为什么还要多一个计算属性这个东西呢？
- 原因：计算属性会进行缓存，如果多次使用,计算属性只会调用一次，极大提高了性能
- 除非原属性发生改变，才会重新调用计算属性，更改属性值

````html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
</head>
<body>
<!--引入vue-->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<div id="app">
  <!--  1.直接拼接:语法过于繁琐-->
  <h2>{{firstName}} {{lastName}}</h2>
  <!--  2.通过定义methods-->
  <h2>{{getFullName()}}</h2>
  <h2>{{getFullName()}}</h2>
  <h2>{{getFullName()}}</h2>
  <!--  3.通过computed-->
  <h2>{{fullName}}</h2>
  <h2>{{fullName}}</h2>
  <h2>{{fullName}}</h2>
</div>
<script>
  const app = new Vue({
    el: "#app",
    data:{
      message:"Hello zk I am Vue !",
      firstName:'Lebron',
      lastName:'James'
    },
    methods:{
      getFullName() {
        console.log('getFullName');
        return this.firstName+" "+this.lastName
      }
    },
    computed:{
      fullName() {
        console.log('fullName')
        return this.firstName+' '+this.lastName
      }
    }
  })
</script>
</body>
</html>
````

![](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Snipaste_2021-04-21_13-21-42.png)

## 3.9 ES6补充

这里不做笔记，后续学习ES6知识点！



## 3.10 V-on指令

**事件监听**

- 作用：绑定事件监听器
- 缩写：@
- 预期：Function | Inline Statement | Object
- 参数：event

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
</head>
<body>
<!--引入vue-->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<div id="app">
  <h2>当前计数器是:{{counter}}</h2>
  <button v-on:click="counter++">+</button>
  <button v-on:click="counter--">-</button>
  <button v-on:click="increment">+</button>
  <button v-on:click="decrement">-</button>
  <!--  简便写法-->
  <button @click="increment">+</button>
  <button @click="decrement">-</button>
</div>
<script>
  const app = new Vue({
    el: "#app",
    data:{
      message:"Hello zk I am Vue !",
      counter:0
    },
    methods:{
      increment(){
        this.counter++
      },
      decrement(){
        this.counter--
      }
    }
  })
</script>
</body>
</html>
```

![](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Snipaste_2021-04-21_13-30-56.png)

## 3.11 V-on参数传递问题

**参数**

当通过methods中定义方法，以供@click调用时，需要注意参数问题：

- 情况一：如果该方法不需要额外参数，那么方法后的()可以不添加。但是注意：如果方法本身中有一个参数，那么会默认将原生事件event参数传递进去
- 情况二：如果需要同时传入某个参数，同时需要event时，可以通过$event传入事件

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
</head>
<body>
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<div id="app">
  <!--  事件调用的方法没有参数-->
  <button style="color: red" @click="btn1Click()">按钮1-1</button>
  <button style="color: red" @click="btn1Click">按钮1-2</button>
  <!--  在事件定义时，写函数省略了小括号，凡是方法本身是需要小括号的-->
  <!--  这个时候，Vue会默认将浏览器生产的event事件对象作为参数传入到方法-->
  <button style="color: green" @click="btn2Click">按钮2-1</button>
  <!--  如果函数需要参数，但是没有传入，如果有括号的话，那么函数的形参为underfined-->
  <button style="color: green" @click="btn2Click()">按钮2-2</button>
  <button style="color: green" @click="btn2Click(123)">按钮2-3</button>
  <!--  定义方法时，我们需要event对象，同时又需要其他参数-->
  <!--  在调用方式，如何手动的获取到浏览器参数的event对象:  $event-->
  <button style="color: blue" @click="btn3Click(123,$event)">按钮3</button>
</div>
<script>
  const app = new Vue({
    el:"#app",
    data:{
      message:"Hello zk! "
    },
    methods:{
      btn1Click(){
        console.log("btn1Click");
      },
      btn2Click(event){
        console.log("------------------",event);
      },
      btn3Click(abc,event){
        console.log("++++++++++++++++++",abc, event);
      }
    }
  })
</script>
</body>
</html>
```

![](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Snipaste_2021-04-21_13-35-40.png)









## 3.12 v-on修饰符

**（一）在某些情况下，我们拿到event的目的可能是进行一些事件处理**
**（二）Vue提供了修饰符来帮助我们方便的处理一些事件：**

- .stop-调用event.stopPropagation()

- .prevent-调用event.preventDefault()

- .{keyCode | keyAlias}-只当事件是从特定键触发时才触发回调

- .once-只触发一次回调

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
</head>
<body>
<!--引入vue-->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<div id="app">
  <h2>{{message}}</h2>
  <div @click="divClick">
<!--    stop修饰符-->
    <button @click.stop="btn1Click">按钮1</button>
  </div>
  <br>
  <form action="www.baidu.com">
<!--    prevent修饰符-->
    <input type="submit" value="提交" @click.prevent="submitClick" >
  </form>
  <br>
<!--  监听keyup.enter-->
  <input type="text" @keyup="keyUp">
  <br>
  <br>
<!--  once修饰符-->
  <button @click.once="btn2Click">按钮2</button>
</div>
<script>
  const app = new Vue({
    el: "#app",
    data:{
      message:"Hello zk I am Vue !",
      counter:0
    },
    methods:{
      btn1Click() {
        console.log("btn1Click");
      },
      divClick(){
        console.log('divClock');
      },
      submitClick() {
        console.log('submitClick');
      },
      keyUp(){
        console.log('keyUp');
      },
      btn2Click() {
        console.log('btn2Click');
      }
    }
  })
</script>
</body>
</html>
```

![](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Snipaste_2021-04-21_13-47-34.png)



## 3.13 v-if v-else v-if-else指令

**v-if,v-else-if,v-else**

- 这三个指令与JavaScript的条件语句if,else,else if类似

- Vue的条件指令可以根据表达式的值在DM中渲染或销毁元素或组件

**v-if原理**

- v-if后面的条件为flase时，对应的元素以及其子元素不会渲染

- 也就是根本没有不会有对应的标签出现在DOM中

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
</head>
<body>
<div id="app">
  <!--  多重分支，不建议直接用标签，建议用计算属性去做-->
  <h2 v-if="score>=90">优秀</h2>
  <h2 v-else-if="score>=80">良好</h2>
  <h2 v-else-if="score>=60">及格</h2>
  <h2 v-else>不及格</h2>
  <hr>
  <hr>
  <h2>{{result}}</h2>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<script>
  const app = new Vue({
    el:"#app",
    data:{
      message:"Hello zk !",
      score:95,
    },
    computed:{
      result(){
        let showMessage = '';
        if(this.score>=90){
          showMessage = '优秀';
        }
        else if(this.score>=80){
          showMessage = '良好';
        }
        else if(this.score>=60){
          showMessage = '及格';
        }
        else{
          showMessage = '不及格';
        }
        return showMessage;
      }
    }
  })
</script>
</body>
</html>
```

![](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Snipaste_2021-04-21_13-52-47.png)



## 3.14 切换登录小案例

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
</head>
<body>
<!--引入vue-->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<div id="app">
  <span v-if="isUser">
    <label for="username">用户登录</label>
    <input type="text" id="username" placeholder="用户账号" key="username">
  </span>
  <span v-else>
    <label for="email">用户邮箱</label>
    <input type="text" id="email" placeholder="用户邮箱" key="email">
  </span>
  <button @click="isUserClick()">切换登录方式</button>
</div>
<script>
  const app = new Vue({
    el: "#app",
    data:{
      message:"Hello zk I am Vue !",
      isUser:true
    },
    methods:{
      isUserClick(){
        this.isUser=!this.isUser;
      }
    }
  })
</script>
</body>
</html>
```

![](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Snipaste_2021-04-21_20-08-29.png)

如果我们在有输入内容的情况下，切换了类型，我们会发现文字一仍然显示之前的输入的内容
但是按道理讲，我们应该切换到另一个input的元素中
在另一个input元素中，我们并没有输入内容
为什么会出现这个问题呢？

- 这是因为Vue在进行DOM渲染时，出于性能考虑，会尽可能的复用已经存在的元素，而不是重新创建新的元素
  在案例中，Vue内部会发现原来的input元素不再使用，直接作为else中input来使用

- 如果我们不考虑Vue出现类似重复利用的问题，可以给对应的input添加key
  并且保证key不相同

```html
<div id="app">  
    <span v-if="isUser">  
        <label for="username">用户账号</label>  
        <input type="text" id="username" placeholder="用户账号" key="username">
    </span>  
    <span v-else>    
        <label for="email">用户邮箱</label>  
        <input type="text" id="email" placeholder="用户邮箱" key="email">  </span> 
    <button @click="isUserClick()">切换登录方式</button>
</div>
```



## 3.15 v-show指令

**（一）v-show的用法和v-if非常相似，也用于决定一个元素是否渲染**
**（二）v-if和v-show都可以决定一个元素是否渲染，那么开发中我们如何选择呢？**

- v-if当条件为false时，压根不会有对应的元素在DOM中
- v-show当条件为false，仅仅是将元素的display属性设置为none而已

**（三）开发种如何选择呢？**

- 当需要在显示与隐藏之间切片很频繁时，使用v-show
- 当只有一次切换时，通过使用v-if

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
</head>
<body>

<div id="app">
  <!--  v-if :当条件为false时，包含v-if指令的元素。根本就不会存在dom中-->
  <h2 v-if="isShow">{{message}}</h2>
  <!--  v-show: 当条件为false时，v-show只是给我们的元素添加一个行内样式：display:none-->
  <h2 v-show="isShow">{{message}}</h2>
</div>

<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<script>
  const app = new Vue({
    el:"#app",
    data:{
      message:"Hello zk !",
      isShow:true,
    }
  })
</script>
</body>
</html>
```

## 3.16 v-for数组遍历

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
</head>
<body>
<!--引入vue-->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<div id="app">

  <ul>
    <li v-for="item in movies">{{item}}</li>
  </ul>
</div>
<script>
  const app = new Vue({
    el: "#app",
    data:{
      message:"Hello zk I am Vue !",
      movies:["速度与激情","海贼王","盗墓笔记","成龙历险记"]
    }
  })
</script>
</body>
</html>
```

![](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Snipaste_2021-04-20_21-34-24.png)

## 3.17 v-for对象遍历

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
</head>
<body>
<div id="app">
  <!--  直接输出-->
  <ul>
    <li>{{info.name}}</li>
    <li>{{info.age}}</li>
    <li>{{info.height}}</li>
    <li>{{info.weight}}</li>
  </ul>
  <hr>
  <hr>
  <!--  1.在遍历对象的过程中，如果只是获取一个值，那么获取到的是value-->
  <ul>
    <li v-for="item in info">{{item}}</li>
  </ul>
  <hr>
  <hr>
  <!--  2.获取key和value         前面为value，后面为key   格式(value,key)-->
  <ul>
    <li v-for="(value,key) in info">{{key}}:{{value}}</li>
  </ul>
  <hr>
  <hr>
  <!--  3.获取key和value，index  格式：(value,key,index)-->
  <ul>
    <li v-for="(value,key,index) in info">[{{index}}]{{key}}:{{value}}</li>
  </ul>
</div>
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<script>
  const app = new Vue({
    el:"#app",
    data:{
      message:"Hello zk!",
      info :{
        name:'zk',
        age:20,
        height:1.88,
        weight:68.5,
      },
    },

  })
</script>
</body>
</html>
```

![](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Snipaste_2021-04-21_20-54-14.png)

## 3.18 v-model指令

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
</head>
<body>
<!--引入vue-->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<div id="app">
<!--  双向绑定-->
  <input type="text" v-model="message">{{message}}
  <hr>
<!--  input 有个监听事件，用于监听用户输入，反向绑定message的值，实现双向绑定-->
  <input type="text" :value="message" @input="message=$event.target.value">{{message}}
  <hr>
</div>
<script>
  const app = new Vue({
    el: "#app",
    data:{
      message:"Hello zk I am Vue !"
    },
    methods:{
      valueChange(event) {
        //一旦在界面中产生一个事件，浏览器会自动生成一个event对象
        //enent对象中，有我们需要value,在event中的target中
        //在调用函数中，如果省略的实参，而定义函数时，有形参，会自动将event当做实参传入
        this.message = event.target.value
      }
    }
  })
</script>
</body>
</html>
```

![Snipaste_2021-05-18_21-36-05.png](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Snipaste_2021-05-18_21-36-05.png)

结合radio使用

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
</head>
<body>
<!--引入vue-->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<div id="app">
  <label for="male">
    <!--  如果不加name，会导致可以同时勾选男女，加上name，那么提交给数据库的时候，key是唯一的sex，所以只能选一个-->
    <!--  <input type="radio" id="male" name="sex">男-->
    <!--  当然了，如果我们加上v-model，并且绑定同一个元素的话，name就可以不添加了，因为也是互斥的-->
    <input type="radio" id="male" value="男" v-model="sex">男
  </label>
  <label for="female">
    <input type="radio" id="female" value="女" v-model="sex">女
  </label>
  <h2>您选择的性别是：{{sex}}</h2>
</div>
<script>
  const app = new Vue({
    el: "#app",
    data:{
      message:"Hello zk I am Vue !",
      sex: '男'
    }
  })
</script>
</body>
</html>
```

![Snipaste_2021-05-18_21-36-20.png](https://gitee.com/zhuang-kang/note-picture-2/raw/master/Snipaste_2021-05-18_21-36-20.png)

# 4，购物车小案例

**index.html**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
<div id="app">
  <div v-if="books.length">
  <table>
    <thead>
    <tr>
      <th></th>
      <th>书籍名称</th>
      <th>出版日期</th>
      <th>价格</th>
      <th>购买数量</th>
      <th>操作</th>
    </tr>
    </thead>
    <tbody>
    <tr v-for="(book,index) in books">
      <!--      <td v-for="value in book">{{value}}</td>-->
      <td>{{book.id}}</td>
      <td>{{book.name}}</td>
      <td>{{book.date}}</td>
      <td>{{book.price | showPrice}}</td>
      <td>
        <button @click="decrement(index)":disabled="book.count<=1">-</button>
        {{book.count}}
        <button @click="increment(index)">+</button>
      </td>
      <td>
        <button @click="removeHandle(index)">移除</button>
      </td>
    </tr>
    </tbody>
  </table>
  <h2>总价格：{{totalPrice | showPrice}}</h2>
</div>
<h1 v-else>购物车为空</h1>
</div>
<!--引入vue-->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<script src="main.js"></script>
</body>
</html>
```

**main.js**

```javascript
const app = new Vue({
  el:'#app',
  data:{
    books:[
      {
        id:1,
        name:'代码重构',
        date:'2006-9',
        price:85.00,
        count:1,
      },
      {
        id:2,
        name:'Linux大全',
        date:'2006-2',
        price:59.00,
        count:1,
      },
      {
        id:3,
        name:'编程之美',
        date:'2008-10',
        price:39.00,
        count:1,
      },
      {
        id:4,
        name:'代码大全',
        date:'2006-3',
        price:128.00,
        count:1,
      },
    ]
  },
  methods:{
    getFinalPrice(price){
      return '￥' + price.toFixed(2)
    },
    decrement(index){
      this.books[index].count--
    },
    increment(index){
      this.books[index].count++
    },
    removeHandle(index){
      this.books.splice(index,1)
    }
  },
  filters:{
    showPrice(price){
      return '￥' + price.toFixed(2)
    }
  },
  computed:{
    totalPrice(){
      let totalPrice = 0;
      for (let i = 0; i< this.books.length; i++){
        totalPrice += this.books[i].price*this.books[i].count;
      }
      return totalPrice;
    }
  }
})
```

**style.css**

```css
table{
  border: 1px solid #e9e9e9;
  border-collapse: collapse;
  border-spacing: 0;
}

th,td{
  padding: 8px 16px;
  border: 1px solid #e9e9e9;
  text-align: left;
}

th{
  background-color: #f7f7f7;
  color: #5c6b77;
  font-weight: 600;
}
```

**效果展示**

![](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Snipaste_2021-04-22_21-09-46.png)

# 5，JavaScript高阶函数讲解



```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
</head>
<body>
<div id="app">
  原来的数组-->{{nums}}<br>
  小于100后过滤的数组-->{{useFilter()}}<br>
  乘以2的数组-->{{useMap()}}<br>
  乘以2以后的各元素的和的数组-->{{useReduce()}}<br>
  三种函数连用-->{{useOnceCount()}}<br>
  一行代码-->{{useOnceRowCount()}}<br>
</div>
<!--引入vue-->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<script>
  const app = new Vue({
    el: "#app",
    data: {
      message: "Hello zk I am Vue !",
      nums:[10, 20, 30, 40, 50, 60, 70]
    },
    //filter map reduce 讲解
    /*
    filter
    返回一个布尔值 必须
    返回true 函数内部添加回调的值加入数组中
    返回false 不加入
     */
    methods:{
      useFilter() {
        let newNums = this.nums.filter(function (n) {
          //当n小于100是加入数组中
          return n < 100
        })
        return newNums
      },
      useMap() {
        //将newNums中每个数字×2
        let new2Nums = this.useFilter().map(function (n) {
          return n * 2
        })
        return new2Nums
      },
      //作用：对数组中所有的内容进行汇总(相加，相乘等等)
      useReduce() {
        let total = this.useMap().reduce(function (preValue, n) {
          return preValue + n
        }, 0)
        return total
      },
      useOnceCount() {
        let total = this.nums.filter(function (n) {
          return n < 100
        }).map(function (n) {
          return n * 2
        }).reduce(function (preValue, n) {
          return preValue + n
        }, 0)
        return total
      },
      useOnceRowCount() {
        let total = this.nums.filter(n => n < 100).map(n => n * 2).reduce((preValue, n) => preValue + n)
        return total
      }
    }
  })
</script>
</body>
</html>
```

![](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Snipaste_2021-04-22_22-00-11.png)

# 6，组件化开发

## 6.1 组件化的基本使用

**什么是组件化？**

- 人面对复杂问题的处理方式：

任何一个人处理信息的逻辑能力都是有限的

所以，当面对一个非常复杂的问题时，我们不太可能一次性搞定一大堆的内容。

但是，我们人有一种天生的能力，就是将问题进行拆解。

如果将一个复杂的问题，拆分成很多个可以处理的小问题，再将其放在整体当中，你会发现大的问题也会迎刃而解



- 组件化也是类似的思想：

如果我们将一个页面中所有的处理逻辑全部放在一起，处理起来就会变得非常复杂，而且不利于后续的管理以及扩展。

但如果，我们讲一个页面拆分成一个个小的功能块，每个功能块完成属于自己这部分独立的功能，那么之后整个页面的管理和维护就变得非常容易了

![](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Snipaste_2021-04-25_18-10-15.png)

> 我们将一个完整的页面分成很多个组件。
>
> 每个组件都用于实现页面的一个功能块。
>
> 而每一个组件又可以进行细分。



**Vue组件化思想**

组件化是Vue.js中的重要思想

**它提供了一种抽象，让我们可以开发出一个个独立可复用的小组件来构造我们的应用。**

**任何的应用都会被抽象成一颗组件树**

![](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Snipaste_2021-04-25_18-13-10.png)

- 组件化思想的应用：
  - 有了组件化的思想，我们在之后的开发中就要充分的利用它。
  - 尽可能的将页面拆分成一个个小的、可复用的组件。
  - 这样让我们的代码更加方便组织和管理，并且扩展性也更强





**组件的使用分成三个步骤：**

- 创建组件构造器

- 注册组件

- 使用组件

![](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Snipaste_2021-04-25_18-21-26.png)

**Vue.extend()：**

- 调用Vue.extend()创建的是一个组件构造器。 

- 通常在创建组件构造器时，传入template代表我们自定义组件的模板。

- 该模板就是在使用到组件的地方，要显示的HTML代码



**Vue.component()：**

- 调用Vue.component()是将刚才的组件构造器注册为一个组件，并且给它起一个组件的标签名称。

- 所以需要传递两个参数：1、注册组件的标签名 2、组件构造器

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
</head>
<body>
<!--引入vue-->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<div id="app">
  <div>
    <h2>我是标题</h2>
    <p>我是内容</p>
  </div>
  <my_cpn></my_cpn>
</div>
<script>
  const cpnC = Vue.extend({
    template: `<div>
       <h2>我是标题</h2>
       <p>我是内容</p>
        </div>>`
  })

  //注册组件
  //component 使用组件的名字，组件构造器名
  Vue.component('my_cpn', cpnC)
  const app = new Vue({
    el: "#app",
    data: {
      message: "Hello zk I am Vue !"
    }

  })
</script>
</body>
</html>
```

![](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Snipaste_2021-04-25_18-31-46.png)

## 6.2 全局变量和局部变量

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
</head>
<body>
<!--引入vue-->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<div id="app">
  <div>
    <h2>我是标题</h2>
    <p>我是内容</p>
  </div>
  <my_cpn></my_cpn>
</div>
<hr>
<div id="app2">
  <cpn></cpn>
</div>
<script>
  const cpnC = Vue.extend({
    template: `<div>
       <h2>我是标题</h2>
       <p>我是内容</p>
        </div>>`
  })

  //注册组件
  //component 使用组件的名字，组件构造器名
  Vue.component('my_cpn', cpnC)
  const app = new Vue({
    el: "#app",
    data: {
      message: "Hello zk I am Vue !"
    },
  })
  const app2 = new Vue({
    el: '#app2',
    //局部注册，只有APP实例用此组件
    components: {
      //定义标签 cpn
      cpn: cpnC
    }
  })
</script>
</body>
</html>
```

![](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Snipaste_2021-04-25_18-33-35.png)

## 6.3 父组件和子组件

组件和组件之间存在层级关系
而其中一种非常重要的关系就是父子组件的关系

**父子组件错误用法：以子标签的形式在Vue实例中使用**

- 因为当子组件注册到父组件的components时，Vue会编译好父组件的模块

- 该模块的内容已经决定了父组件将要渲染的HTML（相当于父组件中已经有了子组件中内容了）

- 是只能在父组件中被识别的

- 类似这种用法，是会被浏览器忽略的

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
</head>
<body>
<!--引入vue-->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<div id="app">
  <cpn2></cpn2>
</div>
<script>
  const cpnC1 = Vue.extend({
    template: `
      <div>
        <h2>我是cpnC1</h2>
        <p>我是内容</p>
      </div>
    `
  })
  // 2.创建第二个组件
  const cpnC2 = Vue.extend({
    template: `
      <div>
      <h2>我是cpnC2</h2>
      <p>我是内容</p>
      <cpn1></cpn1>
      </div>
    `,
    components: {
      cpn1: cpnC1
    }
  })
  //可以看成根组件
  const app = new Vue({
    el: "#app",
    data: {},
    components: {
      cpn2: cpnC2
    }
  })
</script>
</body>
</html>
```

![](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Snipaste_2021-04-24_16-04-47.png)



## 6.4 注册组件的语法糖写法

在上面注册组件的方式，可能会有些繁琐。

- Vue为了简化这个过程，提供了注册的语法糖。

- 主要是省去了调用Vue.extend()的步骤，而是可以直接使用一个对象来代替。

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
</head>
<body>
<!--引入vue-->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<div id="app">
  <h1>全局变量</h1>
  <my_cpn></my_cpn>
  <h1>局部变量</h1>
  <my_cpn1></my_cpn1>
</div>
<script>
  /*
  全局组件注册的语法糖
  创建组件构造器
  注册组件
  内部调用extend
   */
  Vue.component('my_cpn', {
    template: `<div>
       <h2>我是标题</h2>
       <p>我是内容</p>
       <p>语法糖写法</p>
        </div>`
  })
  const app = new Vue({
    el: "#app",
    data: {
      message: "Hello zk I am Vue !"
    },
    components: {
      my_cpn1: {
        template: `<div>
      <h2>我是标题</h2>
      <p>我是内容</p>
      <p>my_cpn1</p>
</div>`
      }
    }
  })
</script>
</body>
</html>
```

![](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Snipaste_2021-04-24_16-33-27.png)



## 6.5 组件模板分离写法

通过语法糖简化了Vue组件的注册过程，另外还有一个地方的写法比较麻烦，就是template模块写法。

如果我们能将其中的HTML分离出来写，然后挂载到对应的组件上，必然结构会变得非常清晰。

Vue提供了两种方案来定义HTML模块内容：

**p使用<script>标签**

**p使用<template>标签**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
</head>
<body>
<!--引入vue-->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<div id="app">
  <!--  <cpn></cpn>-->
  <cpn1></cpn1>
</div>
<srcipt type="text/x-template" id="cpn">
  <div>
    <h2>我是标题</h2>
    <p>我是cpn</p>
    <p>script标签抽离 注意类型type="text/x-template"</p>
  </div>
</srcipt>
<template id="cpn1">
  <div>
    <h2>我是标题</h2>
    <p>我是内容</p>
    <p>我是cpn1</p>
  </div>
</template>
<script>
  //注册一个全局组件
  Vue.component('cpn', {
    template: '#cpn'
  })

  Vue.component('cpn1', {
    template: '#cpn1'
  })

  const app = new Vue({
    el: "#app",
    data: {
      message: "Hello zk I am Vue !"
    }
  })
</script>
</body>
</html>
```

![](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Snipaste_2021-04-24_16-43-55.png)



## 6.6 组件的数据

**一丶组件可以访问Vue实例数据吗？**

- 组件是一个单独功能模块的封装：
- 这个模块有属于自己的HTML模板，也应该有属性自己的数据data
- 组件中的数据是保存在哪里呢？顶层的Vue实例中吗？
- 组件中不能直接访问Vue实例中的data
- Vue组件应该有自己保存数据的地方

**二丶组件数据的存放**

- 组件自己的数据存放在哪里呢？
- 组件对象也有一个data属性（也可以有methods等属性）
- 只是这个data属性必须是一个函数
- 而且这个函数的返回一个对象，对象内部保存着数据

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
</head>
<body>

<div id="app">
  <my_count></my_count>
  <my_count></my_count>
  <my_count></my_count>
</div>
<template id="cpn">
  <div>
    <h2>当前计数：{{counter}}</h2>
    <button @click="increment">+</button>
    <button @click="decrement">-</button>
  </div>
</template>
<!--引入vue-->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<script>
  Vue.component('my_count', {
    template: '#cpn',
    data() {
      return {
        counter: 0
      }
    },
    methods: {
      increment() {
        this.counter++;
      },
      decrement() {
        this.counter--;
      }
    }
  })
  const app = new Vue({
    el: "#app",
    data() {

    }
  })
</script>
</body>
</html>
```

![](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Snipaste_2021-04-25_13-15-53.png)



**为什么data在组件中必须是一个函数呢?**

- 首先，如果不是一个函数，Vue直接就会报错。

- 其次，原因是在于Vue让每个组件对象都返回一个新的对象，因为如果是同一个对象的，组件在多次使用后会相互影响。



## 6.7 父子通信—父传子

**一丶父子组件的通信**

- 子组件是不能是不能引用父组件或者Vue实例的数据的
- 在开发中，往往一些数据确实需要从上层传递到下层
- 比如在一个页面中，我们从服务器请求到了很多数据其中一些数据，并非是我们整个页面的大组件来展示的，而是需要下面的子组件进行展示这个时候，并不会让子组件再次发送一个一次网络请求，而是直接让大组件（父组件）将数据传递给小组件（子组件）
  **如何进行父子组件间的通信呢？Vue官方提到**
  - 通过props向子组件传递数据
  - 通过自定义事件向父组件发送消息

![](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Snipaste_2021-04-25_18-47-25.png)

**二丶props数据验证**
props可以是一个数组，也可以是对象
当需要对props进行类型验证时，就需要对象写法了
对象写法支持类型：

> String
> Number
> Boolean
> Array
> Object
> Date
> Function
> Symbol
>
> 当我们有自定义构造函数时，验证也支持自定义类型

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
</head>
<body>
<div id="app">
  <my_cpn :cmovies="movies" :cmessage="message"></my_cpn>
</div>
<template id="my_cpn">
  <div>
    <ul>
      <li v-for="movie in cmovies">{{movie}}</li>
    </ul>
    <p>{{cmovies}}</p>
    <p>{{cmessage}}</p>
  </div>
</template>
<!--引入vue-->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<script>
  const my_cpn = {
    template: '#my_cpn',
    /*
    传递数组
    传递对象 控制传递类的限制
    传递对象，提供默认值
     */
    props: {
      cmessage: {
        type: String,
        default: 'full',
        require: false
      },
      cmovies: {
        type: Array,
        default() {
          return []
        }
      }
    },
    data() {
      return {}
    }
  }
  const app = new Vue({
    el: "#app",
    data: {
      message: "Hello zk I am Vue !",
      movies: ['海贼王', '海王', '海尔兄弟']
    },
    components: {
      my_cpn
    }
  })
</script>
</body>
</html>
```

![](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Snipaste_2021-04-25_13-40-48.png)

- 如果props使用的驼峰标识，在动态绑定的时候，不能直接绑定，在每个大写字母前面需要加一个-

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
</head>
<body>
<!--引入vue-->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<div id="app">
  <cpn :c-Info="info"></cpn>
</div>
<template id="cpn">
  <h2>{{cInfo}}</h2>
</template>
<script>
  const cpn ={
    template:'#cpn',
    props:{
      //驼峰标识
      cInfo:{
        type:Object,
        default(){
          return{

          }
        }
      }
    }
  }
  const app = new Vue({
    el: "#app",
    data:{
      message:"Hello zk I am Vue !",
      info:{
        name:'zk',
        age:20,
        height:1.90
      }
    },
    components:{
      cpn
    }
  })
</script>
</body>
</html>
```

![](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Snipaste_2021-04-25_13-48-24.png)

## 6.8 父传子-自定义事件

**一丶子级向父级传递**
props用于父组件向子组件传递数据，还有一种比较常见的事子组件传递数据或事件到父组件中
我们需要用自定义事件来完成
什么时候需要自定义事件呢？
当子组件需要向父组件传递数据时，就要用到自定义事件
v-on不仅仅可以用于监听DOM事件，也可以用于组件间的自定义事件
自定义事件的流程：
**在子组件中，通过$emit()来监听事件**
**在父组件中，通过v-on来监听子组件事件**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
</head>
<body>
<!--引入vue-->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<div id="app">
  <cpn @itclick="cpnclick"></cpn>
</div>
<!--子组件模板-->
<template id="cpn">
  <div>
    <button v-for="item in categories" @click="itemClick(item)">
      {{item.name}}
    </button>
  </div>
</template>
<script>

  const cpn={
    template:'#cpn',
    data(){
      return{
        categories:[
          {id:'100',name:'热门推荐'},
          {id:'101',name:'手机数码'},
          {id:'102',name:'家用电器'},
          {id:'103',name:'电脑办公'},
        ]
      }
    },
    methods:{
      itemClick(item){
        console.log(item)
        /*
        将item传给父组件，自定义
        this.$emit('itemClick',item)
         */
        this.$emit('itemClick',item)
      }
    }
  }
  const app = new Vue({
    el: "#app",
    data:{
      message:"Hello zk I am Vue !"
    },
    components:{
      cpn
    },
    methods:{
      cpnclick(item){
        console.log('cpnclick',item);
      }
    }
  })
</script>
</body>
</html>
```

![](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Snipaste_2021-04-25_13-58-09.png)

## 6.9 父传子-双向绑定

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
</head>
<body>

<div id="app">
  <cpn :number1="num1"
       :number2="num2"
       @num1change="num1change"
       @num2change="num2change">
  </cpn>
</div>

<template id="cpn">
  <div>
    <!--    使用data中的计算属性进程改动数据，双向绑定-->
    <h2>props:{{number1}}</h2>
    <h2>data:{{dnumber1}}</h2>
    <!--    单向绑定，只绑定子组件的数据-->
    <input type="text" v-model="dnumber1"> 单向绑定，只绑定子组件的数据
    <hr>
    <!--    <input type="text" v-bind:value="dnumber1" v-on:input="dnumber1=$event.target.value"> 双向绑定-->
    <!--    @input后面太长了 可以写成函数-->
    <input type="text" :value="dnumber1" @input="num1Input">
    <!--    <input type="text" :value="dnumber1" @input="num1Input">-->
    <h2>props:{{number2}}</h2>
    <h2>data:{{dnumber2}}</h2>
    <input type="text" v-model="dnumber2"> 单向绑定，只绑定子组件的数据
    <hr>
    <input type="text" :value="dnumber2" @input="num2Input">双向绑定
    <!--    单向绑定，只绑定子组件的数据-->
    <!--    <input type="text" v-model="dnumber2">-->
    <!--    直接绑定，Vue不支持-->
    <!--    <h2>{{number1}}</h2>-->
    <!--    <input type="text" v-model="number1">-->
    <!--    直接绑定，Vue不支持-->
    <!--    <h2>{{number2}}</h2>-->
    <!--    <input type="text" v-model="number2">-->
  </div>
</template>

<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
  const app = new Vue({
    el: "#app",
    data: {
      num1: 1,
      num2: 0
    },
    methods: {
      num1change(value) {
        this.num1 = parseFloat(value)
      },
      num2change(value) {
        this.num2 = parseFloat(value)
      }
    },
    components: {
      cpn: {
        template: '#cpn',
        props: {
          //如果只是想直接展示的话，就可以直接用
          //如果要改数据的话，最好是放在data中用计算属性更改
          number1: Number,
          number2: Number
        },
        data() {
          return {
            dnumber1: this.number1,
            dnumber2: this.number2
          }
        },
        methods: {
          num1Input() {
            this.dnumber1 = event.target.value;
            this.$emit('num1change', this.dnumber1)
          },
          num2Input() {
            this.dnumber2 = event.target.value;
            this.$emit('num2change', this.dnumber2)
          }
        }
      }
    }
  })
</script>
</body>
</html>
```

![](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Snipaste_2021-04-25_15-20-09.png)

## 6.10 父子组件的访问方式：$children $refs

- 有时候我们需要父组件直接访问子组件，子组件直接访问父组件，或者是子组件访问根组件
- 父组件访问子组件：使用$ children或者$ refs

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
</head>
<body>
<!--引入vue-->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<div id="app">
  <cpn></cpn>
  <cpn></cpn>
  <cpn ref="reference属性"></cpn>
  <button @click="btnClick">按钮</button>
</div>
<template id="cpn">
  <div>我是子组件</div>
</template>
<script>
  const app = new Vue({
    el: "#app",
    data:{
      message:"Hello zk I am Vue !"
    },
    methods:{
      btnClick(){

        console.log(this.$refs)
      }
    },
    components:{
      cpn:{
        template:'#cpn',
        data(){
          return{
            name:'我是子组件的name'
          }
        },
        methods:{
          showMessage(){
            console.log("showMessage");
          }
        }
      }
    }
  })
</script>
</body>
</html>
```

![](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Snipaste_2021-04-25_15-28-23.png)

## 6.11 子访问父-parent-root

- 有时候我们需要父组件直接访问子组件，子组件直接访问父组件，或者是子组件访问根组件
- 子组件访问父组件：使用$ parent
- 子组件访问根组件：使用$ root

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
</head>
<body>
<!--引入vue-->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<div id="app">
  <cpn></cpn>
</div>
<template id="cpn">
  <div>
    <h2>我是cpn</h2>
    <ccpn></ccpn>
  </div>
</template>
<template id="ccpn">
  <div>
    <h2>我是ccpn</h2>
    <button @click="btnClick">按钮</button>
  </div>
</template>
<script>
  const app = new Vue({
    el: "#app",
    data:{
      message:"Hello zk I am Vue !"
    },
    components:{
      cpn:{
        template:'#cpn',
        data(){
          return{
            name:'我是cpn组件的name'
          }
        },
        components:{
          ccpn:{
            template: '#ccpn',
            methods:{
              btnClick() {

                console.log(this.$parent);
                console.log(this.$parent.name);

                console.log(this.$root);
                console.log(this.$root.message);
              }
            },
          },
        }
      }
    }
  })
</script>
</body>
</html>
```

![](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Snipaste_2021-04-25_15-43-50.png)

## 6.12 slot-插槽的基本使用

**一丶为什么使用slot**

**（一）slot翻译为插槽**

在生活中很多地方都有插槽，电脑的USB插槽，插板当中的电源插槽
插槽的目的是让我们原来的设备具有更多的扩展性
比如电脑的USB我们可以插入U盘，硬盘，手机，音响，键盘，鼠标等
**（二）组件的插槽**

组件的插槽也是为了让我们封装的组件更具有扩展性
让使用者可以决定组件内部的一些内容到底展示什么
**（三）例子：移动网站中的导航栏**

移动开发中，几乎每个页面都有导航栏
导航栏我们必然会封装成一个插件，比如nav-bar组件
一旦有了这个组件，我们就可以在多个页面中复用
**二丶如何封装这类插件呢？slot**

**（一）如何去封装这类的组件呢？**

它们也很多区别，但是也有很多共性
如果，我们每一个单独去封装一个组件，显然不合适
比如每个页面都返回，这部分内容我们就要重复去封装
但是，如果我们封装成一个，好像也不合理
有些左侧是菜单，有些是返回，有些中间是搜索，有些事文字等等
**（二）如何封装合适呢？抽取共性，保留不同**

最好的封装方式就是将共性抽取到组件中，将不同暴露为插槽
一旦我们预留了插槽，就可以让使用者根据自己的需求，决定插槽中插入什么内容
是搜索框，还是文字，还是菜单，由调用者自己来决定

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
</head>
<body>
<!--引入vue-->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<div id="app">
  <cpn>
    <button>按钮</button>
  </cpn>
  <cpn>
    <i>哈哈哈</i>
    <div>我是div标签</div>
    <p>我是P标签</p>
  </cpn>
  <cpn></cpn>
</div>
<template id="cpn">
<div>
  <h2>我是h2标签</h2>
  <p>我是P标签</p>
  <slot></slot>
  <hr>
</div>
</template>
<script>
  const app = new Vue({
    el: "#app",
    data:{
      message:"Hello zk I am Vue !"
    },
    components:{
      cpn:{
        template:'#cpn'
      }
    }
  })
</script>
</body>
</html>
```

![](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Snipaste_2021-04-25_19-23-52.png)

## 6.13 slot-具名插槽的使用

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
</head>
<body>
<div id="app">
  <cpn>
  </cpn>
  <hr>
  <cpn>
    <!--    取名name 调用slot-->
    <span slot="center">标题</span>
    <span slot="left">返回</span>
    <span slot="right">更多</span>
  </cpn>
</div>

<template id="cpn">
  <div>
    <!--    slot中间均是默认值-->
    <slot name="left"><span>左边</span></slot>
    <slot name="center"><span>中间</span></slot>
    <slot name="right"><span>右边</span></slot>
  </div>
</template>
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<script>
  const app = new Vue({
    el: "#app",
    data: {},
    components: {
      cpn: {
        template: '#cpn'
      }
    }
  })
</script>

</body>
</html>
```

![](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Snipaste_2021-04-25_19-28-07.png)

## 6.14 编译作用域

**在真正学习插槽之间，我们需要首先理解一个概念：编译作用域**

- 官方对于编译的作用域解析比较简单，我们自己来通过一个例子来理解这个概念

- 我们来考虑下面的代码是否最终是可以渲染出来的

- <my-cpn v-show="isShow"></my-cpn>中，我们使用了isShow属性 isShow属性包含在组件中，也包含在Vue实例中

- 我们可以从下面的案例代码运行结果得出：都只作用于当前作用域

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
</head>
<body>
<div id="app">
  <cpn v-show="isShow"></cpn>
</div>

<template id="cpn">
  <div>
    <h2>我是子组件</h2>
    <p>我是内容，zk</p>
    <button v-show="isShow">按钮</button>
  </div>
</template>
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<script>
  const vue = new Vue({
    el:"#app",
    data:{
      message:'hello,zk ',
      isShow:true
    },
    components:{
      cpn:{
        template:'#cpn',
        data(){
          return{
            isShow:false
          }
        }
      }
    }
  })
</script>
</body>
</html>
```



## 6.15 作用域插槽的使用

子组件中包含一组数据，比如：pLanguages：[‘JavaScript’,‘Python’,‘Swift’,‘Go’,‘C++’]

> （一）在父组件使用我们子组件时，从子组件中拿到数据需要在多个页面进行展示
> 某些页面是以水平方向一一展示的
> 某些界面是以列表形式展示的
> 某些界面直接展开一个数组
> 内容在子组件，希望父组件告诉我们如何展示，怎么办呢？
> 利用slot作用域插槽就可以了

我们通过<template slot-scope="slot">获取到slot的属性
在通过slot.data就可以获取到我们传入的data了

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Title</title>
</head>
<body>
<div id="app">
  <!--  列表形式展示-->
  <cpn></cpn>
  <!--  水平形式展示-->
  <cpn>
    <!--    目的是获得子组件中的pLanguages-->
    <!--    Vue2.5.x以下必须使用template模板-->
    <template slot-scope="slot">
      <span v-for="item in slot.data">{{item}}——</span>
      <br>
      <span>{{slot.data.join('——')}}</span>
    </template>
  </cpn>
</div>

<template id="cpn">
  <div>
    <slot :data="pLanguages">
      <ul>
        <li v-for="item in pLanguages">{{item}}</li>
      </ul>
    </slot>
    <hr>
  </div>
</template>
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<script>
  const app = new Vue({
    el: "#app",
    data: {},
    components: {
      cpn: {
        template: "#cpn",
        data() {
          return {
            pLanguages: ['JavaScript', 'C++', 'Java', 'Python', 'Go', 'Solidity']
          }
        }
      },
    }
  })
</script>
</body>
</html>
```

![](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Snipaste_2021-04-25_19-31-01.png)

# 7，前端模块化

## 7.1 JavaScript原始功能

**随着ajax异步请求的出现，慢慢形成了前后端的分离**

- 客户端需要完成的事情越来越多，代码量也是与日俱增。

- 为了应对代码量的剧增，我们通常会将代码组织在多个js文件中，进行维护。

- 但是这种维护方式，依然不能避免一些灾难性的问题。

> 全局变量同名问题

**匿名函数来解决方面的重名问题**

![](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Snipaste_2021-04-28_17-09-20.png)

**在aaa.js文件中，我们使用匿名函数**

![image-20210428171002122](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210428171002122.png)

**使用模块作为出口**

> 在匿名函数内部，定义一个对象。
>
> 给对象添加各种需要暴露到外面的属性和方法(不需要暴露的直接定义即可)。
>
> 最后将这个对象返回，并且在外面使用了一个MoudleA接受

![image-20210428171118528](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210428171118528.png)

**常见的模块化规范**：

- CommonJS、AMD、CMD，也有ES6的Modules

![](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Snipaste_2021-04-28_17-12-19.png)

## 7.2 **export**基本使用

**export**指令用于导出变量

两种写法

![image-20210428171329647](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210428171329647.png)

## 7.3 export default

**某些情况下，一个模块中包含某个的功能，我们并不希望给这个功能命名，而且让导入者可以自己来命名**

**这个时候就可以使用export default**

![image-20210428171451032](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210428171451032.png)

<font color=red>**需要注意**：export default在同一个模块中，不允许同时存在多个</font>



# 8，Wbepack使用

## 8.1 什么是Webpack?

At its core, **webpack** is a *static module bundler* for modern JavaScript applications. 

从本质上来讲，webpack是一个现代的JavaScript应用的静态**模块打包**工具。

![image-20210428171748396](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210428171748396.png)

**打包如何理解**

> 理解了webpack可以帮助我们进行模块化，并且处理模块间的各种复杂关系后，打包的概念就非常好理解了。
>
> 就是将webpack中的各种资源模块进行打包合并成一个或多个包(Bundle)。
>
> 并且在打包的过程中，还可以对资源进行处理，比如压缩图片，将scss转成css，将ES6语法转成ES5语法，将TypeScript转成JavaScript等等操作。

### 8.1.1 安装Webpack

```shell
#先要安装node.js 安装步骤不赘述，node.js自带npm包管理工具#查看node版本$ node -v#全局安装webpack$ npm install webpack -g# cd 对应目录 局部安装webpack$ npm install webpack --save--dev
```

> 为什么全局安装后，还需要局部安装呢？
>
> - 在终端直接执行webpack命令，使用的全局安装的webpack
>
> - 当在package.json中定义了scripts时，其中包含了webpack命令，那么使用的是局部webpack

## 8.1.2 打包文件 

> webpack只是一个工具 后续都是有vue-cli脚手架，这里不再赘述打包的步骤！



## 8.2 Webpack打包css样式

**参考官网：[loader | webpack 中文网 (webpackjs.com)](https://www.webpackjs.com/concepts/loaders/)**

在src下新建一个目录css 写一个css文件

```css
body{  background-color: aquamarine;}
```

在main.js文件中导入

![](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Snipaste_2021-04-26_13-59-05.png)

几个命令搞定

```shell
$ npm install --save-dev css-loader$ npm install --save-dev style-loader$ webpack
```

**参考生成文件如下！**

**webpack.config.js**

```javascript
const path = require('path')
module.exports = {
  entry: path.join(__dirname, './src/main.js'),// 入口，表示，要使用 webpack 打包哪个文件
  output: { // 输出文件相关的配置
    path: path.join(__dirname, './dist'), // 指定 打包好的文件，输出到哪个目录中去
    filename: 'bundle.js' // 这是指定 输出的文件的名称
  }
}
```

**package.json**

```json
{
  "name": "webpack_test",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack"
  },
  "author": "",
  "license": "ISC"
}
```

在页面导入生成的bundle.js文件即可

```javascript
<script src="./03-webpack的loader/dist/bundle.js"></script>
```



## 8.3 Webpack打包图片样式

**参考官网：[url-loader | webpack 中文网 (webpackjs.com)](https://www.webpackjs.com/loaders/url-loader/)**

在src目录下新建img目录 随便放进图片

在css文件中当背景使用

```css
body{  /*background-color: aquamarine;*/  background: url("../img/minibus.jpg");}
```

安装依赖

```shell
$ npm install --save-dev url-loader
```

**配置webpack.config.js**

```javascript
const path = require('path')
module.exports = {
  entry: path.join(__dirname, './src/main.js'),// 入口，表示，要使用 webpack 打包哪个文件
  output: { // 输出文件相关的配置
    path: path.join(__dirname, './dist'), // 指定 打包好的文件，输出到哪个目录中去
    filename: 'bundle.js', // 这是指定 输出的文件的名称
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          { loader: 'style-loader' },
          {
            loader: 'css-loader',
            options: {
              modules: true
            }
          },
        ]
      },
      {
        test: /\.(png|jpg|gif)$/,
        use: [
          {
            loader: 'url-loader',
            options: {
              limit: 20,
              name: 'img/[name].[hash:8].[ext]'
            }
          }
        ]
      },
      {
        test: /\.js$/,
        exclude: /(node_modules|bower_components)/,
        use: {
          loader: 'babel-loader',
          options: {
            presets: ['@babel/preset-env']
          }
        }
      }
    ],

  },
  mode:'development'
};
```

**limit参数详解**

> 当加载的图片小于limit，会将图片编译为base64字符串形式
>
> 大于limit，需要使用file-loader 安装依赖！

```shell
$ npm install --save-dev file-loader
```

**再次打包运行会在dist目录中多出一个很长的文件**

![](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Snipaste_2021-04-26_15-20-20.png)

**package.json**

```json
{
  "name": "webpack_test",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack"
  },
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "css-loader": "^5.2.4",
    "file-loader": "^6.2.0",
    "style-loader": "^2.0.0",
    "url-loader": "^4.1.1",
    "webpack-cli": "^4.6.0"
  },
  "dependencies": {
    "@babel/core": "^7.13.16",
    "@babel/preset-env": "^7.13.15",
    "babel-core": "^6.26.3",
    "babel-loader": "^8.0.0-beta.0",
    "babel-preset-env": "^1.7.0",
    "less": "^4.1.1",
    "less-loader": "^8.1.1",
    "webpack": "^5.35.1"
  }
}
```

**在webpack.config.js文件中可以指定图片生成的名字，这样就不会那么长了**

```javascript
{
        test: /\.(png|jpg|gif)$/,
        use: [
          {
            loader: 'url-loader',
            options: {
              limit: 20,
              name: 'img/[name].[hash:8].[ext]'
            }
          }
        ]
      }
```

![](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Snipaste_2021-04-26_15-23-15.png)



## 8.4 Webpack ES6转为ES5

**参考地址：[babel-loader | webpack 中文网 (webpackjs.com)](https://www.webpackjs.com/loaders/babel-loader/)**

几个命令搞定，有些浏览器不兼容ES6语法 所以转为ES5语法

```shell
$ npm install babel-loader@8.0.0-beta.0 @babel/core @babel/preset-env webpack
```

**在配置文件中添加rule**

```javascript
{
        test: /\.js$/,
        exclude: /(node_modules|bower_components)/,
        use: {
          loader: 'babel-loader',
          options: {
            presets: ['@babel/preset-env']
          }
        }
      }
```

## 8.5 Webpack添加插件

插件 plugin

**plugin是什么？**

- plugin是插件的意思，通常是用于对某个现有的架构进行扩展。

- webpack中的插件，就是对webpack现有功能的各种扩展，比如打包优化，文件压缩等等。

**loader和plugin区别**

- loader主要用于转换某些类型的模块，它是一个转换器。

- plugin是插件，它是对webpack本身的扩展，是一个扩展器。

**plugin的使用过程：**

- 步骤一：通过npm安装需要使用的plugins(某些webpack已经内置的插件不需要安装)

- 步骤二：在webpack.config.js中的plugins中配置插件

### 8.5.1 **添加版权的**Plugin

该插件名字叫BannerPlugin，属于webpack自带的插件

**修改webpack.config.js的文件**

![image-20210428172746941](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210428172746941.png)

重新打包程序：查看bundle.js文件的头部，看到如下信息

![image-20210428172808322](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210428172808322.png)

### 8.5.2 打包html的plugin

执行命令

```shell
npm install html-webpack-plugin --save-dev
```

> 自动生成一个index.html文件(可以指定模板来生成)
>
> 将打包的js文件，自动通过script标签插入到body中

**修改webpack.config.js的文件**

![image-20210428172941733](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210428172941733.png)

### 8.5.3 搭建本地服务器

执行命令

```shell
$ npm install --save-dev webpack-dev-server@2.9.1
```

提供了一个可选的本地开发服务器，这个本地服务器基于node.js搭建，内部使用express框架，可以实现我们想要的让浏览器自动刷新显示我们修改后的结果

webpack.config.js文件配置修改

是否实时更新，立即打开浏览器

![image-20210428173609655](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210428173609655.png)

# 9，Vue-CLI

**参考地址：[创建一个项目 | Vue CLI (vuejs.org)](https://cli.vuejs.org/zh/guide/creating-a-project.html#vue-create)**

**类似于SpingBoot 开箱即用**

**如果你只是简单写几个Vue的Demo程序, 那么你不需要Vue CLI.**

**如果你在开发大型项目, 那么你需要, 并且必然需要使用Vue CLI**

> 使用Vue.js开发大型应用时，我们需要考虑代码目录结构、项目结构和部署、热加载、代码单元测试等事情。
>
> 如果每个项目都要手动完成这些工作，那无以效率比较低效，所以通常我们会使用一些脚手架工具来帮助完成这些事情。

**CLI** **是什么意思**

> CLI是Command-Line Interface, 翻译为命令行界面, 但是俗称脚手架.
>
> Vue CLI是一个官方发布 vue.js 项目脚手架
>
> 使用 vue-cli 可以快速搭建Vue开发环境以及对应的webpack配置

**准备工作**

> cnpm安装
>
> 由于国内直接使用 npm 的官方镜像是非常慢的，这里推荐使用淘宝 NPM 镜像。
>
> 你可以使用淘宝定制的 cnpm (gzip 压缩支持) 命令行工具代替默认的 npm:
>
> $ npm install -g cnpm --registry=https://registry.npm.taobao.org
>
> 这样就可以使用 cnpm 命令来安装模块了：
>
> $ cnpm install [name]

**什么是NPM?**

- NPM的全称是Node Package Manager

- 是一个NodeJS包管理和分发工具，已经成为了非官方的发布Node模块（包）的标准。

- 我们会经常使用NPM来安装一些开发过程中依赖包.

**Vue.js官方脚手架工具就使用了webpack模板**

- 对所有的资源会压缩等优化操作

- 它在开发过程中提供了一套完整的功能，能够使得我们开发过程中变得高效

先全局安装

```shell
$ npm install webpack -g
```

这里以CLI2版本讲解 后续都是用CLI4版本！



## 9.1 CLI2详解

![image-20210428174409473](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210428174409473.png)

![image-20210428174425424](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210428174425424.png)

**Runtime-Compiler和Runtime-only的区别**

![image-20210428174548295](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210428174548295.png)

**Vue程序运行过程**

![image-20210428174809941](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210428174809941.png)

## 9.2 Render函数使用

![image-20210428174949810](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210428174949810.png)

**npm run build 命令**

![image-20210428175012473](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210428175012473.png)

**npm run dev 命令**

![image-20210428175136301](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210428175136301.png)

## 9.3 CLI3详解

![image-20210428175219668](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210428175219668.png)

![image-20210428175229649](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210428175229649.png)

启动可视化界面 管理配置

```shell
$ vue ui
```

**在8080端口访问即可看到界面！**



# 10, Vue-router

路由是一个网络工程里面的术语。

**路由**（**routing**）就是通过互联的网络把信息从源地址传输到目的地址的活动. --- 维基百科

**什么是路由器？**

> 路由器提供了两种机制: 路由和转送.
>
> 路由是决定数据包从**来源**到**目的地**的路径.
>
> 转送将**输入端**的数据转移到合适的**输出端**.
>
> 路由中有一个非常重要的概念叫路由表.
>
> 路由表本质上就是一个映射表, 决定了数据包的指向

## 10.1 后端路由的阶段

早期的网站开发整个HTML页面是由服务器来渲染的.

服务器直接生产渲染好对应的HTML页面, 返回给客户端进行展示.

**但是, 一个网站, 这么多页面服务器如何处理呢?**

一个页面有自己对应的网址, 也就是URL.
URL会发送到服务器, 服务器会通过正则对该URL进行匹配, 并且最后交给一个Controller进行处理.
Controller进行各种处理, 最终生成HTML或者数据, 返回给前端.
这就完成了一个IO操作.

**上面的这种操作, 就是后端路由.**
当我们页面中需要请求不同的路径内容时, 交给服务器来进行处理, 服务器渲染好整个页面, 并且将页面返回给客户顿.
这种情况下渲染好的页面, 不需要单独加载任何的js和css, 可以直接交给浏览器展示, 这样也有利于SEO的优化.

**后端路由的缺点:**

- 一种情况是整个页面的模块由后端人员来编写和维护的.
- 另一种情况是前端开发人员如果要开发页面, 需要通过PHP和Java等语言来编写页面代码.
- 而且通常情况下HTML代码和数据以及对应的逻辑会混在一起, 编写和维护都是非常糟糕的事情

## 10.2 前端路由的阶段

**前后端分离阶段：**
随着Ajax的出现, 有了前后端分离的开发模式.

> 后端只提供API来返回数据, 前端通过Ajax获取数据, 并且可以通过JavaScript将数据渲染到页面中.
> 这样做最大的优点就是前后端责任的清晰, 后端专注于数据上, 前端专注于交互和可视化上.
> 并且当移动端(iOS/Android)出现后, 后端不需要进行任何处理, 依然使用之前的一套API即可.
> 目前很多的网站依然采用这种模式开发.



**单页面富应用阶段:**
其实SPA最主要的特点就是在前后端分离的基础上加了一层前端路由.
也就是前端来维护一套路由规则.

**前端路由的核心是什么呢？**
改变URL，但是页面不进行整体的刷新



## 10.3 认识和安装 Vue-router

vue-router是Vue.js官方的路由插件，它和vue.js是深度集成的，适合用于构建单页面应用。

**我们可以访问其官方网站对其进行学习 [Vue Router (vuejs.org)](https://router.vuejs.org/zh/)**



**vue-router是基于路由和组件的**

- 路由用于设定访问路径, 将路径和组件映射起来.

- 在vue-router的单页面应用中, 页面的路径的改变就是组件的切换



- 步骤一：安装命令

```shell
$ npm install vue-router --save
```

- 步骤二：模块化工程中使用它(因为是一个插件, 所以可以通过Vue.use()来安装路由功能)
  - 第一步：导入路由对象，并且调用 Vue.use(VueRouter)

  - 第二步：创建路由实例，并且传入路由映射配置 
  - 第三步：在Vue实例中挂载创建的路由实例 

```javascript
import Vue from 'vue' import VueRouter from 'vue-router' Vue.use(VueRouter)
```

**使用vue-router的步骤:**

- 第一步: 创建路由组件

- 第二步: 配置路由映射: 组件和路径映射关系

- 第三步: 使用路由: 通过<router-link>和<router-view>



**创建Vue-router实例**

![](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Snipaste_2021-04-29_18-39-17.png)

**挂载到Vue实例中**

![](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Snipaste_2021-04-29_18-40-34.png)

**步骤一：创建路由组件**

![image-20210429184238044](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210429184238044.png)

**Home.vue**

```vue
<template>
<div id="app">
  <h2>我是首页</h2>
  <p>我是首页内容，哈哈哈</p>

  <router-link to="/home/news">新闻</router-link>
  <router-link to="/home/message">消息</router-link>
  <router-view></router-view>
</div>
</template>

<script>
export default {
  name: "Home"
}
</script>

<style scoped>

</style>
```

**About.vue**

```vue
<template>
  <div id="app">
    <h2>我是关于</h2>
    <p>我是关于内容，哈哈哈</p>
  </div>
</template>

<script>
export default {
  name: "About"
}
</script>

<style scoped>

</style>
```

**步骤二：配置组件和路由的映射关系**

![image-20210429184406195](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210429184406195.png)

**index.js**

```javascript
//通过vue.use插件 安装插件
import Vue from "vue";
import VueRouter from "vue-router";
// import About from "@/components/About";
// import Home from "@/components/Home";
import User from "@/components/User";
// import HomeNews from "@/components/HomeNews";
// import HomeMessage from "@/components/HomeMessage";

const Home=()=> import('../components/Home')
const news=()=> import('../components/HomeNews')
const message=()=> import('../components/HomeMessage')
const about=()=> import('../components/About')
const Profile=()=> import('../components/Profile')

Vue.use(VueRouter)

//创建Vuerouter对象
const routes = [
  {
    path: '/',
    redirect: '/home'
  },
  {
    path: '/home',
    component: Home,
    children: [
      {
        path: 'news',
        component: news
      }, {
        path: 'message',
        component: message
      }
    ],
    meta:{
      title:'首页'
    }
  },
  {
    path: '/about',
    component: about,
    meta: {
      title: '关于'
    }
  },
  {
    path: '/user/:userId',
    component: User
  },
  {
    path: '/profile',
    component:Profile,
    meta: {
      title: '档案'
    }
  }
]

const router = new VueRouter({
  routes,
  mode: 'history'
})

router.beforeEach((to,from,next)=>{
  window.document.title=to.meta.title
  console.log('哈哈哈')
  next()
})
//将router对象传入vue实例
export default router
```

**步骤三：使用路由**

![image-20210429184542990](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210429184542990.png)

> - <router-link>: 该标签是一个vue-router中已经内置的组件, 它会被渲染成一个<a>标签.
> - <router-view>: 该标签会根据当前的路径, 动态渲染出不同的组件.
> - 网页的其他内容, 比如顶部的标题/导航, 或者底部的一些版权信息等会和<router-view>处于同一个等级.
> - 在路由切换时, 切换的是<router-view>挂载的组件, 其他内容不会发生改变.

**App.vue**

```vue
<template>
  <div id="app">
<!--    <img alt="Vue logo" src="./assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/>-->
    <router-link to="/home" tag="button" replace>首页</router-link>
    <router-link to="/about" tag="button" replace>关于</router-link>
    <router-link v-bind:to="'/user/'+userId" tag="button" replace>用户</router-link>
    <router-link :to="{
      path:'/profile/'+123,
      query:{name:'zk',age:20}
    }" tag="button" replace>档案</router-link>
    <router-view></router-view>
  </div>
</template>

<script>

export default {
  name: 'App',
  data(){
    return{
      userId:'zhuangkang'
    }
  }
  /*components: {
    HelloWorld,
    About,
    Home,
  }*/
}
</script>

<style>
/*#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}*/
.router-link-active{
  color: crimson;
}
</style>
```

**效果**

![](https://gitee.com/zhuang-kang/note-picture-2/raw/master/image-20210429184737178.png)



**设置路由默认路径**

![image-20210429184815709](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210429184815709.png)

> 我们在routes中又配置了一个映射. 
> path配置的是根路径: /
> redirect是重定向, 也就是我们将根路径重定向到/home的路径下, 这样就可以得到我们想要的结果了.

**HTML5的History模式**

![image-20210429184907487](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210429184907487.png)

测试

![image-20210429184918287](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210429184918287.png)



**router-link补充**

- 在前面的<router-link>中, 我们只是使用了一个属性: to, 用于指定跳转的路径.
- <router-link>还有一些其他属性:
  - tag: tag可以指定<router-link>之后渲染成什么组件, 比如上面的代码会被渲染成一个<li>元素, 而不是<a>
  - replace: replace不会留下history记录, 所以指定replace的情况下, 后退键返回不能返回到上一个页面中
  - active-class: 当<router-link>对应的路由匹配成功时, 会自动给当前元素设置一个router-link-active的class, 设置active-class可以修改默认的名称.
  - 在进行高亮显示的导航菜单或者底部tabbar时, 会使用到该类.
    但是通常不会修改类的属性, 会直接使用默认的router-link-active即可.  

![image-20210429185058134](C:\Users\dell\AppData\Roaming\Typora\typora-user-images\image-20210429185058134.png)



**路由代码跳转**

将代码修改如下

![image-20210429185142200](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210429185142200.png)

**动态路由**

> 在某些情况下，一个页面的path路径可能是不确定的，比如我们进入用户界面时，希望是如下的路径：/user/aaaa或/user/bbbb
> 除了有前面的/user之外，后面还跟上了用户的ID
> 这种path和Component的匹配关系，我们称之为动态路由(也是路由传递数据的一种方式)。

![image-20210429185250739](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210429185250739.png)



## 10.4 路由的懒加载

> **官方给出了解释:**
> 当打包构建应用时，Javascript 包会变得非常大，影响页面加载。
> 如果我们能把不同路由对应的组件分割成不同的代码块，然后当路由被访问的时候才加载对应组件，这样就更加高效了
>
> **官方在说什么呢?**
> 首先, 我们知道路由中通常会定义很多不同的页面.
> 这个页面最后被打包在哪里呢? 一般情况下, 是放在一个js文件中.
> 但是, 页面这么多放在一个js文件中, 必然会造成这个页面非常的大.
> 如果我们一次性从服务器请求下来这个页面, 可能需要花费一定的时间, 甚至用户的电脑上还出现了短暂空白的情况.
> 如何避免这种情况呢? 使用路由懒加载就可以了.
>
> **路由懒加载做了什么?**
> 路由懒加载的主要作用就是将路由对应的组件打包成一个个的js代码块.
> 只有在这个路由被访问到的时候, 才加载对应的组件

效果如下

![image-20210429185731621](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210429185731621.png)



**懒加载的方式**

![](C:\Users\dell\Desktop\Notes\Vue\img\Snipaste_2021-04-29_18-58-37.png)



## 10.5 嵌套路由

嵌套路由是一个很常见的功能
比如在home页面中, 我们希望通过/home/news和/home/message访问一些内容.
一个路径映射一个组件, 访问这两个路径也会分别渲染两个组件.

![image-20210429190352003](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210429190352003.png)

> - 实现嵌套路由有两个步骤:
> - 创建对应的子组件, 并且在路由映射中配置对应的子路由.
> - 在组件内部使用<router-view>标签.

**嵌套路由实现**

![image-20210429191209978](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210429191209978.png)

![Snipaste_2021-04-29_19-11-29](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Snipaste_2021-04-29_19-11-29.png)

**嵌套默认路径**

![image-20210429191558955](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210429191558955.png)



> **演示传递参数, 我们这里再创建一个组件, 并且将其配置好**
> 第一步: 创建新的组件Profile.vue 
> 第二步: 配置路由映射 
> 第三步: 添加跳转的<router-link>

![image-20210429192013898](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210429192013898.png)

**传递参数的方式**

> **传递参数主要有两种类型: params和query**
>
> - **params的类型:**
>   配置路由格式: /router/:id
>   传递的方式: 在path后面跟上对应的值
>   传递后形成的路径: /router/123, /router/abc
>
> - **query的类型:**
>   配置路由格式: /router, 也就是普通配置
>   传递的方式: 对象中使用query的key作为传递方式
>   传递后形成的路径: /router?id=123, /router?id=abc



**传递参数方式一: <router-link>**

![image-20210429192148916](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210429192148916.png)

**传递参数方式二: JavaScript代码**

![image-20210429192208324](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210429192208324.png)

![Snipaste_2021-04-29_19-24-27](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/Snipaste_2021-04-29_19-24-27.png)



## 10.6 导航守卫

> 我们来考虑一个需求: 在一个SPA应用中, 如何改变网页的标题呢?
>
> - 网页标题是通过<title>来显示的, 但是SPA只有一个固定的HTML, 切换不同的页面时, 标题并不会改变.
> - 但是我们可以通过JavaScript来修改<title>的内容.window.document.title = '新的标题'.
> - 那么在Vue项目中, 在哪里修改? 什么时候修改比较合适呢?
>   - 普通的修改方式:
>     - 我们比较容易想到的修改标题的位置是每一个路由对应的组件.vue文件中.
>       通过mounted声明周期函数, 执行对应的代码进行修改即可.
>       但是当页面比较多时, 这种方式不容易维护(因为需要在多个页面执行类似的代码).
>       有没有更好的办法呢? 使用导航守卫即可.

**什么是导航守卫?**(**“导航”表示路由正在发生改变。**)

- vue-router提供的导航守卫主要用来监听监听路由的进入和离开的.
- vue-router提供了beforeEach和afterEach的钩子函数, 它们会在路由即将改变前和改变后触发.



### 全局前置守卫

```javascript
const router = new VueRouter({ ... })

router.beforeEach((to, from, next) => {
  // ...
})
```

当一个导航触发时，全局前置守卫按照创建顺序调用。守卫是异步解析执行，此时导航在所有守卫 resolve 完之前一直处于 **等待中**。

每个守卫方法接收三个参数：

- **`to: Route`**: 即将要进入的目标 [路由对象](https://router.vuejs.org/zh/api/#路由对象)
- **`from: Route`**: 当前导航正要离开的路由
- **`next: Function`**: 一定要调用该方法来 **resolve** 这个钩子。执行效果依赖 `next` 方法的调用参数。
  - **`next()`**: 进行管道中的下一个钩子。如果全部钩子执行完了，则导航的状态就是 **confirmed** (确认的)。
  - **`next(false)`**: 中断当前的导航。如果浏览器的 URL 改变了 (可能是用户手动或者浏览器后退按钮)，那么 URL 地址会重置到 `from` 路由对应的地址。
  - **`next('/')` 或者 `next({ path: '/' })`**: 跳转到一个不同的地址。当前的导航被中断，然后进行一个新的导航。你可以向 `next` 传递任意位置对象，且允许设置诸如 `replace: true`、`name: 'home'` 之类的选项以及任何用在 [`router-link` 的 `to` prop](https://router.vuejs.org/zh/api/#to) 或 [`router.push`](https://router.vuejs.org/zh/api/#router-push) 中的选项。
  - **`next(error)`**: (2.4.0+) 如果传入 `next` 的参数是一个 `Error` 实例，则导航会被终止且该错误会被传递给 [`router.onError()`](https://router.vuejs.org/zh/api/#router-onerror) 注册过的回调。

**确保 `next` 函数在任何给定的导航守卫中都被严格调用一次。它可以出现多于一次，但是只能在所有的逻辑路径都不重叠的情况下，否则钩子永远都不会被解析或报错**。这里有一个在用户未能验证身份时重定向到 `/login` 的示例：

```javascript
// BAD
router.beforeEach((to, from, next) => {
  if (to.name !== 'Login' && !isAuthenticated) next({ name: 'Login' })
  // 如果用户未能验证身份，则 `next` 会被调用两次
  next()
})
// GOOD
router.beforeEach((to, from, next) => {
  if (to.name !== 'Login' && !isAuthenticated) next({ name: 'Login' })
  else next()
})
```

- 补充一:如果是后置钩子, 也就是afterEach, 不需要主动调用next()函数.

- 补充二: 上面我们使用的导航守卫, 被称之为全局守卫.

  - 路由独享的守卫.
  - 组件内的守卫.

  

**其余参考官网：[导航守卫 | Vue Router (vuejs.org)](https://router.vuejs.org/zh/guide/advanced/navigation-guards.html#组件内的守卫)**

### router-view

`<router-view>` 组件是一个 functional 组件，渲染路径匹配到的视图组件。`<router-view>` 渲染的组件还可以内嵌自己的 `<router-view>`，根据嵌套路径，渲染嵌套组件。

其他属性 (非 router-view 使用的属性) 都直接传给渲染的组件， 很多时候，每个路由的数据都是包含在路由参数中。

因为它也是个组件，所以可以配合 `<transition>` 和 `<keep-alive>` 使用。如果两个结合一起用，要确保在内层使用 `<keep-alive>`：

```html
<transition>
  <keep-alive>
    <router-view></router-view>
  </keep-alive>
</transition>
```



# 11，Promise

**官方文档：[Promise - JavaScript | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise)**

> ES6中一个非常重要和好用的特性就是Promise
> 但是初次接触Promise会一脸懵逼，这TM是什么东西？
>
> Promise到底是做什么的呢？
> Promise是异步编程的一种解决方案。
> 那什么时候我们会来处理异步事件呢？
>
> 一种很常见的场景应该就是网络请求了。
> 我们封装一个网络请求的函数，因为不能立即拿到结果，所以不能像简单的3+4=7一样将结果返回。
> 所以往往我们会传入另外一个函数，在数据请求成功时，将数据通过传入的函数回调出去。
> 如果只是一个简单的网络请求，那么这种方案不会给我们带来很大的麻烦。

## 11.2 网络请求的回调地狱

- 我们需要通过一个url1从服务器加载一个数据data1，data1中包含了下一个请求的url2
- 我们需要通过data1取出url2，从服务器加载数据data2，data2中包含了下一个请求的url3
- 我们需要通过data2取出url3，从服务器加载数据data3，data3中包含了下一个请求的url4
- 发送网络请求url4，获取最终的数据data4

![image-20210504181753747](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210504181753747.png)

这里，我们用一个定时器来模拟异步事件：

- 假设下面的data是从网络上1秒后请求的数据
- console.log就是我们的处理方式

![image-20210504181837349](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210504181837349.png)

换成promise方法

![image-20210504181905331](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210504181905331.png)



## 11.2 定时器异步事件解析

new Promise很明显是创建一个Promise对象
小括号中((resolve, reject) => {})也很明显就是一个函数，而且我们这里用的是之前刚刚学习过的箭头函数。

但是resolve, reject它们是什么呢？
我们先知道一个事实：在创建Promise时，传入的这个箭头函数是固定的（一般我们都会这样写）
resolve和reject它们两个也是函数，通常情况下，我们会根据请求数据的成功和失败来决定调用哪一个。

成功还是失败？
**如果是成功的，那么通常我们会调用resolve(messsage)，这个时候，我们后续的then会被回调
如果是失败的，那么通常我们会调用reject(error)，这个时候，我们后续的catch会被回调**



## 11.3 Promise 状态

一个 `Promise` 必然处于以下几种状态之一：

- *待定（pending）*: 初始状态，既没有被兑现，也没有被拒绝。
- *已兑现（fulfilled）*: 意味着操作成功完成。
- *已拒绝（rejected）*: 意味着操作失败。

![image-20210504183229167](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210504183229167.png)

## 11.4 链式调用

![image-20210504183328133](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210504183328133.png)

使用链式写法后

![image-20210504183346165](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210504183346165.png)



# 12，VueX

## 12.1 什么是VueX？

Vuex 是一个专为 Vue.js 应用程序开发的**状态管理模式**。它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化。



**什么是状态管理模式？**

```javascript
new Vue({
  // state
  data () {
    return {
      count: 0
    }
  },
  // view
  template: `
    <div>{{ count }}</div>
  `,
  // actions
  methods: {
    increment () {
      this.count++
    }
  }
})
```

这个状态自管理应用包含以下几个部分：

- **state**，驱动应用的数据源；
- **view**，以声明方式将 **state** 映射到视图；
- **actions**，响应在 **view** 上的用户输入导致的状态变化



**单向数据流图示**

![image-20210510101723005](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210510101723005.png)

但是，当我们的应用遇到**多个组件共享状态**时，单向数据流的简洁性很容易被破坏：

- 多个视图依赖于同一状态。
- 来自不同视图的行为需要变更同一状态。

对于问题一，传参的方法对于多层嵌套的组件将会非常繁琐，并且对于兄弟组件间的状态传递无能为力。对于问题二，我们经常会采用父子组件直接引用或者通过事件来变更和同步状态的多份拷贝。以上的这些模式非常脆弱，通常会导致无法维护的代码。

因此，我们为什么不把组件的共享状态抽取出来，以一个全局单例模式管理呢？在这种模式下，我们的组件树构成了一个巨大的“视图”，不管在树的哪个位置，任何组件都能获取状态或者触发行为！

通过定义和隔离状态管理中的各种概念并通过强制规则维持视图和状态间的独立性，我们的代码将会变得更结构化且易维护。

![image-20210510101835431](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210510101835431.png)

- 安装VueX

```shell
$ npm install vuex --save
```

- 在一个模块化的打包系统中，您必须显式地通过 `Vue.use()` 来安装 Vuex：

```javascript
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)
```

- 每一个 Vuex 应用的核心就是 store（仓库）。“store”基本上就是一个容器，它包含着你的应用中大部分的**状态 (state)**。Vuex 和单纯的全局对象有以下两点不同：

1. Vuex 的状态存储是响应式的。当 Vue 组件从 store 中读取状态的时候，若 store 中的状态发生变化，那么相应的组件也会相应地得到高效更新。
2. 你不能直接改变 store 中的状态。改变 store 中的状态的唯一途径就是显式地**提交 (commit) mutation**。这样使得我们可以方便地跟踪每一个状态的变化，从而让我们能够实现一些工具帮助我们更好地了解我们的应用。



来创建一个 store。创建过程直截了当——仅需要提供一个初始 state 对象和一些 mutation：

```javascript
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increment (state) {
      state.count++
    }
  }
})
```

可以通过 `store.state` 来获取状态对象，以及通过 `store.commit` 方法触发状态变更

```js
store.commit('increment')

console.log(store.state.count) // -> 1
```

为了在 Vue 组件中访问 `this.$store` property，你需要为 Vue 实例提供创建好的 store。Vuex 提供了一个从根组件向所有子组件，以 `store` 选项的方式“注入”该 store 的机制：

```javascript
new Vue({
  el: '#app',
  store: store,
})
```

**ES6简写**

```javascript
new Vue({
  el: '#app',
  store
})
```

我们可以从组件的方法提交一个变更：

```javascript
methods: {
  increment() {
    this.$store.commit('increment')
    console.log(this.$store.state.count)
  }
}
```

我们通过提交 mutation 的方式，而非直接改变 `store.state.count`，是因为我们想要更明确地追踪到状态的变化。这个简单的约定能够让你的意图更加明显，这样你在阅读代码的时候能更容易地解读应用内部的状态改变。



## 12.2 VueX核心概念

### 12.2.1 State

Vuex 使用**单一状态树**——是的，用一个对象就包含了全部的应用层级状态。至此它便作为一个“唯一数据源 ([SSOT (opens new window)](https://en.wikipedia.org/wiki/Single_source_of_truth))”而存在。这也意味着，每个应用将仅仅包含一个 store 实例。单一状态树让我们能够直接地定位任一特定的状态片段，在调试的过程中也能轻易地取得整个当前应用状态的快照。

**存储在 Vuex 中的数据和 Vue 实例中的 `data` 遵循相同的规则，例如状态对象必须是纯粹 (plain) 的**



**在 Vue 组件中获得 Vuex 状态**

由于 Vuex 的状态存储是响应式的，从 store 实例中读取状态最简单的方法就是在[计算属性](https://cn.vuejs.org/guide/computed.html)

```js
// 创建一个 Counter 组件
const Counter = {
  template: `<div>{{ count }}</div>`,
  computed: {
    count () {
      return store.state.count
    }
  }
}
```

每当 `store.state.count` 变化的时候, 都会重新求取计算属性，并且触发更新相关联的 DOM。

然而，这种模式导致组件依赖全局状态单例。在模块化的构建系统中，在每个需要使用 state 的组件中需要频繁地导入，并且在测试组件时需要模拟状态。

Vuex 通过 `store` 选项，提供了一种机制将状态从根组件“注入”到每一个子组件中（需调用 `Vue.use(Vuex)`）：

```js
const app = new Vue({
  el: '#app',
  // 把 store 对象提供给 “store” 选项，这可以把 store 的实例注入所有的子组件
  store,
  components: { Counter },
  template: `
    <div class="app">
      <counter></counter>
    </div>
  `
})
```

通过在根实例中注册 `store` 选项，该 store 实例会注入到根组件下的所有子组件中，且子组件能通过 `this.$store` 访问到。

```js
const Counter = {
  template: `<div>{{ count }}</div>`,
  computed: {
    count () {
      return this.$store.state.count
    }
  }
}
```

**mapState 辅助函数**

当一个组件需要获取多个状态的时候，将这些状态都声明为计算属性会有些重复和冗余。为了解决这个问题，我们可以使用 `mapState` 辅助函数帮助我们生成计算属性，让你少按几次键：

```js
// 在单独构建的版本中辅助函数为 Vuex.mapState
import { mapState } from 'vuex'

export default {
  // ...
  computed: mapState({
    // 箭头函数可使代码更简练
    count: state => state.count,

    // 传字符串参数 'count' 等同于 `state => state.count`
    countAlias: 'count',

    // 为了能够使用 `this` 获取局部状态，必须使用常规函数
    countPlusLocalState (state) {
      return state.count + this.localCount
    }
  })
}
```

当映射的计算属性的名称与 state 的子节点名称相同时，我们也可以给 `mapState` 传一个字符串数组。

```javascript
computed: mapState([
  // 映射 this.count 为 store.state.count
  'count'
])
```



### 12.2.2 Getters

有时候，我们需要从store中获取一些state变异后的状态，比如下面的Store中：

获取学生年龄大于20的个数

![image-20210510105113712](C:\Users\dell\Desktop\Notes\Vue\img\image-20210510105113712.png)

我们可以在Store中定义getters

![image-20210510105136807](C:\Users\dell\Desktop\Notes\Vue\img\image-20210510105136807.png)

![image-20210510105144014](C:\Users\dell\Desktop\Notes\Vue\img\image-20210510105144014.png)



如果我们已经有了一个获取所有年龄大于20岁学生列表的getters, 那么代码可以这样来写

![image-20210510105620419](C:\Users\dell\Desktop\Notes\Vue\img\image-20210510105620419.png)

### 12.2.3 Mutation

更改 Vuex 的 store 中的状态的唯一方法是提交 mutation。Vuex 中的 mutation 非常类似于事件：每个 mutation 都有一个字符串的 **事件类型 (type)** 和 一个 **回调函数 (handler)**。这个回调函数就是我们实际进行状态更改的地方，并且它会接受 state 作为第一个参数：

```js
const store = new Vuex.Store({
  state: {
    count: 1
  },
  mutations: {
    increment (state) {
      // 变更状态
      state.count++
    }
  }
})
```

你不能直接调用一个 mutation handler。这个选项更像是事件注册：“当触发一个类型为 `increment` 的 mutation 时，调用此函数。”要唤醒一个 mutation handler，你需要以相应的 type 调用 **store.commit** 方法：

```js
store.commit('increment')
```

**提交载荷（Payload）**

你可以向 `store.commit` 传入额外的参数，即 mutation 的 **载荷（payload）**

```js
// ...
mutations: {
  increment (state, n) {
    state.count += n
  }
}
```

```js
store.commit('increment', 10)
```

在大多数情况下，载荷应该是一个对象，这样可以包含多个字段并且记录的 mutation 会更易读：

```js
// ...
mutations: {
  increment (state, payload) {
    state.count += payload.amount
  }
}


store.commit('increment', {
  amount: 10
})
```

**对象风格的提交方式**

提交 mutation 的另一种方式是直接使用包含 `type` 属性的对象：提交 mutation 的另一种方式是直接使用包含 type 属性的对象：

```js
store.commit({
  type: 'increment',
  amount: 10
})
```

当使用对象风格的提交方式，整个对象都作为载荷传给 mutation 函数，因此 handler 保持不变：

```js
mutations: {
  increment (state, payload) {
    state.count += payload.amount
  }
}
```

**Mutation 需遵守 Vue 的响应规则**

既然 Vuex 的 store 中的状态是响应式的，那么当我们变更状态时，监视状态的 Vue 组件也会自动更新。这也意味着 Vuex 中的 mutation 也需要与使用 Vue 一样遵守一些注意事项：

1. 最好提前在你的 store 中初始化好所有所需属性。
2. 当需要在对象上添加新属性时，你应该

- 使用 `Vue.set(obj, 'newProp', 123)`, 或者

- 以新对象替换老对象。例如，利用[对象展开运算符 (opens new window)](https://github.com/tc39/proposal-object-rest-spread)我们可以这样写：

  ```js
  state.obj = { ...state.obj, newProp: 123 }
  ```

**使用常量替代 Mutation 事件类型**
使用常量替代 mutation 事件类型在各种 Flux 实现中是很常见的模式

```js
// mutation-types.js
export const SOME_MUTATION = 'SOME_MUTATION'
```

```js
// store.js
import Vuex from 'vuex'
import { SOME_MUTATION } from './mutation-types'

const store = new Vuex.Store({
  state: { ... },
  mutations: {
    // 我们可以使用 ES2015 风格的计算属性命名功能来使用一个常量作为函数名
    [SOME_MUTATION] (state) {
      // mutate state
    }
  }
})
```

例子

![image-20210510111422775](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210510111422775.png)

![image-20210510111432070](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210510111432070.png)

![image-20210510111438694](https://gitee.com/zhuang-kang/note-picture/raw/master/Vue%E7%AC%94%E8%AE%B0%E5%9B%BE%E7%89%87/image-20210510111438694.png)

**Mutation同步函数**

通常情况下, Vuex要求我们Mutation中的方法必须是同步方法.

- 主要的原因是当我们使用devtools时, 可以devtools可以帮助我们捕捉mutation的快照.
- 但是如果是异步操作, 那么devtools将不能很好的追踪这个操作什么时候会被完成

```js
mutations: {
  someMutation (state) {
    api.callAsyncMethod(() => {
      state.count++
    })
  }
}
```

现在想象，我们正在 debug 一个 app 并且观察 devtool 中的 mutation 日志。每一条 mutation 被记录，devtools 都需要捕捉到前一状态和后一状态的快照。然而，在上面的例子中 mutation 中的异步函数中的回调让这不可能完成：因为当 mutation 触发的时候，回调函数还没有被调用，devtools 不知道什么时候回调函数实际上被调用——实质上任何在回调函数中进行的状态的改变都是不可追踪的



**在组件中提交 Mutation**
你可以在组件中使用 this.$store.commit('xxx') 提交 mutation，或者使用 mapMutations 辅助函数将组件中的 methods 映射为 store.commit 调用（需要在根节点注入 store）。

```js
import { mapMutations } from 'vuex'

export default {
  // ...
  methods: {
    ...mapMutations([
      'increment', // 将 `this.increment()` 映射为 `this.$store.commit('increment')`

      // `mapMutations` 也支持载荷：
      'incrementBy' // 将 `this.incrementBy(amount)` 映射为 `this.$store.commit('incrementBy', amount)`
    ]),
    ...mapMutations({
      add: 'increment' // 将 `this.add()` 映射为 `this.$store.commit('increment')`
    })
  }
}
```



### 12.2.4 Actions

Action 类似于 mutation，不同在于：

- Action 提交的是 mutation，而不是直接变更状态。
- Action 可以包含任意异步操作。

**来注册一个简单的 action：**

```javascript
const store = new Vuex.Store({
  state: {
    count: 0
  },
  mutations: {
    increment (state) {
      state.count++
    }
  },
  actions: {
    increment (context) {
      context.commit('increment')
    }
  }
})
```

**context是什么?**

- context是和store对象具有相同方法和属性的对象.
- 也就是说, 我们可以通过context去进行commit相关的操作, 也可以获取context.state

**分发 Action**
Action 通过 store.dispatch 方法触发：

```js
store.dispatch('increment')
```

**mutation 必须同步执行**这个限制 Action 就不受约束！我们可以在 action 内部执行**异步**操作：

```js
actions: {
  incrementAsync ({ commit }) {
    setTimeout(() => {
      commit('increment')
    }, 1000)
  }
}
```

**Actions 支持同样的载荷方式和对象方式进行分发：**

```js
// 以载荷形式分发
store.dispatch('incrementAsync', {
  amount: 10
})

// 以对象形式分发
store.dispatch({
  type: 'incrementAsync',
  amount: 10
})
```

### 12.2.5 Modules

由于使用单一状态树，应用的所有状态会集中到一个比较大的对象。当应用变得非常复杂时，store 对象就有可能变得相当臃肿。

为了解决以上问题，Vuex 允许我们将 store 分割成**模块（module）**。每个模块拥有自己的 state、mutation、action、getter、甚至是嵌套子模块——从上至下进行同样方式的分割：

```javascript
const moduleA = {
  state: () => ({ ... }),
  mutations: { ... },
  actions: { ... },
  getters: { ... }
}

const moduleB = {
  state: () => ({ ... }),
  mutations: { ... },
  actions: { ... }
}

const store = new Vuex.Store({
  modules: {
    a: moduleA,
    b: moduleB
  }
})

store.state.a // -> moduleA 的状态
store.state.b // -> moduleB 的状态
```

**模块的局部状态**
对于模块内部的 mutation 和 getter，接收的第一个参数是模块的局部状态对象

```javascript
const moduleA = {
  state: () => ({
    count: 0
  }),
  mutations: {
    increment (state) {
      // 这里的 `state` 对象是模块的局部状态
      state.count++
    }
  },

  getters: {
    doubleCount (state) {
      return state.count * 2
    }
  }
}
```

同样，对于模块内部的 action，局部状态通过 `context.state` 暴露出来，根节点状态则为 `context.rootState`：

```js
const moduleA = {
  // ...
  actions: {
    incrementIfOddOnRootSum ({ state, commit, rootState }) {
      if ((state.count + rootState.count) % 2 === 1) {
        commit('increment')
      }
    }
  }
}
```

**对于模块内部的 getter，根节点状态会作为第三个参数暴露出来：**

```js
const moduleA = {
  // ...
  getters: {
    sumWithRootCount (state, getters, rootState) {
      return state.count + rootState.count
    }
  }
}
```



# 13，Axios

后续补充...



# 14，项目实战

**源码：[Vue-Project: Vue购物车实战项目 (gitee.com)](https://gitee.com/zhuang-kang/vue-project)**

**学习视频：[https://www.bilibili.com/video/BV15741177Eh?p=148](https://www.bilibili.com/video/BV15741177Eh?p=148)**

## 14.1 耦合

- 耦合是指两个或多个体系或运动形式间通过相互作用而彼此影响以至联合起来的现象。
- 对象之间的耦合度就是对象之间的依赖性。对象之间的耦合越高，维护成本越高

**解耦：**
降低耦合度，让数据模型、业务逻辑和视图显示三层关系降低耦合，把关联依赖降到最低，不至于牵一发而动全身。

例：如果两者间需要交互，可以通过接口、通过消息、甚至引入框架，就是不要直接交叉写。
观察者模式：解耦的模式，它使观察者和被观察者的逻辑不再搅在一起，而是彼此独立、互不依赖。例：当用户切换为夜间模式时，被观察者就会通知所有的观察者‘设置改变，大家快蒙上遮罩’

例：封装多个数据的请求
从服务器拿数据，就是对网络模块进行封装，再使用网络模块进行对数据请求
封装 request.js 直接在相应的页面做网络请求
单独封装首页的网络请求至 home.js 文件中
因为首页可能有多个请求，封装后可对其做统一管理，数据请求的url就不会与首页组件内容耦合
使得 home.vue 面向 home.js 开发，可降低耦合度

```javascript
import {request} from './request'
export function getHomeMultidata() {
    return request({
        url: '/home/multidata'
    })
}
export function getHomeGoods(type, page) {
  return request({
      url: '/home/data',
      params: {
          type,
          page
      }
  })
}
```

## 14.2 发送网络请求
created
要在组件创建好之后、页面渲染之前发送请求获取数据，在 created 生命周期函数中做请求
保存数据
获取数据后，将数据保存在 data 中，因为函数里都是局部变量，函数一旦执行完，函数里的变量就会被回收掉
函数调用时，是将数据压入函数栈里，函数栈就可保存函数调用过程中所有变量。函数调用结束，就会将数据弹出函数栈，即释放函数中的所有变量
简化 created 中的代码
created 钩子中只处理主要逻辑，将具体的方法实现放到 methods 中

```javascript
created () {
    // 将created钩子中的代码简化，处理主要逻辑，具体的方法实现交给methods
    // 1.请求多个数据
    this.getHomeMultidata()
    // 2.请求商品数据
    this.getHomeGoods('pop')
    this.getHomeGoods('new')
    this.getHomeGoods('sell')
  },
  methods: {
    // 网络请求相关方法
    getHomeMultidata () {
      getHomeMultidata().then(res => {
    //   console.log(res)
    // this.result = res
    this.banners = res.data.banner.list
    this.recommends = res.data.recommend.list
    })
    },
    getHomeGoods (type) {
      const page = this.goods[type].page + 1
      getHomeGoods(type, page).then(res => {
      this.goods[type].list.push(...res.data.list)
      this.goods[type].page += 1  /* 更新data中的页码 */
      this.$refs.scroll.finishPullUp() // 调用方法，多次上拉加载更多
    })
    }
  }
```



## 14.3 better-scroll

在相应的组件中为 scroll 组件设置固定高度，才能滚动

计算高度

```css
父元素：height: 100vh;
滚动元素：
height: calc(100% - 98px); 
 overflow:hidden;
```

定位

```css
overflow: hidden;
  position: absolute;
  top: 44px;
  bottom: 49px;
  left: 0;
  right: 0;
```

**probeType**

- probe 侦测
- probeType 0 不侦测， 1 延时侦测， 2 手指滚动的过程中才侦测，手指离开后的惯性滚动不侦测，3 侦测所有滚动

1. scroll 组件中配置

```javascript
// 2.监听滚动的位置
        if(this.probeType===2 || this.probeType===3){
          this.scroll.on('scroll', position => {
          // console.log(position)
          this.$emit('scroll',position)
        })
```

2. 使用 scroll 的组件中设置，通过 `@scroll='contentScroll'` 传递事件

```javascript
contentScroll(position) {
      // console.log(position.y)
        // this.showTop = (-position.y) > 1000  绝对值Math.abs
        this.showTop = Math.abs(position.y) > 1000
        // 2.设置吸顶效果
        this.isTabFixed = (-position.y) > this.tabOffsetTop
    },
```

**pullUpLoad**

上拉加载更多

1. scroll 组件中配置

```javascript
if(this.pullUpLoad){ // pullUpLoad为true时，到达底部，为false的组件不用监听此事件，提升效率
        this.scroll.on('pullingUp',()=>{
          // console.log('bottom'); 到达底部时才触发函数
          this.$emit('pullingUp')
      })
```

2. 使用 scroll 的组件中设置

```javascript
getHomeGoods (type) {
      const page = this.goods[type].page + 1
      getHomeGoods(type, page).then(res => {
      this.goods[type].list.push(...res.data.list)
      this.goods[type].page += 1  /* 更新data中的页码 */
      this.$refs.scroll.finishPullUp() // 调用方法，多次上拉加载更多
    })
    }
```

3. 在更新完第一页的数据后，滚动到底部时触发上拉加载更多的方法，这个数据请求要在初始化数据方法的后面执行

```javascript
activated () {
    // 进入时滚动到离开时的位置this.saveY
    this.$refs.scroll.scrollTo(0, this.saveY, 0)
    this.$refs.scroll.refresh() // 进入时再刷新，避免出现小问题
  },
```



## 14.4 可滚动区域bug

better-scroll 是根据 scrollerHeight 属性决定有多少区域可滚动的，而 scrollerHeight 属性值是根据content中元素的高度决定的
遇到数据请求是异步加载时，比如加载异步加载图片时，刚开始的 scrollerHeight 可能没有计算到图片，当图片加载完成后，scrollerHeight 值明显小于想要的值，所以滚动会出现问题

监听每一张图片是否加载完成，每一张图片加载完成，都执行一次refresh
可利用 vuex 状态管理器，在 vuex 里设置一个属性，当item里图片加载完成后切换状态，当切换状态后，在home组件中刷新
利用事件总线（专门管理事件的，可用于非父子组件间的通信

```javascript
main.js  Vue.prototype.$bus = new Vue()
item组件  @load='imageLoad'  methods: imageLoad(){this.$bus.$emit('load')}
home组件  mounted () {this.$bus.$on('load',()=>{this.$refs.scroll.refresh()})}
```

1. 利用防抖函数 `debounce`

```javascript
mounted () {
    // 1.监听item中图片加载完成，刷新防抖函数放到了utils.js中，工具库
     const refresh = debounce(this.$refs.scroll.refresh,50)
      // 对监听的事件进行保存，方便离开home组件时取消此事件监听
       this.itemImgListener = () => { refresh() }
       this.$bus.$on('itemImageLoad',() => {
         this.$refs.scroll.refresh()
         refresh() // 此处是闭包，局部变量不会被销毁
     })
       this.$bus.$on('itemImageLoad', this.itemImgListener)
  },
```



## 14.5 tabControl

**点击切换数据**

```javascript
methods: {
    itemClick (index) {
      this.currentIndex = index
      this.$emit('tabClick',index) /* 将子组件数据传递到父组件 */
    }
  }
```

```javascript
tabClick (index) {
      switch (index) {
        case 0:
          this.currentType = 'pop'
          break
        case 1:
          this.currentType = 'new'
          break
        case 2:
          this.currentType= 'sell'
          break
      }
```

**吸顶效果**

1. 获取到 tabControl 的 offsetTop （滚动这个距离后即吸顶）
2. 利用 $el 获取组件中的元素，图片加载完成后 offsetTop 值才正确
3. 监听轮播图图片加载完成

```javascript
swiperImageLoad () {
      // 获取tabOffsetTop的offsetTop
    this.tabOffsetTop = this.$refs.tabControl2.$el.offsetTop
    },
```

```javascript
contentScroll(position) {
      // console.log(position.y)
        // this.showTop = (-position.y) > 1000  绝对值Math.abs
        this.showTop = Math.abs(position.y) > 1000
        // 2.设置吸顶效果
        this.isTabFixed = (-position.y) > this.tabOffsetTop
    },
```

**两个 tabControl 解决闪退**

分别设置 ref=“tabControl1” 和2，再拿到 this.$refs.tabControl1.currentIndex
再在点击时将其等于index，就可关联两个 tabControl 的点击

```javascript
tabClick (index) {
      switch (index) {
        case 0:
          this.currentType = 'pop'
          break
        case 1:
          this.currentType = 'new'
          break
        case 2:
          this.currentType= 'sell'
          break
      }
      this.$refs.tabControl1.currentIndex = index
      this.$refs.tabControl2.currentIndex = index
    },
```

## 14.6 保留home组件离开时的状态和位置
路由组件会自动 destroyed，要用 keep-alive ,让组件不自动销毁
离开home组件时，保存一个位置信息 saveY，再进来时，将位置设置为原来保存的位置信息即可
利用生命周期函数 activated(){} 和函数 deactivated(){}

- 进入是触发 activated()，退出时触发 deactivated()
- 页面第一次进入，钩子的出发顺序是 created -> mounted -> activated，退出时触发deactivated
- 当再次进入时，只触发 activated

```javascript
activated () {
    // 进入时滚动到离开时的位置this.saveY
    this.$refs.scroll.scrollTo(0, this.saveY, 0)
    this.$refs.scroll.refresh() // 进入时再刷新，避免出现小问题
  },
  deactivated () {
    // 保存离开时的位置信息到this.saveY
    this.saveY = this.$refs.scroll.getScrollY()
    // console.log(this.saveY);
    // 2.取消全局事件监听（主页图片加载的监听）因为此时设置了keep-alive，所以离开时调用的是deactivated()
    this.$bus.$off('itemImageLoad',this.itemImgListener)
  },
```



## 14.7 home组件跳转至详情页

1. 是 GoodsListItem 中的点击导致跳转到详情页，详情页为路由组件
2. 跳转详情页时，要将 item的 id 携带过去，才能根据 id 请求相应的数据
3. 动态绑定id，`path：'/detail/:id'`
4. 再在 item 组件中设置点击事件，改变路径：

```javascript
itemClick () {
      this.$router.push('/detail/'+this.goodsItem.iid)
    }
```

5. 在 detail 组件中得到 id

```javascript
 created () {
    // 1.保存传入的id
    this.iid = this.$route.params.iid
    // 2.根据id请求数据
    getDetail(this.iid).then(res => {
```

**利用 $ router 改变路径，利用 $route 可获得当前路由的相关信息 params**



## 14.8 详情页detail

1. **轮播图**

由于之前将所有的路由组件 `keep-alive` ,所以此时点每个 goodsitem 都是同样的轮播图，所以要用 `exclude="Detail"`取消对 detail 路由组件的 keep-alive

```javascript
<keep-alive exclude="Detail">
      <router-view/>
    </keep-alive>
```

2. **网络请求数据杂乱**

数据多，且乱，可在传递给组件时先将数据封装为一个对象，再将那一个对象传给组件

```javascript
export class GoodsParam {
    constructor(info,rule) {
        // images可能没有值，所以要判断一下
      this.image = info.images ? info.images[0] : '';
      this.infos = info.set;
      this.sizes = rule.tables
    }
}
```

在 detail.js 中封装为一个类，**面向对象封装**，就能将三个地方的信息整合在这个类（构造函数）里面，实例就能得到这些数据

3. **展示数据**

1. 展示数组中的多个图片，用一个img标签遍历
2. 展示的图片较多时，并且是垂直排列的，就会影响 better-scroll 的滚动，就想要将所有图片全部加载完成后再滚动，

```javascript
methods: {
    imgLoad () {
    //   this.counter +=1
    //   判断，等所有的图片都加载完后，只用进行一次的事件发送
    // 类似于防抖函数，防抖函数在detail组件中接收时应用，
    // 但由于detail组件已经做了混入，混入中也有防抖函数，所以可以将防抖函数保存在data中，就可以在detail组件中任意位置都可用了
      if(++this.counter === this.imagesLengeth){
          this.$emit('imgLoad')
      }
    }
```

**在watch配置中监视图片的length**

```javascript
watch: {
    detailInfo () {
      this.imagesLengeth = this.detailInfo.detailImage[0].list.length
    }
  }
```

3. 得到数据的键名，之后才开始

```javascript
<template>
    <div v-if="Object.keys(detailInfo).length !== 0 && detailInfo.detailImage[0].list !== null">
```

4. v-for 还可以遍历数字 ，但是展示的数字是从1开始的，所以要取到下标还得用 `index-1`

```javascript
<span class="info-service-item" v-for="index in goods.services.length-1"
            :key="index">
                <img :src="goods.services[index-1].icon" alt="">
                <span>{{goods.services[index-1].name}}</span>
```

**时间数据**

服务器返回的时间节点，一定是时间戳（秒），需要用 formatDate 函数转变时间戳为格式化的时间字符串

**监听goodListItem 组件在不同页面展示的图片的加载**

利用路由： if判断

- this.$route.path.indexOf('/home') 找出路径中有/home的路由组件，发送监听 homeImageLoad
- this.$route.path.indexOf('/detail') 对路径中有/detail的路由组件，发送监听detailImageLoad

```javascript
methods: {
    imageLoad () {
      // 1. 利用路由的路径判断发送哪个事件
       if(this.$route.indexOf('/home')){
         this.$bus.$emit('homeImageLoad')
       }else if(this.$route.indexOf('/detail')){
         this.$bus.$emit('detailImageLoad')
       }
    },
```

**离开组件时取消监听**

在 goodListItem 里面仍然只发送一个 imageLoad 图片加载事件

```javascript
methods: {
    imageLoad () {
      this.$bus.$emit("itemImageLoad")
    },
```

1. 在 home 组件中的生命周期函数 deactivated() 离开组件时触发的函数中做出设置：取消全局事件监听 `this.$bus.$off`

```js
deactivated () {
    // 保存离开时的位置信息到this.saveY
    this.saveY = this.$refs.scroll.getScrollY()
    // console.log(this.saveY);
    // 2.取消全局事件监听（主页图片加载的监听）因为此时设置了keep-alive，所以离开时调用的是deactivated()
    this.$bus.$off('itemImageLoad',this.itemImgListener)
  },
```

this.$bus.$off如果只传一个参数，意味着所有组件中这个事件监听都将被取消，利用第二个参数：函数，指定取消的位置，这个函数就是监听这个事件的函数，即mounted()中this.b u s . bus.bus.on后面跟的函数，要拿到这个函数就要将其保存到data里

```javascript
mounted () {
    // 可利用混入，减少重复代码
    // 1.监听item中图片加载完成，刷新防抖函数放到了utils.js中，工具库
     const refresh = debounce(this.$refs.scroll.refresh,50)
      // 对监听的事件进行保存，方便离开home组件时取消此事件监听
       this.itemImgListener = () => { refresh() }
       this.$bus.$on('itemImageLoad',() => {
       // this.$refs.scroll.refresh()
       refresh() // 此处是闭包，局部变量不会被销毁
     })
       this.$bus.$on('itemImageLoad', this.itemImgListener)
  },
```

2. 再在Detail组件中设置 imageLoad 图片加载事件的监听

```javascript
 mounted () {
    // 利用mixin混入后，代替了下面的代码
       const refresh = debounce(this.$refs.scroll.refresh,50)
       this.itemImgListener = () => { refresh() }
      //  在此组件中监听itemImageLoad图片加载事件
       this.$bus.$on('itemImageLoad', this.itemImgListener)
```

没有对 Detail 组件keep-alive，所以在离开组件时取消图片加载事件的监听，要用 destroyed() 生命周期函数取消事件

```javascript
destroyed () {
    // 离开组件时取消图片加载监听事件
    this.$bus.$off('itemImageLoad', this.itemImgListener)
  },
```



## 14.9 点击加入购物车

**加入购物车要携带商品的 iid，所以需要使用 `vuex` 来集中管理数据的变化**

1. 在detail组件中获取到需要展示的商品的信息，封装到对象product中

```js
addToCart () {
      // 1.获取购物车需要展示的信息，因为有多个信息，所以可以放在一个对象里
      const product = {}
      product.image = this.topImages[0]
      product.title = this.goods.title
      product.desc = this.goods.desc
      product.price = this.goods.realPrice
      product.iid = this.iid // id一定要传，因为id是商品的唯一标识，是将id传给服务器获取到对应的商品
```

2. 再将数据传递到store中

- 不能直接改变state中的数据this.$store.state.cartList.push(product)，因为改变 state 中的数据最好使用 mutations 或 actions 方法

```js
addToCart () {
      // 1.获取购物车需要展示的信息，因为有多个信息，所以可以放在一个对象里
      const product = {}
      product.image = this.topImages[0]
      product.title = this.goods.title
      product.desc = this.goods.desc
      product.price = this.goods.realPrice
      product.iid = this.iid // id一定要传，因为id是商品的唯一标识，是将id传给服务器获取到对应的商品

      // 2.将商品添加到购物车里
       this.$store.commit('addCart',product) //commit是将product提交到store中mutations里的方法 addCart
```

3. 在 store 中的 addCart 方法中接收数据，并将数据添加到 state 的常量中去

```js
addCart(state,payload){
        let oldProduct = state.cartList.find(item => item.iid === payload.iid)
        if(oldProduct){
             oldProduct.count += 1 
        }else{
            payload.count =1
             state.cartList.push(payload)
        }
    }
```

**添加购物车成功的弹窗 toast**

- 添加成功 --> 弹出提示
- 在vuex里做了某个操作，想让外面的组件监听这一操作是否成功，就要用到 `promise`
- vuex 中的 actions 中的方法函数可以返回 promise 对象

```js
addCart(context,payload){ // dispatch可以返回一个promise，可用来监听事件成功与否，据此做弹窗toast效果
      return new Promise((resolve,reject) => {
        let oldProduct = context.state.cartList.find(item => item.iid === payload.iid)
        if(oldProduct){
            // oldProduct.count += 1 这样写也可以，但是无法在devtools中监视到状态变化
            // 将数量加1的情况分发到一个特定的方法中
            context.commit(ADD_COUNTER,oldProduct) // commit 提交到store中mutations里的方法addCounter中去
            resolve('当前商品数量+1')
            reject('wrong')
        }else{
            payload.count =1
            // context.state.cartList.push(payload)
            // 将添加商品的情况分发到另一个特定的方法中，使得mutations中的方法只对应一种改变
            context.commit(ADD_TO_CART,payload)
            resolve('添加新的商品')
        }
      })
    }
```

**将 Toast 组件封装为插件**

1. toast文件夹下新建 index.js

```js
const obj = {}
export default obj
```

2. 在 main.js 中导入

```js
import toast from 'components/common/toast' // 1.引入插件
// 2.安装插件,就相当于调用了toast的install函数方法
Vue.use(toast) 
```

3. 返回 index.js 中配置插件

```js
import Toast from './Toast' // 将toast组件导入进来，好添加组件中的元素
const obj = {}
obj.install = function(Vue){
    // 1.创建组件构造器
    const toastConstructor = Vue.extend(Toast)
    // 2.用new的方式，根据组件构造器，可以创建一个组件对象
    const toast = new toastConstructor()
    // 3.将组件对象手动地挂载到某一个元素上
    toast.$mount(document.createElement('div')) // 将toast组件对象挂载到div上
    // 4.挂载完之后，toast.$el对应的就是div
    document.body.appendChild(toast.$el)
    Vue.prototype.$toast = toast // 将toast组件对象放在vue原型上，使得其他任意组件都可使用$toast方法使用toast组件对象
}
export default obj
```



## 14.10 cart 组件数据展示

**navbar上商品的数量**

- 可将实时展示的购物车的数量用 vuex 的 `getters` 做封装，再引入vuex 的 `mapGetters` 做解构引入

```javascript
computed: {
    //   1.普通写法
    // cartLength () {
    // //   return this.$store.state.cartLish.length  将方法封装到getters之前的写法
    //   return this.$store.getters.cartLength   封装到getters之后的写法
    // }
    // 2.利用mapGetters解构 ， 还可用 mapState , mapActions
    ...mapGetters(['cartLength'])
  }
```

```js
// getters.js 中
export default {
    cartLength (state) {
       return state.cartList.length
    },
    cartList (state) {
        return state.cartList
    }
}
```

**购物车商品的展示**

- 刷新界面

由于新加入的商品可能未被 better-scroll 所获取，所以导致不能滚动，可以在进入购物车时就做一次刷新

```js
activated () {
    // console.log('0000');
    this.$refs.scroll.refresh()
  }
```

- 商品的选中状态

**商品状态要保存到 state 中**

记录商品的选中状态，不能用属性记录，要在商品对应的对象模型里记录，之后修改也是修改对象模型里的某个属性来进行修改
对象模型即 cartList[商品1，商品2] 中的商品模型，为商品模型设置一个 checked 属性，记录它的选中与否

```js
[ADD_TO_CART](state,payload){
        payload.checked = true // 购物车中商品是否选中的属性，默认添加即选中
        state.cartList.push(payload)
      }

```

```js
<cart-list-item  v-for="(item,index) in cartList" 
            :key="index" 
            :item-info='item'>
</cart-list-item>
computed: {
    ...mapGetters(['cartList'])
  },
```

- 因为模型发生改变，界面才发生改变，在 mutations 中添加payload.check=true，相当于对 cartList 中的商品对象 itemInfo 添加了一个属性 check

```js
methods: {
    checkClick () {
      this.itemInfo.checked = !this.itemInfo.checked
    } 
  }
```

**底部信息汇总**

- 计算总价

通过 `filter` 选出被选中的商品，再通过 `reduce` 计算总额

```js
computed: {
    ...mapGetters(['cartList']),
    totalPrice () {
    //   return '￥' + this.$store.state.cartList.filter(item => {
      return '￥' + this.cartList.filter(item => {  // mapGetters结构之后这样写
          return item.checked}).reduce((preValue,item) => {
              return preValue + item.price * item.count
          },0).toFixed(2) // toFixed(2) 计算结果保留2位小数
    },
```

**设置全选按钮**

- 初始状态：购物车没有商品时默认不选中

```js
isSelectAll () {
      if(this.cartList.length === 0) return false  // 购物车中没有商品时，默认不选中
```

- 只要有一个未被选中，则不选中全选按钮，用 `find` 性能最高

```js
isSelectAll () {
        // 1. filter会将数组全部遍历完
        // if(this.cartList.length === 0) return false
    //   return !(this.cartList.filter(item => !item.checked).length) // 对未被选中的商品长度进行取反，0取反为true
    // 2.简单遍历，也会全部遍历完
    // for(let item of this.cartList){
    //     if(!item.checked){ // 没有选中的情况为真
    //             return false
    //         }
    //     }
    //         return true
    // 3. find 只找到一个就不找了，性能会高一点
      if(this.cartList.length === 0) return false  // 购物车中没有商品时，默认不选中
      return !(this.cartList.find(item => !item.checked)) // (括号里面有值的情况下再取反，结果就为false)
    }
```



## 14.11 fastclick

解决移动端300ms延迟

```js
import FastClick from 'fastclick'
// 解决移动端300ms延迟
FastClick.attach(document.body)
```



## 14.12 图片懒加载

图片需要显示在屏幕时再加载

安装：

```shell
npm i vue-lazyload -s
```

```js
import VueLazyLoad from 'vue-lazyload'
Vue.use(VueLazyLoad, {
  loading: require('./assets/img/common/placeholder.png')
})
```

**使用**

```js
<img v-lazy="showImage" alt="" @load="imageLoad">
```



## 14.13 服务器

服务器也是一台电脑（没有显示器，只有主机），24小时开着，为用户提供服务
大部分公司都没有自己的服务器主机，因为自己买主机还要建一个机房，还要考虑散热问题，还有维护主机的成本，所以一般都会租借阿里云、华为云、腾讯云的服务器
服务器、主机就必须要有一个操作系统，window(.net)/linux ->安装tomcat/nginx(服务器上的软件、服务器) [nginx可有效处理并发项目、反向代理]
build打包之后有一个dist文件夹，将dist文件夹放到服务器里就行

**部署nginx**
mainline version 主力做的版本，开发版
stable version 最新稳定版，生产环境上建议使用的版本
legacy version 遗留的老版本的稳定版

将自己的电脑变成一台服务器，在window上面部署nginx
**下载nginx**
将打包生成的dist文件夹放入nginx文件夹下就可通过localhost访问
远程部署，需要安装一些软件再操作
linux ubantu操作系统，就可通过图形化界面的方式来进行管理
linux centos操作系统，稳定性更强
在远程主机上通过linux centos 来部署nginx，
安装：通过终端命令来安装nginx