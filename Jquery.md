1.$(document).ready() 可以简写成 $()，它表示一个jquery对象，表示文档加载完毕后被调用；
2.jquery和dom之间的转换：如果你要使用 obj.innerHTML ，那只有当 obj 是一个 DOM 对象时才能用
                         相应地，如果是 jQuery 对象你应该使用 obj.html()，
						 从 DOM 对象转到 jQuery 对象： $(obj) 
                         Query 对象转到 DOM 对象： obj[0]
						 比较正规地从 jQuery 对象到 DOM 的转换，是使用 jQuery 对象的 get() 方法。
3.选择器：常规选择器 ：$("*")选择所有结点，(“#id”) -ID选择器，(“.class”)-类选择器；
          属性选择器：[name~="value"]-属性中包括某单词，[name="value"]-属性完全等于指定值
                      [name!="value"]-属性不等于指定值，[name]-包括有指定属性的元素
		  控件选择器：:checked -选择所有被中的元素 ，:selected-被选择了的元素
		              :disabled :enabled-选择被禁用/未禁用的元素等
4.元素控制相关:attributes 是 XML 结构中的属性节点概念范畴,properties 则是对于 DOM 对象的;
               但是它们对于某些特殊的属性是有共同的访问属性名的，比如 id 。
5.工具函数： .map()-遍历所有成员，.slice()-序列切片, 支持一个或两个参数，支持负数
  Ajax
6.1请求与回调
   jQuery 对 AJAX 的封装参数比较多，jQuery 的 AJAX ，核心的请求处理函数只有一个，就是 $.ajax() ，
   然后就是一个简单的上层函数。$.ajax() 的基本使用形式是：jQuery.ajax( settings )
    settings 是一个对象，里面包含了所有的配置项。常用的有：【
							url
							请求的地址。
							type
							请求的方法类型， GET , POST 。 默认是 GET 。
							data
							要发送出去的数据。
							dataType
							服务器返回的数据类型，支持： 'xml', 'html', 'script', 'json', 'jsonp', 'text' 。
							success
							请求成功时调用的处理函数。 success(data, textStatus, jqXHR) 。
							context
							回调函数执行时的上下文】
    $.ajax(options)----AJAX 请求的默认配置。前面提到过， $.ajax() 是一个核心函数，
	   在其之上，有一些现成的封装，常用的是：
					【$.get( url [, data] [, success(data, textStatus, jqXHR)] [, dataType] )
					GET 请求
					$.post( url [, data] [, success(data, textStatus, jqXHR)] [, dataType] )
					POST 请求】
