- js
   - 判断JS的数据类型的方式
      - Number String Boolean Undefined BigInt Symbol
      - null： typeof null === 'object'
      - 引用类型: object array set
   - 口述函数防抖与节流的实现方法（实际开发中会遇到）
   - ES6的map和foreach的区别
   - 数组的常用的方法
   - 浏览器缓存机制  
      强缓存：不发送都服务器，直接从缓存取。状态码200  expires cache-controls
      协商缓存：发送服务器，通过服务器来告诉缓存时候可用。 状态码304 last-modifies etags
   - 如何解决相邻元素垂直方向的 margin 重合问题？ 
     属于同一个BFC的父元素和子元素，相邻的父子的margin会有重合的问题。
     解决办法。设置成不同的BFC。1.给子元素包一层父元素，设置为overflow:hidden.2子元素float
   - 闭包 -code2  可以 80%
   - this -code4  对
   - 了解eventLoop么  code5 对
   
- css
   - CSS 中隐藏元素有哪几种方式？
      display: none; 
      visibility: hidden; 
      width: 0; height: 0; 
      text-indent: -9999px; 
      绝对定位。
   - 请描述 display: none; 和 visibility: hidden; 的区别。
      - display: none; 隐藏后不占据位置，造成 DOM 树改变引发 Reflow（回流） + Repaint（重绘）；
      - visibility: hidden; 隐藏后占据位置，触发 Repaint（重绘）；
      回流（重排）一定会触发重绘，重绘不一定会触发回流（重排）。
   - 请描述回流（重排）和重绘的区别
      - Reflow（回流/重排）：影响布局的变化会触发浏览器重新渲染，开销大，影响性能，应尽量避免过多的回流。
      - Repaing（重绘）：只改变元素的背景色、文字颜色等，不影响周围或内部布局的属性只会触发浏览器重绘某一部分。
   - css的选择器+和～的区别？
      - `+`同级的相邻元素
      - `～`同级的所有相邻元素
   - 常用的尺寸单位
      px rem
   - css实现垂直左右居中的方案
- React
   - 版本
   - 常用的hooks

- node:
- nodeJS中，a.js 和 b.js 两个文件互相 require 是否会造成死循环，为什么？
- 对 Node 中的 fs 模块的理解? 有哪些常用方法?
- //fs.readFileSync 读取 同步读取
- //fs.readFile 读取 异步读取
- //fs.writeFileSync 写入 同步
- //fs.writeFile 写入 异步
- // fs.appendFileSync 追加写入
- // fs.appendFile 追加写入 第4个参数是回调函数
- // fs.copyFileSync  文件的拷贝 同步
- // fs.copyFile 文件的异步拷贝
- // fs.mkdirSync 同步创建目录
- // fs.mkdir  异步创建，第二个参数是返回值