- 文件指纹
   - chunkhash和热更新没有办法一起使用，所以可以将文件指纹的配置在webpack.pro.js,热更新的配置放置在webpack.dev.js.
   - js的文件指纹:chunkhash
        ```
         output: {
            path: path.join(_dirname,'dist'),
            filename: '[name]_[chunkhash:8].js'
         }
        ```
   - 图片/字体的文件指纹:
        ```
         use:[{
            loader:'file-loader',
            options:{
               name:'[name]_[hash:8][ext]'
            }
         }]
        ``` 
   - css的文件指纹:首先需要用`mini-css-extract-plugin`生成单独的css文件。没法和style-laoder一起使用。 contenthash
        ```
         plugins: [
            new MiniCssExtractPlugin({
               filename: '[name]_[contenthash:8].css'
            })
         ],
         module:{
               rules:[
                  {
                  test:'/.css$/',
                  use:[
                     MiniCssExtractPlugin.loader, 
                     'css-loader'   
                  ]
                  }
               ]
         }
        ```         