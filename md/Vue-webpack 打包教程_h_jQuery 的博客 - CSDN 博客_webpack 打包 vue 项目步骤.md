> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/h_jQuery/article/details/116700650)

> webpack1. 创建项目并在终端初始化 npm init -y 在项目目录下回生成一个 package.json 文件 2. 在项目目录下创建 src 文件夹，在 src 目录下创建 index.html 文件和 index.......

webpack
-------

#### 1. 创建项目并在终端初始化

```
npm init -y
```

在项目目录下回生成一个 package.json 文件

#### 2. 在项目目录下创建 src 文件夹，在 src 目录下创建 index.html 文件和 index.js 文件

index.html 内容为：

```
<ul>
        <li>1</li>
        <li>2</li>
        <li>3</li>
        <li>4</li>
        <li>5</li>
    </ul>
```

#### 3. 在终端安装 jQuery

```
npm install jquery -S
//npm i jquery	 建议这样写
```

会在项目目录下生成一个 **package-lock.json** 文件和 **node_modules** 文件夹（这个文件夹可以删掉，后续需要的话可以重新下载，命令：**npm i**）

#### 4. 向 index.js 填充内容，并实现功能

##### 导入 jQuery

```
import $ from "jquery"
```

##### index.js 内容

```
import $ from "jquery";
$(function () {
    $("li:odd").css("background", "pink");
    $("li:even").css("background", "orange");
})
```

##### 在 index.html 内导入 index.js

```
<script src="./src/index.js"></script>
```

src 路径按照自己创建的路径为准

#### 浏览器报错：

```
Uncaught SyntaxError: Cannot use import statement outside a module
```

#### 安装 webpack 的原因：

因为 **import $ from “jquery”** 是 ES6 的语法代码，浏览器存在兼容性问题，所以需要安装 webpack，将高版本的语法编程浏览器可以识别的语法，webpack 只能打包 js 文件

##### 安装 webpack 步骤：

#### 1. 在终端输入命令：

```
npm install webpack webpack-cli -D
//若有报错就试试把-D改为-g
```

#### 2. 在项目根目录下创建一个 webpack.config.js 的配置文件来配置 webpack。

##### 向 webpack.config.js 添加内容：

```
module.exports = {
	mode:"development",
}
```

##### mode 设置的是项目的编译模式，可以设置的两个值：

**development**（开发模式）：表示项目处于开发阶段，不会进行压缩和混淆，打包速度会快一些

**production**（发布模式）：表示项目处于发布阶段，会进行压缩和混淆，打包速度会慢一些

#### 3. 向项目目录下的 package.json 文件添加运行脚本 dev:

```
"scripts":{
	"dev":"webpack"
}
```

**这个 dev 是自定义命名的**

#### 4. 运行 dev 命令进行项目打包

##### 在终端输入命令, 将会启动 webpack 进行项目打包

**这里的 dev 是第三步里面的 dev，第三步命名什么这里的就是什么**

```
npm run dev
```

成功后会在项目目录下成成一个 **dist** 文件夹，文件夹下有一个 **main.js** 文件

##### 在 index.html 中修改导入的 js 文件

```
<script src="../dist/main.js"></script>
```

页面效果渲染正确

在此之前，修改之后，都要用 **npm run dev** 进行打包，打包之后，效果才会改变

### 设置 webpack 的打包入口 / 出口

在 webpack 4x 中，

会默认将 **src/index.js** 作为默认的打包入口 js 文件;

会默认将 **dist/main.js** 作为默认的打包输出 js 文件;

##### 如果不想使用默认的入口 / 出口 js 文件，可以通过改变 webpack.config.js 来设置入口 / 出口的 js 文件：

```
const path = require("path");
module.exports = {
    // development开发模式、production发布模式
    mode: "development",
    // 设置入口文件路径
    entry: path.join(__dirname, "./src/index.js"),
    // 设置出口文件
    output: {
        // 设置路径
        path: path.join(__dirname, "./dist"),
        // 设置文件名
        filename: "bundle.js"
    }
}
```

### 设置 webpack 的自动打包

修改 js 文件中的代码后，需要重新运行命令 **npm run dev** 打包 webpack, 才能生成出口的 js 文件

#### 1. 安装自动打包功能的包：webpack-dev-server

在终端输入：

```
npm install webpack-dev-server -D
```

#### 2. 修改 package.json 中的 dev 指令

```
"scripts": {
    "dev": "webpack-dev-server"
  },
```

**webpack-dev-server** 自动打包的输出文件（bundle.js），默认放到了服务器的根目录下

##### 自动打包完毕后，默认打开服务器网页

解决：在 package.json 中的 dev 指令

webpack 5 与 webpack-dev-server 3 兼容性问题

```
"scripts": {
    "dev": "webpack serve --open Chrome.exe"
  },
```

在这里可能会出现：

```
[webpack-cli] Would you like to install 'webpack-dev-server' package? (That will run 'npm install -D webpack-dev-server') (Y/n)
```

Y, 然后回车就可以了

在终端执行 npm run dev 后，会自动跳转到浏览器

浏览器默认访问的地址为：http://localhost:8080/

