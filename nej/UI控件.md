###UI控件的创建

UI控件类需要继承NEJ的UI控件基类（ui/base._$$Abstract）

	define([
    	'base/klass',
    	'ui/base'
	],function(_k,_i,_p){
    	_p._$$MyUI = _k._$klass();
    	var _pro = _p._$$MyUI._$extend(_i._$$Abstract);
	});

###UI控件实例的创建
NEJ中的控件通过类的`_$allocate()`方法来创建实例。参数为自定义配置，会被传递给`this.__init()`和`this.__reset()`。`this.__init()`在创建实例时执行，`this.__reset()`在每次分配实例时执行，所以对控件的配置处理最好放在`this.__reset()`中。

	define([
    	'base/klass',
    	'ui/base'
	],function(_k,_i,_p){
    	_p._$$MyUI = _k._$klass();
    	var _pro = _p._$$MyUI._$extend(_i,_$$Abstract);
    	var _opts = {
    		//作为css类添加到控件的HTML结构上
    		clazz:'myCss',
    		//将控件的HTML结构添加到该节点中
    		parent:doucment.body,
    		//绑定自定义事件
    		onxx:function(){}
    	};
    	var instance = _p._$$MyUI._$allocate(_opts);
	});


###UI控件html结构
通过重写父类的`__initXGui()`方法来设置UI控件的HTML结构。通过将`this.__seed_html`赋值为模板序列号(通过`tpl._$addNodeTemplate()`获取)来设置HTML结构。通过将`this.__seed_css`赋值为类名来为HTML结构添加css类.

	define([
    	'base/klass',
    	'ui/base',
    	'util/template/tpl'
	],function(_k,_i,tpl,_p){
    	_p._$$MyUI = _k._$klass();
    	var _pro = _p._$$MyUI._$extend(_i,_$$Abstract);
    	_pro.__initXGui = function(){
    		//方法1：
    		
    		this.__seed_html = tpl._$addNodeTemplate('<div></div>');
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
    	//触发自定义事件（arg1,arg2……将被传递给事件对应的函数,可选）
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
	
###UI控件的销毁和回收
通过重写`this.__destroy()`添加销毁实例时的操作<br>
通过`this._$recycle()`回收实例<br>
通过类的`_$recycle()`方法回收实例

	define([
    	'base/klass',
    	'ui/base'
	],function(_k,_i,_p){
    	_p._$$MyUI = _k._$klass();
    	var _pro = _p._$$MyUI._$extend(_i,_$$Abstract);
    	_pro.__destroy = function(){
    		this.__super();
    		//do something here!
    	};
    	var instance = _p._$$MyUI._$allocate();
    	//回收实例
    	instance._$recycle();
    	//回收实例
    	_p._$$MyUI._$recycle(instance);
    	
    });

回收实例时会调用`this.__destroy()`方法，并将该实例进行缓存，以备下次分配实例时使用。(`_$allocate()`)


###UI控件生命周期函数
添加`onbeforecycle()`和`onaftercycle()`自定义事件，它们会分别在控件回收前和回收后执行。

	define([
    	'base/klass',
    	'ui/base'
	],function(_k,_i,_p){
    	_p._$$MyUI = _k._$klass();
    	var _pro = _p._$$MyUI._$extend(_i,_$$Abstract);
    	
    	var instance = _p._$$MyUI._$allocate({
    		//回收前执行
    		onbeforecycle:function(){},
    		//回收后执行
    		onaftercycle:function(){}
    	});
    	
    	
    });


