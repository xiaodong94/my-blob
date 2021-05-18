   - 热更新：webpack-dev-server
      - 方法一：
           - 和HotModuleReplacementPlugin插件一起使用
           - 不输出文件，放在内存中
           - package.json里面的script`"dev:webpack-dev-sever --open"`,--open每次构建完成之后，自动的开启浏览器
           - 主要在开发环境使用
            ```
               {
                  mode:'development',
                  plugin:[
                     new HotModuleReplacementPlugin()
                  ],
                  devServer:{
                     contentBase:'./dist', // 服务的目录
                     hot:true
                  }
               }
            ```
      - 方法二：
           - webpack-dev-middleware。这种方法更加的灵活，将webpack输出的文件传输给服务器。需要和另外的node-server搭配使用，比如express或者koa.
            ```
               const express = requrie('express')
               const webpack = require('webpack')
               const webpackDevMiddleware = requrie('webpack-dev-middleware')

               const  app = express()
               const config = requrie('./webpack.config.js')
               const compiler = webpack(config)

               app.use(webpackDevMiddleware(compiler,{
                  publicPath:config.output.publicPath
               }))

               app.listen(3000,()=>{
                  console.log('listening on port 3000')
               })
            ```