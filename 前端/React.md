# 参考

[Webpack搭建React开发环境](https://www.cnblogs.com/xps-03/p/12421600.html)
[webpack编译遇到的问题：Error: Cannot find module 'webpack-cli/bin/config-yargs'](https://www.cnblogs.com/jeacy/p/13864454.html)

# React

## 安装node.js

## 创建react项目 

### 使用 create-react-app

```bash
npm create-react-app my-app
```

使用此命令即使用了create-react-app这一个脚手架,直接使用就好

### 使用 webpack

#### 初始化

建立项目目录并使用以下命令初始化项目

```bash
npm init
```

可得到初始的package.json文件

#### 安装react

```bash
npm install react -S
```

#### 安装react-dom

```bash
npm install react-dom -S
```

#### 全局安装webpack

```bash
npm install -g webpack
```

#### 本项目安装webpack

```bash
npm install webpack -D
```

#### 安装webpack脚手架

```bash
npm install webpack-cli -D
```

由于安装4.xxx版本的webpack-cli会产生一个错误，因此最好指定webpack-cli为3.xxx版本

```bash
npm install webpack-cli@3 -D
```

#### 安装babel和babel preset-react

##### 安装babel

```bash
npm i babel-loader @babel/core @babel/preset-env -D
```

##### 安装 babel preset-react

```bash
npm i @babel/preset-react -D
```

#### 结果

经由以上配置得到package.json 文件

```javascript
{
  "name": "reactnewtest",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "@babel/core": "^7.12.10",
    "@babel/preset-env": "^7.12.11",
    "@babel/preset-react": "^7.12.10",
    "babel-loader": "^8.2.2",
    "html-webpack-plugin": "^4.5.1",
    "webpack": "^4.44.2",
    "webpack-cli": "^3.3.12",
  },
  "dependencies": {
    "react": "^17.0.1",
    "react-dom": "^17.0.1"
  }
}
```


#### webpack配置

新增webpack.config.js文件，包括 入口文件配置、入口文件输出配置、加载器配置以及一些其他配置。
首先先在配置文件中添加对JSX语法的Babel编译支持

```js
module: {
        rules: [
            {
                test: /\.jsx?$/, // jsx/js文件的正则
                    exclude: /node_modules/, // 排除 node_modules 文件夹
                use: {
                    // loader 是 babel
                    loader: 'babel-loader',
                    options: {
                        // babel 转义的配置选项
                        babelrc: false,
                        presets: [
                            // 添加 preset-react
                        require.resolve('@babel/preset-react'),
                            [require.resolve('@babel/preset-env'), {modules: false}]
                        ],
                        cacheDirectory: true
                    }
                 }
             }
        ]
    },
```

#### 搭建页面

在项目根目录创建 src/index.js文件，src是源文件目录，index.js文件内容如下：

```js
import React from 'react'
import ReactDOM from 'react-dom'

ReactDOM.render(
<div>hello  webpack !!!</div>
    , document.getElementById("root"))
```

在webpack配置文件内增加入口配置

```js
module.exports = {
    entry: './src/index.js',
    ...后续不变
```

#### 创建首页的html文件

在项目根目录创建 index.html文件,index.html文件内容如下:

```html
<!DOCTYPE html>
<html lang="en">
<head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>webpack练习</title>
</head>
<body>
     <div id="root"></div>
</body>
</html>
```

需要使用 html-webpack-plugin 插件来复制 public/index.html 到 dist 文件夹下,获取此插件指令是：

```bash
npm i html-webpack-plugin -D
```

修改webpack.config.js文件的配置,增加plugins配置：

```js
plugins:[
            new HtmlWebPackPlugin({
                template: 'index.html',
                filename: 'index.html',
                inject: true
            })
        ]
```

#### 执行打包命令

```bash
npx webpack --mode development
```

若出现以下错误

```bash
Error: Cannot find module 'webpack-cli/bin/config-yargs'
```

解决方法：
卸载当前的 webpack-cli

```bash
npm uninstall webpack-cli
```

安装 webpack-cli 3.* 版本

```bash
npm install webpack-cli@3 -D
```

若能成功打包，这时在package.json文件中配置以下 的内容：

```json
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack --mode production"
  },
```

可直接使用以下命令直接打包

```bash
npm run build
```

#### React本地服务

搭建一个页面的本地服务器

配置服务

##### 第一步：安装webpack-dev-server依赖

```bash
npm i webpack-dev-server -D
```

##### 第二步：在webpack.config.js配置文件中添加服务相关配置，完整配置如下：

```js
const HtmlWebPackPlugin = require('html-webpack-plugin');
const path = require('path');
module.exports = {
    mode: 'development',
    devtool: 'cheap-module-source-map',
    devServer: {
        contentBase: path.join(__dirname, './src/'),
        publicPath: '/',
        host: '127.0.0.1',
        port: 3000,
        stats: {
            colors: true
        }
    },
    ...下面相同
```

##### 第三步：修改package.json文件的scripts配置，添加start字段

用于直接启动

```bash
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack --mode production",
    "start": "webpack-dev-server --mode development --open"
  },
```

##### 第四步：执行启动服务命令

```bash
npm start
```

#### 配置热加载

即可以使用自动刷新功能，有三个步骤

##### 第一步：在src目录中新建dev.js文件，添加以下代码

```js
if (module.hot) {
    module.hot.accept(err => {
        if (err) {
            console.error('Cannot apply HMR update.', err);
        }
    });
}
```

上面代码用于触发HMR，这部分代码不属于业务代码。　

##### 第二步：在webpack.config.js配置文件中添加热加载配置

```js
// webpack.config.dev.js

const webpack = require('webpack'); //增加导入webpack

module.exports = {
    devServer: {
        ...
        hot: true, //在devServer中增加hot字段
        ...
    },
    ...
    entry: ['./src/index.jsx', './src/dev.js'], //在entry字段中添加触发文件配置
    ...
    plugins: [
        // plugins中增加下面内容，实例化热加载插件
        new webpack.HotModuleReplacementPlugin(),
    ...
    ]
    ...
}
```

##### 第三步：启动服务，测试热加载

```bash
npm start
```

## 最终的webpack配置文件和package.json

packagee.json

```json
{
  "name": "reactnewtest",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack --mode production",
    "start": "webpack-dev-server --mode development --open"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "@babel/core": "^7.12.10",
    "@babel/preset-env": "^7.12.11",
    "@babel/preset-react": "^7.12.10",
    "babel-loader": "^8.2.2",
    "html-webpack-plugin": "^4.5.1",
    "webpack": "^4.44.2",
    "webpack-cli": "^3.3.12",
    "webpack-dev-server": "^3.11.1"
  },
  "dependencies": {
    "react": "^17.0.1",
    "react-dom": "^17.0.1"
  }
}

```

webpack.config.js

```js
const HtmlWebPackPlugin = require('html-webpack-plugin');
const path = require('path');
const webpack = require('webpack'); //增加导入webpack

module.exports = {
    mode: 'development',
    devtool: 'cheap-module-source-map',
    devServer: {
        contentBase: path.join(__dirname, './src/'),
        publicPath: '/',
        host: '127.0.0.1',
        port: 3000,
        stats: {
            colors: true
        },
        hot: true,
    },
    entry: ['./src/index.js', './src/dev.js'],
    resolve: {
        extensions: ['.wasm', '.mjs', '.js', '.json', '.jsx']
    },
    module: {
        rules: [
            {
                test: /\.jsx?$/, // jsx/js文件的正则
                    exclude: /node_modules/, // 排除 node_modules 文件夹
                use: {
                    // loader 是 babel
                    loader: 'babel-loader',
                    options: {
                        // babel 转义的配置选项
                        babelrc: false,
                        presets: [
                            // 添加 preset-react
                        require.resolve('@babel/preset-react'),
                            [require.resolve('@babel/preset-env'), {modules: false}]
                        ],
                        cacheDirectory: true
                    }
                 }
             }
        ]
    },
    plugins:[
            new HtmlWebPackPlugin({
                template: 'index.html',
                filename: 'index.html',
                inject: true
            }),
            new webpack.HotModuleReplacementPlugin()
        ]
};
```

## 插件支持补充

### decorate装饰器支持插件的补充 错误信息：Support for the experimental syntax 'decorators-legacy' isn't currently enabled

首先安装支持的插件

```bash
npm install @babel/plugin-proposal-decorators
```

然后再webpack.config.js中添加配置
在 module rules内找到babel-loader
添加以下配置

```js
plugins: [
        [
            "@babel/plugin-proposal-decorators",
            {
            "legacy": true
            }
        ]
        ],
```

完整配置如下

```bash
{
    test: /\.jsx?$/, // jsx/js文件的正则
        exclude: /node_modules/, // 排除 node_modules 文件夹
    use: {
        // loader 是 babel
        loader: 'babel-loader',
        options: {
            // babel 转义的配置选项
            babelrc: false,
            presets: [
                // 添加 preset-react
            require.resolve('@babel/preset-react'),
                [require.resolve('@babel/preset-env'), {modules: false}]
            ],
            plugins: [
                [
                    "@babel/plugin-proposal-decorators",
                    {
                    "legacy": true
                    }
                ]
                ],
            cacheDirectory: true
        }
        }
    }
```

## 对'classProperties'的扩展，报错信息： Support for the experimental syntax 'classProperties' isn't currently enabled

在lable-loader中添加

```bash
plugins: ["@babel/plugin-proposal-class-properties"],
```

# React-Router

[官方文档](https://reactrouter.com/web/example/basic)

## 下载React-router

```bash
npm install -S react-router
```

# 理解

react的界面是由js语言写的，在react中称之为jsx，这个语言通过构建一些组件，层层嵌套的组件来搭建一整个页面，通过React-router路由控制设置每个路由内的组件，来形成一系列的页面。
问题：一般只有一个入口index.html?
