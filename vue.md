# 1.初始化渲染步骤

```html
<div id = "app">
    // 1. 准备容器
    {{ msg }} //插值表达式 双括号中只能写表达式 且不能在标签属性中用
</div>

// 2. 引包
<script src="https://cdn.jsdelivr.net/npm/vue@2.7.16/dist/vue.js"></script>

<script>
	// 3. 创建vue实例
    const app = new Vue({
        el:'#app',
        data:{
            msg:'Hello World'
        }
    })
    
    // 4. 访问数据 实例.属性名
    console.log(app.msg)
    
    // 5. 修改数据 实例.属性名 = 值
    app.msg = 'Hello'
</script>
```

# 2.vue指令

```html
<div id="app">
    // 1. v-html = "表达式" 设置元素的innerHTML(可解析标签)
    <div v-html="msg"></div>
    
    // 2. v-show = "表达式" 表达式值true显示，false隐藏
    // 原理：切换display:none控制显示隐藏 适合频繁切换显示隐藏的场景
    <div v-show="flag">v-show</div>
    
    // 3. v-if = "表达式" 表达式值true显示，false隐藏
    // 原理：基于条件判断，是否创建或移除元素节点 适合不频繁切换的场景
    <div v-if="flag">v-if</div>
    
    // 4. v-else 配合v-if使用
    <div v-if="gender === 1">性别：男</div>
    <div v-else>性别：女</div>
    
    // 5. v-else-if = "表达式" 配合v-if使用
    <div v-if="score >= 90">成绩：A</div>
    <div v-else-if="score >= 80">成绩：B</div>
    <div v-else-if="score >= 70">成绩：C</div>
    <div v-else>成绩：D</div>
    
    // 6. v-on:事件名 = "函数名" | @事件名 = "函数名" 注册事件
    <button v-on:click="f1">按钮</button>
    <button @click="f2(5)">按钮</button>
    <button @click="num++">{{num}}</button>//函数可换为内联语句
    
    // 7. v-bind:属性名="表达式" 动态设置html的标签属性
    <img v-bind:src="url">
    <img :src="url"> // 简写 :属性名="表达式"
    <img :class="{类名1:布尔值,类名2:布尔值}"> // 适合频繁切换的类
    <img :class="[类名1,类名2,类名3]"> // 适合批量添加删除的类
    // 适合动态操纵单个属性
    <img :style="{css属性名1:css属性值,css属性名1:css属性值}">
    
    // 8. v-for="(item,index) in 数组" 基于数据循环渲染元素
    <p v-for="(item,index) in fruit">{{item.name}}</p>
    <p v-for="item in fruit">{{item.name}}</p> // index可省略
    <p v-for="(item,index) in fruit" :key="item.id">
        // key给每项添加唯一标识，便于Vue进行正确排序复用删除
        // key的值只能是字符串或数字类型,且具有唯一性
        {{item.name}} 
    </p>
    
    // 9. v-model='变量' 双向数据绑定，快速获取或设置表单元素内容
    账户：<input type="text" v-model="username">
    密码：<input type="password" v-model="password">
</div>

<script>
    const app = new Vue({
        el:'#app',
        data:{
            msg:'<a>Hello World</a>'
            flag: false,
            gender: 1,
            score: 80,
            num: 0,
            url:'图片路径',
            fruit:[
            	{id:1, name:'芒果'},
        		{id:2, name:'西瓜'}
        		{id:3, name:'百香果'}
            ],
            username:'',
            password:''
        },
        methods:{ // 其中的函数this均指向Vue实例
            f1(){
                console.log('点击事件')
            },
            f2(x){
                let count += x
                console.log(count)
            }
        }
    })
</script>
```

# 3.指令修饰符

```html
<div>
    // 1. @keyup.enter 键盘回车监听
    
    // 2. v-model.trim 去除首尾空格
    
    // 3. v-model.number 转数字
    
    // 4. @事件名.stop 阻止冒泡
    
    // 5. @事件名.prevent 阻止默认行为
</div>
```

