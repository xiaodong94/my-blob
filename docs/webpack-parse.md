- 内置函数功能：
   - development:会设置process.env.NODE_ENV的值为development.开启NamedChunksPlugin和NameModulesPlugin,这两个插件会在代码热更新阶段，可以在控制台打印出是哪个模块发生了变化，路径目录是什么。很实用。
   - production:会设置process.env.NODE_ENV的值为production.会默认生产阶段的代码压缩等。
   - none.不开启任何设置。

- 解析ES6和.react JSX文件
   - 添加babel-loader
   - 添加.babelrc
      ```
         {
            "presets":[
               "@babel/preset-env",      // 增加ES6的babel preset配置
               "@babel/preset-react"     // 增加react的babel preset配置
            ],
            "plugins":[
               "@babel/proposal-class-properites"
            ]
         }
      ```

- 解析CSS,less
   - css-loader用于加载.css文件,并且转换为commonjs对象
   - style-loader将样式通过`<style>`标签插入到head中
      ```
         {
            module:{
               rules:[
                  {
                  test:'/.css$/',
                  use:[
                     'style-loader',      // 注意的点：style-loader必须在css-loader之前
                     'css-loader'   
                  ]
                  },{
                     test:'/.less$/',
                     use:[
                        'style-loader',
                        'css-loader',
                        'less-loader'
                     ]                     //  安装less-loader要一定安装less 
                  }
               ]
            }
         }
      ```

- 解析图片和字体资源
   - file-loader
      ```
         {
            module:{
               rules:[
                  {
                  test:'/\.(png|svg|jpg|gif)$/',
                  use:[
                     'file-loader',  
                  ]
                  },
                  {
                     test:'/.(woff|woff2|eot|ttf)$/',
                     use:[
                        'file-loader'
                     ]
                  } 
               ]
            }
         }
      ```
   - 另一种方式是采用url-loader。url-loader和file-loader功能上基本一致，除此之外还可以将文字和图片设置成较小资源的转化，自动base64
      ```
         {
            module:{
               rules:[
                  {
                  test:'/\.(png|svg|jpg|gif)$/',
                  use:[
                     'file-loader',  
                  ]
                  },
                  {
                     test:'/.(woff|woff2|eot|ttf)$/',
                     use:[{
                        loader:'url-loader',    // 内部也是file-loader
                        options:{
                           limit:10240    // 单位是字节. 10K的大小
                        }
                     }
                     ]
                  } 
               ]
            }
         }
      ```
