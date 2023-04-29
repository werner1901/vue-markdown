### 1.举例说明 props 自定义 validator 校验输入

```
# 自定义表单校验是在data中声明一个函数，并将这个函数添加到校验规则里。比如验证是否为正确手机号时,验证函数如下：
const validatePhoneNumber = (rule, value, callback) => {
  const reg = /^[1][3,4,5,7,8][0-9]{9}$/
  if (value == '' || value == undefined || value == null) {
    callback()
  } else {
    if (!reg.test(value) && value != '') {
      callback(new Error('请输入正确的电话号码'))
    } else {
      callback()
    }
  }
}
#在data中声明一个函数，并将这个函数添加到校验规则里
export default {
 data(){
  rules:{
    phonenumber: [
        { required: true, message: '请输入电话号码', trigger: 'blur' }
        { validator: validatePhoneNumber, trigger: 'blur' }
      ],
    }
  }
}
```

### 2.说明 slot 的用途，并且列举代码片段说明 slot 应用

插槽在组件中给 HTML 模板占了一个位置，用来传入一些东西。插槽又分为匿名插槽、具名插槽、作用域插槽。
```
#匿名插槽
# 一个 <SubmitButton> 组件,在父组件没有提供任何插槽内容时在 <button> 内渲染“Submit”,如果提供了内容，显式提供的内容会取代默认内容
<button type="submit">
  <slot>
    Submit <!-- 默认内容 -->
  </slot>
</button>

#具名插槽
#一个 <BaseLayout> 组件，没有提供 name 的 <slot> 出口会隐式地命名为“default”。
<div class="container">
  <header>
    <slot name="header"></slot>
  </header>
  <main>
    <slot></slot>
  </main>
  <footer>
    <slot name="footer"></slot>
  </footer>
</div>
#在父组件中使用 <BaseLayout> 时，需要一种方式将多个插槽内容传入到各自目标插槽的出口。此时就需要用到具名插槽了
<BaseLayout>
  <template #header>
    <h1>Here might be a page title</h1>
  </template>

  <template #default>
    <p>A paragraph for the main content.</p>
    <p>And another one.</p>
  </template>

  <template #footer>
    <p>Here's some contact info</p>
  </template>
</BaseLayout>

#作用域插槽
#一个<MyComponent>组件，向一个插槽的出口上传递 attributes：
<div>
  <slot :text="greetingMessage" :count="1"></slot>
</div>
#默认插槽接受 props
<MyComponent v-slot="slotProps">
  {{ slotProps.text }} {{ slotProps.count }}
</MyComponent>
```

### 3.举例说明 emit 的应用
```
#在组件的模板表达式中，可以直接使用 $emit 方法触发自定义事件
<button @click="$emit('someEvent')">click me</button>
#在组件实例上也同样以 this.$emit() 的形式可用
export default {
  methods: {
    submit() {
      this.$emit('someEvent')
    }
  }
}
#组件可以显式地通过 emits 选项来声明它要触发的事件
export default {
  emits: ['inFocus', 'submit']
}
```

