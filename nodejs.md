#开始学习nodejs了
###第一天
*	npm init 初始化项目
*	npm install --dev 安装依赖
*	npm install utility --save 使用`utility`库，包含一些常用功能[utility](https://github.com/node-modules/utility)
*	`https://github.com/expressjs/body-parser` 处理post报文中body中的数据。
*	`req.query`对象获取报文中的get参数

###第二天
*	[superagent](http://visionmedia.github.io/superagent/) http方面的库
*	[cheerio](https://github.com/cheeriojs/cheerio) 解析html的库
*	`url.resolve`计算真实的跳转链接. `url.resolve("http://a/b", "/c")` 结果 `"http://a/c"`