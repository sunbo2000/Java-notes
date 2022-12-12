# >>>>>>>>>>NPM

# 一、简介 

## 1、什么是NPM

NPM全称Node Package Manager，是Node.js包管理工具，是全球最大的模块生态系统，里面所有的模块都是开源免费的；也是Node.js的包管理工具，相当于前端的Maven 。





## 2、NPM工具的安装位置

我们通过npm 可以很方便地下载js库，管理前端工程。

Node.js默认安装的npm包和工具的位置：Node.js目录\node_modules

- 在这个目录下你可以看见 npm目录，npm本身就是被NPM包管理器管理的一个工具，说明 Node.js已经集成了npm工具


```
#在命令提示符输入 npm -v 可查看当前npm版本
npm -v
```

# **二、使用npm管理项目**

## 1、创建文件夹npm

## **2、项目初始化**

```
#建立一个空文件夹，在命令提示符进入该文件夹  执行命令初始化
npm init
#按照提示输入相关信息，如果是用默认值则直接回车即可。
#name: 项目名称
#version: 项目版本号
#description: 项目描述
#keywords: {Array}关键词，便于用户搜索到我们的项目
#最后会生成package.json文件，这个是包的配置文件，相当于maven的pom.xml
#我们之后也可以根据需要进行修改。
#如果想直接生成 package.json 文件，那么可以使用命令
npm init -y
```

## **2、修改npm镜像**

NPM官方的管理的包都是从 http://npmjs.com下载的，但是这个网站在国内速度很慢。

这里推荐使用淘宝 NPM 镜像 http://npm.taobao.org/ ，淘宝 NPM 镜像是一个完整 npmjs.com 镜像，同步频率目前为 10分钟一次，以保证尽量与官方服务同步。

**设置镜像地址：**

```
#经过下面的配置，以后所有的 npm install 都会经过淘宝的镜像地址下载
npm config set registry https://registry.npm.taobao.org 

#查看npm配置信息
npm config list
```

## **3、npm install命令的使用**

- 使用 npm install 安装依赖包的最新版，
- 模块安装的位置：项目目录\node_modules
- 安装会自动在项目目录下添加 package-lock.json文件，这个文件帮助锁定安装包的版本
- 同时package.json 文件中，依赖包会被添加到dependencies节点下，类似maven中的 \<dependencies\>

```
npm install jquery
```

- npm管理的项目在备份和传输的时候一般不携带node_modules文件夹

```
npm install #根据 package.json 中的配置下载依赖，初始化项目
```

- 如果安装时想指定**特定的版本**

```
npm install jquery@2.1.x
```

- devDependencies节点：开发时的依赖包，项目打包到生产环境的时候不包含的依赖
- 使用 -D 参数将依赖添加到 devDependencies 节点

```
npm install --save-dev eslint
```

或

```
npm install -D eslint
```

- 全局安装
- Node.js全局安装的npm包和工具的位置：用户目录\AppData\Roaming\npm\node_modules
- 一些命令行工具常使用全局安装的方式

```
npm install -g webpack
```




## **4、其它命令**


```
#更新包（更新到最新版本）
npm update 包名

#全局更新
npm update -g 包名

#卸载包
npm uninstall 包名

#全局卸载
npm uninstall -g 包名
```

 

# >>>>>>>>>>Babel

# 一、简介  

Babel是一个广泛使用的转码器，可以将ES6代码转为ES5代码，从而在现有环境执行执行。

这意味着，你可以现在就用 ES6 编写程序，而不用担心现有环境是否支持。

# 二、安装

## 安装命令行转码工具

Babel提供babel-cli工具，用于命令行转码。它的安装命令如下：

```
npm install --global babel-cli
#查看是否安装成功
babel --version
```



# **三、Babel的使用**

## 1、初始化项目


```
npm init -y
```

## 2、创建文件

src/example.js

下面是一段ES6代码：



```js
// 转码前

// 定义数据

let input = [1, 2, 3]

// 将数组的每个元素 +1

input = input.map(item => item + 1)

console.log(input)
```

## 2、配置.babelrc

Babel的配置文件是 .babelrc ，存放在项目的根目录下，该文件用来设置转码规则和插件，基本格式如下。

