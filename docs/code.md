#### 实现函数能够深度克隆基本类型：
- 浅克隆：
```
  function shallowClone(obj){
    let cloneObj = {}
    for(let i in obj){
      cloneObj[i] = obj[i]
    }
    return cloneObj
  }
```
- 深克隆：
 ```
function  deepClose(obj)){
    let result
    if(typeof obj=== 'object'){
      let result = Object.prototype.toString.call(obj)=== '[object Array]' ? [] : {}
      for (let i in obj ){
        result[i] = typeof obj[i] === 'object' ? deepClose(obj[i]) : obj[i]
      }
    } else {
      result = obj
    }
    return result
}
 ``` 
   - 考虑基本类型
   - 引用类型
     - RegExp,Date,函数， 用JSON.Stringify()和JSON.parse()无效
     - 考虑循环引用

#### 手写代码实现柯里化
```
  function createCurry(func,args){
    var argity = func
  }
```

#### 柯里化：
- 实现一个add方法，使计算结果能够满足如下预期：
add(1)(2)(3) = 6
add(1)(2)(3)(4) = 10
add(1)(2)(3)(4)(5) = 15
```
  function add(){
    // 第一次执行时，定义一个数组专门用来存储所有的参数
    var _args = Array.prototype.slice.call(arguments)

    // 在内部申明一个函数，利用闭包的特性保存_args并收集所有的参数值
    var _adder = function(){
      _args.push(...arguments)
      return _adder
    }

    // 利用toString隐式转换的特性，当最后执行时隐式转换，并计算最终的值返回
    _adder.toString = function() {
      return _args.reduce(function(a,b){
        return a + b
      })
    }
    return _adder
  }
```

#### 手写bind,apply,call
- call
```
  Function.prototype.call = function (context,...args) {
    context = context || window
  
    const fnSymbol = Symbol('fn')

    context[fnSymbol] = this

    context[fnSymbol](...args)
    delete context[fnSymbol]
  }
```

- apply 

```
  Function.prototype.apply = function (context,argsArr) {
    context = context || window
  
    const fnSymbol = Symbol('fn')

    context[fnSymbol] = this

    context[fnSymbol](...argsArr)
    delete context[fnSymbol]
  }
```

- bind

```
  Function.prototype.bind = function (context,..._args) {
    context = context || window
    const fnSymbol = Symbol('fn')
    context[fnSymbol] = this

    return function(..._args){
      args = args.concat(_args)

      context[fnSymbol](...args)
      delete context[fnSymbol]
    }
  }
```

#### this的指向
- 默认绑定：没有其他修饰符（bind,apply,call）,在非严格模式下定义指向全局对象，在严格模式下定义指向undefined
```
  function foo() {
    console.log(this.a)  // 2
  }

  var a = 2
  foo()
```
- 隐式绑定：调用位置是否有上下文对象,或者**是否被某个对象拥有或者包含**，那么隐式绑定规则会把**函数调用中this绑定到这个上下文环境中**。
```
  function foo() {
    console.log(this.a)
  }

  var obj = {
    a:2,
    foo:foo
  }

  obj.foo() //2
```
- 显示绑定：通过在函数上运行call和apply,来显示的绑定this
```
  function foo(){
    console.log(this.a)
  }
  var obj={
    a:2
  }
  foo.call(obj) // 2
```
- New绑定
```
  function foo (a) {
    this.a = a
  }
  var bar = new foo(2)
  console.log(bar.a) // 2
```

#### 手写promise
```
  class MyPromise{
    constructor(fn){
      this.resolvedCallbacks = []
      this.rejectedCallbacks = []

      this.state = 'PENDING'
      this.value = ''

      fn(this.resolve.bind(this),this.reject.bind(this))
    }

    resolve(value){
      if(this.state === 'PENDING'){
        this.state = 'RESOLVING'
        this.value = value

        this.resolvedCallbacks.map(cb => cb(value))
      }
    }

    rejected(value){
      if(this.state === 'PENDING'){
        this.state = 'REJECTED'
        this.value = value

        this.rejectedCallbacks.map(cb => cb(value))
      }
    }

    then(onFulfilled,onRejected){
      if(this.state === 'PENDING'){
        this.resolvedCallbacks.push(onFulfilled)
        this.rejectedCallbacks.push(onRejected)
      }

      if(this.state === 'RESOLUVED'){
        onFulfilled(this.value)
      }

      if(this.state === 'REJECTED'){
        onRejected(this.value)
      }
    }
  }
```

#### 数组扁平化
```
  function flatten(arr){
    let result = []

    for(let i = 0; i < arr.length; i++){
      if(Array.isArray(arr[i])){
        result = result.concat(flatten(arr[i]))
      } else {
        result = result.concat(arr[i])
      }
    }

    return result
  }
```

#### 请实现一个 sleep 函数
- 方法一：setTimeout一段时间之后执行
```
  function sleep(ms,fuc) {
    setTimeout(func,ms)
  }
  sleep(100,()=>{
    console.log('get up~')
  })
```
- 方法二：Promise等待一段时间之后执行then
```
  function sleep2(ms){
    return new Promise(function(resolve,reject){
      setTimeout(resolve,ms)
    })
  }
  sleep(1000).then(() => {
    console.log('get up')
  })
```
- 让await等待一段时间之后，再执行
```
  async function init(ms){
    await sleep3(ms)
  }
  function sleep3(ms){
    return new Promise((resolve,reject)=>{
      setTimeout(resolve,ms)
    })
  }
  init(ms).then(()=>{
    console.log(3000)
  })
```