# 4.计算属性

简写：

```html
<div id="app">
    // 数据修改后会自动重新计算
    <p>总价：{{ totalCount }}</p>
</div>
<script>
	const app = new Vue({
        el:'#app',
        data:{
            list:[
                {id:1,name:'芒果',num:1},
                {id:1,name:'西瓜',num:2},
                {id:1,name:'草莓',num:3}
            ]
        },
        computed:{
            totalCount () {
                let total = this.list.reduce((sum,item) => sum + item.num,0)
                return total
            }
        }
    })
</script>
```

完整写法：

```html
<script>
	const app = new Vue({
        el:'#app',
        computed:{
            计算属性名:{
                get(){
                    代码逻辑
                    return 结果
                },
                set(修改的值){
                    修改逻辑
                }
            }
        }
    })
</script>
```

# 5.监听器

监听的数据变化时，执行对应方法

```html
<script>
	const app = new Vue({
        el:'#app',
        data:{
            word:'hello',
            obj:{
                word:'fruit',
                type:'芒果'
            }
        },
        watch:{
            // 简单数据类型直接用数据名作方法名 
            word(newValue,oldValue){
                业务逻辑
            },
            'obj.word'(newValue){ // oldValue可省略
                业务逻辑
            }
            // 完整写法 添加额外配置项
            obj:{
            	deep:true, // 监视复杂类型中的所有数据
            	immediate: true, // 初始化时立刻执行一次方法
            	handler(newValue){
        			console.log(newValue)
    			}
        	}
        }
    })
</script>
```

# 6.生命周期

一个Vue实例从创建到销毁的整个过程

四个阶段：
① 创建 将普通数据转换为响应式数据  // 发送初始化渲染请求在创建阶段最后
② 挂载 渲染模版  // 操作dom在挂载结束之后
③ 更新 监听数据修改，更新视图（循环执行多次）
④ 销毁 关闭页面时销毁实例

生命周期函数（钩子函数）：

```html
<div id="app">
    
</div>
<script>
	const app = new Vue({
        el:'#app',
        data:{
            
        },
        // 1. 创建阶段（准备数据）
        beforeCreate(){
            console.log('beforeCreate')
        },
        created(){
            console.log('created')
        },
        // 2. 挂载阶段（渲染模版）
        beforeMount(){
            console.log('beforeMount')
        },
        mounted(){
            console.log('mounted')
        },
        // 3. 更新阶段
        beforeUpdate(){
        	console.log('beforeUpdate')
    	},
        updated(){
            console.log('updated')
        },
        // 4. 销毁阶段
        beforeDestroy(){
            console.log('beforeDestroy')
        },
        destroyed(){
            console.log('destroyed')
        }
    })
</script>
```

# 7.柄图渲染

echarts官网查询

# 8.工程化开发

## 步骤：

①  全局安装 (一次) ：npm i @vue/cli -g
②  查看 Vue 版本：vue --version
③  创建项目架子：vue create project-name（项目名不能用中文）
④  启动项目：npm run serve（找package.json）

## 局部注册：

①  创建.vue文件
②  在使用的组件内导入注册

```html
<script>
	// 导入
    import 组件名 from '文件路径'
    // 注册
    export default {
        // 局部注册
        components:{
            '组件名':组件对象 
            // 把组件名当html标签使用 <组件名></组件名>
        }
    }
</script>
```

## 全局注册：

①  创建.vue文件
②  在main.js中进行全局注册

```html
<script>
	// 导入
    import 组件名 from '文件路径'
    // 调用 Vue.component 进行全局注册
    Vue.component('组件名',组件对象) 
    // 把组件名当html标签使用 <组件名></组件名>
</script>
```

## 组件样式冲突:

给组件 style 加上 scoped 属性, 可以让样式只作用于当前组件 
`<style scoped></style>`

