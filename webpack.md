# webpack 
### https://doc.webpack-china.org/
### 概念

> * JavaScript应用程序的模块打包器（module bundler）
  * 递归构建依赖关系图（dependency graph），包含程序的每个模块
  * 高度可配置，入口（entry）、输出（output）、loader、插件（plugins）
    - Loader: 仅在每个文件的基础上执行转换
        识别出(identify)应该被对应的 loader 进行转换(transform)的那些文件。(test 属性)
        转换这些文件，从而使其能够被添加到依赖图中（并且最终添加到 bundle 中）(use 属性)
    - Plugins: 具有 apply 属性的 JavaScript 对象
        常用于（但不限于）在打包模块的 “compilation” 和 “chunk” 生命周期执行操作和自定义功能
    ###### webpack.config.js(webpack配置对象)
            
            const HtmlWebpackPlugin = require('html-webpack-plugin'); //installed via npm
            const webpack = require('webpack'); //to access built-in plugins
            const path = require('path');
            
            module.exports = {
                entry: './path/to/my/entry/file.js',
                output: {
                    // filename 用于输出文件的文件名,目标输出目录 path 的绝对路径。
                    path: path.resolve(__dirname, 'dist'),
                    filename: 'bundle.js'
                },
                module: {
                    rules: [
                       // 碰到「在 require()/import 语句中被解析为 '.txt' 的路径」时，在你对它打包之前，先使用 raw-loader 转换一下
                      { test: /\.txt$/, use: 'raw-loader' }
                    ]
                },
                plugins: [
                    new webpack.optimize.UglifyJsPlugin(),
                    new HtmlWebpackPlugin({template: './src/index.html'})
               ]
             }
                  
            module.exports = config;
      
##### 模块解析
 * resolver 是一个库(library)，用于帮助找到模块的绝对路径。resolver 帮助 webpack 找到 bundle 中需要引入的模块代码（require/import）。 当打包模块时，webpack 使用 enhanced-resolve 来解析文件路径
 
        webpack 能够解析三种文件路径：
        
            绝对路径：
                import "/home/me/file";
                import "C:\\Users\\me\\file";
                
            相对路径：
               import "../src/file1";
               import "./file2";
                
            模块路径：
                import "module";
                import "module/lib/file";
        
        
 * 每个文件系统访问都被缓存，以便更快触发对同一文件的多个并行或穿行请求。在观察模式下，只有修改过的文件会从缓存中摘出。如果关闭观察模式，在每次编译前清理缓存。

##### Mainfest(webpack 的 runtime 和 manifest，管理所有模块的交互。)
 * runtime，以及伴随的 manifest 数据，主要是指：在浏览器运行时，webpack 用来连接模块化的应用程序的所有代码。
    - runtime：模块交互时，连接模块所需的加载和解析逻辑。包括浏览器中的已加载模块的连接，以及懒加载模块的执行逻辑。
    - Mainfest:  webpack 如何管理所有模块之间的交互(manifest 数据用途的由来),通过使用 manifest 中的数据，runtime 将能够查询模块标识符，检索出背后对应的模块。（import 或 require 语句现在都已经转换为 __webpack_require__ 方法）