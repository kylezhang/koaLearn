>1）Koajs是被寄予厚望的下一代Nodejs网络服务框架，它正在因为优雅，简洁和卓越的性能赢得nodejs开发者的青睐；
2）用时两天时间，[桑大](https://cnodejs.org/user/i5ting)在线下首秀，带领大家快速上手koajs框架

![0_1467251262722_2.pic.jpg](//qn-rockq1.rockq.org/Fk55NcUK_ydgpWiQ-EUmLIW-tpnJ)

* 列表两天时间非常的充实，下面就一两天的时间为线索回顾和温习这次精彩而又充实的研讨课；
# 日程
## 第一天上午：6月19日 9：00 -12：00
### 1. Nodejs基础
	1). nvm:简单方便的管理nodejs版本；以MacOS为主,
	--> 安装:brew install nvm
    --> 使用:nvm install [-s] <version> //Download and install a <version>, [-s] from source. Uses .nvmrc if available
    --> eg. nvm install 5.11.0
    --> 使用详情:nvm help
      
	2). nrm:管理npm源
    --> 安装:npm install nrm -g
    --> 使用:nrm ls //查看所有的npm源，
    	     nrm use taobao       //使用淘宝源
    --> 使用详情: nrm --help
    
	3). npm:管理nodejs模块之间的依赖
	--> 安装:在nvm install node的时候就安装好了
	--> 使用:npm install koa --save     //本地安装koa模块
	        npm install express -g     //全局安装express
	--> 使用详情: npm --help
  
### 2. Koa入门
	1) koa之前，以express为主的代码：
	
	   var express = require('express')
	   var app = express()
	   app.use(function(req, res, next){
	   	res.send('Hello World')
	   })
	   app.listen(3000)
	   
	2) koa时代的代码：
	
	   =>koa1.x....
	   var koa = require('koa')
	   var app = koa()
	   app.use(fucntion *(){
	   	this.body = 'Hello World'
	   })
	   app.listen(3000)
	   
	   =>koa2.x....
	   const koa = require('koa')
	   const app = new koa()
	   app.use(ctx => {
	   	ctx.body = 'Hello World'
	   })
	   app.listen(3000)
	   
	3) 概念部分；主要以koajs源码https://github.com/koajs/koa为基础讲解
	   ****这里要有图片
### 3.ES6 && ES7
	可以阅读阮一峰的blog
### 4. 调试和测试：ava测试
	*** 这里主要以桑大自己的blog为大纲；https://github.com/i5ting/ava-practice
	1）ava安装
		npm install ava	
	2）使用
	=>代码, index.test.js
	   'use strict'	
	    import test from 'ava'
	    test('my test', t => {
	    	t.is(3, 3)
	    })
	=>执行测试
	  ava -v index.test.js
	  
	  ✔ my test
	  1 test passed [21:51:19]
	  
	3) 当然关于ava还有很多好玩儿的，请上：
	   https://github.com/i5ting/ava-practice
	   https://github.com/avajs/ava
	    
## 第一天下午：6月19日 13：00 -19：30
### 5. Http with Koa
	*** 主要以get系和post系为线索；
	1）get:
		=> xx/:id     	//获取：ctx.parmas(ctx.request.parmas)
		=> xx/?id=123  	//获取：ctx.parmas(ctx.request.query)
	2）post：请安装中间件koa-bodyparse  
		获取：ctx.body()
	3) file： 文件上传请安装中间件koa-multer
		使用：
		const multer = require('koa-multer')
		const upload = multer({dest: 'uploads/'})
		router.post('/file', upload.array('avatar'), function (ctx, next) {})
	
	4）postman：chrome浏览器中模拟发送http请求的插件
	5）curl：是一种命令行工具，作用是发出网络请求，然后得到和提取数据，显示在"标准输出"（stdout）上面。
	6）superkoa:桑大自己写的测试工具（koa with supertest for ava or mocha：https://www.npmjs.com/package/superkoa）

