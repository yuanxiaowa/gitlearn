# XHR跨域请求
## 后端
需要加入如下响应头
*	`Access-Control-Allow-Origin`: 授权的源控制，若允许跨域，则必须指明
	> 可以允许任何域，`*`

	> 如果需要客户端发送cookie，则必须指定具体的域

*	`Access-Control-Allow-Methods`: 允许的请求方法，默认为任何请求
*	`Access-Control-Max-Age`: 授权时间
*	`Access-Control-Allow-Credentials`: 是否允许客户端发送cookie
*	`Access-Control-Allow-Headers`: 控制哪些header能发送真正的请求

## 前端
### 普通xhr
若要发送cookie, 则`xhr.withCredentials = true;`, 否则什么都不做
### jquery
```js
$.ajax(url, {
	xhrFields: {
		withCredentials: true
	},
	crossDomain: true
})
```
### angular
*
```js
$http({withCredentials: true, ...}).post(...)
```
*
```js
$httpProvider.defaults.withCredentials = true;
```