#### 数据类型：
- 类型
   - 基本数据类型: 
Number String Boolean Undefined BigInt Symbol
   - null： typeof null === 'object'
   - 引用类型: object array set
- 判断
   - typeof 可以判断基本数据类型
  
   ```
   typeof '' // "String"
   typeof 1  // "Number"
   typeof null // "object"
   typeof new Function() // "function"
   ```
   - instanceof 可以判断左操作数对象的原型链上是否有右边这个构造函数的prototype属性，最后返回布尔值。
      **注意⚠️：instanceof 只能用于对象，不实用原始类型的值**
  
   ```
   [] instanceof Array; // true
   [] instanceof Object; // true
   new Date() instanceof Date; // true
   new Date() instanceof Object; // true
   new Person() instanceof Object; true
   new Person() instanceof Person; true
   ```
   - Object.prototype.toString
  
   ```
   Object.prototype.toString.call('') // [object String]
   Object.prototype.toString.call(1) // [object Number]
   Object.prototype.toString.call(true) // [object Boolean]
   Object.prototype.toString.call(undefined) // [object Undefined]
   Object.prototype.toString.call(null)  // [object Null]
   Object.prototype.toString.call(new Function()) // [object Function]
   Object.prototype.toString.call(new Date())  // [object Date]
   Object.prototype.toString.call([]) // [object Array]
   Object.prototype.toString.call(new RegExp()) // [object RegExp]
   Object.prototype.toString.call(new Error())  // [object Error]
   Object.prototype.toString.call(document)  // [object HTMLDocument]
   Object.prototype.toString.call(window)  // [object Window]
   ```

#### 精度丢失可能出现在进制转换和对阶运算中：
- 0.1+0.2===0.3么，为什么？
   - 不等于。两个数字相加的时候，会先转换成二进制，0.1和0.2转换成二进制的时候尾部会发生无限循环，js引擎对二进制进行截断，
所以造成精度丢失。
- JS整数是怎么表示的？
   - 通过Number类型来表示，通过64位来表示一个数字，（1+11+52）1符号位，0表示+，1表示-；11指数位（e）,52尾数，小数部分（即有效数字）。最大安全数字是Math.pow(2,53)-1。
  
#### 事件如是如何实现的？
- DOM事件级别
   - DOM 0级事件 on-enevent(HTML属性)  **注意⚠️：现在已经不建议用此方法绑定事件**
   - DOM 2级事件  addEventListener('click',function(){},false).第三个参数为`true`表示在事件捕获，`false`表示在事件冒泡，默认是false.
   - DOM 3级事件 ： UI事件（load,scroll） 焦点事件（blur,focus）....
- DOM事件流
   - 事件捕获
   - 处于目标元素
   - 事件冒泡
- 事件流执行顺序
   - 点击子元素的时候，父层的`捕获`先触发，然后到子层的`捕获`或者`冒泡`,最后回到父层的`冒泡`。
   - 点击父元素的时候，不会经过子元素，子层的`捕获`和`冒泡`都不会触发。
   - 子层的`捕获`或者`冒泡`的执行顺序，由代码的顺序决定。**书写顺序**
- 事件监听和移除
   - 监听：一个事件，可以绑定多个函数，执行顺序按照绑定顺序。
   - 移除：removeEventListener('click',clickHander,false)
      - **注意⚠️：removeEventListener移除addEventListener事件的时候，2个事件必须提取出Hander的函数**
      ```
      var btn = document.getElementById('btn');
      btn.addEventListener('click', function(){
        console.log('HI');
      }, false);

      // 移除事件，但是没用
      btn.removeEventListener('click', function(){
        console.log('HI');
      }, false);
      ```
      ```
      var btn = document.getElementById('btn');

      // 把 event 函数程序拉出來
      var clickHandler = function(){
        console.log('HI');
      };

      btn.addEventListener('click', clickHandler, false);

      // 移除 clickHandler， ok!
      btn.removeEventListener('click', clickHandler, false);
      ```

#### new一个函数发生了什么
- 为什么要用new来调用，而不是直接调用?
   - 用new调用的话，会新建一个空的实例对象，将构造函数的显示原型等于实例对象的隐式原型，会将构造函数原型上的属性和方法都继承到实例对象上，会将this指向这个新实例对象，如果不用new调用的话就相当于是一个普通函数，this会指向window,也无法继承构造函数的任何属性。