#### 3. 修改 index.html 中 js 文件的路径

bundle.js 会保存到内存里面（在根目录下面），而不是在 dist 文件夹下面的那个

```
<script src="/bundle.js"></script>
```

在此之后，修改相关文件后，保存即可，不用再执行命令 **npm run dev**，也不用跳转到浏览器去，会自动跟随保存更新的

但是这里一开始浏览器打开的是项目目录页面（文件和文件夹），而不是 index.html 页面，需要自行点开 src 文件夹才打开 index.html 页面

### 生成预览页面——html-webpack-plugin

在内存里面（根目录）生成一个预览页面而不是在本地生成

实现步骤：

#### 1. 安装默认预览功能的包：html-webpack-plugin

```
npm install html-webpack-plugin -D
```

#### 2. 修改 webpack.config.js 文件：

```
//导包
const HtmlWebpackPlugin = require("html-webpack-plugin");
// 创建对象
const htmlPlugin = new HtmlWebpackPlugin({
//是键值对，用:相连的，而不是用=号，后面加逗号
    // 设置生成预览页面的模板文件
    template: "./src/index.html",
    // 设置生成的预览页面名称
    filename:"index.html",
})
module.exports = {
    ......
    plugins: [htmlPlugin]
}
```

设置完，**npm run dev** 之后打开的就是 index.html 页面了

### webpack 中的加载器（loader）

通过 loader 打包非 js 模块：在默认情况下，webpack 只能打包 js 文件，如果下载想要打包非 js 文件，需要调用 loader 加载器才能打包

loader 加载器包含：

1).less-loader：处理 less 文件

2).sass-loader

3).url-loader: 打包处理 css 中与 url 路径有关的文件

4).babel-loader：处理高级 js 语法的加载器

5).postcss-loader

6).css-loader,style-loader

** 注意：** 指定多个 loader 时的顺序是固定的，而调用 loader 的顺序是从后往前进行调用的

#### 安装 style-loader，.css-loader 来处理样式文件

##### 1. 安装包

在终端输入

```
npm install style-loader css-loader -D
```

##### 2. 配置规则：更改 webpack.config.js 的 module 中的 rules 数组

```
plugins: [htmlPlugin],		//这是前面添加过的
    module: {
        rules: [{
            // test设置需要匹配的文件类型，支持正则\.是.,前面的\是解析的意思，$是文件名一定要是.css结尾
            test: /\.css$/,
            // use表示该文件类型需要调用的loader(加载器)
            use: ['style-loader', 'css-loader'],
        }]
    }
```

在项目目录下的 src 文件夹下面创建一个 css 文件夹，在 css 文件夹下面创建一个 index.css 文件，自行添加内容

在入口文件 index.js 中引入（导入）index.css 文件

```
import "./css/index.css";
```

因为导入的是一个文件，所以不用 **from**

最后在终端执行 **npm run dev** 命令即可

#### 安装 less，less-loader，来处理 less 文件

##### 1. 安装包

```
npm install less-loader less -D
```

##### 2. 配置规则：更改 webpack.config.js 的 module 中的 rules 数组

```
rules: [
            // 这里是css文件的配置规则
            {
                // test设置需要匹配的文件类型，支持正则
                test: /\.css$/,
                // use表示该文件类型需要调用的loader(加载器)
                use: ['style-loader', 'css-loader'],
            },
            // 这里是less文件的配置规则
            {
                test: /\.less$/,
                use: ['style-loader', 'css-loader', 'less-loader'],
            }
        ]
```

在 css 文件夹下创建一个 index.less 文件，自定义内容为：

```
ul{
    li{
        color: white;
    }
}
```

在 index.js 中导入 index.less 文件

```
import "./css/index.less";
```

最后在终端执行 **npm run dev** 命令即可

#### 安装 sass-loader,node-sass 处理 scss 文件

##### 1. 安装包

```
npm install sass-loader node-sass -D
```

这一步可能会失败会报错（因为 npm 是从国外拿数据的），解决方案如下：

解决方案一：

设置全局镜像源：

```
npm config set sass_binary_site https://npm.taobao.org/mirrrors/node-sass/
```

然后再重新安装

```
npm install sass-loader node-sass -D
```

解决方案二：

```
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

然后再执行命令：

```
cnpm install sass-loader node-sass --save-dev
```

解决方案三：

安装 python, 安装目录必须在 C:/Python27

然后执行命令：

```
cnpm install sass-loader node-sass --save-dev
```

##### 2. 配置规则：更改 webpack.config.js 的 module 中的 rules 数组

```
module: {
        // rules里面存放的是对象
        rules: [
            // 这里是css文件的配置规则
            {
                // test设置需要匹配的文件类型，支持正则
                test: /\.css$/,
                // use表示该文件类型需要调用的loader(加载器)
                use: ['style-loader', 'css-loader'],
            },
            // 这里是less文件的配置规则
            {
                test: /\.less$/,
                use: ['style-loader', 'css-loader', 'less-loader'],
            },
            // 这里是scss文件的配置规则
            {
                test: /\.scss$/,
                use: ['style-loader', 'css-loader', 'sass-loader'],
            }
        ]
    }
