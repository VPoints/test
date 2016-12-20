# webpack 配置项

## module.exports：
#### 1. entry : 配置入口文件
    entry: {
        mmRouter: './scripts/mmRouter',
        router: './scripts/router'
    },
#### 2. output : 配置出口文件
    output: {
        path: path.join(__dirname, 'dist'),
        filename: '[name].js',
    }, //页面引用的文件
#### 3. module : 配置解析文件
    module: {
        loaders: [
            {test: /\.html$/, loader: 'raw!html-minify'},
        ]
    },
    //安装可以装换JSON的loader
    npm install --save-dev json-loader
    // npm一次性安装多个依赖模块，模块之间用空格隔开,babel转化器的安装
    npm install --save-dev babel-core babel-loader babel-preset-es2015 babel-preset-react
    //安装 css 解析器
    npm install --save-dev style-loader css-loader
#### 4. devServer ： webpack-dev-server
    devServer: {
      port:3000,
      // 这个地方是
      contentBase: "./public",
      //默认webpack-dev-server会为根文件夹提供本地服务器，如果想为另外一个目录下的文件提供本地服务器，应该在这里设置其所在目录（本例设置到“public"目录）
      colors: true,
      //设置为true，使终端输出的文件为彩色的
      historyApiFallback: true,
      //设置为true，当源文件改变时会自动刷新页面
      inline: true
      //在开发单页应用时非常有用，它依赖于HTML5 history API，如果设置为true，所有的跳转将指向index.html
    }，
---

```
var webpack = require('webpack');
var path = require('path');

module.exports = {
    entry: {
        mmRouter: './scripts/mmRouter',
        router: './scripts/router'
    },
    output: {
        path: path.join(__dirname, 'dist'),
        filename: '[name].js',
    }, //页面引用的文件
    module: {
        loaders: [
            {test: /\.html$/, loader: 'raw!html-minify'},
                {
                    test: /\.json$/,
                    loader: "json"
                },
                {
                    test: /\.js$/,
                    exclude: /node_modules/,
                    loader: 'babel',//在webpack的module部分的loaders里进行配置即可
                    query: {
                      presets: ['es2015','react']
                    }
                },
                  {
                    test: /\.css$/,
                    loader: 'style!css'//添加对样式表的处理
                  }
        ]
    },
    devServer: {
      contentBase: "./public",
      colors: true,
      historyApiFallback: true,
      inline: true
    }，
    resolve: {
        extensions: ['.js', '']
    }
}

```
babel 有一个配置文件
```
    //.babelrc
    {
      "presets": ["react", "es2015"]
    }
```
关于目录方面的：

__dirname 这个是文件目录

path 是node的内置模块

具体path的参数可以参考 http://www.css88.com/archives/4497

关于webpack指令方面：

$ webpack --config webpack.min.js //另一份配置文件

$ webpack --display-error-details //显示异常信息

$ webpack --watch   //监听变动并自动打包
 
$ webpack -p    //压缩混淆脚本，这个非常非常重要！
 
$ webpack -d    //生成map映射文件，告知哪些模块被最终打包到哪里了
