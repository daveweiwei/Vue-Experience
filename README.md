# Vue 学习笔记
## author：yangdawei
## 简介
Vue.js 是一套构建用户界面的渐进式框架(根据需求来选择Vue.js的技术栈)，只关注视图层，响应式数据驱动，采用声明式的将数据渲染进DOM。
## vue技术栈
vue.js   vue-router路由     vuex大规模状态管理 vue-cli快速构建vue项目
## 安装vue
1.  通过传统的页面引用方式：
```
<script src="https://unpkg.com/vue"></script>
```
2.  通过npm包管理器方式：
```
1. 首先安装node环境，同时安装npm包管理器
2. npm init 来初始化项目进行包管理
3. npm install vue --save 把vue安装在本地项目中
```

## 使用Vue

### 声明 Vue实例
```
new Vue({
	option......
}) 
script标签 / .js 文件中通过这个方式来声明一个vue实例，构建vue应用
```
### option选项
option选项是传入给vue构造函数的特性。定义了vue能做的事情。

1. el: 把Vue实例与vue整个应用根节点绑定，所有的组件或模板都是根节点的子组件。
```
	el:'#app' //其值是CSS选择器
```
2. data: Vue 实例的数据对象。在对象中定义的属性值是响应数据变化。
```
	Vue实例中data是一个对象，-> data:{}
	compoents组件中data是一个函数，return {} -> data(){return {}}
```
3. methods:用来定义方法的，是个对象类型。
```
	methods:{}
```
4. computed: 需要根据其它数据计算才得到结果
```
	computed:{}
```
5. watch:监控数据的变化，当有一些数据需要随着其它数据变动而变动时。
```
	watch:{}
```
6. components:用来定义组件。
```
	components:{}
```
7. template：定义模板。
```
	template:'<span></span>' 模板中必须包含唯一个roo根节点
```
8. filters：定义过滤规则函数。在模板中给要过滤的值用 |过滤函数。
```
	filters:{}
```
9. created：
生命周期钩子，当vue实例创建完之后可以调用，在这种情况下可以进行AjAx请求
```
	created:{}
```
10. mounted：
生命周期钩子，当vue实例挂在到el定义的模板上调用，这种情况可以获取真实DOM
获取真实DOM的方式：给模板上添加ref属性，通过this.$refs.添加的属性值就可以获取。
ref如果写在dom 表示获取dom元素 如果写在组件上，表示是当前组件的实例
```
	mounted:{}
```
11. directives：自定义指令。
```
	directives:{}
```

## 声明式渲染
Vue采用声明式方式把数据渲染到DOM上，无需关心如何实现，通过给HTML元素添加属性式的指令的方式，指令以v-开头。

