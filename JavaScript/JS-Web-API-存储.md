# JS-Web-API-存储

## 1. cookie localStorage sessionStorage的区别

主要从`容量`、`API易用性`、`是否跟随HTTP请求发送出去`三方面来说

### cookie

- 存储大小，最大4KB
- HTTP请求时需要发送到服务端，增加请求数据量
- 只能用`document.cookie = '...'`来修改，太过简陋

### localStorage

- HTML5专门为存储而设计，最大可存5M
- API简单易用 `setItem getItem`
- localStorage数据会永久存储，除非代码或手动删除

### sessionStorage

- HTML5专门为存储而设计，最大可存5M
- API简单易用 `setItem getItem`
- sessionStorage数据只存在于当前会话，浏览器关闭则清空



另外， **不同浏览器**无法共享`localStorage`或`sessionStorage`中的信息。**相同浏览器的不同页面**间可以共享相同的 `localStorage`（**页面属于同源页面），但是不同页面或标签页间无法共享`sessionStorage`的信息。这里需要注意的是，**页面及标 签页仅指顶级窗口**，如果一个标签页包含多个`iframe`标签且他们属于同源页面，那么他们之间是可以共享`sessionStorage`的。