### 6. Koa核心：middleware
	1）“洋葱”模型
	2）koa三种实现方式：
		①Common：
			app.use((ctx, next) => {
				const start = new Date()
				return next().then(() => {
					const ms = new Date() - start
					console.log(`${ctx.method} ${ctx.url} - ${ms}ms`)
				});
			});
			
		②Gernerator:
			const co = require('co')
			app.use(co.wrap(fucntion * (ctx, next){
				const start = new Date()
				yeild next()
				const ms = new Date() - start
				console.log(`${ctx.method} ${ctx.url} - ${ms}ms`)
			}))
		③Async(Babel required,在执行时runkoa app.js)
			app.use(async (ctx, next)=>{
				const start = new Date()
				await next()
				const ms = new Date() - start
				console.log(`${ctx.method} ${ctx.url} - ${ms}ms`)
			})
			
		*** runkoa的使用：https://www.npmjs.com/package/runkoa
			
### 7. 异步流程控制
	1)Thunk
	2)Promise
	3)Generator
	4)Async
### 8. 数据库
	1)mongodb：https://www.mongodb.com/
	2)node中mongose的使用：
		①=>
		var mongoose = require('mongoose');
		mongoose.connect('mongodb://localhost/test');

		var Cat = mongoose.model('Cat', { name: String });

		var kitty = new Cat({ name: 'Zildjian' });
		kitty.save(function (err) {
  		if (err) {
    		console.log(err);
  		} else {
    	console.log('meow');
  		}
		});
		
		②koa中有个koa-mongose的模块：https://www.npmjs.com/package/koa-mongoose
		
		var app = require('koa')()
		var mongoose = require('koa-mongoose')
		var User = require('./models/user')
 
		app.use(mongoose({
	    mongoose: require('mongoose-q')(),//custom mongoose 
    	user: '',
	    pass: '',
    	host: '127.0.0.1',
	    port: 27017,
    	database: 'test',
	    db: {
    	    native_parser: true
	    },
    	server: {
        	poolSize: 5
	    }
		}))
 
		app.use(function* (next) {
	    var user = new User({
    	    account: 'test',
        	password: 'test'
	    })
    	yield user.saveQ()
	    this.body = 'OK'
		})		
		
### 9. 课后作业
	1）使用koa和mongodb完成用户的登陆，注册；
		*可以参考：https://github.com/moajs/moa
		
## 第二天上午：6月25日  9：00 -12：00
### 10. 项目实战:
	1）以https://github.com/moajs/moa2为基础，从工程结构，文件目录，源码分析了使用koa2开发web项目的方方面面；
	2）从package.json开始，对每一行作分析；
	

### 11. “丰衣足食”--npm模块
	1）编写，测试模块代码；
	2）使用github管理
	3）发布到npm上
	4) 拓展了到了写一个开源项目需要的流程(真是良心讲师啊O(∩_∩)O哈哈~)
## 第二天下午：6月19日 13：00 -19：30
### 12. koa实战(从0开始写一个基于koa的web框架)
	** 以https://github.com/i5ting/moag为例，开发一个脚手架为例
	1）编写一个脚手架所需一般知识：都有对应的模块，当然可以自己再造轮子
	目录的创建：https://github.com/substack/node-mkdirp
	文件的读写：https://github.com/i5ting/dirw
	时间的利用：https://github.com/moment/moment
	单词的处理：https://github.com/martinandert/inflected
	模板 引擎： https://github.com/i5ting/tpl_apply
### 13. 最佳实践
	1）具备上面的12的脚手架知识，就可以来个最佳实践了，开始动手写吧；可以参考https://github.com/i5ting/moag
:ballot_box_with_check: 以上就简单粗略的总结下koa研讨课总结，有很多都没有写出来，当然最佳效果是现场；如果感兴趣，可以关注rockq社区[网站](https://rockq.org)或者公众号，后续还有这样的研讨课；

附件：![0_1467684631308_FnJNdS6_jNyKfQD_t4cx4-4fk5QW.jpeg](//qn-rockq1.rockq.org/FnJNdS6_jNyKfQD_t4cx4-4fk5QW) 