- 构造调用的过程做了啥？
   - 新创建一个空的对象
      - `var fn = new Object()`
   - 构造函数的显示原型等于实例对象的隐式原型
      - `Fn.prototpe = fn._proto_`
   - 通过调用call，apply方法执行构造函数并改变this对象，绑定到实例对象上
      - Fn.call(fn)
   - 如何没有手动返回其他任何对象或返回值是基本数据类型（Number,String,Boolean）的值，会返回this指向的新对象，也就是实例。若返回值是引用类型（Object,Array,Function）,则实际返回值为这个引用类型。

#### symbol有什么用处
- symbol是基本数据类型
   - typeof symbol('1')  // 'symbol'
   - symbol('1') instanceof Aymbol // false
- 解决变量命名的冲突
```
const b = Symbol('test')
const a = Symbol('test')
a === b // false
const obj={}
obj[a] = 'hello'
obj[b] = 'world'
```
- 模拟私有属性
   - `Symbol`不会出现在`Object.keys()`和`JSON.stringify()`的结果里，除非用`Object.getOwnPropertySymbols()`,否则其他代码无法访问这个属性。

#### 闭包
- 闭包是指有权访问另一个函数作用域中的变量的函数
- 闭包产生的本质
   - 当前环境中存在指向父级作用域的引用
- 闭包的应用场景
   - 柯里化
   - 模块

#### NaN是什么，用typeof会输出什么：
- Not a Number
- typeof NaN === 'number'

#### JS隐式转换，显示转换

一般非基础类型进行转换时会先调用valueOf，如果valueOf无法返回基本类型值，就会调用toString

#### 了解this嘛，bind,call，apply具体指什么
- this是什么？
   - 可以是全局对象，当前对象或者任意对象，这完全取决于函数的调用方法。
   - js中函数的调用有以下几种方式：作为**对象方法**进行调用，作为**函数**调用，作为**构造函数**调用和使用**apply或者call**调用。
   - this关键字虽然会根据环境变化，但是它始终代表的是**调用当前函数的对象**。
- 改变this的几种方法？
   - 箭头函数
      - 箭头函数的this始终指向函数定义是的this,箭头函数没有this绑定，必须通过查找作用域链来决定其值，如果箭头函数被非箭头函数包含，则this绑定的是最近一层非箭头函数的this，否则，this为undefined.
   - 使用apply,call,bind
      - 区别：apply()接受两个参数，第一个是要绑定的this指向的对象，第二个是一个包含多个参数的数组。call()和apply()的区别在于第二个参数不同，call()接受多个参数列表。bind()方法创建一个新的函数，当被调用时,需要手动调用。

#### 如何判断一个对象是不是空对象
- Object.keys(obj).length === 0

#### js脚本加载问题，async,defer问题
- 同步加载
   - 我们平时使用最多的方式。会阻止浏览器的后续处理，停止后续的解析，只有当当前加载完成，才能进行下一步操作。一般建议把`<script>`标签放在`<body>`结尾处，这样能尽可能减少页面阻塞。
- 异步加载
   - async 如果与DOM和其他脚本依赖不强，使用async
   - defer 如果依赖其他脚本和DOM结果，使用defer

#### 外部js文件先加载还是onload先执行
- 外部js文件和body的加载顺序：body--->js文件。在body里面加载的js文件onload函数，onload函数会优先js文件的加载。
  
#### 说一下原型链和原型链的继承吧
- 原型链
   - 当对象查找一个属性的时候，如果没有在自身找到，那么就会查找自身的原型，如果原型还没有找到，那么会继续查找原型的原型，直到找到Object.prototype的原型时，此时原型为null,查找停止。这种通过原型链接的逐级向上的查找链被称为原型链。
- 原型继承
   - 一个对象可以使用另外一个对象的属性或者方法，就称之为继承。具体是通过将这个对象的原型设置为另一个对象，这样根据原型链的规则，如果查找一个对象属性且在自身不存在时，就会查找另外一个对象，相当于一个对象可以使用另外一个对象的属性和方法了。

#### 数组能够调用的函数有那些？
- [].push
- [].pop
- [].splice 从数组中添加或者删除元素，然后返回被删除的数组元素
- [].slice  从已有的数组中返回你选择的某段数组元素
- find
- findIndex
- reduce/map/filter
- shift
- unshift
- sort

#### 如果一个构造函数，bind了一个对象，用这个构造函数创建出的实例会继承这个对象的属性么？为什么？
- 不会继承。因为根据this的规则，new绑定的优先级高于bind的显式绑定，通过new进行构造函数调用时，会创建一个新对象，这个新对象会代替bind绑定，作为此函数的this.并且在此函数没有返回对象的情况下，返回新建的对象。

