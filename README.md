
我现在需要在WebView中添加缓存，需要在 shouldInterceptRequest 中拦截资源链接，我的策略如下：
- 本地是否有缓存
	- 有
		- 返回本地资源
	- 没有
		- 直接走网络资源
		- 同时将该资源缓存到本地
- 文件名称：md5(url)
- 缓存文件范围：js、css、jpg、png、webp、gif

<!--stackedit_data:
eyJoaXN0b3J5IjpbMjA0ODEyNDIzNCwtMTU2MzQ3ODA4XX0=
-->