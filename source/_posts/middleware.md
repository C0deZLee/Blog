title: Use Middleware to Check User Auth Status in Express (Chinese)
categories:
  - Tech
date: 2015-11-23 18:19:56
tags:
 - JavaScript
 - Node.js
---
Middleware（中间件）在Express中扮演了相当重要的角色，然而很多新手一开始并不清楚如何使用Middleware，今天我们用一个典型的Middleware使用场景（判断用户是否登陆）来做一个简单的教程。
<!-- more -->
## 什么是Middleware？
相信这是很多初学者第一个想问的问题，事实上Middleware的定义十分广泛，此文中我们仅仅探讨Middleware在Express中的定义。
在[Express文档](http://expressjs.com/guide/using-middleware.html)中写道:

> Middleware is a function with access to the request object (req), the response object (res), and the next middleware in the application's request-response cycle, commonly denoted by a variable named next.
Middleware can:
* **Execute any code.**
* **Make changes to the request and the response objects.**
* **End the request-response cycle.**
* **Call the next middleware in the stack.**

由此我们可知在Express中Middlesware的作用是插入在request-response cylcle之中，具有res和req两个参数的访问权限的一种特殊函数。同时Middleware还可以

* 执行任何代码
* 对req和res进行更改
* 结束request-response cylcle
* 调用堆栈中的下一个Middleware

因此事实上Middleware在Expres中的作用是对用户的请求（request）进行预处理和过滤，而且可以将过滤后的东西传递下去。

## 示例代码

在本示例中我们将实现只有登录后的用户才可以访问"/home"路径。

route.js (Before)

	module.exports = function(app) {
	    app.get('/home', function(req, res) {
	        res.send('Hello world!');
	    });
	};

现在任何人都可以访问"/home"，然而我们只想让已登录的用户访问这个路径。

controller.js

	...
	function isAuthenticated(req, res, next) {
		if (!req.isAuthenticated()) {
			// if user is not authenticated, then redirect to root
			res.redirect('/');
		}
		next();
	}
	...

此Middleware的作用就是将未登录的用户“过滤”出去。
现在我们对route.js做一些更改，使Middleware可以发挥作用：

route.js (After)

	module.exports = function(app) {
	    app.get('/home', isAuthenticated, function(req, res) {
	        res.send('Hello world!');
	    });
	};

## 小结
相信此时大家已经对middleware的使用有了一些初步的了解，下面是一些进阶的资料，可以帮助对Express的理解。
[Express Guide - Using Middleware](http://expressjs.com/guide/using-middleware.html)
[Express 4.x API](http://expressjs.com/4x/api.html)
