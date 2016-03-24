###针对短时间内会执行多次的函数的优化处理
其实是understore中一个函数的解读。对原函数进行封装，返回新函数。当短时间内新函数被调用多次时，会在新函数最后一次调用的`wait`时间后，调用原函数。

上代码啦~~


	/**
	 *@param {Function} 原函数
	 *@param {Number} 上文提到的wait
	 *@param {Boolean} 当新函数在短时间内被调用时是否立即执行原函数
	 *@return {Function} 新函数
	 */
	_.debounce = function(func, wait, immediate) {
    	var timeout, args, context, timestamp, result;
		
		//这个函数的名字就叫‘延迟函数’吧
    	var later = function() {
    	
    		//最近一次新函数执行时距离现在的时间
      		var last = _.now() - timestamp;
			
			//如果在新函数短时间内第一次被执行后新函数仍被调用，继续延迟执行延迟函数
      		if (last < wait && last >= 0) {
        		timeout = setTimeout(later, wait - last);
      		} else {
        		timeout = null;
        		if (!immediate) {
          		result = func.apply(context, args);
          		if (!timeout) context = args = null;
        		}
      		}
    	};
		
		//返回‘新函数’
    	return function() {
      		context = this;
      		// 每次调用新函数，会跟新原函数执行时的参数
      		args = arguments;
      		
      		//记录最近一次新函数被执行的时间
      		timestamp = _.now();
      		
      		//是否在新函数被执行时立即执行原函数
      		var callNow = immediate && !timeout;
      		
      		//当新函数在短时间内第一次内被执行时，延迟执行延迟函数。
      		if (!timeout) timeout = setTimeout(later, wait);
      		if (callNow) {
        		result = func.apply(context, args);
        			context = args = null;
      		}

      		return result;
    	};
  	};
  	
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
