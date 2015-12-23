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
  	

  	
