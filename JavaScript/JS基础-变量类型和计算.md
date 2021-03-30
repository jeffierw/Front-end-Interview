# JS基础 - 变量类型和计算

## 变量类型

该部分可分为`值类型，引用类型`、`typeof运算符`、`深拷贝`

### 1.  `typeof `能判断哪些类型

首先变量类型分为**值类型**和**引用类型**，`typeof`可以判断所有的值类型，包括：

`Number,String,Null,Undefined,Boolean,Symbol`,另外还能识别`function`函数类型

而引用类型包括：`Object,Array`,仅仅只能判断为Object。

### 2. 值类型和引用类型的区别

值类型存在**栈**中，直接存入的是**值**。

引用类型在**栈**中存入的是**地址**，该地址指向**堆内存**，在**堆内存**存入的是**具体值**。因此，在拷贝时，基本数据类型拷贝的是**具体值**，而复杂数据类型拷贝的指向**堆内存**的**引用地址**。

### 3.手写深拷贝

```js
function deepClone(obj) {
    // 校验传入值
    if(typeof obj !== 'object' || typeof obj == null) {
		return obj
    }
    // 初始化返回值
    let result
    if (results instanceof Array) {
        result = []
    } else {
        result = {}
    }
    // 处理obj
    for(let key in obj) {
		// 保证属性key不是原型上的属性
        if(obj.hasOwnProperty(key)) {
            result[key] = deepClone(obj[key])
        }
    }
    return result
}

modules.export = deepClone
```

## 变量计算

可分为`字符串拼接`，`== ===`， `if语句和逻辑运算`

### 1. 字符串拼接

```js
const a = 100 + 10 // 110
const b = 100 + '10' // 10010
const c = true + '10' // 'true10'
```

### 2. == 和 ===

```js
100 == '100' // true
0 == '' // true
0 == false // true
false == '' // true
null == undefined // true
```

正常开发中，为了避免**隐式类型转换**这些问题，一般这样做：

```js
// 除了 == null 之外，一律使用 === 强等于
const obj = {
    x: 100
}
if (obj.a == null) {...}
// 相当于：
// if (obj.a == null || obj.a === undefined) {}
```

### 3. if语句和逻辑运算

- truly变量： `!!a === true`的变量

- falsely变量：`!!a == false`的变量

```js
// 以下是falsely变量，除此之外都是truly变量
!!0 === false
!!NaN === false
!!'' === false
!!null === false
!!undefined === false
!!false === false
```

在if语句里不同的变量判断可能执行步骤不一致

```js
// truly变量 就可以进入判断中
const a = true
if (a) {
    // ...
}
const b = 100
if (b) {
	// ...
}

// falsely 变量 无法进入判断内部
const c = ''
if (c) {
    // ...
}
const d = null
if (d) {
	// ...
}
let d 
if (e) {
    // ...
}
```

在逻辑判断里，执行结果也会有所不同

```js
console.log(10 && 0) // 0 因为10为true &&继续执行 返回后面的0
console.log('' || 'abc') // 'abc' 因为''为false || 返回后面的'abc'
console.log(!window.abc) // true
```







