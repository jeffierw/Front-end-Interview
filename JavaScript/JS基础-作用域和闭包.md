# JS基础 - 作用域和闭包

## 1. this的不同应用场景如何取值？

`this`主要有以下五种应用场景：

- 作为普通函数
- 使用 call apply bind
- 作为对象方法被调用
- 在class方法中被调用
- 箭头函数



```js
// 作为普通函数 this指向window对象
function fn1() {
	console.log(this)
}
fn1() // window

// 使用call apply bind this指向绑定的元素
fn1.call({x: 100}) // {x: 100}

const fn2 = fn1 = fn1.bind({x: 100})
fn2() // {x: 100}

// 作为对象方法被调用时
const zhangsan = {
	name: '张三',
    sayHi() {
        // this 指向当前对象
        console.log(this)
    },
    wait() {
        setTimeout(function() {
            // 这里触发this执行的是定时器内部的函数，不是对象的方法，所以这里的this 指向 window对象
            console.log(this)
        })
    },
    waitAgain() {
		setTimeout(() => {
            // 箭头函数中的this指向上级作用域中this的指向，这里 即当前对象
            console.log(this)
        })
    }
}

// 在class方法中被调用 指向class创建的实例对象
class People {
	constructor(name) {
        this.name = name
        this.age = age
    }
    sayHi() {
		console.log(this)
    }
}
const zhangsan = new People('张三')
zhangsan.sayHi() // this 指向当前实例对象
```



### 2.手写bind函数

首先bind从哪来的？

```js
typeof fn // 'function'
fn.__proto__ === Function.prototype // true
```

从以上代码可知`call bind apply`等函数都来自Function的显式原型上

```js
Function.prototype.bind = function () {
    // 将传入的参数拆解为数组
    const args = Array.from(arguments)
    
    // 获取 this （数组第一项）
    const t = args.shift()

    return ()=> {
		return this.apply(t,args)
    }
}
```



### 3.实际开发中闭包的应用场景，举例说明

```js
// 闭包隐藏数据，只提供API
function createCache() {
    const data = {} // 内部闭包数据，不被外界访问
    return {
		set(key,val) {
            data[key] = val
        },
        get(key) {
			retrn data[key]
        }
    }
}

const c = createCache()
c.set('a',100)
console.log(c.get('a'))
```





