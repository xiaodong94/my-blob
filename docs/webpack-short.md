- html压缩
   - html-webpack-plugin，一个页面一个html-webpack-plugin
   ```
    plugins:[
      new html-webpack-plugin({
        template:path.join(dirname,'src/search.html'),
        filename:'search.html',  // 指定打包出来的html的文件名称
        chunk:['search'],
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
    ]
   ```
- css压缩
   - 使用optimize-css-assets-webpack-plugin.同时使用cssnano.css的预处理器
   ```
    plugins:[
      new OptimizeCSSAssetsPlugin({
        assetNameRegWxp:/\.css$/g,
        cssProcessor:require('cssnano')  // css预处理器
      })
    ]
   ```
- js压缩
   - webapck4内置了uglifyjs-webpack-plugin，可以自动压缩，不需要另外的操作