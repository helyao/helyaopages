---
layout: post
title:  "Let QtWebkit support bind"
date:   2017-3-13 16:10:21 +0800
categories: helyao update
---

### QtWebkit

> 在简单的Native App的任务中，为了减少界面和跨平台等繁杂的事情，我会选择htmlPy来开发，可以实现使用Jinja2实现界面的部署，可是内嵌的QtWebkit由于不支持bind()方法，使得很多js库无法使用，因此需要自己将该方法实现

### bind()函数

> bind()方法会创建一个新函数

	fun.bind(thisArg[, arg1[, arg2[, ...]]])

> 当这个新函数被调用时，bind()的第一个参数将作为它运行时的this，之后的一序列参数将会在传递的实参前传入作为他的参数

### 通用兼容方法

	<script type="application/javascript">
	    if (!Function.prototype.bind) {
                Function.prototype.bind = function (oThis) {
            	    if (typeof this !== "function") {
              	        throw new TypeError("Function.prototype.bind - what is trying to be bound is not callable");
            	    }
            	    var aArgs = Array.prototype.slice.call(arguments, 1), 
			fToBind = this, fNOP = function () {}, fBound = function () {
                    	    return fToBind.apply(this instanceof fNOP ? this : oThis || this,
                                aArgs.concat(Array.prototype.slice.call(arguments)));
                	};
            	    fNOP.prototype = this.prototype;
            	    fBound.prototype = new fNOP();
            	    return fBound;
        	};
    	    }
	</script>

> 该方法可以兼容各种浏览器

### 支持QtWebkit的简易方法

	<script type="application/javascript">
    	    Function.prototype.bind = function (bind) {
        	var self = this;
        	return function () {
            	    var args = Array.prototype.slice.call(arguments);
            	    return self.apply(bind || null, args);
        	};
    	    };
	</script>

> 对于确定在QtWebkit下使用，该方法即可


