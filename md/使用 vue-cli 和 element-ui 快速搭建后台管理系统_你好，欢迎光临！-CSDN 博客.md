> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/u010622874/article/details/112625396)

> 使用 vue-cli 和 element-ui 入门实践 1. 环境配置安装 node 从 Node 官网下载对应平台的安装程序 https://nodejs.org/en / 在 window 上安装时要注意：选择全部组件，包括勾选 Add to Path。

1. 环境配置
-------

安装 node

从 Node 官网下载对应平台的安装程序 https://nodejs.org/en/

在 window 上安装时要注意：选择全部组件，包括勾选 Add to Path。

安装完成后，在 window 环境下，打开命令行窗口，然后输入`node -v`，如果安装正常，可以看到输出如图所示：  
![](https://img-blog.csdnimg.cn/20210114181249343.png)

2. 安装 vue-cli
-------------

参考文档 https://cli.vuejs.org/zh/

```
npm install -g @vue/cli
```

目前按照这种方式安装应该是 Vue CLI 4.X（需要 Node.jsV8.9 或更高版本）

### 2.1 查看 Vue cli 的所有版本号

vue-cli3.X 和 vue-cli4.x 所有版本号查看

```
npm view @vue/cli versions --json
```

vue-cli1.x 和 vue-cli2.x 的所有版本号查看

```
npm view vue-cli versions --json
```

### 2.2 安装指定版本号的 vue-cli

安装 3.x 和 4.x 版本（推荐安装这两个版本）

```
npm install -g @vue/cli@版本号
```

安装 1.x 和 2.x 版本

```
npm install -g vue-cli@版本号
```

安装完成后，命令行执行`vue -V`，正常输出版本号，则说明安装成功。

3. 项目搭建
-------

### 3.1 创建一个 vue-cli3 项目

```
vue create mmp-subapp1
```

mmp-subapp1 为自定义项目名称，回车之后依次选默认的即可

### 3.2 使用 element-ui 组件

1.  安装 element-ui 依赖
    
    ```
    npm install element-ui
    ```
    
2.  安装 sass 和 sass-loader ，因为 element-ui 中的样式默认是使用 sass 来编写的
    
    ```
    npm install sass --save-dev
    npm install sass-loader --save-dev
    ```
    
3.  在 main.js 文件中引入 element-ui 和样式，添加以下代码即可
    
    ```
    import ElementUI from 'element-ui'; //引入ElementUI
    import 'element-ui/lib/theme-chalk/index.css' //引入element-ui样式
    
    Vue.use(ElementUI)
    ```
    
4.  使用 elment-ui 组件
    
    1.  例如在 App.vue 中：
        
        ```
        <template>
          <div id="app">
            <!-- 全局引入的element-ui的组件 -->
            <el-button>按钮组件</el-button>
          </div>
        </template>
        
        <script>
        export default {
          name: 'App'
        }
        </script>
        
        <style>
        #app {
          font-family: Avenir, Helvetica, Arial, sans-serif;
          -webkit-font-smoothing: antialiased;
          -moz-osx-font-smoothing: grayscale;
          color: #2c3e50;
        }
        </style>
        ```
        
    2.  页面效果
        
        [外链图片转存失败, 源站可能有防盗链机制, 建议将图片保存下来直接上传 (img-PYbbGpbq-1610618885917)(file://D:/Users/dandanhu/AppData/Roaming/Typora/typora-user-images/image-20201214203157993.png?lastModify=1607949117)]
        

### 3.3 使用自定义组件 (以 header 组件为例)

1.  components 文件夹中添加如下结构文件

![](https://img-blog.csdnimg.cn/20210114181319492.png)

2.  Index.vue 文件中添加头部组件的内容如下
    
    ```
    <template>
        <div class="base-header">
            <div class="base-header-logo">
                <img src="../../assets/logo.png"/><span >支付模块管理平台</span>
            </div>
            <div class="base-header-userInfo">
                <div class="base-header-userName">{{userName}}</div>
                <span class="base-header-logout-btn" @click="logout">退出登录</span>
            </div> 
        </div>
    </template>
    
    <script>
    export default {
        name:'CusHeader',
        data() {
            return {
                userName: '韦小宝' //要显示的用户名称
            }
        },
        methods: {
            logout() {
                //退出登录操作
                console.log('退出登录')
            }
        },
    }
    </script>
    
    <style lang="scss" scoped>
    .base-header {
        background-color:#001529;
        height:calc(5vh);
        color:#fff;
        .base-header-logo {
            margin-left:10px;
            display: inline-block;
            img {
            display: inline-block;
            width: 2.8em;
            height: 2.8em;
            vertical-align: middle;
            }
            span {
                font-size:1.5em;
                display: inline-block;
                vertical-align: middle;
            }
        }
        .base-header-userInfo {
            float: right;
            height: 100%;
            line-height: 2.8em;
            margin-right: 20px;
            .base-header-userName {
                display: inline-block;
                margin-right: 10px;
            }
            .base-header-logout-btn {
                display: inline-block;
                cursor: pointer;
                font-size:1.2em;
                font-weight: 700;
                margin-right: 10px;
            }
        }
    }
    
    </style>
    ```
    
3.  App.vue 中使用组件
    
    ```
    <template>
      <div id="app">
        <!-- 全局引入的element-ui的组件 -->
        <el-button>按钮组件</el-button>
        <!-- 自定义组件 -->
        <cus-header></cus-header>
      </div>
    </template>
    
    <script>
    
    import CusHeader from '@/components/Header/Index.vue'
    export default {
      name: 'App',
      components: {
        CusHeader
      }
    }
    </script>
    
    <style>
    #app {
      font-family: Avenir, Helvetica, Arial, sans-serif;
      -webkit-font-smoothing: antialiased;
      -moz-osx-font-smoothing: grayscale;
      color: #2c3e50;
    }
    </style>
    ```
    

### 3.4 带路由功能的导航菜单组件

1.  安装 vue-router 依赖（官方文档：https://router.vuejs.org/zh/）
    
    ```
    npm install --save vue-router
    ```
    
2.  src 文件夹下添加页面文件
    

![](https://img-blog.csdnimg.cn/20210114181514520.png)

以 View1.vue 为例，文件添加的内容为：

```
<template>
    <div>
        这是View111111111
    </div>
</template>

<script>
    export default {
        
    }
</script>

<style lang="scss" scoped>

</style>
```

3.  src 文件夹下添加如下结构文件，并添加路由配置

![](https://img-blog.csdnimg.cn/2021011418154143.png)

index.js

```
const routes = [
    {
      /**
       * path: 路径为 / 时触发该路由规则
       * name: 路由的 name 为 Home
       * component: 触发路由时加载 `Home` 组件
       */
      path: "/View1",
      name: "View1",
      component: resolve => require(['../views/View1.vue'], resolve),
    },
    {
      path: "/View2",
      name: "View2",
      component: resolve => require(['../views/View2.vue'], resolve),
    },
    {
      path: "/View3",
      name: "View3",
      component: resolve => require(['../views/View3.vue'], resolve),
    }
  ];
  
  export default routes;
```

添加 Vue Router，其实就是将组件 (components) 映射到路由 (routes)，然后告诉 Vue Router 在哪里渲染它们

4.  main.js 引入 vue-router 并使用
    
    ```
    import Vue from 'vue'
    import App from './App.vue'
    //引入UI组件
    import ElementUI from 'element-ui';
    import 'element-ui/lib/theme-chalk/index.css'
    // 引入路由
    import VueRouter from 'vue-router'
    import routes from './router'
    
    
    Vue.config.productionTip = false
    Vue.use(ElementUI)
    
    Vue.use(VueRouter)
    
    const router = new VueRouter({
      mode: "history",
      routes,
    })
    
    
    new Vue({
      router,
      render: h => h(App),
    }).$mount('#app')
    ```
    
    通过注入路由器，我们可以在任何组件内通过 `this.$router` 访问路由器，也可以通过 `this.$route` 访问当前路由
    
5.  components 文件夹下面添加 menu 组件，结构如图
    
    ![](https://img-blog.csdnimg.cn/2021011418160239.png)
    
    Index.vue
    
    ```
    <template>
        <div>
            <el-menu :default-active="activeIndex" class="el-menu-demo" mode="horizontal" @select="handleSelect" router>
                <template v-for="item in menuData" >
                    <el-submenu v-if='item.children' :index="item.index" :key="item.index">
                        <template slot="title">
                            <i :class="item.icon"></i>
                            <span>{{item.name}}</span>
                        </template>
                        <el-menu-item-group>
                            <el-menu-item v-for="subItem in item.children" :index="subItem.index" :key="subItem.index">{{subItem.name}}</el-menu-item>
                        </el-menu-item-group>
                    </el-submenu>
                    <el-menu-item v-else :index="item.index" :key="item.index">
                        <i :class="item.icon"></i>
                        <span slot="title">{{item.name}}</span>
                    </el-menu-item>
                </template>
            </el-menu>
        </div>
    </template>
    
    <script>
        export default {
            name:'CusHorizMenu',
            props:{
                menuData:{
                    type:Array,
                    default(){
                        return[]
                    }
                }
            },
            data() {
                return {
                    activeIndex: 'View1'
                }
            },
            methods: {
                handleSelect() {
                    console.log('选中导航菜单的时候触发') 
                }
            },
            
        }
    </script>
    
    <style lang="scss" scoped>
    
    </style>
    ```
    
    更多`el-menu`相关配置见官方文档（https://element.eleme.cn/#/zh-CN/component/menu）
    
6.  App.vue 文件中引入菜单组件，并定义菜单栏数据 menuData
    
    ```
    <template>
      <div id="app">
        <!-- 全局引入的element-ui的组件 -->
        <el-button>按钮组件</el-button>
        <!-- 自定义组件 -->
        <cus-header></cus-header>
        <!-- 带路由的自定义组件（内部是element-ui组件）-->
        <horiz-menu :menuData="menuData"></horiz-menu>
        <!-- 路由组件内容展示区 -->
        <router-view></router-view>
      </div>
    </template>
    
    <script>
    
    import CusHeader from '@/components/Header/Index.vue'
    import HorizMenu from '@/components/HorizMenu/Index.vue'
    export default {
      name: 'App',
      components: {
        CusHeader,
        HorizMenu
      },
      data() {
        return {
          menuData: [
            {
              index:'View1',
              name:'View1页',
              icon:'el-icon-location',
            },
            {
              index:'View2',
              name:'View2页',
              icon:'el-icon-location',
            },
            {
              index:'View3',
              name:'View3页',
              icon:'el-icon-location',
            }
          ]
        }
      },
    }
    </script>
    
    <style>
    #app {
      font-family: Avenir, Helvetica, Arial, sans-serif;
      -webkit-font-smoothing: antialiased;
      -moz-osx-font-smoothing: grayscale;
      color: #2c3e50;
    }
    </style>
    ```
    

### 3.5 路由跳转

1.  普通跳转
    
    使用 `<router-link>` 标签 创建 a 标签来定义导航链接
    
    View1.vue
    
    ```
    <template>
        <div>
            这是View111111111
            <h1>使用 router-link 标签 创建 a 标签来定义导航链接</h1>
            <router-link to="/View2">Go to Foo</router-link>
        </div>
    </template>
    
    <script>
        export default {
            name:'View1'
        }
    </script>
    
    <style lang="scss" scoped>
    
    </style>
    ```
    
    常用的编程式导航
    
    View2.vue
    
    ```
    <template>
        <div>
            这是View22222222222
            <h2>编程式导航</h2>
            <el-button @click="toView3">跳转到View3页面</el-button>
            <h3>编程式导航的两种带参数跳转</h3>
            <div>
                <el-button @click="toView3Param">带参数跳转到View3页面name和params组合</el-button>
                <el-button @click="toView3Query">带参数跳转到View3页面path和query组合</el-button>
            </div>
        </div>
    </template>
    
    <script>
        export default {
            name:'View2',
            methods: {
                toView3() {
                    this.$router.push('View3')
                    // this.$router.push({path:'View3'})
                },
                toView3Param() {
                    this.$router.push({ name: 'View3', params: { userId: '地址栏看不见的参数' }})
                },
                toView3Query() {
                    this.$router.push({ path: '/View3', query: { userId: '地址栏看得见的参数' }})
                }
            },
            
        }
    </script>
    
    <style lang="scss" scoped>
    
    </style>
    ```
    
    导航守卫–组件内的导航守卫
    
    View3
    
    ```
    <template>
        <div>
            这是View33333333333
            <div>接收到从View2带来的参数</div>
            <div>param={{param}}</div>
            <div>query={{query}}</div>
        </div>
    </template>
    
    <script>
        export default {
            name:'View3',
            data() {
                return {
                    param:'',
                    query:''
                }
            },
            beforeRouteEnter(to, from, next) {
                //to, 即将要进入的目标路由对象
                //from, 当前导航正要离开的路由
                //next() 进行管道中的下一个钩子，如果钩子全部执行完了，则导航的状态就是comfirmed
                // 在渲染该组件的对应路由被 confirm 前调用
                // 不！能！获取组件实例 `this`
                // 因为当守卫执行前，组件实例还没被创建
                console.log(to, from, next)
                //next必须写，否则跳转不会成功，通过 `vm` 访问组件实例
                next(vm=>{
                    vm.param = to.params.userId ? to.params.userId : ''
                    vm.query = to.query.userId ? to.query.userId : ''
                })
            },
            beforeRouteUpdate (to, from, next) {
                console.log(to, from, next)
                // 在当前路由改变，但是该组件被复用时调用
                // 举例来说，对于一个带有动态参数的路径 /foo/:id，在 /foo/1 和 /foo/2 之间跳转的时候，
                // 由于会渲染同样的 Foo 组件，因此组件实例会被复用。而这个钩子就会在这个情况下被调用。
                // 可以访问组件实例 `this`
            },
            beforeRouteLeave (to, from, next) {
                console.log(to, from, next)
                // 导航离开该组件的对应路由时调用
                // 可以访问组件实例 `this`
            }
        }
    </script>
    
    <style lang="scss" scoped>
    
    </style>
    ```
    
    导航守卫参考（[https://router.vuejs.org/zh/guide/advanced/navigation-guards.html#%E5%85%A8%E5%B1%80%E5%89%8D%E7%BD%AE%E5%AE%88%E5%8D%AB](https://router.vuejs.org/zh/guide/advanced/navigation-guards.html#%E5%85%A8%E5%B1%80%E5%89%8D%E7%BD%AE%E5%AE%88%E5%8D%AB)）
    

### 3.6 添加配置

文件夹根目录位置添加 vue.config.js

常用的几个：

```
module.exports = {
	publicPath:'',//部署应用包时的基本 URL,如果你的应用被部署在 https://www.my-app.com/my-app/,则设置 publicPath 为 /my-app/
	outputDir:'', //生成的生产环境构建文件的目录 buil时使用，目标目录在构建之前会被清除 (构建时传入 --no-clean 可关闭该行为)。
	assetsDir：'',//放置生成的静态资源 (js、css、img、fonts) 的 (相对于 outputDir 的) 目录,相对地址
    devServer: {
      port: 10001, //指定启动项目所用的端口号
    //   proxy: 'localhost:8080', //代理地址，你的前端应用和后端 API 服务器没有运行在同一个主机上，你需要在开发环境下将 API 请求代理到 API 服务器
    //   open: true,
    //   // 关闭主机检查，使微应用可以被 fetch
    //   disableHostCheck: true,
    //   // 配置跨域请求头，解决开发环境的跨域问题
    //   headers: {
    //     'Access-Control-Allow-Origin': '*',
    //   },
    }
}
```

具体的各项配置含义：https://cli.vuejs.org/zh/config/#vue-config-js