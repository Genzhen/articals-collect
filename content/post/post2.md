---
# Common-Defined params
title: "Webpack核心概念讲解"
date: "2019-12-03"
description: "在现在，做前端不用webpack好像被时代抛弃了一样，每天开发的时候npm run dev,上线的时候npm run build,总之执行一个命令静静的等着打包就好了，甚至根本不需要知道执行命令后整个过程究竟干了什么。webapck就像一个黑盒，需要小心翼翼遵循它的配置，配好了就是万幸。可能对于刚学webapck来学，在配置中有可能会配到怀疑人生，会有各种报红，或者会遇到一些依赖的坑等。"
categories:
  - "Category 1"
tags:
  - "Test"
menu: side # Optional, add page to a menu. Options: main, side, footer

# Theme-Defined params
comments: false # Enable Disqus comments for specific page
authorbox: false # Enable authorbox for specific page
toc: false # Enable Table of Contents for specific page
mathjax: true # Enable MathJax for specific page
---

This article offers a sample of basic Markdown syntax that can be used in Hugo content files, also it shows whether basic HTML elements are decorated with CSS in a Hugo theme.

# Webpack核心概念讲解

## webpack是什么

在现在，做前端不用webpack好像被时代抛弃了一样，每天开发的时候npm run dev,上线的时候npm run build,总之执行一个命令静静的等着打包就好了，甚至根本不需要知道执行命令后整个过程究竟干了什么。webapck就像一个黑盒，需要小心翼翼遵循它的配置，配好了就是万幸。可能对于刚学webapck来学，在配置中有可能会配到怀疑人生，会有各种报红，或者会遇到一些依赖的坑等。

现在都在使用webpack，我们有没有思考过，我们为什么要使用webpack?它究竟解决了什么痛点？

结合日常搬砖的场景来分析下：

- 开发的时候需要一个开发环境，要是我们修改一下代码保存之后浏览器就自动展现最新的代码那就好了(**热更新服务**)
- 本地写代码的时候，要是调后端接口不跨域就好了-**代理服务**
- 为了跟上时代，要是能用上什么ES678N等新东西就好了-**编译服务**
- 项目要上线了，要是能一键压缩代码、图片什么的就好了-**压缩打包服务**
- 等等各种服务。。。

有了webpack就可以帮助我们去实现上边的一系列任务，这也是我们为什么使用webapck的理由，虽然也有grunt,parcel，rollup等构建工具，但是webpack给他们提供了更多，更强大的功能，生态也是不断的壮大与丰富。

**什么是webpack?**

- **本质上是一个JS应用程序的静态模块打包器（static module bundler）**
- **在Webpack处理应用程序是，它会在内部创建一个依赖图（dependency graph），映射到项目需要的每个模块，然后将所有这些依赖生成到一个或多个bundle**

## 什么是模块化

在webpack中像，一切皆模块可以理解为

- webpack模块
  - ES2015的import语句
  - CommonJS require语句
  - AMD define和require语句
  - css/sass/less文件中的import语句
  - 样式（url(...)）或html文件（<img src=..>）中的图片链

webpack在进行模块打包构建的过程，会把源代码转换成发布到线上的课执行javaScript,css.html代码，基本包括以下内容：

1.代码转换：TypeScript 编译成 JavaScript、SCSS或Less 编译成 CSS 等。

2.文件优化：压缩 JavaScript、CSS、HTML 代码，压缩合并图片等。

3.代码分割：提取多个页面的公共代码、提取首屏不需要执行部分的代码让其异步加载。

4.模块合并：在采用模块化的项目里会有很多个模块和文件，需要构建功能把模块分类合并成一个文件。

5.自动刷新：监听本地源代码的变化，自动重新构建、刷新浏览器,nodemon。

6.代码校验：在代码被提交到仓库前需要校验代码是否符合规范，以及单元测试是否通过。

7.自动发布：更新完代码后，自动构建出线上发布代码并传输给发布系统。

构建其实是工程化、自动化思想在前端开发中的体现，把一系列流程用代码去实现，让代码自动化地执行这一系列复杂的流程。 构建给前端开发注入了更大的活力，解放了我们的生产力,更加方便了我们的开发。

## 搭建webpack环境

webpack是基于nodejs开发的模块化打包工具，所以本质上是由node实现的，所以首先我们要安装node环境，

> 注意：
>
> 模块化的规范有：CommonJS,AMD,ES6 Module,CMD,UMD等等
>
> - CommonJS:是NodeJS广泛使用的一套模块化规范，是一种同步加载模块依赖的方式
>   - require引入一个模块
>   - exports导出模块内容
>   - module模块本身
> - AMD：是js模块加载库RequireJS提出并完善的一套模块化规范，AMD是一条异步加载模块依赖的方式
>   - id 模块的id
>   - dependencies 模块依赖
>   - factory 模块的工厂函数，即模块的初始化操作函数
>   - require 引入模块
> - ES6 Module：ES6推出的一套模块化规范
>   - import 引入模块依赖
>   - export 模块导出
> - CMD:国产库SeaJS提出的一套模块化规范
> - UMD:兼容CommonJS和AMD的一套规范
>
> 目前多数模块的封装，即可以NodeJS环境运行，又可以在web环境运行。一般会采用UMD规范。

### 安装Node

