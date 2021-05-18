   - entry:入口文件，可以是单入口：entry:'xxx'。也可以是多入口：entry:{app:'xxx',anotherapp:'xxx'}
   - output:
      - 单入口：
       ```
          output:{
            filename:'bludle.js',
            path:path.join(_dirnname,'dist')
          }
       ```
       - 多入口： **利用占位符确保文件名称的唯一性**
        ```
          output:{
            filename:'[name].js',
            path:path.join(_dirname,'dist')
          }
        ```
   - loaders:webpack开箱即用只支持JS和JSON两种文件类型，通过Loaders去支持其他文件类型并且把他们转化成有效的模块，并且添加到依赖图中。本身是一个函数，接受源文件作为参数，返回转换的结果。
      - 常用的loader：
           - babel-loader:转换ES6 ES7等JS新特性语法
           - css-loader:支持css文件的加载和解析，并且转换成commonjs对象
           - less-loader
           - ts-loader
           - file-loader:进行图片，字体,富媒体的解析等的打包
           - raw-loader:将文件以字符串的形式导入 // 首屏需要内联的情况
           - thread-loader:多进程打包js和css
      - 用法：
         ```
            module:{
               rules:[
                  {
                     test:/\.txt$/,    // 指定匹配规则
                     use:'raw-loader'  // 指定使用的loader名字
                  }, 
                  {}
               ]
            }
         ``` 
   - plugins:loader没有做的事情。用于bundle文件的优化，资源管理和环境变量的注入。作用于整个构建过程
      - 常用的plugins：
           - CommonsChunkPlugin:将多个文件引入的相同的文件提取出来，成为公共的js
           - CleanWebpackPlugin:清理构建目录
           - ExtractTextWebpackPlugin:将css从bundle文件中提取出来成为一个独立的css文件
           - CopyWebpackPlugin:将文件或者文件夹拷贝到构建的输出目录
           - HtmlWebpackPlugin:创建html文件去承载输出的bundle
           - UglifyjsWebpackPlugin:压缩js
           - ZipWebpackPlugin:将打包出来的资源生成一个ZIP包
      - 用法：
         ```
            plugin:[
               new HtmlWebpackPlugin({
                  template:'./src/index.html'    // 放在plugin数组里面
               })
            ]
         ```   
   - mode:用来指定当前的构建环境是：production,development还是none.wepack4之后才有的属性，比如设置成development，webpack会默认开启在开发阶段的一些参数和函数的功能
      - 内置函数功能：
           - development:会设置process.env.NODE_ENV的值为development.开启NamedChunksPlugin和NameModulesPlugin,这两个插件会在代码热更新阶段，可以在控制台打印出是哪个模块发生了变化，路径目录是什么。很实用。
           - production:会设置process.env.NODE_ENV的值为production.会默认生产阶段的代码压缩等。
           - none.不开启任何设置。
  