## data:

必须写成函数形式，保证每个组件实例维护独立的数据对象

```html
<script>
	export default {
        data () {
            return {
                count: '',
                ...
            }
        }
    }
</script>
```

## 父子通信：

利用prop和$emit传值

父传子:

```html
父：
<template>
    <div>
        // 1. 给组件添加动态属性传值
        <son :title="myTitle"></son>
    </div>
</template>

<script>
    import Son from './components/Son.vue'
	export default {
        data () {
            return {
                myTitle: '父传子'
            }
        },
        components: {
            Son
        }
    }
</script>

子：
<template>
    <div>
        // 3. 渲染使用
        子组件：{{ title }}
    </div>
</template>

<script>
	export default {
        // 2. 通过props接收数据，与属性名一致
        props:['title']
    }
</script>
```

子传父：

```html
子：
<template>
    <div>
        子组件：{{ title }}
        // 1. 添加事件触发消息传送
        <button @click="change">修改标题</button>
    </div>
</template>

<script>
	export default {
        props:['title'],
        methods:{
            change(){
                // 2. 通过$emit向父组件发送消息
                this.$emit('changeTitle','子传父')
            }
        }
    }
</script>

父：
<template>
    <div>
        // 3. 父组件对消息进行监听
        <son :title="myTitle" @changeTitle="handleChange"></son>
    </div>
</template>

<script>
    import Son from './components/Son.vue'
	export default {
        data () {
            return {
                myTitle: '父传子'
            }
        },
        methods: {
            // 4. 通过参数获得传递的数据
            handleChange(newTitle){
                this.myTitle = newTitle
            }
        }
        components: {
            Son
        }
    }
</script>
```

## prop：

定义：组件上注册的自定义属性

类型校验：

```html
<script>
    // 写成对象形式，以键值对的形式校验类型
	props:{
        属性名1:类型1, //Numbe String Boolean ...
        属性名2:类型2,
    }
</script>
```

非空校验、默认值、自定义校验：

```html
<script>
	// 每个属性写成对象形式，以键值对的形式校验
	props:{
        属性名1:{
            type: 类型, // Number String Boolean ...
            required: true, // 是否必填
            default: 默认值, // 默认值
            validator (value) {
                // 自定义校验逻辑,布尔值判断是否通过校验
                // value可拿到传递的值
            	return true 
            }
        },
        属性名2:{...}
    }
</script>
```

## 非父子通信：

```html
// 1. 新建目录utils，创建文件EventBus.js，创建空Vue实例
<script>
	import Vue from 'vue'
    const Bus = new Vue()
    export default Bus
</script>

// 2. 接收方监听Bus实例事件
<script>
    import Bus from '../utils/EventBus.js'
    export default {
        created(){
            // .$on监听事件，参数为接收的消息
            Bus.$on('sendMsg', (msg) => {
            	console.log(msg)
            })
        }
    }
</script>

// 3. 发送方触发Bus实例事件
<script>
    import Bus from '../utils/EventBus.js'
    export default {
        methods:{
            send(){
                // 事件名要相同
                Bus.$emit('sendMsg','发送的消息')
            }
        }
    }
</script>
```

## 跨层级共享数据：

```html
// 1. 父组件使用 provide 提供数据
<script>
	import SonA from './components/SonA.vue'
    import SonB from './components/SonB.vue'
    export default {
        provide(){
            return {
                name:this.name, // 普通数据类型 非响应式
                obj:this.obj	// 复杂数据类型 响应式
            }
        },
        data(){
            return {
                name:'hm',
                obj:{
                    age:18
                }
            }
        }
    }
</script>

// 2. 子/孙组件使用 inject 取值使用
<tmplate>
    <div>
        {{ name }} - {{ obj.age }}
    </div>
</tmplate>
<script>
    export default {
        inject: ['name','obj']
    }
</script>
```

## .sync修饰符：