```

在 css 文件夹下创建 index.scss 文件（后缀名一定是 scss，而不是 sass），自定义内容：

```
ul {
    li{
        border: 1px solid blue;
        margin: 10px auto;
    }
}
```

在 index.js 中引入 index.scss 文件

```
import "./css/index.scss";
```

最后在终端执行 **npm run dev** 命令即可

#### 打包样式表的图片以及字体字体

使用 url-loader 和 file-loader 来处理

##### 1. 安装包

```
cnpm install url-loader file-loader -D
```

如果在这里报错的话，就试试先执行下面的命令再进行安装,

```
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

##### 2. 配置规则：更改 webpack.config.js 的 module 中的 rules 数组

```
// 图片的配置规则
            {
                // 设置图片的后缀名
                test: /\.jpg|png|gif|bmp|ttf|eot|svg|woff|woff2$/,
                // limit用来设置字节数，只有小于limit值的图片，才会转换
                use: "url-loader?limit=16940",
            }
```

在 index.html 中添加一个 div

```
<div class="box">

    </div>
```

在 index.css 中添加相关样式

```
.box {
  width: 200px;
  height: 200px;
  border: 1px solid firebrick;
  background-image: url(../img/sortPhone_03.png);
}
```

最后在终端执行 **npm run dev** 命令即可

如果在 index.html 中直接添加图片, 浏览器中是不显示的

```
<img src="./img/sortPhone_03.png" alt="">
```

因为装了插件，所以图片到内存里面去了，地址就要改一改

```
<img src="/src/img/sortPhone_03.png" alt="">
```

最后在终端执行 **npm run dev** 命令即可

#### 打包 js 文件中的高级语法

如果是 webpack 4 版本的话，可能要装一下, 如果是 5 的话，可以不用搞这一步

安装 babel 系列的包

##### 1. 安装 babel 转换器

```
npm install babel-loader @babel/core @babel/runtime -D
```

##### 2. 安装 babel 语法插件包

```
npm install @babel/preset-env @babel/plugin-transform-runtime @babel/plugin-proposal-class-properties -D
```

##### 3. 在项目根目录下创建并配置 babel.config.js 文件

```
module.exports={
	presets:["@babel/preset-env"],
	plugins:["@babel/plugin-transform-runtime","@babel/plugin-proposal-class-properties"]
}
```

##### 4. 配置规则：更改 webpack.config.js 的 module 中的 rules 数组

```
{
test:/\.js$/,
use:"babel-loader",
exclude:/node_modules/
}
```

### Vue 单文件组件

vue 安装 Vetur 插件可以使得. vue 文件中的代码高亮

配置. vue 文件的加载器

##### 1. 安装 vue 组件的加载器

```
npm install vue-loader vue-template-compiler -D
```

上一个命令不可以的话就试试下面这个命令

```
cnpm install vue-loader vue-template-compiler -D
```

##### 4. 配置规则：向 webpack.config.js 添加一些内容

```
const VueLoaderPlugin = require("vue-loader/lib/plugin");
const vuePlugin = new VueLoaderPlugin();
module.exports ={
plugins: [htmlPlugin, vuePlugin],
module: {
 rules:{
 ......其他规则
 // vue文件的配置规则
            {
                test: /\.vue$/,
                loader: "vue-loader"
            }
 }
}

}
```

#### 在 webpack 中使用 vue

##### 1. 安装 vue

```
npm install vue -S
```

上面命令报错就使用下面的命令

```
cnpm install vue -S
```

##### 2. 在 index.js 中导入 vue

```
import Vue from "vue";
```

##### 3. 在 inde.html 中创建：

```
<div id="app">

    </div>
```

##### 在 src 文件夹下创建 test.vue 文件，内容为

```
<template>
  <!-- 这里面内容自定义 -->
  <div>hello</div>
</template>
<script>
export default {};
</script>
<style>
</style>
```

##### 4. 在 index.js 中创建 Vue 实例对象，并引入 test.vue

```
import test from "./test.vue";
const vm = new Vue({
    el: "#app",
    // render函数渲染单文件组件
    //h()里面test是上面import来的test
    render: (h) => h(test),
})
```

可能有个报错：

```
DevTools failed to load SourceMap: Could not load content for webpack://package/node_modules/sockjs-client/dist/sockjs.js.map: HTTP error: status code 404, net::ERR_UNKNOWN_URL_SCHEME
```

```
module.exports ={
	plugins: [htmlPlugin, vuePlugin],
	//加上这个配置
	devtool: 'inline-source-map',
}
```

#### 使用 webpack 打包发布项目

##### 1. 配置 package.json

```
"scripts": {
    "dev": "webpack serve --open Chrome.exe",
    //"build": "webpack -p"		这是webpack4版本的写法
    "build": "webpack"
  },
```

##### 2. 项目打包

```
npm run build
```

在项目打包 (**npm run build**) 之前, 可以将 dist 目录删除，打包完成后会生成全新的 dist 目录

**dist** 文件夹下生成一个 **bundle.js** 文件和一个 **index.html** 文件，就说明打包成功