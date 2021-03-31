# JS基础 - 异步和单线程

## 1. 同步和异步的区别是什么？

- 基于JS是单线程语言
- 异步不会阻塞代码执行
- 同步会阻塞代码执行

## 2.手写用Promise加载一张图片

```js
const url = 'xxx.xxx.xxx'

function loadImg(src) {
    return new Promise((resolve,reject)=> {
        const img = document.createElement('img')
        img.onload = ()=> {
			resolve(img)
        }
        img.onerror = ()=> {
            const err = new Error(`图片加载失败 ${src}`)
            reject(err)
        }
        img.src = src
    })
}

loadImg(url).then(img => {
	console.log(img.width)
    return img
}).then(img => {
	console.log(img.height)
}).catch(ex => console.error(ex))

// 多个请求写法
const url1 = 'xxx.xxx.xxx'
const url2 = 'xxx.xxx.xxx'

loadImg(url1).then(img1 => {
    console.log(img1.width)
    return img1
}).then(img1 => {
	console.log(img1.height)
    return loadImg(url2)
}).then(img2 => {
    console.log(img2.width)
    return img2
}).then(img2 => {
	console.log(img1.height)
}).catch(ex => console.error(ex))
```



### 3. 前端使用异步的场景有哪些？

- 网络请求，如ajax图片加载
- 定时任务，如`setTimeout`

