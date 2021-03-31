

# JS基础 - 原型和原型链

### 1. 如何准确判断一个变量是不是数组？

`instanceof`的本质是对比某个变量的隐式原型`__proto__`属性是否指向想要对比的类型(例如`Array`)的显式原型`prototype`

```js
a instanceof Array
```

```js
// 手写instanceof
function instanceof (left, right) {
  // left必须是个对象或者函数（其实也是对象）
  // 否则返回false
  if (typeof left !== 'object' && typeof left !== 'function') {
    return false
  }
  const rightProtoType = right.prototype
  let leftProto = Object.getPrototypeOf(left)
  while (true) {
    if (leftProto === rightProtoType) {
      return true
    }
    if (leftProto === null) {
      return false
    }
    leftProto = Object.getPrototypeOf(leftProto)
  }
}

module.exports = instanceof
```



### 2. class的原型本质，如何理解？

首先基于ES6，其原型关系为：

- 每个`class`都有显式原型`prototype`
- 每个实例都有隐式原型`__proto__`
- 实例的`__proto__`指向对应class的`prototype`

```js
class People {
	constructor(name) {
        this.name = name
    }
}
class Student extends People {
	constructor() {
		super(name)
    }
}

// class 实际上是函数，本质是语法糖
typeof People // 'function'
typeof Student // 'function'
```

基于原型的JS执行规则：

- 获取实例属性`xxx.name`或执行方法`xxx.sayHi()`时
- 先在自身属性和方法寻找
- 如果找不到则自动去`__proto__`中查找



### 3. 手写一个简易的jQuery考虑插件和扩展性

```js
class jQuery {
    constructor(selector) {
		const result = document.querySelectorAll(selector)
        const length = result .length
        for (let i = 0; i < length; i++) {
			this[i] = result[i]
        }
        this.length = length
    }
    get(index) {
        return this[index]
    }
    each(fn) {
		for (let i = 0; i < this.length; i++) {
			const elem = this[i]
            fn(elem)
        }
    }
    on(type,fn) {
		return this.each(elem => {
			elem.addEventListener(type,fn,false)
        })
    }
}

// 插件
jQuery.prototype.dialog = funtion (info) {
    alert(info)
}

// 可扩展性
class myjQuery extends jQuery {
    constructor(selector) {
		super(selector)
    }
    // 扩展自己的方法...
}
```