### 插值
{{variable}} 
往元素之间添加内容可以使用插值的方法，{{}}两个嵌套的大括号包含着一个在data option里定义的varible，就可以把数据动态的渲染到元素之间。
```
<div id="app">
  {{ message }}
</div>

var app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue!'
  }
})

结果：Hello Vue!
```
### 指令
一个定义在HTML元素上的属性
1. v-text:  (一般不用)
给元素之间添加内容，跟{{插值}}是一样的效果，值是字符串类型，如果字符串中包含HTML元素，那么在页面中也会转换成字符串原封不动的渲染到页面中
可以防止插值的闪烁问题
```
<div v-text="html"></div>
data:{
	html:'<span>hello</span>'
}
结果：<span>hello</span>
```
2.  v-html：(因安全，不应用此指令来写模板结构，对用户输入也不应使用此指令)
与v-text是一样的，给元素之间添加内容，不同点是可以把字符串中的HTML转换成真实的DOM节点插到DMO树中。
```
<div v-text="html"></div>
data:{
	html:'<span>hello</span>'
}
结果：hello
```
3.  v-show 
条件渲染DOM，根据表达式的真假来切换元素的display:none属性，该元素会真实的渲染到DOM中，如果频繁的操作DOM就要使用该指令。
4. v-if 
条件渲染DOM，跟v-show一样，都是根据条件来渲染DOM，但使用该指令的元素如果条件不为true，不会真实的渲染到DOM中，同时，还有v-else是可以跟v-if来搭配使用，效果跟if..else语句一样，当不满足条件的时候渲染v-else的内容。2.0新增v-else-if，当使用v-else是要经跟在v-if之后否则无效。
v-if与v-for同时使用时，v-for的优先级要高于v-if，也就是先循环然后根据条件来渲染。
5.  v-for 
迭代数据对象来渲染元素或者模板，迭代的数据对象为数组或者是对象
```
迭代数组：
let array = [1,2,3,4,5]
v-for="val in array"，v-for="(val,index) in array"  index是索引
迭代对象：
v-for="(val,key) in array" key是键值
```
`如果循环组件不加key属性 默认会有警告提示，v-for和v-if一起使用，优先级高于v-if`
6. v-on 
绑定事件 简写方式@ ，指令用于给元素绑定事件，参数是定义的函数名，函数要定义在methods选项中，函数内部的this永远指当前vue实例，如果传递事件源需要给函数传入$event。data中的名字和method中不能相同。
7. v-bind 
指令用于给HTML属性动态绑定一个或多个值，或一个父组件通过 props传递的值，props传递的值也是动态绑定到一个属性上的。当要给属性是动态绑定值时要用这个指令。否则属性上的值是一个字符串。
在绑定 class 或 style 时，其值可以是数组或对象。在数组中定义的类都会生效，对象中定义的类可以根据条件来生效，除了可以用:class来动态绑定，同时还可以跟原生class一起使用，这两者的样式根据CSS样式权重的优先级来显示。
简写方式 : 冒号
```
<span :class="{red:tred}">span</span>
data:{
	tred:true
}
当tred为真时class的值为red
```
8. v-model
双向数据绑定，给表单控件绑定值，其相当于是给表单控件的value属性绑值。
```
<input type="text" v-model="num">
data:{
	num:1
}
而这个绑定是双向绑定的，当值改变data属性里的值会受影响，相反data属性里的值改变v-model里绑定的值也会受到影响。
```
9.  v-cloak
解决整个模板中因使用插值导致的多次刷新页面出现的闪烁问题
要与CSS搭配使用如下：
```
[v-cloak] {
  display: none;
}
<div v-cloak>
  {{ message }}
</div>
```
10. v-once
数据只绑定一次,当数据在更改时不会更新内容

### 修饰符
传统原生js中事件会产生事件传播，还有些特定的事件，比如键盘事件和鼠标事件。
vue中定义了修饰符来描述这些特性，这样可以搭配事件来达到一些效果。
修饰符可以连续调用。
```
<button @keyup.enter="add">按回车</button> 
enter就是回车的修饰符，通过.点修饰符方式来使用 
这样当按下了回车就会执行add函数
```
`
stop 调用的是event.stopPropagation 阻止事件传播
prevent 调用的是event.preventDefault 阻止默认行为
`
常用的修饰符：
keyup.enter
.stop  代表阻止冒泡
.capture 表示事件捕获 先捕获 -> 在到目标 -> 在到冒泡
.self 表示只在自己身上触发
.prevent 阻止默认事件
.键盘事件  修饰符可以连续调用
### 计算属性computed
定义时是函数，使用时是变量
特点：
当需要根据其它数据来计算才能得到的结果，就要使用computed，数据是双向的。data中的数据发生改变，computed的结果也会受到影响。
computed不支持异步，定义的函数不能用箭头函数，否则this 将不会按照期望指向 Vue 实例。
原理：
计算属性由get方法和set方法组成，默认调用的是get方法。
### watch 监控
监控data中的定义的属性，当数据变化时，就干一些事情 支持异步 因为没有返回值
定义在watch对象中，监控的函数名要跟被监控的data中的属性同名才行。
### 组件 Component
- 每个对象就是一个组件，因为compoents的参数就是一个对象。
- 组件中可以使用所有Vue实例 option选项 
- 唯一不同的是，data选项在组件中是一个函数，然后return {}