**进入node官网[nodeJS](https://nodejs.org/en/),安装LTS版本，安装长期稳定版本**，直接下载直接安装就可以了，window/mac都是傻瓜式安装点击下一步下一步就可以了。current版本是不稳定版本，虽然可以尝鲜一些新特性，但是有可能会存在一些小问题。

如果我们仔细阅读webpack官方文档，提升webapck打包速度里边有两个非常重要的点：

- 保持node的版本尽量新
- 保持webpack的版本尽量新

不过如果是旧的项目，版本的升级也是需要人力与精力做版本迁移的。新的项目的话可以启用新的版本。

安装完成之后我们来验证下我们是否安装成功了

```js
node -v // 查看node版本号
npm -v // 查看npm版本号
// 如果npm版本号也显示，说明我们node的npm工具也安装好了

```

### 开始webpack学习

**安装完成后我们来初始化一个项目，开始学习webpack**

```js
mkdir webpack_01
cd webpack_01
npm init  // 回车
// npm init 是什么意思呢？之前我们已经安装好了npm这个包管理工具，它可以帮助我们以node的规范创建一个项目。
// 现在大家如果想用webpack去管理一个项目，首先需要让你的项目符合node的规范，当运行npm init你的项目就符合node的规范了
npm init -y // 就不用手动去回车，会自动生成一个默认的配置项
```

```json
// 现在项目中就已经有了package.json文件
{
  "name": "webpack_01", // 项目名称
  "version": "1.0.0", // 项目版本号
  "description": "", // 项目描述
  "main": "index.js", 
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "yidegn",   
  "license": "ISC"  //许可声明， 如果想开源可以写成IMT,写成ISC也是可以的
}

// 现在这里边我们可以加点其他配置项
{
  "private":true,// 这个项目是私有的它不会被发布到npm的仓库中去
}
```

项目初始化完成，接下来开始安装webpack,打开终端工具

```js
npm install webpack webpack-cli -g  // 全局安装(不推荐)
// 全局安装的问题：如果我们在同时开发两个项目，两个项目中的webpack版本不一样，这样在启动项目的时候有可能就会遇到一些版本匹配的问题
npm uninstall webpack webpack-cli -g

npm install webpack webpack-cli -D  // 在项目中安装webpack(推荐)  --save-dev/-D  安装到开发环境
npx webpack -v  //可以使用npx工具运行这个项目中的安装的webpack版本
// npx这个工具会帮助我们从当前项目的nodemodules目录下去找webpack,而不是去全局的路径中去找
npm install webpack@4.16.1 webpack-cli -D // 安装指定版本，在@后边加版本号即可
npm info webpack // 如果想知道有哪些版本可以安装，可以使用这个命令进行查看
```

注意：

> npm安装走的是国外网络，比较慢容易出现安装失败的现象。可以使用yarn安装`npm install yarn -g`
>
> 或者使用nrm工具快速切换npm源，`npm install -f nrm`
>
> ```js
> // nrm使用
> npm ls 查看可选源
> nrm test npm 测试源的速度
> npm use cnpm 使用哪个源
> ```
>
> 有时可能会出现安装失败的情况，可以采用手机分享热点再安装的方式
>
> webpack-cli的作用就是可以让我们在命令行中执行命令，在webpack3的时候，ebpack本身和它的CLI以前都是在同一个包中，但在第4版中，他们已经将两者分开来更好地管理它们。所以webpack4版本的时候我们需要进行单独安装一下

## 开始webpack配置

我们简单写个小例子来看一下

```js
//index.js
import {list} from "./list";
list();
//list.js
export const list = ()=>{
    console.log("我是list文件");
}

// 运行
npx webpack index.js   // 意思是帮我来打包index.js这个文件
// 我们发现文件被自动处理打包了，这是因为执行的是webpack的默认配置，webpack团队一直在丰富webpack的默认配置选项，如果我们想自己写自己的配置文件应该如何做呢？

//我们说过webpack是一个模块化打包工具，它会帮我们把模块打包到一起。但是当我们引入一个js的时候和引入一个css或图片的时候打包的方式肯定是不同的。webpack团队虽然一直在丰富webpack的默认配置，让webapck开箱即用，可以不需要任何配置文件。然而，webpack会假定项目的入口起点为src/index,然后在dist/main.js输出结果，并且在生产环境开启压缩优化。但是通常在项目的开发中我们需要更多个性化的配置需求，所以我们就需要在项目的根目录下创建一个webpack.config.js文件，wepback会自动使用它。webpack.config.js是webpack配置的默认配置文件。


// 我们可以在根项目目录下新建一个webapck.config.js
//新建好之后我们直接来运行npx webpack发现就报错了，这因为webpack不知道打包的入口文件是哪一个了，现在我们可以自己在webpack.config.js中进行打包的配置


```

### 配置打包入口与出口

```js
//webpack.config.js
const path = require('path');
module.exports = {
  // 默认是production,打包的文件默认被压缩。开发时设置为development,不被压缩
  mode:'production',
  // 入口文件
  entry:'./index.js',
  // 打包文件的输出文件
  output:{
    // 自定义打包文件输出名
    filename:'index.js',
    // 这里要写绝对路径，需要使用node的一个核心模块path,__dirname就是项目的根目录,resolve方法就是对路径的一个转换，这是node提供的一些文件处理方法
    // path.resolve是nodeJs里面方法，可以连接两个相对路径并生成绝对路径；
    // 打包的文件会被输出到项目根目录下的dist文件下，
    // 可以改成path:"./bundle",报什么错
    path:path.resolve(__dirname,'dist')
  }
}

// 流程分析
1.运行npx webpack ,wepbakc并不知道怎么去打包
2.找默认的配置文件，webpack.config.js
3.按配置的进行打包


// 如果我们把这个webpack.config.js改一个名字，比如myconfig.js,现在再运行就报错了
// 因为找不到默认的配置文件了，webpack会默认去找webpack.config.js这个配置文件，那如果我们想用我们自己的配置文件应该怎么办呢？

// npx webpack --confg myconfig.js
```

> 注意：
>
> 配置文件也是可以自己指定的，`npx webpack --config  自定义配置文件(./webpack.myconfig.js)`

**这样最简单的一个webpack打包配置就配置完成了，优化下项目目录**

**一般像index.js,list.js是我们的源代码，放在src目录下，所以我们调整下**

```js
|-webpack_01
	|-src
		|-index.js
		|-list.js
	|-webpack.config.js
	|-package.json

// 相对应的webpack.config.js中的路径配置也要改一下
{
  entry:'./src/index.js'
}

// 在之前使用vue或webpack我们可能都没有使用过npx webpack这样执行，下面我们通过npm 的scripts命令来简化下
{
  "scripts":{
   	"build":'webpack'
  }
}

// 命令行
npm run build

//现在一样可以进行打包，可能有的同学可能有疑问了，这样执行的webpack是项目中的webapck版本还是全局的webpack版本，其实在scripts中运行的webpack命令，它会优先在项目的node_modules中去找，使用的是项目中安装的webpack版本，所以和npx webpack是一样的效果。
// 如果我们做过vue或react的项目开发，现在的这个目录是不是就和他们是挺像的了。

```

在dist目录下新建一个html页面引入打包后的js看是否正常运行

```html
// 在dist目录下新建一个index.html文件,引入打包的js，在浏览器中打开
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>webapck</title>
  </head>
  <body>
    <script src="./bundle.js"></script>
    <script>
        
    </script>
  </body>
</html>

```

### 每次执行打包命令，在命令行中都有一些输出信息，这些信息都是什么意思

```js
Hash: d29c7c464e87b7c2ce5a   // 哈希值，对应的本次打包的一个唯一的值
Version: webpack 4.41.2  // webpack使用的webpack版本
Time: 348ms  // 整体打包的耗时
Built at: 2019-11-26 2:21:45 PM
    Asset       Size  Chunks             Chunk Names   // 打包后的文件名，大小，id,入口文件名
bundle.js  983 bytes       0  [emitted]  main
Entrypoint main = bundle.js
[0] ./src/index.js + 1 modules 104 bytes {0} [built]
    | ./src/index.js 39 bytes [built]
    | ./src/list.js 65 bytes [built]

// 其实入口文件是main.js,是因为默认是这样的，打包出来是一样的
entry:{
  main:"./src/index.js"
}
// chunks,有时候我们会打包出多个bundle文件，每个bundle文件都有自己对应的id

```

## 我们打开webpackd的官网来看一下

在这里有各种配置项，也有官方推荐的各种loaders,plugins，每个loader,plugins又有自己的各种配置项，除了官方推荐的，在npm上还有好多个人开发发布的一些第三loader,plugins,所以如果我们想一下全部掌握是不可能的，只能是用到哪个我们去看他的文档，看他的使用方式，进行使用即可。所以在进行webpack项目搭建选择一些相应的插件loader时也是需要进行实践，进行对比的，一些库还是有些坑的，不过开发过程就是采坑的过程，踩的坑多了经验也就自然多了。

这里我们来学习下webpack一些核心的概念，一些比较常用的配置，plugins,loader。

我们已经明确的知道webpack是一个模块化的打包工具，像模块不只是js文件，图片文件，css文件也是一个模块。在webapck中就是一切皆模块。

看下我们的代码，现在之后js代码，如果我们现在有图文文件呢？

```js
// 新建一个assets文件夹用来存放静态资源
// assets/image  //存放图片
// 放一张图片到文件夹下

// index.js
import { list } from "./list";
list();
import img from "./assets/images/1.png";
console.log(img);
// 现在运行npm run build 报错
//webpack是默认知道如何打包处理js模块的，但是对于图片文件却不知道怎么打包，既然webpack不知道怎么打包，那肯定是需要我们告诉他，在配置文件中进行相应的配置 

```

```js
// 既然要对模块进行处理，在配置文件中对模块进行相应处理配置
// webpack.config.js
module:{
  // 里边会有很多规则,规则的写法基本上是固定的,配置模块规则
  // 假设我们的文件是jpg/png结尾的我们先写一个test:指定检测什么样的文件？
  // 找到文件了然后就是需要进行处理？这里就需要使用相应的loader了
  // 需要什么loader呢？这里我们需要一个file-laoder这样的一个loader
  // 现在用到了file-loader但是还没有进行安装，所以我们需要安装一下file-laoder
  // npm install -D file-loader
  // 现在来运行npm run build
  // 发现成功被打包了，图片被打包到了dist目录下
  // 我们是如何知道file-loader是可以进行打包图片文件的呢？
  // 从官网中进行阅读
  rules:[
    {
      test:/\.(jpg|png)$/,// 正则匹配要进行处理的文件
      use:{
        loader:"file-loader"
      }
    }
  ]
}

//file-loader底层执行的一个流程分析
1.当发现在代码中引入了一个图片这样的一个模块的时候
2.首先会帮你打包移动到dist目录里边，并且会改一个名字，这个名字我们也是可以自定义的
3.当把这个图片移动到dist目录下之后它会得到这个图片的名称
4.然后把这个名称作为一个返回值返回给我们引入模块的这个img变量

// file-loader不仅可以处理图片资源，他可以处理任何类型的静态资源文件，git,.ttf等等

比如
import ttf from "./assets/images/1.ttf";
test:/\.ttf$/,
  
//通过这个例子我们应该也知道了，如果是其它的比如less
import './index.less';
//是以.less结尾的那我们就需要相应的loader了，可以查看官网我们知道
//需要less-loader
use:{
  loader:"less-laoder"
}

```

### 1.什么是loader

通过这个例子，那什么是loader呢？

`loader`官方解释是文件预处理器，通俗点说就是webpack在处理静态文件的时候，需要使用 `loader` 来加载各种文件，比如： ,`css `需要使用`css-loader `、 `style-loader` 等等，html需要html-loader,可以看下官网

其实loader就是一个特定的处理方案，webapck使用loader来处理相应的模块，比如处理css,sass,less,jsx等。webpack本身是不不知道如何对模块进行处理的，通过loader，webpack就知道了。

### 刚刚只是把图片通过file-loader进行了一个简单处理，看下打包后的图片

前边我们提到1.png被打包成了一个默认的名字，并且位置也是直接打包到了dist的目录下，如果我们想自己定义个名字，或者是使用原来图片名字，并且是打包到dist的assets中应该怎么进行配置呢？

在loader配置中还有个options配置选项，可以配置打包后的文件名，打包的文件目录等

```js
  {
  test: /\.(jpg|png)$/,
  use: {
    loader: "file-loader",
    // 选项配置
    options: {
      // name,ext可以理解为就是占位符,占位符有很多，我们可以看到webapck官方的file-laoder
      // name-原先文件的名字
      // ext-文件的类型
      // 这里还可以加hash值，比如我们再实际开发中如果更新了一张图片，还是同样的名字，就有可能由于缓存的原因，用户那边的资源一直还是之前的图片，这里我们就可以加上hash，每次打包如果都是会有个新的hash值
      name: "[name].[hash5].[ext]",
      outputpath: "assets/"
    }
  }
  }
// 如果我们在使用file-loader的过程中有其它需求，可以看官网的api，需要什么配置什么

```

我们都知道在实际开发的时候，其实对于图片资源一般会有一些优化处理，比如很小的图片icon这种图片会将其转成为base64,如果开发中有这个需求？我们要怎么处理？

这时候需要url-loader了，url-loader除了能做file-loader的事情之外，还能做其它的事情。每个loader都有自己擅长的，所以要对图片进行进一步的处理我们就要换一个loader了.如何使用呢？

```js
use: {
          loader: "url-loader",
          options: {
            name: "[name].[hash:5].[ext]",
            outputPath: "assets/"
          }
  }

//执行npm run build
//这时候并没有打到目录中，查看bundle.js，发现，图片以base64的形式被打到了里边
//这样显然是不合适的，需求是很小的图片几kb这种才打成base64,几百k的就正常被打到目录中就可以了
// 生成base64后虽然可以减少一次请求但是js的文件就会特别大了
// 继续配置
  options: {
    name: "[name].[hash:5].[ext]",
    outputPath: "assets/",
    limit: 2048  // 小于20kb的会打包成base64,大于就打包成文件格式
  }

```

### 图片这样处理完，基本就达到我们的要求了，现在引入样式文件



```js
// src/assets/css/index.css
.box {
  border: 2px solid red;
  width: 200px;
  height: 200px;
}

//src/index.js
import "./assets/css/index.css";


```

**现在运行肯定是会报错的，会提示我们需要相应的loader去处理。既然提示我们了，那我们就到模块配置中去配置需要的loader。在打包css的文件的时候一般需要一个style-loader,css-loader**

```js
{
  test: /\.(css|less|scss)/,
   // 如果不配置相关选项的时候，也可以是loadr对应数组的方式
   // 注意的是执行顺序，loader的执行顺序是从右到左，从下到上的
   // css-loader加载.css文件
    // style-loader 使用<style>将css-loader内部样式注入到我们的HTML页面
  loader: ["style-loader", "css-loader"]
}
// 配置完成后发现打包成功了,打包的css文件在bundle.js中。


// 现在项目开发中对于scss,less使用越来越多，需要怎么处理，
// scss文件也是一样的，使用相应的loader就可以
//配置上
loader: ["style-loader", "css-loader", "sass-loader"]

// 运行npm run build 
// 报错，按着错误提示安装node-sass ,在运行就可以了
// 所以在配置的时候，我们一定要仔细的看下提示错误，一般提示缺少什么包就按提示安装即可，
// 有时候如果报各种缺包错误，就把node_modules删除了，再npm install 重新安装

// 这个的执行过程基本就是
// sass其实默认使用的是node-sass将scss编译成css,css-loader在进行加载转换，最后style-loader，通过style的方式将样式注入到html的style中，可以审查一下元素看一下

```

在正常的项目开发中，css样式一般像一些transform，animtion等我们都要设置浏览器兼容，比如：-webkit-transform,只需要安装对应的loader就可以

**postcss-loader**

```js
// 安装postcss-loader
//按着官网提示需要在根目录新建一个postcss.config.js
module.exports = {
  plugins:[
    require("autoprefixer")
  ]
}

//这个选项也是可以配置在loader的options选项中的
 {
        test: /\.(css|less|scss)/,
        loader: [
          "style-loader",
          "css-loader",
          {
            loader: "postcss-loader",
            options: {
              plugins: [require("autoprefixer")]
            }
          },
          "sass-loader"
        ]
   }

// 运行npm run build可以达到一样的效果

```

这里我们再学一点css-loader的其它配置项,比如我们经常在其它脚手架配置中常见的一个配置

css-loader配置选项中的importLoaders，这个是对于在css文件中@import进来的资源的一个处理，是否从哪个laoder进行处理

```js
{
  loader: "css-loader",
  options: {
     // 0 => no loaders (default);
    // 1 => postcss-loader;
    // 2 => postcss-loader, sass-loader
    importLoaders: 2  // 设置后边从哪个loader开始将导入的文件进行解析
  },
    {
      loader: "postcss-loader",
      options: {
        plugins: [require("autoprefixer")]
      }
    },
   "sass-loader"
},
  
// demo.scss
@import "./demo1.scss"

// 如果我们不设置这个importLoaders,里边通过import引入的另一个.scss样式就会直接从css-loader开始解析，而不是从post-loader开始解析



```

现在在写css的时候我们为了避免全局样式冲突，无法共享变量等问题，有的项目中我们会启用css Modules的方式，启用css modules如何让webapck识别处理呢？

```js
// css module的方式
//index.css
.color{
  color:blue;
}

// index.js
import styles from './index.css'; // 导出对象的格式是键值对 

// jsx
// 使用转化后的类名
<div class={styles.color}></div>


// css-loader配置选项中开启
{
  loader: "css-loader",
  options: {
    importLoaders: 2,
    modules: true
  }
},
  
// 这样配置之后就可以对css modules处理了

```

项目中还有一些字体文件，也是可以通过相同的思路就可以了,

```js
// assets/fonts

// index.js
import "./assets/fonts/1.ttf";
//webapck.config.js
  {
        test: /\.(ttf|eto|svg|woff)/,
        use: {
          loader: "file-loader",
          options: {
            name: "[name].[ext]",
            outputPath: "assets/fonts"
          }
        }
  }

```

开发中基本是必须配置的图片loader,样式loader，字体loader我们都有一个认识了，

**在一开始我们还提到过插件plugin也是webpack的非常重要的功能，它可以让我们的打包更加快捷。其实插件是用来拓展webpack功能的。插件可以实现loader所不能完成的复杂功能，使用plugin丰富的自定义api,可以控制webapck编译流程的每个环节，实现对webpack自定义功能的扩展。比如说前边的我们的html都是手动在dist目录中创建的，并且里边的打包后的js资源也是手动引入的？那我们能不能实现自动化呢？**

答案肯定是可以的？我们来看下怎么使用plugin？

要实现我们这个需求需要用到插件：**html-webpack-plugin**

```js
// 安装插件
npm install -D html-webpack-plugin
// webpack.config.js中进行配置，可以看官网
const HtmlWebpackPlugin = require('html-webpack-plugin');
plugins:[
  new HtmlWebpackPlugin()
],
  
// npm run build
// 我们会发现dist目录中帮我们自动生成了一个html,并且打包完的js资源也给我们自动引入了

```

但是有个问题，就是我们的页面中一般还有其它资源怎么办？我们可以指定一个页面模板，每次生成的时候根据指定的模板去自动生成到打包目录中？

肯定是可以的

```js
// src下新建页面模板 html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>webapck</title>
  </head>
  <body>
    <div class="box1">我是盒子</div>
    <script></script>
  </body>
</html>

// webpack.config.js
  plugins: [
    new HtmlWebpackPlugin({
      template: "./src/index.html"
    })
  ]

// 还有好多其它选项都是可以配置的 https://github.com/jantimon/html-webpack-plugin

```

**还有个常用的清理的插件，也是必用的，就是在构建开始前先把之前的一些文件目录清理掉？比如在每次构建前把dist目录清除掉，重新生成，**

**因为如果修改生成的文件名什么的会生成一些重复的文件。**

**clean-webpack-plugin**，配置和上边的思路一样

```js
// webpack.config.js
const { CleanWebpackPlugin } = require("clean-webpack-plugin");
new CleanWebpackPlugin()

```

**其实webpack plugin是plugin提供给了开发者监听webpack生命周期并在特定事件触发时执行指定操作的能力**

webpack的插件是非常多的，不仅有官方推荐的还有第三方的插件，这么多插件全部掌握也不现实，一般就是开发中需要用到哪个插件，再去查就可以了，npm上可以直接搜插件的名字，然后就可以看怎么安装，怎么使用等等。

**比如现在随着项目的复杂，我们的入口文件可能会有多个，所以我们再来进行entry与output的配置？**

## 占位符

看官网

我们先来看下现在的情况

```js
// 现在  ,默认是main
entry:'./src/index.js'
// 等同于
entry:{
  main:'./src/index.js'
}

// 现在
output: {
  filename: "bundle.js",  // 输入文件的名字
  path: path.resolve(__dirname, "dist")
}
// 如果不写filename,默认生成生成main.js
output: {
  path: path.resolve(__dirname, "dist")
}

```

现在我们有两个入口文件怎么办呢？比如由有了一个demo.js

```js
//demo.js
console.log("我是demo入口文件");
//webpack.config.js
entry:{
   index: "./src/index.js",
   demo: "./src/demo.js"
}

// npm run build 是报错的，output中filename是只有一个固定的
//更改，前边学过占位符，在file-laoder的时候，这里是一样的,也是可以设置hash值
output: {
  filename: "[name].[hash:5].js",
  path: path.resolve(__dirname, "dist")
},

```

这样如果入口文件有多个，我们就可以实现多入口打包了，我们再平时的开发中，像一些js，css资源我们一般都是放到cdn上，现在是自动打到了页面里，如果我们想自动也把cdn的地址拼接上怎么设置

```js
// publicpath配置
output: {
  publicPath: "https://yideng.com/",
  filename: "[name].[hash:5].js",
  path: path.resolve(__dirname, "dist")
},

```

**对于需要按需加载或加载外部资源如图片、字体等文件等，publicpath是很重要的选项。如果指定了一个错误的值，则在加载资源的时候会收到404的错误，如果开启了webpack-dev-server也会默认从publicPath为基准，使用它来决定在哪个目录下启用服务，来访问webpack输出的文件。**

publicPath:`输出解析文件的目录，url 相对于 HTML 页面`，

publicPath 并不会对生成文件的路径造成影响，主要是对你的页面里面引入的资源的路径做对应的补全，常见的就是css文件里面引入的图片

### SourceMap的配置

现在文件打包，入口，与打包输出基本简单配置完成，现在有这么个问题，我们的文件都是经过webpack处理打包，最后打包后的js代码并不是源代码，这样在调试的时候，如果有报错，如何定位到我们的源码错误位置呢？

这个需求在开发中是必不可少的，webpack必然也提供了相应的调试工具。

webpack中提供了一个devtool配置选项，可以让我们快速的定位错误，可以定位到源文件中哪一行出错了。

```js
//webpack.config.js
{
  devtool:'cheap-module-eval-source-map'
}

// 在development模式下，是默认开启sourcemap的，可以定位到错误的第几行
// 先将其关闭，来看一下,现在是无法定位到源文件错误位置的，只能定位到打包后文件的错误的位置
{
  devtool:"none"
}
//改成source-map
{
  devtool:"source-map"
}

// 首先我们需要知道的一点是开启后会使构建变慢
// 开发环境是非常需要的，推荐cheap-module-eval-source-map，提示比较全，打包也比较快
// 生产环境可以选择关闭，是一个不错的选择，在生产环境中对于源码还是尽量不能暴露的，可以看一下，如果如果不关闭就使用，cheap-module-source-map，提示效果会好点，而且源码也不显示

```

在不同的开发环境需要配置不同的source-map

- devtoll:'none',关闭
- devetool:'source-map',开启映射打包，打包会变慢，在dist目录下会生成map文件
- devetool:'inline-source-map',和sourcemap一样，只是不单独生成.map文件，会将生成的映射文件以base64的形式插入到打包后的js文件中
- devetool:'cheap-inline-source-map',代码出错不会精确显示到第几行第几个字符出错，只显示第几行出错，会提高一些性能
- devetool:'cheap-module-inline-source-map',不仅定位业务代码报错，也会定位第三方模块和loader的一些报错
- devetool:'eva',执行效率最快，性能最好，但是对于复杂代码的情况下，提示内容不详细
- devetool:'**cheap-module-eval-source-map**',在开发环境推荐使用，提示比较全，打包速度快
- devetool:'**cheap-module-source-map**',在生产环境推荐使用，提示效果比较好

## 8.使用WebpackDevServer提升开发效率

继续来完善配置，解决一些重复的手动工作，提升一下开发效率，接下来我们通过配置和插件解决掉每次改动文件我们都要重新执行打包命令，还有手动刷新页面的的问题

```js
// --watch 监听文件变化自动打包
"scripts":{
  "dev:watch":"webpack --watch"
}

// 修改下index.js中的内容看一下效果
alert(2);

```

现在文件有修改自动进行打包，已经可以了，但是页面还需要手动刷新，下面再来解放下手动的劳动

webpack提供了一个 **devServer**的插件,在本地启一个服务，就像用xampp或者node在本地起服务一样，我们在开发中有的同学应该遇到过这种情况，如果直接本地路径启动，在进行ajax一些请求的时候是会报错的，需要在本地起个服务才行，这个插件就帮我们做到了，不仅可以自动刷新页面，还可以起个本地服务，看下官网是怎么使用的,

```js
 // webpack.config.js
devServer:{
    contentBase:path.join(__dirname,'dist'),  // 定义服务访问的目录
},
// 选项
 port:8081,// 端口也可以自定义
// 配置跨域，访问/api会被代理到localhost:8081,这个是非常有用的在开发中，配置选项中，可以看官网
 proxy:{
   '/api':'localhost:8081'
 }
  
// package.json
  "dev:server": "webpack-dev-server"
  
// 安装
  npm install -D webapck-dev-server
//注意：publicPath的路径设置：/

```

> 注意：
>
> **webpack-dev-server输出的文件只存在于内存中,不输出真实的文件，这样可以有效的提高打包的速度，提升开发效率**
>
> 提示一些警告，修改mode为开发环境

现在我们的需求通过devServer就实现了，可以监听文件的变化，自动打包，但是现在无论我们改动哪个模块的文件都会重新刷新页面，能不能实现如果只改了css代码，就只改变css这个模块的内容呢？

这个也是可以的 webpack提供了一个**热模块更新的功能，Hot Module Replacement**，这个功能在开发中还是非常有用的

当你对代码进行修改并保存后，`webpack` 将对代码重新打包，并将新的模块发送到浏览器端，浏览器通过新的模块替换老的模块，这样在不刷新浏览器的前提下就能够对应用进行更新。例如，我们在开发网站的时候，发现某个元素样式的需要调整，这个时候你只需要在代码里修改对应的样式，然后保存，在浏览器没有刷新的前提下，这个元素的样式已经进行了改变，这种开发体验就好像直接在 `Chrome` 里面直接对元素进行了改变一样，十分舒服。

看下官网是怎么配置的，需要在devServer配置中开启hot:true,还需要设置[`webpack.HotModuleReplacementPlugin`](https://webpack.js.org/plugins/hot-module-replacement-plugin/) 

- HMR开启步骤
  - 使用webpack-dev-server作为服务器启动
  - webpack.config的serServer中配置 hot:true
  - webpack.config的plugins增加HotModuleReplacementPlugin
  - 使用module.hot.accept增加HMR代码

```js
// webpack.config.js
	const webpack = require('webpack');
  devServer:{
    contentBase:path.join(__dirname,'dist'),
    hot:true,
  },
  plugins: [
    new webpack.HotModuleReplacementPlugin()
  ],
    
 // 也是可以在命令行中进行配置的
 "dev:server": "webpack-dev-server --hot"
    
 // index.js
import { list } from "./list";
list();
import img from "./assets/images/1.png";
import "./assets/css/index.css";
import "./assets/css/demo.scss";
import "./assets/fonts/1.ttf";
alert(123);
console.log(11111);
if (module.hot) {
  module.hot.accept("./list", () => {
    console.log("更新list文件模块");
    list();
  });
  // 关闭指定子模块的更新
  module.hot.decline("./list");
}
    
// 这样就实现了热模块更新，hmr，其实实现原理就是服务器与浏览器之前的一个通信，这个有兴趣的可以深入了解下，了解下底层的实现原理

```

> 注意：
>
> 在使用vue等框架的时候，不需要在写module.hot.accept(),因为它已经配置好了HMR，不需要自己写这部分代码了，开启模块热更新还是需要配置的。

### 现在热更新我们也配置完了，但是一开始我们对于js的处理我们没有进行一些额外的配置，像现在开发中我们经常使用一些Es6的语法等，新的语法让我们的开发变得更爽,有些api或者方法还存在一些兼容的问题，需要做一些转义处理操作，所以继续进行完善配置。我们接下来 的配置用到的这个库基本是处理js必备，就是babel,我们希望将ES6的语法转为浏览器可以支持的ES5,这时就需要babel帮我们做一些事情了。

前边我们配置过了css,图片，字体文件的，js的也是一样的套路。

```js
// 首先安装一些必须的库  用来处理ES6语法，将其编译为浏览器可以执行的js语法
// babel-loader  这个我们都知道像css-loader是一样的作用，需要对一些语言进行加载解析的
// @babel/core  babel的核心库，必须要安装的
// @babel/preset-env 来转换ES6语法
npm install -D babel-loader @babel/core @babel/preset-env

// webpack.config.js
{
  test: /.(js|ts)?$/,
  loader: "babel-loader",
  options: {
    presets: ["@babel/preset-env"]
  },
  exclude: /node_modules/
},
  
 //index.js  写一些es6代码
  const arr = [1, 2, 3];
	//Array.map 是 ES6 数组扩展中新增的方法！
	arr.map(item => {
 		 return item++;
	});
  

// 现在运行npm run build 发现const被转换成了var,箭头函数也被转换成了function
// 但是！Array.map 是 ES6 数组扩展中新增的方法！尽管 babel 把 ES6 的语法转换为了 ES5 ，旧版本浏览器依然不支持 Array.map！
// 如果想要让低版本浏览器也能支持 ES6 的新增扩展，正常情况下就需要自己模拟实现 ES6 的扩展方法，然后通过prototype将方法插入JS对象的原型中，扩展原型方法。
// 但是现在我们已经用了webpack,那肯定是实现自动是最好的了

```

```js
// babel提供了两种使用方法扩展的方式@babel/polyfill和@babel/plugin-transform-runtime
//@babel/polyfill 官方给出的解释为，它可以效仿一个完整的 ES2015+ 运行环境，并意图运行于一个应用中而不是一个库/工具。,就是垫片的意思可以理解为，
// 还是需要安装
npm install --save @babel/polyfill
// 安装完成之后，在代码头部引入就可以了
// index.js
import "@babel/polyfill";
const arr = [1, 2, 3];
arr.map(item => {
  return item++;
});

// 在运行打包看index.js发现已经被编译了
// 但是我们来看一下输出信息，包大小好几百k,而我们只写了几行代码
// 这是因为，@babel/polyfill 在模拟 ES2015 环境时，会将添加的 ES6 方法一起打包到 main.js 文件当中。

//在当前代码中我们只用到了map这个扩展方法，能不能只把这个方法的扩展打包呢？
// 肯定是可以的，在@babel/preset-env的配置中我们可以配置一个选项

// webpack.config.js
  {
    test: /.js$/,
    loader: "babel-loader",
    options: {
      presets: [
        [
          "@babel/preset-env",
          {
            useBuiltIns: "usage"
          }
        ]
      ]
    },
    exclude: /node_modules/
  },
    
    
   // 再打包发现又变小了

```

> `useBuiltIns`参数，支持3个值
>
> - `entry :` 只支持引入一次`@babel/polyfill`，如果多次引用会抛出错误
> - `usage :` 只会将文件中用到的 ES6 方法引用到文件中
> - `false :` 默认值，不会自动识别文件中使用的 ES6 方法，会将`@babel/polyfill`作为整体进行填充

```js
//现在的配置符合要求了，不过有个点需要注意一下，@babel/polyfill 虽然可以帮助开发者注入使用到的 ES6 方法，但是它是以 全局变量 的形式将方法注入。如果只是业务开发使用，问题并不是很大。但是如果在开发类库或UI组件时，全局注入的方式会造成变量的 全局污染 。这不是我们期望的结果。我们更希望在注入方法的同时保持全局环境的清洁。这就需要用到 @babel/plugin-transform-runtime 插件

// @babel/plugin-transform-runtime会以闭包的形式注入 ES6 方法，可以最大限度的保证全局环境不被污染
// 安装
npm install --save-dev @babel/plugin-transform-runtime
// npm install --save-dev @babel/runtime  number选项是false选这个
npm install --save @babel/runtime-corejs3

// webpack.config.js
  {
    test: /.js$/,
    loader: "babel-loader",
    options: {
      // presets: [
        // [
          // "@babel/preset-env",
          // {
          //   useBuiltIns: "usage"
          // }
        // ]
      // ],
      plugins: [
        [
          "@babel/plugin-transform-runtime",
          {
            corejs: 3,
            helpers: true,
            regenerator: true,
            useESModules: false
          }
        ]
      ]
    },
    exclude: /node_modules/
  },

// 运行打包看打包文件也是可以的
 babel的配置在options中配置是没有问题的，但是如果配置项太多的话，也容易造成代码比较混乱
 所以babel也支持在再项目根目录新建一个.babelrc中进行配置

```

```js
//.babelrc
{
  "plugins": [
    [
      "@babel/plugin-transform-runtime",
      {
        "corejs": 3,
        "helpers": true,
        "regenerator": true,
        "useESModules": false
      }
    ]
  ]
}

// webpack.config.js中注释

// 运行打包一样是没有问题的

// npm run dev:server
// index.js
console.log('我是index.js');
// 页面正常输出

```

有什么需求就是安装什么就可以了，注意的是，有时会因为版本不匹配的问题报一些错误，这个是要注意的

可以看官网，如果要解析什么js类型安装对应的包就可以，比如ts,react,

https://github.com/zyl1314/blog/issues/16 babel配置指南

现在我们详细来了解下babel

首先非常重要一点，我们要知道ES6增加的都有哪部分内容

语法和api

- ES6增加的内容可以分为语法和api两部分，搞清楚这点是非常重要的，新语法比如箭头函数、解构等：

```js
const fn = () => {}
let [a, b, ...rest] = arr;

```

- 新的api比如Map、Promise等：

- ```js
  const m = new Map();
  const p = new Promise(()=>{})
  
  ```

@babel/core

- @babel/core，看名字就知道这是babel的核心，没他不行，所以首先安装这个包

- ```js
  npm install @babel/core
  
  ```

- 它的作用就是根据我们的配置文件转换代码，配置文件通常为`.babelrc`,也可以在options中配置

Plugins和Presets

- Now, out of the box Babel doesn't do anything. It basically acts like const babel = code => code; by parsing the code and then generating the same code back out again. You will need to add plugins for Babel to do anything.

- 上面是babel官网的一段话，可以理解为babel是基于插件架构的，假如你什么插件也不提供，那么babel什么也不会做，即你输入什么输出的依然是什么。那么我们现在想要把剪头函数转换为es5函数只需要提供一个箭头函数插件就可以了

- ```js
  // .babelrc
  {
    "plugins": ["@babel/plugin-transform-arrow-functions"]    
  }
  
  //现在箭头函数已经被转换了
  
  // 如果再想要解析解构等语法又需要添加相应的插件
  // 这样句存在一个问题了
  ```

- 问题是有那么多的语法需要转换，一个个的添加插件也太麻烦了，幸好babel提供了`presets`，他可以理解为插件的集合，省去了我们一个个引入插件的麻烦，官方提供了很多presets，比如`preset-env`（处理es6+规范语法的插件集合）、`preset-stage`（处理尚处在提案语法的插件集合）、`preset-react`（处理react语法的插件集合）等

- ```js
  /* .babelrc */
  {
    "presets": ["@babel/preset-env"]    
  }
  ```

**preset-env**

- @babel/preset-env is a smart preset that allows you to use the latest JavaScript without needing to micromanage which syntax transforms (and optionally, browser polyfills) are needed by your target environment(s).

- 以上是babel官网对`preset-env`的介绍，大致意思是说`preset-env`可以让你使用es6的语法去写代码，并且只转换需要转换的代码。默认情况下`preset-env`什么都不需要配置，此时他转换所有es6+的代码，然而我们可以提供一个targets配置项指定运行环境：

- ```js
  /* .babelrc */
  {
    "presets": [
      ["@babel/preset-env", {
        "targets": "ie >= 8"
      }]
    ]    
  }
  ```

@babel/polyfill

- 现在再查看打包后的代码返现 promiseapi并没有被转换。我们已经设置了支持大于IE8，IE8还支持Promise。那是不可能的。还记得本文开头提到es6+规范增加的内容包括新的语法和新的api，新增的语法是可以用babel来transform转换的，但是新的api只能被polyfill，因此需要我们安装`@babel/polyfill`，再简单的修改下test.js如下：

- ```js
  /* index.js */
  import '@babel/polyfill'
  
  ```

- 现在代码可以完美的运行在ie8的环境了，但是还存在一个问题：`@babel/polyfill`这个包的体积太大了，我们只需要Promise就够了，假如能够按需polyfill就好了。真巧，`preset-env`刚好提供了这个功能：

- ```js
  /* .babelrc */
  {
    "presets": [
      ["@babel/preset-env", {
        "modules": false,
        "useBuiltIns": "entry",
        "targets": "ie >= 8"
      }]
    ]    
  }
  ```

- 我们只需给`preset-env`添加一个`useBuiltIns`配置项即可，值可以是`entry`和`usage`，假如是`entry`，会在入口处把所有ie8以上浏览器不支持api的polyfill引入进来，如下：

- ```js
  /* index.js */
  import '@babel/polyfill'
  
  const fn = () => {}
  new Promise(() => {})
  
  
  /* index.hash值.js */
  import "core-js/modules/es6.array.copy-within";
  import "core-js/modules/es6.array.every";
  import "core-js/modules/es6.array.fill";
  ...   //省略若干引入
  import "core-js/modules/web.immediate";
  import "core-js/modules/web.dom.iterable";
  import "regenerator-runtime/runtime";
  
  var fn = function fn() {};
  new Promise(function () {});
  
  ```

- 细心的你会发现transform后，`import '@babel/polyfill'`消失了，反倒是多了一堆`import 'core-js/...'`的内容，事实上，`@babel/polyfill`这个包本身是没有内容的，它依赖于`core-js`和`regenerator-runtime`这两个包，这两个包提供了es6+规范的运行时环境。因此当我们不需要按需polyfill时直接引入`@babel-polyfill`就行了，它会把`core-js`和`regenerator-runtime`全部导入，当我们需要按需polyfill时只需配置下`useBuiltIns`就行了，它会根据目标环境自动按需引入`core-js`和`regenerator-runtime`。

- 前面还提到`useBuiltIns`的值还可以是`usage`，其功能更为强大，它会扫描你的代码，只有你的代码用到了哪个新的api，它才会引入相应的polyfill：

- ```js
  /* .babelrc */
  {
    "presets": [
      ["@babel/preset-env", {
        "modules": false,
        "useBuiltIns": "usage",
        "targets": "ie >= 8"
      }]
    ]    
  }
  
  ```

- transform后的test-compiled.js相应的会简化很多：

- ```js
  /* test.js */
  const fn = () => {}
  new Promise(() => {})
  
  /* test-compiled.js */
  import "core-js/modules/es6.promise";
  import "core-js/modules/es6.object.to-string";
  
  var fn = function fn() {};
  new Promise(function () {});
  
  ```

- 事实上假如你是在写一个app的话，以上关于babel的配置差不多已经够了，你可能需要添加一些特定用途的`Plugin`和`Preset`，比如react项目你需要在`presets`添加`@babel/preset-react`，假如你想使用动态导入功能你需要在`plugins`添加`@babel/plugin-syntax-dynamic-import`等等，这些不在赘述。假如你是在写一个公共的库或者框架，下面提到的点可能还需要你注意下。

@babel/runtime

- 有时候语法的转换相对复杂，可能需要一些helper函数，如转换es6的class：

- ```js
  /* test.js */
  class Test {}
  
  
  /* test-compiled.js */
  function _classCallCheck(instance, Constructor) { if (!(instance instanceof Constructor)) { throw new TypeError("Cannot call a class as a function"); } }
  
  var Test = function Test() {
    _classCallCheck(this, Test);
  };
  
  ```

- 示例中es6的class需要一个_classCallCheck辅助函数，试想假如我们多个文件中都用到了es6的class，那么每个文件都需要定义一遍_classCallCheck函数，这也是一笔不小的浪费，假如将这些helper函数抽离到一个包中，由所有的文件共同引用则可以减少可观的代码量。而`@babel/runtime`做的正好是这件事，它提供了各种各样的helper函数，但是我们如何知道该引入哪一个helper函数呢？总不能自己手动引入吧，事实上babel提供了一个`@babel/plugin-transform-runtime`插件帮我们自动引入helper。我们首先安装`@babel/runtime`和`@babel/plugin-transform-runtime`：

- ```js
  npm install @babel/runtime @babel/plugin-transform-runtime
  
  ```

- 然后修改babel配置如下：

- ```js
  /* .babelrc */
  {
    "presets": [
      ["@babel/preset-env", {
        "modules": false,
        "useBuiltIns": "usage",
        "targets": "ie >= 8"
      }]
    ],
    "plugins": [
      "@babel/plugin-transform-runtime"
    ]  
  }
  
  ```

- 现在我们再来看test-compiled.js文件，里面的_classCallCheck辅助函数已经是从`@babel/runtime`引入的了：

- ```js
  /* test.js */
  class Test {}
  
  
  /* test-compiled.js */
  import _classCallCheck from "@babel/runtime/helpers/classCallCheck";
  
  var Test = function Test() {
    _classCallCheck(this, Test);
  };
  
  ```

- 看到这里你可能会说，这不扯淡嘛！几个helper函数能为我减少多少体积，我才懒得安装插件。事实上`@babel/plugin-transform-runtime`还有一个更重要的功能，它可以为你的代码创建一个`sandboxed environment`（沙箱环境），这在你编写一些类库等公共代码的时候尤其重要。

- 上文我们提到，对于Promise、Map等这些es6+规范的api我们是通过提供polyfill兼容低版本浏览器的，这样做会有一个副作用就是污染了全局变量，假如你是在写一个app还好，但如果你是在写一个公共的类库可能会导致一些问题，你的类库可能会把一些全局的api覆盖掉。幸好`@babel/plugin-transform-runtime`给我们提供了一个配置项`corejs`，它可以将这些变量隔离在局部作用域中：

- ```js
  /* .babelrc */
  
  {
    "presets": [
      ["@babel/preset-env", {
        "modules": false,
        "targets": "ie >= 8"
      }]
    ],
    "plugins": [
      ["@babel/plugin-transform-runtime", {
        "corejs": 2
      }]
    ]  
  }
  
  ```

- 注意：这里一定要配置corejs，同时安装`@babel/runtime-corejs2`，不配置的情况下`@babel/plugin-transform-runtime`默认是不引入这些polyfill的helper的。corejs的值现阶段一般指定为2，可以近似理解为是`@babel/runtime`的版本。我们现在再来看下test-compiled.js被转换成了什么：

- ```js
  /* test.js */
  class Test {}
  new Promise(() => {})
  
  
  /* test-compiled.js */
  import _Promise from "@babel/runtime-corejs2/core-js/promise";
  import _classCallCheck from "@babel/runtime-corejs2/helpers/classCallCheck";
  
  var Test = function Test() {
    _classCallCheck(this, Test);
  };
  
  new _Promise(function () {});
  
  ```

- 如我们所愿，已经为Promise的polyfill创建了一个沙箱环境。最后我们再为test.js稍微添加点内容：

- ```js
  /*  test.js */
  class Test {}
  new Promise(() => {})
  
  const b = [1, 2, 3].includes(1)
  
  
  /* test-compiled.js */
  import _Promise from "@babel/runtime-corejs2/core-js/promise";
  import _classCallCheck from "@babel/runtime-corejs2/helpers/classCallCheck";
  
  var Test = function Test() {
    _classCallCheck(this, Test);
  };
  
  new _Promise(function () {});
  var b = [1, 2, 3].includes(1);
  
  ```

- 可以发现，`includes`方法并没有引入辅助函数，可这明明也是es6里面的api啊。这是因为`includes`是数组的实例方法，要想polyfill必须修改`Array`的原型，这样一来就污染了全局环境，因此`@babel/plugin-transform-runtime`是处理不了这些es6+规范的实例方法的。

- 总结

  - 本文没有提到`preset-stage`，事实上babel@7已经不推荐使用它了，假如你需要使用尚在提案的语法，请直接添加相应的plugin。
  - 对于普通项目，可以直接使用`preset-env`配置polyfill
  - 对于类库项目，推荐使用`@babel/runtime`，需要注意一些实例方法的使用

## webpack高级概念

### 1.TreeShaking概念详解

webpackd的核心概念常用的配置完了，可以简单回顾下，我们来学习下webapck的一些高级概念。

第一个我们来了解下什么是tree shaking?

在webpack中treeshaking的作用是可以剔除js中用不上的代码，但是它依赖的是ES6的模块语法，也就是说没有被引用到的模块它是不会被打包进来的，可以减少包的大小，减少加载的时间，提高用户体验。

> 注意：
>
> 让tree shaking工作的前提是：提交给webapck的javascript代码必须采用ES6的模块语法，因为ES6的模块语法是静态的,就是必须是import ,export这样的使用方式，不能是commonjs这样的标准，像require引入等
>
> ```js
> import {fn1,fn2} from 'common/util.js';
> fn1();
> 
> // common.js
> export function fn1() {
> console.log("fn1");
> }
> export function fn2() {
> console.log("fn2");
> }
> 
> ```

修改代码

```js
//src/common/utils.js
export function fn1() {
  console.log("fn1");
}
export function fn2() {
  console.log("fn2");
}

//src/index.js
import { fn1 } from "./common/utils.js";
fn1();

// npm run build
// fn2这个模块即使没有被使用也打到了包中

```

```js
// 配置压缩代码，webapck4版本中只要开发production模式就会自动进行tree-shaking优化
// 这是和webpack3版本的差别之处，webapck3的还需要设置UglifyjsWebpackPlugin，进行激活
// webpack4只要配置mode:production模式就会自动激活了
// webpack.config.js
mode:production

devtool:"false" // 或者设置为生产环境推荐的sourcemap:cheap-module-source-map



// 对于第三方库怎么处理呢？
// 比如我们经常使用的lodash库

// 要使用这个库就要首先进行安装
npm install lodash --save

// index.js
import {chunk} from 'lodash';
console.log(chunk([1,2,3],2));

// npm run build 看到代码包大小变成了70多kb,只引入了一个函数，不应该这么大，这是因为没有进行tree shaking

//需要怎么做呢？
// 前边讲过，js tree shaking 利用的是 es 的模块系统。而 lodash.js 没有使用 CommonJS 或者 ES6 的写法。所以，安装库对应的模块系统即可。
npm install lodash-es --save

// index.js
import { chunk } from "lodash-es";

// npm run build 包已经很小了

// 需要注意在一些对加载速度敏感的项目中使用第三方库，请注意库的写法是否符合 es 模板系统规范，以方便webpack进行tree shaking

```



### 2.Development和Production模式区分打包



现在我们的配置开发环境与生成环境是没有进行区分的，这样是不行的，因为在开发环境与生产环境的一些配置是不一样的，我们应该将development与production环境区分进行打包。

就像我们现在的devtool配置在两个环境中就是不一样的，还有像devserver，热更新都是在开发模式才用到的，生产环境是不需要这些的，还有像开发模式代码一般是不需要进行压缩的，生产环境需要进行代码压缩，那我们应该怎么做呢？

如果不区分，每次进行改动是非常不方便的。

首先理下思路

- 开发环境
  - devServer
  - sourceMap
  - 接口代理
  - ...
- 生产环境
  - tree-shaking
  - 代码压缩
  - 提取公共代码
  - ...
- 共同点
  - 同样的入口
  - 同样的一些代码处理
- 方案
  - webpack.prod.js
  - webpack.dev.js
  - webpack.base.js
  - 借助webpack-merge工具连接，webpack.base.js供开发环境和生产环境同时使用
- 代码修改

```js
// 安装webpack-merge
npm install -D webpack-merge

//package.json
    "prod:build": "webpack --confg ./config/prod.js",
    "dev:server": "webpack-dev-server --confg ./config/dev.js --open"

//config/webapck.prod.js
const merge = require('webpack-merge')
const baseConfig = require('./webpack.base.js')
const prodConfig = {
    mode: "production",
    devtool: "false",
};
module.exports = merge(baseConfig, devConfig);

//config/webapck.dev.js
const webpack = require('webpack');
const merge = require('webpack-merge')
const baseConfig = require('./webpack.base.js')
const devConfig = {
  mode: "development",
  devtool: "cheap-module-eval-source-map",
  devServer: {
    contentBase: path.join(__dirname, "dist"),
    hot: true
    // hotOnly: true
  },
  plugins: [new webpack.HotModuleReplacementPlugin()]
};
module.exports = merge(baseConfig, devConfig);



//config/webapck.base.js
const path = require("path");
const HtmlWebpackPlugin = require("html-webpack-plugin");
const { CleanWebpackPlugin } = require("clean-webpack-plugin");
const webpack = require("webpack");
const baseConfig = 
module.exports = {
  entry: {
    index: "./src/index.js",
    demo: "./src/demo.js"
  },
  output: {
    publicPath: "/",
    filename: "[name].[hash:5].js",
    path: path.resolve(__dirname, "../dist")
  },

  // optimization: {
  //   // 优化导出的模块
  //   usedExports: true
  // },
  module: {
    rules: [
      {
        test: /\.(jpg|png)$/,
        use: {
          loader: "url-loader",
          options: {
            name: "[name].[hash:5].[ext]",
            outputPath: "assets/images",
            limit: 2048
          }
        }
      },
      {
        test: /.js$/,
        loader: "babel-loader",
        // options: {
        // presets: [
        // [
        // "@babel/preset-env",
        // {
        //   useBuiltIns: "usage"
        // }
        // ]
        // ],
        //   plugins: [
        //     [
        //       "@babel/plugin-transform-runtime",
        //       {
        //         corejs: 3,
        //         helpers: true,
        //         regenerator: true,
        //         useESModules: false
        //       }
        //     ]
        //   ]
        // },
        exclude: /node_modules/
      },
      {
        test: /\.(css|less|scss)/,
        loader: [
          "style-loader",
          {
            loader: "css-loader",
            options: {
              importLoaders: 2,
              modules: false
            }
          },
          {
            loader: "postcss-loader",
            options: {
              plugins: [require("autoprefixer")]
            }
          },
          "sass-loader"
        ]
      },
      {
        test: /\.(ttf|eto|svg|woff)/,
        use: {
          loader: "file-loader",
          options: {
            name: "[name].[ext]",
            outputPath: "assets/fonts"
          }
        }
      }
    ]
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: "./src/index.html"
    }),
    new CleanWebpackPlugin(),
  ]
};


```

运行都没有问题，这样开发环境的代码与生成环境的代码就区分开来打包

不要忘记改打包输出目录path: path.resolve(__dirname, "../dist")

### 3.webpack和code spliting1

打包环境区分完，继续进行打包优化，看一下现在我们的代码

```js
// index.js
// 现在又引入了jquery代码
import { fn1 } from "./common/utils.js";
import $ from 'jquery';
fn1();

// 现在运行打包，发现包的体积增大了，2.m

// 这种的一般我们会怎么配置呢，在webpack中一般会有几种方式
1.入口配置：entry入口使用多个入口文件
2.抽取公有代码：使用splitChunks抽取公有代码
3.动态加载：动态加载一些代码，按需加载,也加懒加载

```

- 首先来看入口配置方式

- ```js
  // index.js
  import { fn1 } from "./common/utils.js";
  // import $ from 'jquery'; 删除掉
  fn1();
  
  // webpack.base.js
    entry: {
      index: "./src/index.js",
      demo: "./src/demo.js",
      jquery: "jquery"
    },
     // 这样就不用在每个文件中通过import引入了
     new webpack.ProvidePlugin({
      $: "jquery",
      jQuery: "jquery"
     })
  
  ```

- 动态加载也比较简单就是利用异步加载的方式

- ```js
  // index.js
  import { fn1 } from "./common/utils.js";
  import $ from "jquery";
  import ( /* webpackChunkName: "jquery" */ 'jquery').then(({
      default: $
  }) => {
      console.log($.length);
  })
  fn1();
  
  //现在报错因为是动态加载的方式，所以需要安装一个支持动态加载的插件
  npm install -D @babel/plugin-syntax-dynamic-import
  
  //.babelrc 配置
    "plugins": [
      "@babel/plugin-syntax-dynamic-import",
      [
        "@babel/plugin-transform-runtime",
        {
          "corejs": 3,
          "helpers": true,
          "regenerator": true,
          "useESModules": false
        }
      ]
    ]
  
  // jquery包就单独打包出来了一个
  
  ```

- splitChunks这个方式需要重点学习下

- 在webpack4这个版本中，引入了splitChunksPlugin插件来取代之前版本里的CommonChunksPlugin插件，打包速度也比之前更快了。

- 刚刚我们也看了两个例子，代码分割带来的好处就是将文件进行分割，比如在我们的站点中，第三方依赖是基本不变动的，如果我们不将第三方库提取出去，那改业务代码更新文件，用户需要连第三方库加业务代码重新下载一次，这样对于用户体验是非常不好的，我们需要提高加载性能，所以就需要进行代码分割

- ```js
  // index.js
  import { fn1 } from "./common/utils.js";
  import jquery from 'jquery';
  fn1();
  
  // demo.js
  import jquery from 'jquery';
  
  //webapck.base.config.js
  optimization: {
      splitChunks: {
        chunks: "all",
  },
    
  // 打包  看到打出了一个公共包，里边就是jquery
  // 就是这个会提取文件中的公共代码包
  // 现在还没有进行其它配置，走的是一些默认配置
    // 配置中本身默认固有一个cacheGroups的配置项：
    splitChunks: {
      chunks: "all",
      cacheGroups: {
        vendors: {
          test: /[\\/]node_modules[\\/]/,  // 匹配node_modules目录下的文件
          priority: -10,  // 优先级配置项
          filename:'vendors.js'
        },
        default: {
          minChunks: 2,
          priority: -20,   // 优先级配置项
          reuseExistingChunk: true
        }
      }
    }
        
     //在默认设置中，会将 node_mudules 文件夹中的模块打包进一个叫 vendors的bundle中，所有引用超过两次的模块分配到 default bundle 中。更可以通过 priority 来设置优先级。
  
  ```

- 参数说明

  - chunks：表示从哪些chunks里面抽取代码，除了三个可选字符串值 initial、async、all 之外，还可以通过函数来过滤所需的 chunks；
  - minSize：**表示抽取出来的文件在压缩前的最小大小，默认为 30000**；
  - maxSize：表示抽取出来的文件在压缩前的最大大小，默认为 0，表示不限制最大大小；
  - minChunks：**表示被引用次数，默认为1**；上述配置commons中minChunks为2，表示将被多次引用的代码抽离成commons。
  - maxAsyncRequests：最大的按需(异步)加载次数，默认为 5；
  - maxInitialRequests：最大的初始化加载次数，默认为 3；
  - automaticNameDelimiter：抽取出来的文件的自动生成名字的分割符，默认为 ~；
  - name：抽取出来文件的名字，默认为 true，表示自动生成文件名；
  - cacheGroups: 缓存组。（这才是配置的关键）

> 缓存组会继承splitChunks的配置，但是**test、priorty和reuseExistingChunk只能用于配置缓存组**。cacheGroups是一个对象，按上述介绍的键值对方式来配置即可，值代表对应的选项。除此之外，所有上面列出的选择都是可以用在缓存组里的：chunks, minSize, minChunks, maxAsyncRequests, maxInitialRequests, name。可以通过optimization.splitChunks.cacheGroups.default: false禁用default缓存组。**默认缓存组的优先级(priotity)是负数，因此所有自定义缓存组都可以有比它更高优先级（译注：更高优先级的缓存组可以优先打包所选择的模块）（默认自定义缓存组优先级为0）**

> 值得注意的是，如果没有修改minSize属性的话，而且被公用的代码（假设是utilities.js）size小于30KB的话，它就不会分割成一个单独的文件。在真实情形下，这是合理的，因为（如分割）并不能带来性能确实的提升，反而使得浏览器多了一次对utilities.js的请求，而这个utilities.js又是如此之小（不划算）。

```js
    optimization:{
       splitChunks:{ //启动代码分割,不写有默认配置项
            	chunks: 'all',//参数all/initial/async，只对所有/同步/异步进行代码分割
              minSize: 30000, //大于30kb才会对代码分割
              maxSize: 0,//表示抽取出来的文件在压缩前的最小大小，默认为 30000
              minChunks: 1,//打包生成的文件，当一个模块至少用多少次时才会进行代码分割
              maxAsyncRequests: 5,//同时加载的模块数最多是5个
              maxInitialRequests: 3,//入口文件最多3个模块会做代码分割，否则不会
              automaticNameDelimiter: '~',//文件自动生成的连接符
              name: true,//抽取出来文件的名字，默认为 true，表示自动生成文件名；
            cacheGroups:{//对同步代码走缓存组
             vendors: {
                  test: /[\\/]node_modules[\\/]/,
                  priority: -10,//谁优先级大就把打包后的文件放到哪个组
                	filename:'vendors.js'
                },
            default: {
              minChunks: 2,
              priority: -20,
              reuseExistingChunk: true,//模块已经被打包过了，就不用再打包了，复用之前的就可以
              filename:'common.js' //打包之后的文件名   
            }
        }
        }  
    }

```



### 4.webpack和code spliting2

### 5.SplitChunkPlugin配置参数详解1

### 6.SplitChunkPlugin配置参数详解2

### 9.css文件代码分割

现在还有一个问题，我们的css代码还是和js代码在一块的，我们要把css单独提取出来，提取csswebpack也有相应的插件

可以先看下我们现在的代码，

```js
//mini-css-extract-plugin
npm install --save-dev mini-css-extract-plugin
// webpack.base.prod.js  // 一般在生产环境中使用
const MiniCssExtractPlugin = require("mini-css-extract-plugin");
	// 这里我们修改下publicpath
	// 注意contenthash与hash的区别
	// 设为contenthash后，再修改css中的内容，index.css的hash值就不会改变了
  output: {
    publicPath: "./",
    filename: "[name].[consthash:5].js",
    path: path.resolve(__dirname, "../dist")
  },
/*
	这里要将style-loader替换成MiniCssExtractPlugin.loader
	因为style-loader我们知道是将css代码直接通过style的方式插入到html的head中
	已经将css提取出来了，所以就不需要style-loader了，现在是通过link的方式进行引入的
*/
module: {
  rules: [
    {
      test: /\.css$/,
      use: [
        {
          loader: MiniCssExtractPlugin.loader
        },
        "css-loader"
      ]
    }
  ]
},
plugins: [
  new MiniCssExtractPlugin({
    // 打包出的文件名,也可以设置hash值  // 注意contenghash的值
    filename: "[name].[contenthash:5]css",  })
]

// 现在运行已经被打包出来了，但是有个问题，css代码现在是没有被压缩的，看js都是被压缩的，因为生产环境是默认被压缩的
// 压缩css,安装optimize-css-assets-webpack-plugin
npm install -D optimize-css-assets-webpack-plugin
//webpack.config.prod.js
const OptimizeCSSAssetsPlugin = require("optimize-css-assets-webpack-plugin");
  optimization: {
    minimizer: [new OptimizeCSSAssetsPlugin({})]
  },
    
    
// 现在css代码已经被打包了，但是现在js却没有被打包，这是为什么呢？
// mode:'production'会自动开启tree-shaking和js代码压缩，但是配置optimization。minimizer会使默认的压缩功能失效。所以，指定css压缩插件的同时，务必指定js的压缩插件
    
// 安装  terser-webpack-plugin
npm install -D terser-webpack-plugin

// webpack.config.prod.js
const TerserJSPlugin = require("terser-webpack-plugin");
  optimization: {
    minimizer: [new OptimizeCSSAssetsPlugin(), new TerserJSPlugin()]
  },

```

> **sideEffects：** 生产环境打包的时候，会默认开启tree-shaking，如果不设置`sideEffects`，某些通过`import`方式引入的css文件可能不会被打包，因为tree-shaking会甩掉引入后未使用的代码。通常，css文件一般都是引入就好，很少使用里面的方法或变量，所以很容易被webpack认为是没有用的代码，从而不会被打包。**所以，不希望被tree-shaking的文件，请在sideEffects中配置与之匹配的正则表达式。**
>
> `mini-css-extract-plugin`插件，结合`optimization.splitChunks.cacheGroups`配置，可以把css代码打包到单独的css文件，且可以设置存放路径（通过设置插件的`filename`和`chunkFilename`）。

到现在一些比较核心的点我们也都过了一遍，其它的一些loader,plugin,或者配置都是这样的套路，如果不知道该怎么着插件，可以先google看一下，有哪些包可以用，然后再到npm这里搜索，然后看下基本的使用，配置一下，看看起不起作用，webpack的的配置，推荐的loader，plugin这些一定要看一下。这样再用的时候，可以知道到哪去找，在配置的过程中也不用怕遇到问题。



在开发中我们经常会遇到一个问题就是包会越来越多，比如在我们开发的时候有时候会用到一些第三方的库，引入后如果我们不进行一些处理很可能就造成我们的入口打包文件越来越多，在用户第一次访问的时候就会造成不好的体验。

官网给我们提供了好几个包分析的可视化插件，可以看下官网  https://webpack.js.org/guides/code-splitting/#bundle-analysis

生成一个json文件，上传分析，

用的最多的还是[webpack-bundle-analyzer](https://github.com/webpack-contrib/webpack-bundle-analyzer)，非常好用

```js
// 安装
npm install --save-dev webpack-bundle-analyzer

// webpack.config.prod.js
const BundleAnalyzerPlugin = require('webpack-bundle-analyzer').BundleAnalyzerPlugin;

module.exports = {
  plugins: [
    new BundleAnalyzerPlugin()
  ]
}


// 非常的形象直观，可以看到包的一个大小依赖情况
//一些第三方的依赖包，一般都是单独打包，走cdn

```

### 8.打包分析

还有一个非常有用的配置，就是我们需要拿到环境变量的一些配置，比如通过环境，开发环境还是生产环境来区分执行不同的一些代码。

我们来看下官网的一个用法，取环境的一些参数还是很多的，我们通过插件yargs来实现下

```js
//https://webpack.js.org/guides/environment-variables/

webpack --env production 

npm i yargs

// webapck.config.base.js
console.log("环境参数" + argv.env);
const mode = argv.env;

// 现在就去到了可以使用了

// 这个也是可以自定义的，根据自己的需要进行设置就可以了

```