### 4.写出 vue 注册组件的 3 种方式的代码
```
#全局注册
import { createApp } from 'vue'
const app = createApp({})
app.component(
  // 注册的名字
  'MyComponent',
  // 组件的实现
  {
    /* ... */
  }
)

#全局注册，使用单文件组件
import MyComponent from './App.vue'
app.component('MyComponent', MyComponent)

#局部注册
<script>
import ComponentA from './ComponentA.vue'
export default {
  components: {
    ComponentA
  }
}
</script>
<template>
  <ComponentA />
</template>
```
### 1.说明使用 vue-router 的设置步骤
```
#安装vue-router
#导入路由组件
import Home from './Home'
import Anout from './About'
#定义一些路由
const routes = [
  { path: '/', component: Home },
  { path: '/about', component: About },
]
#创建路由实例并传递 `routes` 配置
const router = VueRouter.createRouter({
  history: VueRouter.createWebHashHistory(),
  routes,
})
#创建并挂载根实例
const app = Vue.createApp({})
app.use(router)
app.mount('#app')
```
### 2.举例说明路由跳转传入参数和目标页面如何获取参数
```
#router-link路由导航
<router-link to="/test/123">routerlinktest</router-link>
#调用$router.push
<button @click="deliverParams(123)">push传参</button>
  methods: {
    deliverParams (id) {
      this.$router.push({
        path: `/d/${id}`
      })
    }
  }
#name属性匹配路由
<button @click="deliverByName()">params传参</button>
    deliverByName () {
      this.$router.push({
        name: 'B',
        params: {
          sometext: 'saysomething'
        }
      })
    }
#通过query来传递参数
<button @click="deliverQuery()">query传参</button>
    deliverQuery () {
      this.$router.push({
        path: '/c',
        query: {
          sometext: 'saysomething'
        }
      })
    }

```
### 3.列举路由全局钩子函数的2个以上使用场景
```
#beforeeach 用户登录
router.beforeEach((to, from, next) => {
  if (to.name !== 'Login' && !isAuthenticated) next({ name: 'Login' })
  else next()
})

aftereach 这个钩子的两个参数和 beforeEach 中的 to 和 from 一样。但是这个钩子不会接受 next 函数，也不会改变导航本身。
router.afterEach((to, from, failure) => {
  if (!failure) sendToAnalytics(to.fullPath)
})
```


### 1.说明使用vuex-store的引入到使用的步骤
```
#安装Vuex
#创建一个store
const store = createStore({
  state () {
    return {
      count: 0
    }
  },
  mutations: {
    increment (state) {
      state.count++
    }
  }
})
const app = createApp({ /* 根组件 */ })
app.use(store)
#通过 this.$store 访问store实例，提交一个变更
methods: {
  increment() {
    this.$store.commit('increment')
    console.log(this.$store.state.count)
  }
}
```
### 2.举例说明 action,mutation, state 的关系
```
state：数据状态
mutations：函数，同步操作。更改state
actions:函数，异步操作。通过调用mutations来更改数据
state :{
  counter: 1000,
}

mutations:{
	increment(state){
		state.counter ++
	}
}
actions:{
 increament(context){
 //context:上下文，这里可以理解为store对象
 	setTimeOut(()=>{
		context.commit('increment')
	},1000)
	}
}


```
### 3.举例说明辅助函数…mapState的用法。
```
#当一个组件需要获取多个状态的时候，将这些状态都声明为计算属性会有些重复和冗余。为了解决这个问题，可以使用 mapState 辅助函数帮助我们生成计算属性
import { mapState } from 'vuex'
export default {
  computed: mapState({
    count: state => state.count,
    countAlias: 'count',
    countPlusLocalState (state) {
      return state.count + this.localCount
    }
  })
}
#mapState 函数返回的是一个对象,为了将它与局部计算属性混合使用，使用对象展开运算符...mapState
computed: {
  localComputed () { /* ... */ },
  // 使用对象展开运算符将此对象混入到外部对象中
  ...mapState({
    // ...
  })
}
```

### 举例出axios post,delete,put,get方法使用
```
#get
<script>
    import axios from 'axios'
    export default {
        name: 'get请求'，
        components: {},
        axios({
            method: 'get',
            params: {
                id: 12,
            },
            url: '后台接口地址',
        }).then(res => {
            //执行成功后代码处理
        })
    }
</script>
#其他的都同上，把method改为对应的方法即可。
```
### 列举代码使用axios的请求拦截中设置token到请求头
```
#设置一个请求拦截器
axios.interceptors.request.use(function (config) {
  let userinfo = window.sessionStorage.getItem('userinfo')
  if (userinfo) {
    let token = JSON.parse(userinfo).token
    config.headers.Authorization = token
  }
  return config
}, function (error) {
  // Do something with request error
  return Promise.reject(error)
})
```