###能量球活动总结（主要为IE8兼容性问题）
#####IE CSS3
IE8不支持通过rgba设置半透明的背景色，通过设置
filter: progid:DXImageTransform.Microsoft.gradient(startColorstr='#b3000000', endColorstr='#b300000'); 
·#b3000000·中b3为透明度，000000表示颜色
[ie css3 兼容写法](http://www.ruanyifeng.com/blog/2010/03/cross-browser_css3_features.html)

#####父元素大小不随着子元素的高度变化
IE8中js动态向inline-block中添加元素的时候，inline-block的高度可能不会随内容高度变化而变化。通过js触发浏览器的重绘来更新inline-block的高度。方法包括

	ele.style.visibility = "inherit";
	ele.style.visibility = "visible";
	
或者
	
	ele.className = ele.className;
	
#####调试工具
IE调试工具，审查元素的时候无法看到js生成的html。
<br>
_解决办法_：关掉开发者工具，刷新页面。在对应的html被生成后再打开开发者工具

#####元素无法被点击
通过filter:progid:DXImageTransform.Microsoft.AlphaImageLoader(src='../../images/login/back-ground.png',  sizingMethod='scale')为一元素设置背景图片，动态向该元素中添加子元素，出现子元素无法被点击的问题。<br>
_解决办法_：为无法被点击的子元素的父元素添加`position:relative`

#####png24出现黑边
以png24为背景的div在用jquery fadeIn，fadeOut的时候会出现黑边。
<br>
_解决方法_:为该div添加一个父元素，为父元素添加`filter: progid:DXImageTransform.Microsoft.gradient(startColorstr='#b3000000', endColorstr='#b300000'); `。对它的父元素使用fadeIn，fadeOut方法
####总结
1.	ie8及以下虽然不支持css3，但是可以通过ie自带的filter实现类似的效果。(ie11不支持filter)

