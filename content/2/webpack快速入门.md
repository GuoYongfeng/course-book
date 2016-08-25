# webpack快速入门

## webpack介绍

Webpack是一个前端模块管理工具，有点类似browserify，出自Facebook的Instagram团队，但功能比browserify更为强大，可以说是目前最为强大的前端模块管理和打包工具。

Webpack将项目中的所有静态资源都当做模块，模块之间可以互相依赖，由webpack对它们进行统一的管理和打包发布，下图为官方网站说明：

![web pack](http://webpack.github.io/assets/what-is-webpack.png)

webpack对React有着与生俱来的良好支持，随着React的流行，webpack也成了React项目中必不可少的一部分。特别是随着ES6的普及，使得webpack有了更广阔的用武之地。

## 安装配置webpack

### 安装nodejs

安装webpack之前，需要确认本机已经安装好了nodejs。

如果还没有安装，请去[nodejs官网](https://nodejs.org)下载安装即可。这里使用的node版本是V4.4.1.

### 初始化项目环境

```
$ mkdir react_boilerplate
$ cd react_boilerplate\

$ npm init -y
Wrote to .\react_boilerplate\package.json:

{
  "name": "react_boilerplate",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}

```

`npm init` 加上一个`-y`选项会生成一个默认的`package.json`,关于这个文件，不是本文重点，在此不会详述，可以参考[官方文档](https://docs.npmjs.com/files/package.json)。可以简单的理解，这个文件是用于管理项目里面的依赖包的。

### 设置.gitignore

如果我们使用git进行版本管理，一个.gitignore文件是必要的。这里我们可以先将项目需要安装的node包目录添加进去。

```
node_modules
```

使用`npm install`安装的node包都会在`node_modules`目录下，这个目录是不需要commit到git的。


### 安装webpack

安装webpack很简单，命令如下：

```
npm i webpack --save-dev
```

其中`--save-dev`表示该包为开发环境依赖包。安装完后会生成一个`node_modules`目录，并且在`package.json`文件中多出如下几行：

```
......
  "devDependencies": {
    "webpack": "^1.13.0"
  }
}

```

如果写为`--save`则表示该包为生产环境依赖包，在`package.json`文件中会新增或者修改`dependencies` 字段。

### 初始化项目结构和代码

安装完`webpack`后，我们可以给项目中增加一些内容了。项目的简单结构如下图所示：

![目录结构](http://7xsxyo.com1.z0.glb.clouddn.com/2016/04/29/Fq4VS1Pe2NGaTyUIkNKJCADwE-iy336.jpg)

`app`目录用于存放项目代码，`dist`目录为编译后的项目文件，`webpack.config.js`为`webpack`的配置文件。

我们给项目中的文件添加一些简单的代码，首先是组件代码：

##### app/component.js
```
module.exports = function () {
  var element = document.createElement('h1');

  element.innerHTML = 'Hello world';

  return element;
};
```

然后需要一个入口文件，在入口文件中使用上面定义的组件：

##### app/index.js
```
var component = require('./component');

document.body.appendChild(component());
```


### 配置webpack

我们需要让webpack知道如何处理我们的项目目录结构，因此需要配置文件`webpack.config.js`。一个简单的配置文件如下所示：

```
var webpack = require('webpack');
var path = require('path');                 //引入node的path库

var config = {
  entry: ['./app/index.js'],                //入口文件
  output: {
    path: path.resolve(__dirname, 'dist'),  // 指定编译后的代码位置为 dist/bundle.js
    filename: 'bundle.js'
  },
  module: {
    loaders: [
      // 为webpack指定loaders
      //{ test: /\.js$/, loaders: ['babel'], exclude: /node_modules/ }
    ]
  }
}

module.exports = config;
```

到目前为止，我们已经可以让webpack工作了，在命令行执行

```
webpack
```

我们看到，会有一个新的文件`/dist/bundle.js`生成出来了。但是我们还需要一个html文件来加载编译后的代码，这就需要用到一个webpack插件：`html-webpack-plugin`。

### 安装`html-webpack-plugin`

使用如下命令安装：

```
npm install html-webpack-plugin --save-dev
```

然后在我们的`webpack.config.js`中增加下面几行：
```
  plugins: [
    new HtmlwebpackPlugin({
      title: 'React Biolerplate by Linghucong'
    })
  ]
```
现在在命令行下再次执行`webpack`命令，会看到在`dist`目录下生成了两个文件：`bundle.js`和`index.html`。其中`index.html`内容如下：

##### dist/index.html
```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title>React Biolerplate by Linghucong</title>
  </head>
  <body>
  <script src="bundle.js"></script></body>
</html>
```

有必要提一下，如果我们安装webpack的时候使用的是全局安装选项（`npm install -g webpack`），可以在命令行中直接执行`webpack`命令；如果没有使用`-g`，那么要用的`webpack`可执行文件位于：
```
./node_modules/.bin/webpack
```

我们可以在`package.json`中为此命令增加一个快捷方式：

```
# package.json
... other stuff
"scripts": {
  "build": "./node_modules/.bin/webpack"
}
```

现在就可以直接使用命令`npm run build`来执行webpack了。

```
$ npm run build

> react_boilerplate@1.0.0 build D:\node\react_boilerplate
> webpack

Hash: cbf754a65493b4d791d7
Version: webpack 1.13.0
Time: 919ms
     Asset       Size  Chunks             Chunk Names
 bundle.js     233 kB       0  [emitted]  main
index.html  179 bytes          [emitted]
   [0] multi main 52 bytes {0} [built]
  [75] ./app/index.js 82 bytes {0} [built]
  [76] ./app/component.js 142 bytes {0} [built]
    + 74 hidden modules
Child html-webpack-plugin for "index.html":
        + 3 hidden modules
```

### 安装`webpack-dev-server`

`webpack-dev-server`可以让我们在本地启动一个web服务器，使我们更方便的查看正在开发的项目。其安装也十分简单：

```
npm i webpack-dev-server --save-dev
```

然后在`webpack.config.js`文件中作如下修改：
```
# webpack.config.js
# ...
  entry: [
    'webpack/hot/dev-server',
    'webpack-dev-server/client?http://localhost:3000',
    './app/index.js'      //入口文件
    ],
# ...
```

我们可以在`package.json`中增加`webpack-dev-server`的快捷方式：

```
# package.json
... other stuff
"scripts": {
  "dev": "webpack-dev-server --port 3000 --devtool eval --progress --colors --hot --content-base dist",
  "build": "./node_modules/.bin/webpack"
}
```

配置中指定web服务器端口号为3000，指定目录为dist。
运行`npm run dev`：
```
$ npm run dev

> react_boilerplate@1.0.0 dev D:\node\react_boilerplate
> webpack-dev-server --port 3000 --devtool eval --progress --colors --hot --content-base dist

......
Time: 1109ms
     Asset       Size  Chunks             Chunk Names
 bundle.js     274 kB       0  [emitted]  main
index.html  179 bytes          [emitted]
chunk    {0} bundle.js (main) 216 kB [rendered]
    [0] multi main 52 bytes {0} [built]
 ......
 ......
 ......
   [77] ./app/component.js 142 bytes {0} [built]
Child html-webpack-plugin for "index.html":
    chunk    {0} index.html 505 kB [rendered]
        [0] ./~/html-webpack-plugin/lib/loader.js!./~/html-webpack-plugin/default_index.ejs 540 bytes {0} [buil
t]
        [1] ./~/lodash/lodash.js 504 kB {0} [built]
        [2] (webpack)/buildin/module.js 251 bytes {0} [built]
webpack: bundle is now VALID.

```

web服务器启动完毕，此时访问 http://localhost:3000/ 就可以看到我们的“Hello world”了。
![hello world](http://7xsxyo.com1.z0.glb.clouddn.com/2016/04/29/Fg7Cmp52pR7rdmvgdzobkVRGD2d_524.jpg)

需要特别说明的是，`webpack-dev-server`是支持热加载的，也就是说我们对代码的改动，保存的时候会自动更新页面。比如我们在文件中将“Hello world”改为“Linghucong”，会看到页面实时更新了，无须再按F5刷新，爽吧？！
![linghucong](http://7xsxyo.com1.z0.glb.clouddn.com/2016/04/29/Fp02wNcFcOPzZwNHB0QndYcFZ6b8336.jpg)

`webpack-dev-server`的配置还可以放在`webpack.config.js`中，需要使用一个`devServer`属性，详细可以[参考官方文档](https://webpack.github.io/docs/webpack-dev-server.html)。

### 处理CSS样式

项目中使用CSS是必不可少的。webpack中使用
loader的方式来处理各种各样的资源，根据设定的规则，会找到相应的文件路径，然后使用各自的loader来处理。CSS文件也需要特定的loader，一般需要使用两个：`css-loader`和 `style-loader`，如果使用LESS或者SASS还需要加载对应的loader。这里我们使用LESS，因此安装loaders:

```
npm install css-loader style-loader less-loader --save-dev
```

##### 踩坑提醒

npm3.0以上需要单独安装less：`npm install less --save-dev`。

然后在文件`webpack.config.js`中配置：

```
      {
        test: /\.less$/,
        loaders: ['style', 'css', 'less'],
        include: path.resolve(__dirname, 'app')
      }
```

可以看到，test里面包含一个正则，包含需要匹配的文件，loaders是一个数组，包含要处理这些文件的loaders，注意loaders的执行顺序是从右到左的。

新建一个LESS文件`/app/index.less`，其内容如下：
```
h1 {
    color: green;
}
```

在入口文件`index.js`中引入这个文件：
```
require('./index.less');
```

然后运行webpack进行编译：`npm run build`:
```
$ npm run build

> react_boilerplate@1.0.0 build D:\node\react_boilerplate
> webpack

Hash: 0c25c4bacdc334db1e04
Version: webpack 1.13.0
Time: 1902ms
     Asset       Size  Chunks             Chunk Names
 bundle.js     243 kB       0  [emitted]  main
index.html  179 bytes          [emitted]
   [0] multi main 52 bytes {0} [built]
  [75] ./app/index.js 110 bytes {0} [built]
  [80] ./app/component.js 141 bytes {0} [built]
    + 78 hidden modules
Child html-webpack-plugin for "index.html":
        + 3 hidden modules
```

可以看到， http://localhost:3000/ 页面上的文字已经变成绿色了。
![green](http://7xsxyo.com1.z0.glb.clouddn.com/2016/04/29/Fr2AQN-7eCUyPuWVoFQBiVuiSmEU814.jpg)
