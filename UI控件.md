###UI控件的创建

UI控件类需要继承NEJ的UI控件基类（ui/base._$$Abstract）

	define([
    	'base/klass',
    	'ui/base'
	],function(_k,_i,_p){
    	_p._$$MyUI = _k._$klass();
    	var _pro = _p._$$MyUI._$extend(_i,_$$Abstract);
	});

###UI控件实例的创建
NEJ中的控件通过类的`_$allocate()`方法来创建实例

	define([
    	'base/klass',
    	'ui/base'
	],function(_k,_i,_p){
    	_p._$$MyUI = _k._$klass();
    	var _pro = _p._$$MyUI._$extend(_i,_$$Abstract);
    	var instance = _p._$$MyUI._$allocate();
	});


###UI控件html结构
通过重写父类的`__initXGui()`方法来设置UI控件的HTML结构。通过将`this.__body`赋值为DOM节点或者`this.__seed_html`赋值为html字符串来设置HTML结构。通过将`this.__seed_css`赋值为类名来为HTML结构添加css类.

	define([
    	'base/klass',
    	'ui/base'
	],function(_k,_i,_p){
    	_p._$$MyUI = _k._$klass();
    	var _pro = _p._$$MyUI._$extend(_i,_$$Abstract);
    	_pro.__initXGui = function(){
    		//方法1：
    		this.__body = document.createElement('div');
    		//方法2：
    		this.__seed_html = '<div></div>';
    		//添加类
    		this.__seed_css = 'myCss';
    	};
	});
	
###UI控件添加浏览器事件
通过	`this.__doInitDomEvent()`为控件添加浏览器事件，在控件被回收时，添加的事件会自动清除。

	define([
    	'base/klass',
    	'ui/base'
	],function(_k,_i,_p){
    	_p._$$MyUI = _k._$klass();
    	var _pro = _p._$$MyUI._$extend(_i,_$$Abstract);
    	
    	_pro.__reset = function(){
    	
    		var dom = document.getElementByid('id');
    		
    		this.__doInitDomEvent([
    			[dom,'click',this.__onclick._$bind(this)]
    		]);
    	};
    	_pro.__onclick = function(){
    		//do something here!
    	};
	});

###UI控件添加自定义事件
通过`this._$batEvent()`批量添加自定义事件。<br>
通过类的`_$allocate()`方法添加自定事件<br>
通过`this._$dispatchEvent()`来触发自定义事件。


	define([
    	'base/klass',
    	'ui/base'
	],function(_k,_i,_p){
    	_p._$$MyUI = _k._$klass();
    	var _pro = _p._$$MyUI._$extend(_i,_$$Abstract);
    	
    	_pro.__reset = function(){
    		//添加
    		this._$batEvent({
    			e1:function(){},
    			e2:function(){}
    		});
    	};
    	//添加自定义事件
    	var instance = _p._$$MyUI._$allocate({
    		e3:function(){},
    		e4:function(){}
    	});
    	//触发自定义事件（arg1,arge2……将被传递给事件对应的函数,可选）
    	instance._$dispatchEvent('e1',arg1,arg2);
	});
	
###将UI控件添加到页面中
通过`this._$appendTo()`将控件添加到页面中<br>
通过类的`_$allocate()`将控件添加到页面中


	define([
    	'base/klass',
    	'ui/base'
	],function(_k,_i,_p){
    	_p._$$MyUI = _k._$klass();
    	var _pro = _p._$$MyUI._$extend(_i,_$$Abstract);
    
    	var instance = _p._$$MyUI._$allocate({
    		//方法1
    		parent:document.body
    	});
    	//方法2
    	instance._$appendTo(document.body);
    	//方法3
    	instance._$appendTo(function(_body){
    		document.body.appendChild(_body);
    	});
	});
	
###UI控件的回收

	define([
    	'base/klass',
    	'ui/base'
	],function(_k,_i,_p){
    	_p._$$MyUI = _k._$klass();
    	var _pro = _p._$$MyUI._$extend(_i,_$$Abstract);
    
    	var instance = _p._$$MyUI._$allocate();
    	
    	instance._$recycle();
    });


