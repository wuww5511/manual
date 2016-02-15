#模块间通信规范(个人总结)
注：当一个子模块的某些方法需要被许多父模块调用时，抽离出这些方法封装成公用模块，通过中介者与需要调用它的模块通信
###父模块调用子模块方法

	//父模块中某方法实现
	function () {
		var _sub = _$$sub._$allocate();
		//调用子模块的共有方法，以函数返回值的形式接受处理结果
		var _res = _sub._$func();
	}
	
###子模块调用父模块方法

	//子模块中某方法
	function () {
	
		//子模块中触发指定事件，通过函数接收处理结果(事件名以request开头，表示请求父模块的操作)
		this._$dispatchEvent('requestFunc', {
			//传递数据
			data:{
			},
			cb:function (_res) {
				//显示处理结果
				console.log(_res);
			}
		})
	}
	
	//父模块中某方法
	function () {
		var _sub = _$$sub._$allocate({
		
			//父模块执行指定操作，并将处理结果传递给回调函数
			'requestFunc':function (_opts) {
				var _res = doSomething(_opts.data);
				_opts.cb(_res);
			}
		});
	}
	

#操作处理结果返回格式
		
		{
			//操作是否成功
			success: true,
			//返回的数据
			data: {
			}
		}