# 安装

### 全局安装

```bash
npm install -g webpack
```

### 本项目安装

```bash
npm install webpack --save-dev
```

# 使用

在每个项目下配置一个webpack.config.js文件
用于告诉 webpack 它需要做什么，结构类似如下

```javascript
/**
 * Created by mac on 16/7/14.
 */
var webpack = require('webpack');
var path = require('path');

module.exports = {
    //页面入口文件配置
    entry: {
        index: [
            'webpack-dev-server/client?http://localhost:5000',
            'webpack/hot/only-dev-server',
            './js/entry.js'
        ]
    },
    //入口文件输出配置
    output: {
        path: __dirname + '/assets/',
        filename: 'bundle.js'
    },
    module: {
        //加载器配置
        loaders: [
            {
                test: /\.css$/,
                loader: 'style-loader!css-loader'
            },

            {
                test: /\.js$/,
                loader: 'jsx-loader?harmony'
            },
            {
                test: /\.(png|jpg)$/,
                loader: 'url-loader?limit=8192'
            },
            {
                test: /\.js|jsx$/,
                loaders: ['react-hot', 'babel?presets[]=es2015,presets[]=react,presets[]=stage-0'],
                include: path.join(__dirname, 'js')
            }
        ]
    },
    //其它解决方案配置
    resolve: {
        extensions: ['', '.js', '.json', '.scss']
    },
    //插件项
    plugins: [
        new webpack.HotModuleReplacementPlugin(),
        new webpack.NoErrorsPlugin()
    ]
};
```

# 打包项目

```bash
webpack --display-error-details
```