#### 事件循环机制
- 同步任务
- 微任务 process.nextTick()/Promise.then里面
- 宏任务 script/setTimeout/setInterval/setImmediate/IO/UI rendering
```
   async function async1() {
      console.log('async1 start');
      await async2();
      console.log('async1 end');
   }

   async function async2() {
      console.log('async2');
   }

   console.log('script start');

   async1();

   new Promise(function(resolve) {
      console.log('promise1');
      resolve();
      throw new Error('error')
      console.log('promise3');
   }).then(function() {
      console.log('promise2');
   }).catch(function(){
      console.log('error');
   });

   console.log('script end');
```
```
   同： script start /  async1 start / async2 /promise1  /  script end / async1 end
   微： promise2 
   宏：
```
#### H5有那些缓存机制？
- localstorage
  - 长久有效，需要手动删除，保存在浏览器中
  - localstorage.setItem('key',data) //data一般为JSON.stringify
  - localstorage.getItem('key') 
- sessionstorage
  - 浏览器会话期间有效，关闭浏览器，缓存消失
  - sessionstorage.setItem('key',data) // data一般为JSON.Stringify
  - sessionstorage.getItem('key')
- cookie
  - 浏览器到服务器之间的传递
  - 可以设置过期时间
  - 缓存的大小有限制
  - http请求头里面设置cookie 
  - fetch里面设置cookie credentials : include // 跨域ajax带上cookie

#### Last-Modified和Etag之间的区别？
- 相同
   - 客户端请求页面A
   - 服务端返回页面A，并在给A加上一个Last-Modefied/Etag.
   - 客户端展示该页面，并将页面连同Last-Modefied/Etag一起缓存。
   - 客户再次请求页面A，并将上次请求时服务端返回的Last-modefied/Etag一起传递给服务器
   - 服务器检测该Last-modefined或Etag,并判断出该页面子上次客户端请求之后还未被修改，直接返回响应304和一个空的响应体。
- 差别
   - 一些文件也许会周期性的更改，但是他的内容并不改变（仅仅改变的是修改时间），这时我们并不希望服务器认为这个文件修改了。
   - 某些文件的修改比较频繁，在秒以下。If-modefined-since能检测的颗粒是s级的。

#### 如何设置允许跨域的请求？
- 客户端：xhr.withcredetials = true
- 服务端：res.header('Access-Control-Allow-Credentials',true)
  
#### `<label/>`标签的作用？
- label里面设置上for属性的话，label可以和表单元素绑定到一起。比如
```
   <label for='test'>name </label>
   <input id='test' type='radio'/> 
```

#### css的选择器+和～的区别？
- `+`同级的相邻元素
- `～`同级的所有相邻元素

#### 如何解决相邻元素垂直方向的 margin 重合问题？
- 属于同一个BFC的父元素和子元素，相邻的父子的margin会有重合的问题。
- 解决办法。设置成不同的BFC。1.给子元素包一层父元素，设置为overflow:hidden.2子元素float

#### 函数防抖和函数节流有什么区别?
- 防抖和节流的作用都是防止函数多次调用。区别在于，假设一个用户一直触发这个函数，且每次触发函数的间隔小于wait,防抖的情况下只会调用一次，而节流的情况会每隔一定时间（参数wait）调用函数。
- 防抖-触发高频事件后N秒内函数只执行一次，如果N秒内高频事件再次被触发，则重新计算时间
```
   // func是用户传入需要防抖的函数 // wait是等待时间
   const debounce = (func,wait=500) => {
      // 绑定一个定时器
      let timer = {}
      // 这里返回的函数是每次用户实际调用的防抖函数
      // 如果已经设定过定时器了就清空上次的定时器
      // 开始一个新的定时器，延迟执行用户传入的方法
      return function(...args){
         if(timer) {clearTimeout(timer)}
         timer = setTimeout(()=>{
            fun.apply(this,args)
         },wait)
      }
   }
```
- 节流-高频事件触发，但在n秒内只执行一次，所以节流会稀释函数的执行频率
```
   function throttle (fn,wait=500) {
      let flag = true 
      // 通过闭包保存一个标记
      return function(){
         if(!flag) return
         flg = false
         setTimeout(()=>{
            fn.apply(this,arg)
            // 最后在setTimeout执行完毕后再把标记设置为true(关键)表示可以执行下一次循环了。当定时器没有执行的时候标记永远是false
            flag = true
         },wait)
      }
   }
```
- 应用场景
- 节流：
   - 滚动加载，加载更多或滚到底部监听
   - 搜索框，搜索联想
- 防抖：
   - 搜索框搜索输入。只需用户最后一次输入完，再发送请求。
   - 手机号，邮箱验证输入检测
   - 窗口大小resize.只需窗口调整完成后，计算窗口大小。防止重复渲染。

#### 打开一个网站，经历了什么？
