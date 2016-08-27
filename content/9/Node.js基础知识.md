
# Node.js基础知识

> 导读：在这一部分的基础内容中，将会学习如何安装并体验Nodejs，了解Nodejs里面的模块以及相关的代码规范。同时，还可以学习npm包管理器的使用。

## 安装

### windows上安装

- 先去下载一下[git](http://git-scm.com/download/)的客户端，可以运行git bash，方便使用shell命令

`step1.` 进入[nodejs.org](https://nodejs.org/)下载

`step2.` 下载完成后，双击默认安装。安装程序会自动添加环境变量

`step3.` 检测nodejs是否安装成功。打开cmd命令行 输入 :

```
node - v
```

`step4.` 检查npm是否安装。由于新版的NodeJS已经集成了npm，所以之前npm也一并安装好了。同样可以使用cmd命令行进行确认。

```
npm -v
```

### mac上安装

- 升级系统
- 升级xcode
```
xcode-select -p
xcode-select --install
```
- 安装Homebrew
前提是python和ruby安装好
官网查看方法
- 安装
```
brew install node
```
- 检查
```
node -v
```

### linux上安装

- 先要扫平环境问题
也可以到官网查看
要求gcc 4.2+和g++ 4.2+以及python 2.6和gnu的版本要求
- 检查
```
cat /etc/redhat-release
rpm -q gcc rpm -q gcc-c++
yum -y install gcc gcc-c++ kernel-devel
```
ubuntu下可以apt-get
```
cd /usr/src
wget 链接
tar -xf node包名
cd node包
./config
make
sudo make install
```
- 检查安装是否成功
```
node -v
npm -v
```
### 版本说明

> 目前最新的都已经到`5.1.0 `，这是从0.12版本后，nodejs和iojs(由于部分开发者对管理模式的不满，便fork了nodejs后创建了io.js，采用独立的社区驱动模式运营这个开源项目，每周一个版本迭代)合并了，直接从4.0版本开始发展了，到如今已经是5.1版本。当然，还在持续更新迭代中。

**关于nodejs版本号的说明**

偶数位的版本是稳定版本，而一般奇数位的就是非稳定版本，这几乎是在业界大家都达成共识了。比如0.6.x就是稳定版本，而0.11.x就是新功能测试的非稳定版本

建议选择最新的稳定版本进行使用。由于版本较多，为了方便node版本管理，推荐几个工具：

- osx, linux系统下

推荐使用n和nvm进行node的多版本管理，n是node的一个模块，TJ大神开发的。

```
npm install -g n
n 5.0.0
```

- windows系统下

推荐使用nvmw进行node的多版本管理。

```
// 选择一个目录下载nvmw，比如放在C:\Program Files\nvmw
git clone https://github.com/hakobera/nvmw.git
// 配置环境变量，在path中增加C:\Program Files\nvmw
// 运行nvmw命令
nvmw ls
nvmw use v5.0.0
nvmw install v5.0.0
```

不过，这个比较坑，git bash下无法运行，而且，对新版node还不支持

## node初体验

### nodejs的REPL交互环境
在浏览器中控制台，我们可以直接编写js代码进行运行

在你的CMD窗口，键入node后回车，即可进入node的repl环境运行js代码。

[更多关于REPL的简单说明戳这里](http://segmentfault.com/a/1190000002673137)

### 写个脚本

代码参见[git仓库](https://github.com/iUAP-FE/nodejs)。

### 起个web服务

```
//http.js

var http = require('http');

var app = http.createServer(function(req, res){
  res.writeHead(200, {"Content-Type": "text/plain"});
  res.end("Hello world");
});

app.listen(1337);

```

是的，就是这样神奇，短短几行代码，就创建了一个web服务，而且，请不要轻视它，这还是一个高性能的web服务器。

## Nodejs模块概述

> nodejs的模块分为提供的核心模块以及我们编写的业务模块

### 加载模块

Node.js采用模块化结构，按照CommonJS规范定义和使用模块。模块与文件是一一对应关系，即加载一个模块，实际上就是加载对应的一个模块文件。

require方法的参数是模块文件的名字。它分成两种情况。

第一种情况是参数中含有文件路径，这时路径是相对于当前脚本所在的目录：

```
var myfile = require('./myfile.js');
```

第二种情况是参数中不含有文件路径，这时Node到模块的安装目录，去寻找已安装的模块（比如下例）。

```
var gulp = require('gulp');
```

### 核心模块

如果只是在服务器运行JavaScript代码，用处并不大，因为服务器脚本语言已经有很多种了。Node.js的用处在于，它本身还提供了一系列功能模块，与操作系统互动。这些核心的功能模块，不用安装就可以使用，下面是它们的清单。

```
http：提供HTTP服务器功能。
url：解析URL。
fs：与文件系统交互。
querystring：解析URL的查询字符串。
child_process：新建子进程。
util：提供一系列实用小工具。
path：处理文件路径。
crypto：提供加密和解密功能，基本上是对OpenSSL的包装。
```

上面这些核心模块，源码都在Node的lib子目录中。为了提高运行速度，它们安装时都会被编译成二进制文件。

核心模块总是最优先加载的。如果你自己写了一个HTTP模块，`require('http')`加载的还是核心模块。


### 自定义模块

Node模块采用CommonJS规范。只要符合这个规范，就可以自定义模块。

下面是一个最简单的模块，假定新建一个test.js文件，写入以下内容。

```
module.exports = function(str) {
    alert(str);
};
```

上面代码就是一个模块，它通过module.exports变量，对外输出一个方法。

这个模块的使用方法如下。

```
var test = require('./test');

test("hello world");
```

上面代码通过require命令加载模块文件test.js（后缀名省略）。


## commonjs代码规范说明

也许你对上面的代码有所好奇，所以我们就来简单分析下以上代码。

首先，Node程序它是由许多个模块组成的，每个模块就是一个文件。Node模块采用了CommonJS规范。

根据CommonJS规范，一个单独的文件就是一个模块。每一个模块都是一个单独的作用域，也就是说，在一个文件定义的变量（还包括函数和类），都是私有的，对其他文件是不可见的。

而且，CommonJS规定，每个文件的对外接口是module.exports对象。这个对象的所有属性和方法，都可以被其他文件导入。

如下，我们来定义一个模块

```javascript
var x = 5;

var addX = function(value) {
  return value + x;
};

// 导出变量和方法
module.exports.x = x;
module.exports.addX = addX;
```

理解：上面代码通过module.exports对象，定义对外接口，输出变量x和函数addX。module.exports对象是可以被其他文件导入的，它其实就是文件内部与外部通信的桥梁。

好，继续，我们来引用刚才定义的模块
```javascript
var example = require('./example.js');

console.log(example.x); // 5
console.log(addX(1)); // 6
```

### module对象
每个模块内部，都有一个module对象，代表当前模块。它有以下属性。
```
module.id 模块的识别符，通常是带有绝对路径的模块文件名。
module.filename 模块的文件名，带有绝对路径。
module.loaded 返回一个布尔值，表示模块是否已经完成加载。
module.parent 返回一个对象，表示调用该模块的模块。
module.children 返回一个数组，表示该模块要用到的其他模块。
```
- module.exports属性
module.exports属性表示当前模块对外输出的接口，其他文件加载该模块，实际上就是读取module.exports变量。

- exports变量
为了方便，Node为每个模块提供一个exports变量，指向module.exports。这等同在每个模块头部，有一行这样的命令。
### require命令的解读
Node.js使用CommonJS模块规范，内置的require命令用于加载模块文件。

require命令的基本功能是，读入并执行一个JavaScript文件，然后返回该模块的exports对象。如果没有发现指定模块，会报错。

```
// example.js
var invisible = function () {
  console.log("invisible");
}

exports.message = "hi";

exports.say = function () {
  console.log(message);
}
```

### require加载模块的规则
require命令用于加载文件，可以加载后缀名为`.js` `.json` `.node`的文件，比如：
```
var foo = require('foo');
//  等同于
var foo = require('foo.js');
```

举例来说，脚本/home/user/projects/foo.js执行了require('bar.js')命令，Node会依次搜索以下文件。
```
/usr/local/lib/node/bar.js
/home/user/projects/node_modules/bar.js
/home/user/node_modules/bar.js
/home/node_modules/bar.js
/node_modules/bar.js
```
这样设计的目的是，使得不同的模块可以将所依赖的模块本地化。

如果指定的模块文件没有发现，Node会尝试为文件名添加.js、.json、.node后，再去搜索。.js件会以文本格式的JavaScript脚本文件解析，.json文件会以JSON格式的文本文件解析，.node文件会以编译后的二进制文件解析。

require匹配文件的流程图例

![](../images/require.jpg)


## npm包管理器

### 介绍

npm有两层含义：
* Node.js的开放式模块登记和管理系统，网址为[http://npmjs.org](http://npmjs.org)。
* Node.js默认的模块管理器，是一个命令行下的软件，用来安装和管理node模块。

NPM是随同NodeJS一起安装的包管理工具，能解决NodeJS代码部署上的很多问题，常见的使用场景有以下几种：

允许用户从NPM服务器下载别人编写的第三方包到本地使用。
允许用户从NPM服务器下载并安装别人编写的命令行程序到本地使用。
允许用户将自己编写的包或命令行程序上传到NPM服务器供别人使用。

### 全局配置
`step1.` npm作为一个NodeJS的模块管理，我们要先配置npm的全局模块的存放路径以及cache的路径，例如我希望将以上两个文件夹放在NodeJS的主目录下，便在NodeJs下建立**node_global**及**node_cache**两个文件夹。我们就在cmd中键入两行命令：
```
npm config set prefix "D:\Program Files\nodejs\node_global"
npm config set cache "D:\Program Files\nodejs\node_cache"
```
`step2.` 下面这一步非常关键，我们需要设置系统变量。进入我的电脑→属性→高级→环境变量。
```
在系统变量下新建“NODE_PATH”
输入内容“D:\Program Files\nodejs\node_global\node_modules”
```
`step3.` 安装bower或是gulp等常用工具
```
npm install bower gulp -g
```
这个时候，你在命令行就可以全局的使用安装的工具了，可以命令行中直接确认
```
gulp -v
bower -v
```
`step4.` 接下来我们可以进一步学习如何用npm发布自己的包
```
// 初始化一个package
npm init
// 添加你的包的管理用户
npm adduser
// 发布你的包
npm publish --tag 0.1.0
```

### 学习使用

1. npm set
npm set用来设置环境变量。

2. npm info
npm info命令可以查看每个模块的具体信息

3. npm search
npm search命令用于搜索npm仓库，它后面可以跟字符串，也可以跟正则表达式。

4. npm install
Node模块采用npm install命令安装。每个模块可以“全局安装”，也可以“本地安装”。两者的差异是模块的安装位置，以及调用方法。

5. npm update
npm update 命令可以升级本地安装的模块。

6. npm uninstall
卸载掉安装的模块

7. npm publish
发布你的包
8. npm adduser
给你的包添加用户信息
9. npm cache clear
清除缓存
