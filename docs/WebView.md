# WebView
## 1.说一下 JS 和 OC 互相调用的几种方式？ 
- js调用oc的三种方式:
	
	根据网页重定向截取字符串通过url scheme判断
	
	替换方法.context[@"copyText"]
	
	注入对象:遵守协议JSExport,设置context[@
- oc调用js代码两种方式

	通过webVIew调用 webView stringByEvaluatingJavaScriptFromString: 调用
	
	通过JSContext调用[context evaluateScript:];

## 2.在使用 WKWedView 时遇到过哪些问题？

白屏问题，Cookie 问题，在WKWebView上直接使用NSURLProtocol无法拦截请求，在WKWebView 上通过loadRequ发起的post请求body数据被丢失，截屏问题等