### 组件化开发
- 将一个页面分割成若干个部分，一个页面js+css+html,将自己的内容分割出来，
方便开发，更好维护代码。
- 每个组件封装自己的js+css+html，为每个组件定义自己的样式给style添加scoped属性
- 组件分类：页面级组件（页面） 基础组件（页面的一部分）
- 组件的目的为了实现复用

### 创建组件
1. 全局组件  不需要引用 一般都是插件(工作上不会用到)
2. 局部组件

创建组件的流程：
1. 定义组件 并引入到当前页面
```
	全局组件：
	Vue.component('todo-item', {
	  template: '<li>这是个待办项</li>'
	})
	局部组件：
	new Vue({
		components:{
			'todo-item':{
				template: '<li>这是个待办项</li>'
			}
		}
	})
	-----
	<template id="banana">
	    <div>{{name}}</div>
	</template>
	let panel = { //定义一个panel组件
        template:'#banana', //引入模板id叫panel的组件
    };
    let vm = new Vue({
        el:'#app',
        components:{ //注册组件
            panel
        }
    })
```
2. 注册组件  全局组件不用注册
3. 使用组件  把组件引入到Html中使用。

`每个组件都是独立的，this都是指向当前组件实例。`

### .vue文件
开发中更多的是使用.vue文件模块，包含三部分：
```
<template></template>  声明模板
<script>
	exprot defualt {}  逻辑代码
</script>    
<style scoped></style>  当前组件的样式
```

### 组件之间的通信(数据传递)
组件之间的通信 1.父与子 2.子与父  3.兄弟之间
通信的方法：
- 默认组件是独立的相互不能引用数据。
- 给子组件绑定属性来传递值，子组件通过props接收父组件传递的值，
- props还可以校验数据的类型，如果是校验数据其是个对象。
```
//这里会将show挂载在当前实例上
props:{ 
     show:{
	     type:Boolean, //js类型
	     required: true, //必须要填写内容
	     default:true //默认值 这个属性不能和required同时使用
	 }
},
```
```
//父亲-> 儿子 默认组件是独立的相互不能引用数据，可以通过属性的方式传递给儿子
let vm = new Vue({
    el:'#app',
    data:{money:100},//根实例数据都是对象，组件中都是函数
    template:'<child :m="money" o="美女"/>', // 根实例上的父组件引用了child子组件
    components:{
       child:{//定义子组件
    //如果传递多个可以数组中写多个
    //这个m会挂载在儿子的实例上，相当于挂在了data上,父亲的数据儿子不能更改
         props:['m','o'],
         computed:{
            b(){ return '大'+this.o} //this当前的是当前组件的实例
         },
         //子组件模板并把父组件传递过来的值插进去
            template:'<div>child {{m}} {{b}}</div>'
        }
     }
 })
```
- 父组件拿子组件的属性，通过的就是发布订阅，
- 父组件声明一个方法，子组件触发父亲的方法$emit()
```
let vm = new Vue({
  el:'#app',
  //在子组件上绑定了一个自定义事件child来出发父组件中定义的方法say，收到了子组件传递的值
  template:'<div>父 <child @child="say"></child></div>',
  methods:{
      say(data){
          console.log(data);  //打印子组件传的参数
      }
  },
 components:{
     child:{
         created(){
         //子组件通过$emit触发自定义的事件，并把data定义的值传递给父组件
                    this.$emit('child',this.msg);
	     },
	     data(){
           return {msg:'我饿了'}
        },
	     template:'<div>子</div>'
     }
   }
})
子组件不能直接该父组件传递过来的值，要想改变父组件的值，那么就要子组件改变了自己的值之后，把这个修改过后的值，当做$emit的参数传递给父组件，在通知父组件自己改。父更改后，属性会重新传递，子会刷新。

子组件传递给父组件值简单的方式通过sync：
//msg是父组件传递给子组件的值，用sync来监控属性a的变化，一旦变化就反向就更新msg
 template:'<div>父{{msg}}<child :a.sync="msg"/></div>',
 data:{
    msg:'美女'
 },
 child:{
   props:['a'],
   template:'<div>child {{a}} <button @click="change">换</button></div>',
   methods:{
      change(){ //固定的写法，update来更新父组件传递过来的值
           this.$emit('update:a','丑女');
      }
   }
}

```
- 平级交互 eventBus 但是不用 -> vuex
- 父组件调用子类方法 ref=> this.$refs.xxx.子类方法