1

```js
{

    "presets": [],

    "plugins": []

}
```

presets字段设定转码规则，将es2015规则加入 .babelrc：


```js
{

    "presets": ["es2015"],

    "plugins": []

}
```

## 3、安装转码器

在项目中安装

```
npm install --save-dev babel-preset-es2015
```



## 4、转码


```
# 转码结果写入一个文件

mkdir dist1

# --out-file 或 -o 参数指定输出文件

babel src/example.js --out-file dist1/compiled.js

# 或者

babel src/example.js -o dist1/compiled.js

# 整个目录转码

mkdir dist2

# --out-dir 或 -d 参数指定输出目录

babel src --out-dir dist2

# 或者

babel src -d dist2
```



# >>>>>>>>>WebPack

# 一、什么是Webpack

Webpack 是一个前端资源加载/打包工具。它将根据模块的依赖关系进行静态分析，然后将这些模块按照指定的规则生成对应的静态资源。

从图中我们可以看出，Webpack 可以将多种静态资源 js、css、less 转换成一个静态文件，减少了页面的请求。 

# 二、Webpack安装

## 1、全局安装


```
npm install -g webpack webpack-cli
```

## 2、安装后查看版本号


```
webpack -v
```

# 三、初始化项目

## 1、创建webpack文件夹

进入webpack目录，执行命令


```
npm init -y
```

## **2、****创建src文件夹**

## 3、**src下****创建common.js**


```
exports.info = function (str) {

    document.write(str);

}
```

## **4、src下创建utils.js**


```
exports.add = function (a, b) {
    return a + b;
}
```

## **5、src下创建main.js**


```
const common = require('./common');
const utils = require('./utils');
common.info('Hello world!' + utils.add(100, 200));
```

# **四、JS打包**

## **1、webpack目录下创建配置文件****webpack.config.js**

以下配置的意思是：读取当前项目目录下src文件夹中的main.js（入口文件）内容，分析资源依赖，把相关的js文件打包，打包后的文件放入当前目录的dist文件夹下，打包后的js文件名为bundle.js


```
const path = require("path"); //Node.js内置模块
module.exports = {
    entry: './src/main.js', //配置入口文件
    output: {
        path: path.resolve(__dirname, './dist'), //输出路径，__dirname：当前文件所在路径
        filename: 'bundle.js' //输出文件
    }
}
```

## **2、命令行执行编译命令**


```
webpack #有黄色警告
webpack --mode=development #没有警告
#执行后查看bundle.js 里面包含了上面两个js文件的内容并惊醒了代码压缩
```

也可以配置项目的npm运行命令，修改package.json文件


```
"scripts": {
    //...,

    "dev": "webpack --mode=development"
 }
```

运行npm命令执行打包


```
npm run dev
```

## **3、webpack目录下创建index.html**

引用bundle.js


```
<body>
    <script src="dist/bundle.js"></script>
</body>
```


## 4、浏览器中查看index.html

# **五、CSS打包**

## **1、安装style-loader和 css-loader**

Webpack 本身只能处理 JavaScript 模块，如果要处理其他类型的文件，就需要使用 loader 进行转换。

Loader 可以理解为是模块和资源的转换器。

首先我们需要安装相关Loader插件，css-loader 是将 css 装载到 javascript；style-loader 是让 javascript 认识css


```
npm install --save-dev style-loader css-loader 
```

## **2、修改webpack.config.js**


```
const path = require("path"); //Node.js内置模块
module.exports = {
    //...,
    output:{},
    module: {
        rules: [  
            {  
                test: /\.css$/,    //打包规则应用到以css结尾的文件上
                use: ['style-loader', 'css-loader']
            }  
        ]  

    }

}
```

## 3、在src文件夹创建style.css


```
body{
    background:pink; 
}
```

## **4、修改main.js** 

在第一行引入style.css


```
require('./style.css');
```

## 5、浏览器中查看index.html 

看看背景是不是变成粉色啦？ 

# >>>>>>>>跨域问题解决方案

**1.在后端接口 controller 添加注解 (常用)**

```java
@CrossOrigin
public Controller{ ... }
```

**2.使用网关解决**