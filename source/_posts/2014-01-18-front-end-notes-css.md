title: Web前端笔记 - CSS篇
date: 2014-01-18 19:00:06
categories: 学习笔记
tags: 
	- Web
	- CSS
---

### 垂直居中
---

1.	**line-height**  
	设置元素的line-height与height相等。  
	注：仅适用于单行文本或图片，可以用于button等小元素。

2.	**绝对定位**  
	设置position为absolute*（注意父元素的position不能为默认值，否则将以浏览器窗口进行定位）*，top为50%，固定高度，margin-top为高度的一半取负。  
	注：不能自适应内容的高度，元素脱离文档流。

<!--more-->

3.	**模拟表格**  
	定义一个父元素，设置display属性为table，子元素display设置为table-cell，并且vertical-align为middle。  
	注：父元素需要定高，能够自适应内容宽度，无法在IE6、IE7下使用。
	
	``` html HTML
		<div id="container"> 
		    <div id="content">content</div> 
		</div>
	```
	``` css CSS
		#container { 
		    height: 300px; 
		    display: table;    /*以表格形式渲染*/ 
		} 
		#content { 
		    display:table-cell; 
		    vertical-align: middle; 
		}   
	``` 

4.	**inline-block**  
	添加一个额外的块元素，设置为内联对象，高度为100%。注：#parent需要定高。  

	``` html HTML
		<div id="parent"> 
		    <div id="vertically_center"> 
		        <p>I am vertically centered!</p> 
		    </div> 
		    <div id="extra"></div> 
		</div>
	```
	``` css CSS
		#vertically_center, #extra { 
		    display: inline-block; /*把元素转为行内块显示*/ 
		    vertical-align: middle; /*垂直居中*/ 
		} 
		#extra { 
		    height: 100%;  /*设置线盒型为父元素的100%高度*/ 
		} 
	```

5.	**padding**  
	给元素设置相同的padding-top和padding-bottom值。注：元素不能定高。

6.	**jQuery**

	```
	$(window).resize(function(){ 
	    $('.container').css({ 
		    position:'absolute', 
		    left: ($(window).width() - $('.container').outerWidth())/2, 
		    top: ($(window).height() - $('.container').outerHeight())/2 
		}); 
	}); 
	```

### 水平居中
---

1. **margin : auto**  
	注：元素需要定宽。

2. **绝对定位**  
	类似垂直居中的绝对定位。

3. **text-align ：center**

### position用法
---

1. relative  
	基于对象margin的左上侧相对于自身的正常位置并进行偏移，其余元素不受影响。

2. absolute  
	+ 若父对象设置了position属性且不是默认值,按此父对象定位，且忽略padding，此时，该对象脱离普通流，不占据空间。  
	+ 若所有父对象均没有设置position属性，则按**浏览器窗口**定位。

3. fixed  
	也可以认为是absolute的特殊情况，即总是以浏览器窗口进行定位，与background-attachment:fixed相似。

4. static  
	默认值，按正常的文档流进行排列。


*整理自 http://www.w3cplus.com/css/vertically-center-content-with-css*