### 路由

```
<div id="app">
    <!--路由导航 默认是a标签，可以通过tag改变默认标签-->
    <router-link to="/list" tag="span">列表</router-link>
    <!--使用了别名的路由，还传递了参数通过params-->
    <router-link v-for="article in articles" :to="{name:'zfpx',params:{id:article.id}}" tag="div">
        {{article.title}}</router-link>
    <!--4.路由渲染的位置-->
    <router-view></router-view>
    <div slot="sl">我是一个slot</div>
</div>
```
```
<!--定义模板 必须有一个唯一的root根元素-->
<template id="home">
    <div>首页
        <slot name="sl">我是一个默认值</slot>
    </div>
    <!--
     slot插槽就是一个占位符，可以通过name来指定把root下的带有slot属性的元素替换到当前位置
     slot中写的内容是默认值，如果root下的元素没有内容，就显示slot的内容
    -->
</template>
<template id="list">
    <ol><li v-for="(films,index) in Film" :key="index">{{films.title}}</li></ol>
</template>
```
```
 /*
    * 路由使用的三部曲
    * 1.创建一个VueRouter实例，定义路由 默认路由就是hash规格的 (mode:'history'改变规格H5方式，一般不用)
    * 2.配置路由映射表 列表变量就叫routes，不能叫别的名字
    * 3.vue实例中注册router实例 将router引入到实例中 会在实例上提供两个属性 this.$route(属性) this.$router(方法)
    * $route属性  可以获取定义在router实例上的属性和值，通常用在动态获取URL传参
    * $router方法 可以使用history.push增加一个历史记录，history.go表示前进和后退 去到第几个历史页面，
    * history.replace是将当前的历史管理替换掉
    * 4.router-view 路由渲染位置
    * 5.(可选)router-link 路由的方式定义一个导航，必须要有to属性 to是定义跳转的路径
    * */
    //定义组件
    let Home = {
        template: '#home'
    }
    let List={ //每个{}对象可以看作是一个组件
        template:'#list',
        data(){ //组件中定义自身的数据，data必须是一个函数，return{}一个对象包含着初始数据
            return{
                Film:[
                    {"title":"进击的巨人"},
                    {"title":"侠盗车手"}
                ]
            }
        }
    }
    //2.配置路由映射表 列表变量就叫routes，不能叫别的名字
    const routes = [
        {path: '/home', component: Home},
        //路径不能写死 应该是可变的 :id必须传递参数 但是内容是可以随机的
        //会将当前冒号后的值 和真实传递的值组成一个对象 this.$route.params = {id:123,name:123} this是当前模板实例
        {path: '/list/:id/:name', component: List,name:'zf'}//name是给这条路由起了一个别名，这样在to中用
        {path:'*',redirect:'/article/1'} //一般用作404 会导致路径改变，redirect重定向
    ]

    //1.创建一个VueRouter实例，定义路由
    const router = new VueRouter({
        routes
    })
    new Vue({
        el: '#app',
        router //在实例定义router选项，如果创建路由时用的变量时router，那么在这里可以简写。router
    })
```