   - 文件的监听：是在源码发生变化的时候，自动的进行代码的构建
        - 方法：
           - 启动webpack的时候，带上--watch参数    // 缺点：控制台构建完之后，浏览器不会自动的刷新。需要手动的刷新看效果
           - 在配置webpack.config.js的时候，设置watch:true
        - 原理： 轮询的判断文件的最后编辑时间是否发生了变化，某个文件发生了变化，并不会立刻告诉监听者，而是先缓存起来，等aggregateTimeout之后一起处理
         ```
            module.export = {
               watch: true,
               watchOptions:{
                  // 默认为空，不监听的文件或者文件夹，
                  ignore:/node_modules/,
                  // 监听到变化发生后等待300ms再去执行，默认300ms
                  aggregateTimeout:300,
                  // 判断文件是否发生变化是通过不停询问系统指定文件有没有变化，默认每秒问1000次
                  poll:1000
               }
            }
         ```