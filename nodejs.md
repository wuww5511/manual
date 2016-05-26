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
*	`res.send("string");` 会同时发送报头和报文。如果在该函数之前调用`res.write("string")`会报错。报头必须在报文的前面发送
*	[eventproxy](https://github.com/JacksonTian/eventproxy)实现事件的绑定和触发
*	[async](https://github.com/caolan/async) 用来控制并发数量

<strong>……过了好多天~~~</strong>

###零散的记录
*	Buffer存储的数据为二进制，类似于数组，`buffer.length`,`buffer[0] = 12`.
*	Buffer间的`+`运算 `buffer1 + buffer2` 等同于 `new Buffer(buffer1.toString() + buffer2.toString())`.在分段接收数据时，这种拼接buffer的方式对于中文字符存在问题。正确的拼接姿势是

		var chunks = [buf1, buf2, buf3];//记录所有的buffer
		var size = 0;//所有buffer长度和. buf1.length + buf2.length + buf3.length
		Buffer.concat(chunks, size);
		
*	在网络传输中，以buffer传输数据效率更高
*	require('path')模块处理文件路径的处理；path.join('/a/b', '/c') => '/a/b/c'
*	require默认会将已经加载过的模块缓存；想要删除缓存的话，`delete require.cache[require.resolve(_file)];`