本质：属性名 和 @update:属性名 的合写

作用：实现父子通信的数据双向绑定

```html
// 父组件
<tmplate>
    <Son :属性名.sync='值'></Son>
</tmplate>

//子组件
<tmplate>
    <button @click="方法名"></button>
</tmplate>
<script>
    props:['属性名'],
    methods:{
        方法名(){
            this.$emit('update:属性名', 值)
        }
    }
</script>
```

## ref和$refs：

获取dom元素（当前组件内）：

```html
// 1. 给目标标签添加 ref 属性
<div ref="属性名"></div>
// 2. 通过 this.$refs.属性名 获取目标标签
mounted () {
	console.log(this.$refs.属性名)
}
```

获取组件实例：

```html
// 1. 给目标组件添加 ref 属性
<son ref="属性名"></son>
// 2. 通过 this.$refs.属性名 获取目标组件
mounted () {
	this.$refs.属性名.组件方法()
}
```

## $nextTick：

作用：等DOM更新后再出发方法里的函数

语法: 

```html
this.$nextTick(() => {
	this.$refs.属性名.方法名()
})

```

# 9.自定义指令

可以在自定义指令中封装dom操作

```html
// 3. 使用指令
<input v-指令名>

<script>
	// 1. 全局注册
    Vue.directive('指令名', {
        // inserted 在指令所在的元素被插入到页面中时触发
        inserted (el) {
            el.focus()
        }	
    })
    // 2. 局部注册
    directives: {
        "指令名": {
            inserted (el) {
                el.focus()
            }
        }
    }
</script>
```

自定义指令传参：

```html
// 3. 使用指令
<input v-color='color'>

<script>
    data () {
        return {
            color:'blue'
        }
    },
	directives: {
        color: {
            // 元素添加到页面时触发
            inserted (el,binding) {
                // el是标签元素 binding.value是指令值
                el.style.color = binding.value
            },
            // 指令值修改时触发
            update (el,binding) {
                el.style.color = binding.value
            }
        }
    }
</script>
```

# 10.插槽

作用：让组件内部的一些结构支持自定义

```html
// 1. 在组件内部需要自定义的部分用<slot></slot>占位
<template>
    <div>
        // 默认插槽 无默认值
        <slot></slot>
        
        // 默认插槽 有默认值
        <slot>这是默认值</slot>
        
        // 具名插槽 使用name属性区分多个slot
        <slot name="属性名"></slot>
        
        // 作用域插槽（传参） :变量名="数据名" 所有数据收集为对象
        <slot :data="data" msg="默认插槽"></slot>
        <slot :data="data" msg="具名插槽"></slot>
    </div>
</template>

// 2. 使用组件时，在标签内部传入结构替换slot
<template>
    // 无默认值
    <son>我是儿子</son>
    
    // 有默认值，没有传值时显示默认值
    <son></son>
    
    // 具名插槽 使用 template标签 和 v-slot:属性名 区分 简写#属性名
    <son>
    	<template v-slot:title>我是标题</template>
        <template #content>我是内容</template>
    </son>
    
    // 作用域插槽 通过 #插槽名="变量名" 接收 所有数据收集为对象
    <son>
        // 默认插槽名为default
        <template #default="obj">{{ obj.msg }}</template>
        <template #插槽名="obj">{{ obj.msg }}</template>
    </son>
</template>
```

# 11.路由

## VueRouter：

使用步骤：

1. 下载 Vue2-v3.6.5 Vue3-v4

