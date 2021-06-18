> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/qq_42391904/article/details/104759753)

> Vue 入门——Vue-CLI 结合 ElementUI0. 前言 Vue 入门——Vue-CLI 4.X 脚手架搭建在搭建好 Vue-CLI 脚手架后，想结合比较流行的 ElementUI1. 项目目录下安装 ElementUI 我的项目路径为 D 盘 vue 文件夹下的 vueblog，切换到这个路径后执行以下安装命令：cnpm i element-ui -S 一定要切换到项目路径，不然后面运行会有引用错误。

### 0. 前言

[Vue 入门——Vue-CLI 4.X 脚手架搭建](https://blog.csdn.net/qq_42391904/article/details/104753789)

在搭建好 Vue-CLI 脚手架后，想结合比较流行的 ElementUI

### 1. 项目目录下安装 ElementUI

我的项目路径为 D 盘 vue 文件夹下的 vueblog，**切换到这个路径后**执行以下安装命令：

```
cnpm i element-ui -S
```

![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9naXRlZS5jb20vbGlwaWFvTUtYL2Jsb2dpbWFnZS9yYXcvbWFzdGVyL2ltZy9WVUUlRTUlOUMlQTglRTklQTElQjklRTclOUIlQUUlRTglQjclQUYlRTUlQkUlODQlRTQlQjglOEIlRTUlQUUlODklRTglQTMlODVlbGVtZW50VUkuanBn?x-oss-process=image/format,png)

一定要切换到项目路径，不然后面运行会有引用错误。

### 2. 手动配置 ElementUI 并添加 ElementUI 按钮

在项目 src 下的 app.vue 中添加如下代码：

```
//手动配置ElementUI
import ElementUI from 'element-ui'
import 'element-ui/lib/theme-chalk/index.css'
Vue.use(ElementUI)
```

![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9naXRlZS5jb20vbGlwaWFvTUtYL2Jsb2dpbWFnZS9yYXcvbWFzdGVyL2ltZy92dWVDTEklRTYlODklOEIlRTUlOEElQTglRTklODUlOEQlRTclQkQlQUVlbGVtZW50VUkuanBn?x-oss-process=image/format,png)

添加 elementUI 的按钮组件代码到 App.vue，官网可复制： https://element.eleme.cn/#/zh-CN/component/button

```
<el-row>
			<el-button>默认按钮</el-button>
			<el-button type="primary">主要按钮</el-button>
			<el-button type="success">成功按钮</el-button>
			<el-button type="info">信息按钮</el-button>
			<el-button type="warning">警告按钮</el-button>
			<el-button type="danger">危险按钮</el-button>
		</el-row>
```

![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9naXRlZS5jb20vbGlwaWFvTUtYL2Jsb2dpbWFnZS9yYXcvbWFzdGVyL2ltZy9lbGVtZW50VUklRTYlOEMlODklRTklOTIlQUUlRTclQkIlODQlRTQlQkIlQjYlRTQlQkIlQTMlRTclQTAlODEuanBn?x-oss-process=image/format,png)

### 3. 运行查看效果

切换至项目目录下执行运行命令：

```
npm run serve
```

效果如图，可看到加上去的按钮组件：

![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly9naXRlZS5jb20vbGlwaWFvTUtYL2Jsb2dpbWFnZS9yYXcvbWFzdGVyL2ltZy9lbGVtZW50VUklRTYlOTUlODglRTYlOUUlOUMuanBn?x-oss-process=image/format,png)

如果运行有报错，检查第一步安装 ElementUI 是否在项目目录下。