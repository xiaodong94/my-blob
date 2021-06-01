- 自动清理构建目录
   - clean-webpack-plugin 默认会删除output指定的输出目录
   ```
    plugin:[
      new CleanWebpackPlugin()
    ]
   ```

- css功能的增强
   - 如何在编写css不需要添加前缀
      - postcss-loader
      - antoprefixer
        ```
          module:{
            rules:[
              {
                loader:'postcss-loader',
                options:{
                  plugins:()=> [
                    requrie('autoprefixer')({
                      browers:['last 2 version','>1%','ios 7']
                    })]
                  }
                }
              }
            ]
          }
        ```
   - 如何做rem的转换
      - px2rem-loader
        ```
          module:{
            rules:[
              {
                loader:'postcss-loader',
                options:{
                  plugins:()=> [
                    requrie('autoprefixer')({
                      browers:['last 2 version','>1%','ios 7']
                    })]
                  }
                }
              }
            ]
          }
        ```

   - 多页面打包
      - 每个页面对应一个entry,一个html-webpack-plugin
      - 动态获取entry和设置html-webpack-plugin数量
      - 利用glob.sync entry:glob.sync(path.join(_dirname,'./src/*/index.js'))
        ```
          const glob = require('glob')
          const setMPA = () => {
            const entry = {}
            const htmlWebpackPlugins = []

            const entryFiles = glob.sync(path.join(_dirname,'./src/*/index.js'))

            Object.keys(entryFiles)
              .map((index)=>{
                const entryFile = entryFiles[index]

                const match = entryFile.match(/src\(.*)/index\.js/)
                const pageName = match && match[1]

                entry[pageName] = entryFile
                htmlWebpackPlugins.push(
                  new html-webpack-plugin({
                    template:path.join(dirname,'src/${pageName}.html'),
                    filename:'${pageName}.html',  // 指定打包出来的html的文件名称
                    chunk:[pageName],
                    inject:true,   //  打包出来的css,js自动打包到html
                    minify:{
                      html5:true,
                      collapseWhitespace:true,
                      preserveLineBreaks:false,
                      minifyCSS:true,
                      minifiyJS:true,
                      removeComments:false
                    }
                })
                )
              })
            return {
              entry,
              htmlWebpackPlugins
            }
          }
        ```

   - 使用source map
      - 作用：通过source map定位到源代码
      - 开发环境开启，线上环境关闭。线上环境排查问题的时候可以将sourcemap上传到错误监控系统 
      ```
        devtool:'source-map'
      ```
   - 提取公共资源
      -  SpitChunksPlugin 具体参数配置可以看文档
      -  用的插件 HtmlWebpackExternalsPlugin
   - treeshaking
      - tree shaking就是只把用到的方法打到bundle中，没用到的方法会在uglify阶段被擦除掉。必须要是ES6的语法写。