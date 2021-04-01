# JS-Web-API-BOM

BOM可分为`navigator，screen，location，history`API

# 1. 如何识别浏览器类型？

```js
const ua = navigator.userAgent
const isChrome = ua.indexOF('Chrome')
console.log(isChrome)
```

## 2.分析拆解URL各个部分

```js
console.log(location.href) // 网页地址
console.log(location.protocol) // 协议
console.log(location.pathname) // 路径
console.log(location.search) // 查询信息 类似?a=1&b=1
console.log(location.hash) // 哈希锚点内容 类似#xxx内容
```



