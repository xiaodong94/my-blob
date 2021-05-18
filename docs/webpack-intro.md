- 为什么需要构建工具？
   - 转化ES6语法
   - 转换JXS
   - css前缀补全/预处理器
   - 压缩混淆
   - 图片压缩

- 前端构建演变之路
   - ant + YUI Tool ---- grunt(本地磁盘的操作) ------fis3/gulp(文件流，存放在内存) ----- rollup/webpack/parcel

- 为什么选择webpack?
   - 社区生态丰富
   - 配置灵活和插件化扩展
   - 官方更新迭代速度快（19年开始...）

- 初识webpack:配置文件名称
   - webpack默认配置文件：webpack.config.js
   - 可以通过webpack --config指定配置文件

- 运行
   - 原理：webpack的二进制文件的目录 ./node_modules/.bin/webpack
   - 简便：package.json里面写script语句：scripts:{build:'webpack'}.因为模块局部安装会在node_modules/.bin目录创建软链接。package.json可以默认读取到node_modules/.bin下面的命令