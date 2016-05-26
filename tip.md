
###获取浏览器宽度
	
	var _ww = window.innerWidth || document.documentElement.clientWidth;
	var _wh = window.innerHeight || document.documentElement.clientHeight;
	
 window.innerWidth, window.innerHeight(IE8及以前的不支持)
 
 document.documentElement.clientWidth, document.documentElement.clientHeight(标准模式下获取浏览器宽高)
 
 document.body.clientWidth,document.body.clientHeight(混杂模式，应该用不上~~)
 
###鼠标事件位置
`event.screenX`, `event.screenY` 相对屏幕左上角的位置

`event.clientX`, `event.clentY` 相对浏览器左上角的位置

###页面跳转时的ajax
当页面注销时发出ajax请求，个别浏览器可能无法成功发出请求。
解决方法：ajax异步请求改为同步

###正则表达式
1，选取一个指定字符串

		
		var reg = /\s/;
		reg.test('absa dfc');
		console.log(RegExp.$1)

2,选取多个指定字符串

		var reg = /\s/g;
		var res = 'abc dd'.match(reg);
		
3,选取多个字符串的指定部分

		var reg = /\s(\S)\s/g;
		var str = " a  b  c  d  e  f  ";
		var res = reg.exec(str);
		while(res){
			console.log(RegExp.$1);
			res = reg.exec(str);
		}
		
4,默认为贪婪模式，在使正则能够正常匹配的条件下，尽可能多的匹配

5,非贪婪模式，与贪婪模式相对应，尽可能少的匹配

		//在量词后加一个问号‘？’，表示进入非贪婪模式
		var reg = /aabb(adsf)(\s+?)asdfsdf/g;
		
		
###获取元素宽高
通过css设置元素宽高的时，形式可能是‘100%’， ‘auto’， ‘1em’， ……。
要获取以px为单位的大小，可以通过`window.getComputedStyle（）`， 但是IE8不支持啊~~~
IE中的`node.currentStyle`返回的不是以px为单位的大小。
怎么办呢？用`node.clientHeight`,`node.clientWidth`。（包括padding）
`node.offsetWidth`, `node.offsetHeight`是包括边框的宽高。

###获取元素内的文本
ele.innerText || ele.textContent
(fixFox下无innerText)

###获取窗口的滚动距离
`document.documentElement.scrollTop || document.body.scrollTop`

###keydown,keypress
keydown事件返回的code为按键在键盘上的代码；keypress返回的code为ASCII码，keypress只会在字符键按下时触发

###浏览器内粘贴文本
亲测，兼容ie8~（如果不放心可以捕获一下异常~）

```
<textarea id="ta">to copy</textarea>

```

```
//选择输入框内内容
document.getElementById('ta').select();
//复制
document.execCommand('copy');
```

###table布局
`display:table-cell`的特性：
*	只能和table-cell和float的元素在一行
*	可以为它设置宽度，但是在布局被压缩时，它的宽度会缩小。类似给它设置了一个最大宽带。
*	无法为它设置margin
*	多个table-cell放在一起，那么它们会是等高的
*	`float`,`position:absolute`会破坏它的display属性
*	

###zIndex（待总结）

### 全局变量可以作为参数传递给局部函数，方便局部函数调用