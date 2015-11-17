#UI控件的创建
UI控件类需要继承NEJ的UI控件基类（ui/base._$$Abstract）
`define([
    'base/klass',
    'ui/base'
],function(_k,_i,_p){
    _p._$$MyUI = _k._$klass();
    var _pro = _p._$$MyUI._$extend(_i,_$$Abstract);
});`