```html
// main.js文件
<script>
    // 6.1 创建需要的组件（views目录下）
    import Find from './views/Find.vue'
    import My from './views/My.vue'
    
    // 2. 引入
	import VueRouter from 'vue-router'
    // 3. 安装注册
    Vue.use(Vuerouter)
    
    // 4. 创建路由对象
    const router = new VueRouter({
        // 6.2 配置路由规则 路径为地址栏路径
        routes:[
            { path: '/find', component: Find },
            { path: '/my', component: My },
        ]
    })
    
    // 5.将路由对象注入到vue实例中建立关联
    new Vue({
		render: h => h(App),
		router: router
	}).$mount('#app')
</script>

// App.vue文件
<template>
    // 7. 配置路由出口
	<div>
        // 放在哪 匹配的组件就展示在哪（上 中 下）
        <router-view></router-view>
    </div>
</template>
```

 路由封装抽离：

```html
// router/index.js文件
<script>
    // @ = src目录
    import Find from '@/views/Find'
    import My from '@/views/My'
    
    import Vue from 'vue'
	import VueRouter from 'vue-router'
    Vue.use(Vuerouter)
    
    const router = new VueRouter({
        routes:[
            { path: '/find', component: Find },
            { path: '/my', component: My },
        ],
        // link自定义高亮类名
        linkActiveClass: '' 		// 配置模糊匹配类名
        linkExactActiveClass: '' 	// 配置精确匹配类名
    })
    
    export default router
</script>

// main.js文件
<script>
    import router from './router/index'
    
	new Vue({
		render: h => h(App),
		router: router
	}).$mount('#app')
</script>
```

## router-link（导航链接/声明式导航）:

作用：取代a标签，实现路径跳转功能，默认提供高亮类名，可直接设置高亮样式

```html
<template>
    <div class="footer">
        <router-link to='/find'>发现音乐</router-link>
        <router-link to='/my'>我的音乐</router-link>
    </div>
</template>

<style>
    // router-link-active 模糊匹配 to="/my" 可以匹配 /my /my/a 
    .footer a.router-link-active {
        background-color: blue;
    }
    // router-link-exact-active 精确匹配 to="/my" 仅可匹配 /my
    .footer a.router-link-exact-active {
        background-color: blue;
    }
</style>
```

查询参数传参（适合传多个参数）：

```html
<template>
    <div>
        热门搜索：
        // 传递参数 路径中携带查询参数 ?参数名1=值1&参数名2=值2
        <router-link to="/search?key=音乐">音乐</router-link>
        <router-link to="/search?key=图片">图片</router-link>
    </div>
</template>

<template>
    <div class="search">
        // 模版中获取参数 $route.query.参数名
        <p>搜索关键字：{{ $route.query.key }}</p>
    </div>
</template>
<script>
	export default {
        // js中获取参数 this.$route.query.参数名
        created () {
            console.log(this.$route.query)
        }
    }
</script>
```

动态路由传参（适合传单个参数）：

```html
// 1. 配置路由 router/index.js文件
<script>
	const router = new VueRouter({
        routes:[
            // 修改路径 '/路径:参数名' 
            { path: '/search:key', component: Search },
            
            // ?表示可选，不加则必须传参数，否则无法正常加载组件
            { path: '/search:key?', component: Search }
        ]
    })
</script>

// 2. 配置导航链接
<template>
    <div>
        // 路径中携带参数 /参数值
        <router-link to="/search/音乐">音乐</router-link>
        <router-link to="/search/图片">图片</router-link>
    </div>
</template>

// 3. 获取参数
<template>
    <div class="search">
        // 模版中获取参数 $route.params.参数名
        <p>搜索关键字：{{ $route.params.key }}</p>
    </div>
</template>
<script>
	export default {
        // js中获取参数 this.$route.params.参数名
        created () {
            console.log(this.$route.params)
        }
    }
</script>
```

## 重定向：

作用：匹配到某个路径时，强制跳转到指定路径

```html
<script>
	const router = new VueRouter({
        routes:[
            // 保证打开网页时加载home首页
            { path: '/', redirect: '/home' },
            { path: '/home', component: Home },
            { path: '/search:key', component: Search },
            // 配置在路由最后，路径找不到匹配时（404），展示这个组件
            { path: '*', component: NotFind }
        ]
    })
</script>
```

## 路由模式设置：

