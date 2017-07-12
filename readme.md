
### seajs引入jQuery技巧

> 参阅链接

```
    https://www.zhihu.com/question/21703739
    
    http://blog.csdn.net/y491887095/article/details/53230492
    
    http://blog.csdn.net/vtegong/article/details/54925824
    
    http://blog.csdn.net/bob_baobao/article/details/50803590

    github:
        
        https://github.com/glenlzk/seajs_jQuery

```

> jQuery.js

```
    

    // Register as a named AMD module, since jQuery can be concatenated with other
    // files that may use define, but not via a proper concatenation script that
    // understands anonymous AMD modules. A named AMD is safest and most robust
    // way to register. Lowercase jquery is used because AMD module names are
    // derived from file names, and jQuery is normally delivered in a lowercase
    // file name. Do this after creating the global so that if an AMD module wants
    // to call noConflict to hide this version of jQuery, it will work.
    
    if ( typeof define === "function" ) {       // 添加源码处
    	define(function() {
    		return jQuery;
    	});
    }
    
    
    
    
    var
    	// Map over jQuery in case of overwrite
    	_jQuery = window.jQuery,
    
    	// Map over the $ in case of overwrite
    	_$ = window.$;
    
    jQuery.noConflict = function( deep ) {
    	if ( window.$ === jQuery ) {
    		window.$ = _$;
    	}
    
    	if ( deep && window.jQuery === jQuery ) {
    		window.jQuery = _jQuery;
    	}
    
    	return jQuery;
    };
    
    // Expose jQuery and $ identifiers, even in
    // AMD (#7102#comment:10, https://github.com/jquery/jquery/pull/557)
    // and CommonJS for browser emulators (#13566)
    if ( typeof noGlobal === strundefined ) {
    	window.jQuery = window.$ = jQuery;
    }
    
    
    
    
    return jQuery;





```


> jquery-2.0.0.min.js


```
        // 一般都在jQuery末尾

        // jQuery支持的是AMD规范，在jQuery文件的末尾会有以下的代码:
        
        if ( typeof define === "function" && define.amd && define.amd.jQuery ) {    
            define( "jquery", [], function () { return jQuery; } );
            
        }
        
       // 将jQuery里源码里的define.amd && define.amd.jQuery删除，jQuery便可以自动模块化:
       
       if ( typeof define === "function") {    
            define( "jquery", [], function () { return jQuery; } );
        }
        
       // -------------------------- 实例1：
       
       // 压缩的代码源码：
        "function"==typeof define&&define.amd&&define("jquery",[],function(){return x})
        
       // 修改源码后：
        "function"==typeof define&&define(function(){return x;})
```

> 方案三：为实验过

```
    define(function(){
        //jquery源代码
        return $.noConflict();
    });

```

> 实例引用

```
    <!--html里无需引入jquery.js-->
    <!--<script src="js/h5tool/lib/jquery.js"></script>-->
    <!--<script src="js/h5tool/lib/jquery-2.0.0.min.js"></script>-->
    <script src="js/h5tool/lib/sea-debug.js"></script>
    <script>
    
        seajs.config({
            alias: { 'jQuery': '../lib/jquery-2.0.0.min.js' }
        });
    
        seajs.use("./js/h5tool/layout/init.js");
        
        // init.js
        define(function(require, exports, module) {
            //引用jQuery模块
            var $ = require( 'jQuery' );     // alias
        
            //右侧顶部导航 全局
            $(".dialog-cate").on("click", 'a', function(){
                $(".dialog-cate a").removeClass("current");
                $(this).addClass("current");
            });
        
        });
    </script>

```