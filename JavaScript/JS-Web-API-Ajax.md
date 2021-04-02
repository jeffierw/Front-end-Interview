# JS-Web-API-Ajax

## 1. 手写一个简易的ajax

```js
// GET请求
const xhr = new XMLHttpRequest()
xhr.open('GET', '/data/test.json', true) // 最后的参数为true 表示异步请求
xhr.onreadystatechange = function () {
    if (xhr.readyState === 4) { // xhr对象请求状态 4表示服务器已完成响应
        if (xhr.status === 200) {
            // console.log(
            //     JSON.parse(xhr.responseText)
            // )
            alert(xhr.responseText)
        } else if (xhr.status === 404) {
            console.log('404 not found')
        }
    }
}
xhr.send(null) // 最后需要发送响应的数据 GET请求无数据发送

// POST请求 封装成Promise
function ajax(url) {
    const p = new Promise((resolve, reject) => {
        const xhr = new XMLHttpRequest()
        xhr.open('POST', url, true)
        xhr.onreadystatechange = function () {
            if (xhr.readyState === 4) {
                if (xhr.status === 200) {
                    resolve(
                        JSON.parse(xhr.responseText)
                    )
                } else if (xhr.status === 404 || xhr.status === 500) {
                    reject(new Error('404 not found'))
                }
            }
        }
        const postData = {
			userName: 'zhangsan',
            password: 'xxx'
        }
        xhr.send(JSON.stringify(postData))
    })
    return p
}

const url = '/data/test.json'
ajax(url)
.then(res => console.log(res))
.catch(err => console.error(err))
```



## 2.跨域

`ajax`请求时，浏览器要求当前网页和服务器地址必须`同源`

同源指的是：`协议、域名、端口，`三者都必须一致

### 2.1出现跨域前端的解决方法

#### 2.1.1 jsonp

因为加载`img` `css` `js` 可无视同源策略，其中：

- `<img />` 可用于统计PV操作，可使用第三方统计服务
- `<link />  <script>`可使用CDN，CDN一般是外域
- `<script>` 便可实现jsonp

而所有的跨域，都**必须经过服务端允许和配合**，未经服务端允许就实现跨域，说明浏览器有漏洞，很危险。

访问`https://baidu.com`服务器不一定是返回一个HTML文件，服务端可以任意拼接数据返回，只要符合HTML格式要求；

同理`<script src="https://baidu.com/getData.js" >`也可以和服务端配合返回任意想要的数据。

- `<script>`可绕过跨域限制
- 服务器可以任意动态拼接数据返回
- 所以，`<script>`就可以获得跨域的数据，**只要服务端愿意返回**

#### 2.1.2 CORS

CORS，跨域共享资源，是一种基于HTTP头的机制，该机制通过允许服务器标示除了它自己以外的其它origin（域，协议和端口），这样浏览器可以访问加载这些资源。