hash路由(默认)：http://localhost:8080/#/home

history路由(常用) ：http://localhost:8080/home

```html
<script>
	const router = new VueRouter({
        routes:[
            // 添加配置项更改模式，但需要后台配置相应的访问规则
            mode: 'history',
            { path: '/home', component: Home }
        ]
    })
</script>
```

## 编程式导航：

基本跳转：

```html
<template>
	<button @click="goSearch">搜索</button>
</template>
<script>
	methods:{
        goSearch(){
            // 1.1 path路径跳转 简写
            this.$router.push('/search'),
            // 1.2 path路径跳转 完整写法
            this.$router.push({
                path:'/search'
            })
            
            // 2. 命名路由跳转 需要给路由取名 适合长路径
            this.$router.push({
                name:'search'
            })
    }
</script>

<script>
	const router = new VueRouter({
        routes:[
            // 给路由取名 name:'路由名'
            { name: 'search', path: '/search', component: Search }
        ]
    })
</script>
```

查询参数传参：

```html
<template>
    <div>
        <input v-model="inp" type="text">
        <button @click="goSearch">搜索</button>
    </div>
</template>
<script>
	export default {
        data(){
            return () {
                inp:''
            }
        },
        goSearch(){
            // 1.1 path路径跳转传参 简写 适合单个参数
            this.$router.push(`/search?key=${this.inp}`),
                
            // 1.2 path路径跳转传参 完整写法 适合多个参数
            this.$router.push({
                path:'/search',
                query:{
                    key:this.inp,
                    参数名:参数值,
                    ...
                }
            }),
            
            // 2. 命名路由跳转传参
            this.$router.push({
                name:'search',
                query:{
                    key:this.inp,
                    参数名:参数值,
                    ...
                }
            }),
        }
    }
</script>
```

动态路由传参：

```html
<template>
    <div>
        <input v-model="inp" type="text">
        <button @click="goSearch">搜索</button>
    </div>
</template>
<script>
	export default {
        data(){
            return () {
                inp:''
            }
        },
        goSearch(){
            // 1.1 路径中携带查询参数 简写
            this.$router.push(`/search/${this.inp}`),
                
            // 1.2 路径中携带查询参数 完整写法
            this.$router.push({
                path:`/search/${this.inp}`
            }),
                
            // 2. 命名路由跳转传参
            this.$router.push({
                name:'search',
                params:{
                    key:this.inp
                }
            }),
        }
    }
</script>
```

## 嵌套子路由：

```html
<script>
	const router = new VueRouter({
        routes:[{
            path: '/', 
            component: Home,
            // 1. 在父级路由中配置子路由
            children: [{
                path: '/', 
                component: Home
            }]
        }]
    })
</script>

<template>
    <div>
        <div>
            // 2. 在父组件中配置路由出口
            <router-view></router-view>
        </div>
        <nav>
        	<a>首页</a>
            <a>我的</a>
        </nav>
    </div>
</template>
```

# 12.组件缓存

```html
<template>
	<div>
        // keep-alive 包裹动态组件可以缓存不活动的组件实例，而不销毁
        // 三个配置项：
        // include="组件名数组"，只有匹配的组件会被缓存
        // exclude="组件名数组"，匹配的组件都不会被缓存
        // max 最多可以缓存的组件实例数量
        <keep-alive :include="['Home']">
        	<router-view></router-view>
        </keep-alive>
    </div>	
</template>

<script>
	export default {
        // 组件缓存后会触发两个生命周期函数
        // activated() 组件激活时触发 → 进入页面时触发
        // deactivated() 组件失活时触发 → 离开页面时触发
        // 不再执行created() mounted() destroyed()
        activated(){
        	console.log('actived 激活 → 进入页面');
        },
        deactivated(){
        	console.log('deactived 失活 → 离开页面');
        }
    }
</script>
```

# 13.自定义创建项目

黑马 vue2+3 P88

