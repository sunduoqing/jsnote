## 持续更新ing（觉得对你有用就star一个呗～）
1. underfined不是关键字，要让一个变量指向未定义或删除该变量，`xxx = underfined`是错误的写法，因为
underfined可以当成一个变量来定义，就是说`var underfined = xxx`这样是合法的，
所以正确写法是使用void 加上任何数字，如`var xxx = void 0`，void加数字结果总是返回underfined。
（ps: typeof undefined  // 结果为underfined）

2. underfined、null、0、false、NaN、空字符串的逻辑结果均为false

3. 数组传递和复制
	
	```javascript
	ar a = [1,2,3];
	var b = a;
	delete b[1];
	console.log(a); // [1, undefined, 3]
	
	var a = [4,5,6];
	var b = a.slice(0);
	delete b[1];
	console.log(a); // [4, 5, 6]
	console.log(b); // [4, undefined, 6]
	```

4. 关于length
	1. 字符串的length值等于字符串个数.
	* 数组的length值等于数组长度.
	* 函数的length值等于形参个数.
	* arguments的length值等于实参个数.
	* object对象无length值.

5. 字符串API `charCodeAt`返回的unicode编码，通过toString(16)转成16进制，利用正则  
`/\u(00)“转换后的16位编码”/`可精确匹配。

6. 关于浮点数的运算误差
所有的支持二进制浮点数运算（绝大部分都是 IEEE 754[1] 的实现）都存在浮点数的运算误差。
	
	```javascript
	0.7 + 0.1  // 输出 0.7999999999  
	0.2 + 0.6  // 输出 0.8
	```

	其原因就是，在有限的存储空间下，绝大部分的十进制小数都不能用二进制浮点数来精确表示。例如，0.1 这个简单的十进制小数就不能用二进制浮点数来表示。
	你输入两个十进制数，转化为二进制运算过后再转化回来，在转化过程中自然会有损失了，
	但一般的损失往往在乘除运算中比较多，而JS在简单的加减法里也会出现这类问题，你也看到了，这个误差也是非常小的，但是却是不该出现的。
	
	```javascript
	// 解决方法  
	// 通过toFixed方法来指定四舍五入的小数位数  
	(0.2 + 0.1).toFixed(1)  // 0.3  
	(0.2 * 0.1).toFixed(1)  // 0.2
	```

7. 关于Ajax回调函数执行window.open等打开新窗口方法浏览器不支持解决方案。
	
	```javascript
	// 执行ajax时关闭异步，也就是把asnyc属性设置为false即可解决
	$.ajax({
		...
		asnyc: false
	});
	```

8. 今天面试YY遇到一道javascript笔试题，大概意思就是数组去重，当时自己写的方法不够高效，过后科普了一下，以此记录下来。

	```javascript
	function unique (arr) {
		var ret = []
		var hash = {}

		for ( var i = 0, len = arr.length; i < len; i++ ) {
			var elem = arr[i];
    		
    		if ( !hash[elem] ) {
      			ret.push(elem);
  				hash[elem] = true;
    		}
  		}
  		return ret
  	}
	```

9. js获取当前url参数值接口

	```javascript
	function getQueryString (name) {
	    var reg = new RegExp( "(^|&)"+ name +"=([^&]*)(&|$)", "i" );
	    var url = window.location.search; // 得到当前url的查询字串(?以及后面的字段)
	    var ret = url.substr(1).match(reg); // reg没有全局标志，所以ret[0]是完整的匹配，arr[1]是第一个括号里捕获的字串，依此类推。

	    if ( ret ) 
	   		return unescape(ret[2]); // ret[2]保存的是reg第二个括号捕获的字串，unescape用来解码url中的字符。
	   	else 
	   		return null;
	}
	```

10. JSONP跨域原理解析
	
	```javascript
	原理：利用在页面中创建<script>节点的方法向不同域提交HTTP请求并且可在url中指定回调函数，  
		  获取json数据并在指定的回调函数中执行。  

	缺点：如果返回的数据格式有问题或返回失败了，并不会报错。而且只支持GET而不支持POST等其它类型的HTTP请求。
	```

11. 也是YY一道面试题，考察this作用域。
	
	```javascript
	var foo = {
		bar: '你好',
		func: function () {
			alert(this.bar);
		}
	},
	bar = foo.func;
	bar();
	/* 
	 * 答案：function () { alert(this.bar) }
	 * 把foo.func函数赋值给全局变量bar然后执行，这等价于执行全局函数。全局函数的this是指向window的，
	 * 所以alert(this.bar)等于alert(window.bar)，也就是alert函数自身。
	 */
	```

12. 定义一个函数function add (x) { }，实现alert( add(2)(3)(4) )的结果能够等于9，且可复用。
	
	```javascript
	/* 
	 * 这实际上就是 currying（柯里化），也就是把一个多变量的函数变成一系列单变量的函数。
	 * 每个函数接收一个参数，然后返回一个接收余下参数并返回结果的新函数。
	 * 这个过程中利用了闭包（closure）。也就是说，这种情况下，是一个函数返回另一个函数。
	 */
	function add (x) {
	    var sum = x;
	    var all = function (y) {
	        sum = i + y;
	        return all;
	    };
	    all.toString = all.valueOf = function () {
	        return sum;
	    };
	    return all;
	}
	/* 
	 * 这个知识点主要用到，在JavaScript中，打印和相加计算，会分别调用toString或valueOf函数，
	 * 所以我们可以重写all的toString或valueOf方法，使其返回sum的值。
	 */
	```

13. 下面这道题，同样是在面试YY时遇到，主要考察作用域、变量声明、anguments。

	```javascript
	var b = 10;  
	function a (c) {  
		alert(b);
	    var b = 100;  
	    alert(b);  
	    anguments[0] = 2; 
	    alert(c);  
	}  
	a(3);
	alert(b);

	/* 
	 * 答案：underfined 100 2 10
	 * 1、a作用域内，js引擎会先查找所有声明的变量并赋值underfined，b已经声明，但此时还未赋值，所以b的值还是underfined。
	 * 2、b已经赋值100，alert结果100。
	 * 3、c传进去时是3，但c = anguments[0] = 2，即此时c等于2。
	 * 4、当前环境是全局作用域下，变量b在该环境下的值为10，故alert的结果为10。
	 */
	```

14. 了解变量声明提升和上下文对象。
	
	```javascript
	var a = 10;  
	function test () {  
	    a = 100;  
	    alert(a);  
	    alert(this.a);  
	    var a;  
	    alert(a);  
	}  
	test();  

	/* 
	 * 答案：100 10 100
	 * 1、test作用域内，var a由于变量声明提升，a ＝ 100并不是重置全局变量a，而是对当前作用域下a的赋值，所以alert的结果为100。
	 * 2、全局函数的this对指向window对象，所以this.a等于window.a，故alert的结果为10。
	 * 3、由于变量声明提升和赋值，此时的a还是100。
	 */
	```

15. JavaScript replace(RegExp, Function)解析
	
	template引擎的实现原理是通过正则匹配目标字串，replace传匿名函数对即将替换目标文本的字符串进行操作，想要了解template实现原理，我们要先了解replace的高级用法和实现原理。
	```javascript
	var str = 'dongshaohan';
	var reg = /a/g;
	str.replace(reg, function () {
		for ( var i = 0, len = arguments.length; i < len; i++ ) {   
        	console.log("第"+ (i + 1) +"个参数的值："+ arguments[i]);   
		};   
	});
	// 结果如下
	第1个参数的值：a
	第2个参数的值：6
	第3个参数的值：dongshaohan
	第1个参数的值：a
	第2个参数的值：9
	第3个参数的值：dongshaohan
	/*
	 * 匿名函数执行了两次，因为匹配到两次
	 * 第一个参数是通过reg匹配出来的结果，也就是表示匹配到的字符
	 * 第二个参数是匹配结果在str中的索引位置
	 * 第三个参数是str字符串本身
	 */
	
	var str = 'dongshaohan';
	var reg = /(h)a/g;
	str.replace(reg, function () {
		for ( var i = 0, len = arguments.length; i < len; i++ ) {   
        	console.log("第"+ (i + 1) +"个参数的值："+ arguments[i]);   
		};   
	});
	// 结果如下
	第1个参数的值：ha
	第2个参数的值：h
	第3个参数的值：5
	第4个参数的值：dongshaohan
	第1个参数的值：ha
	第2个参数的值：h
	第3个参数的值：8
	第4个参数的值：dongshaohan
	/*
	 * 匿名函数执行了两次，因为匹配到两次
	 * 第一个参数是通过reg匹配出来的结果，也就是表示匹配到的字符
	 * 第二个参数是reg中分组(h)所产生的结果
	 * 第三个参数是匹配结果在str中的索引位置
	 * 第四个参数是str字符串本身
	 */

	var str = 'dongshaohan';
	var reg = /(h)(a)/g;
	str.replace(reg, function () {
		for ( var i = 0, len = arguments.length; i < len; i++ ) {   
        	console.log("第"+ (i + 1) +"个参数的值："+ arguments[i]);   
		};   
	});
	// 结果如下
	第1个参数的值：ha
	第2个参数的值：h
	第3个参数的值：a
	第4个参数的值：5
	第5个参数的值：dongshaohan
	第1个参数的值：ha
	第2个参数的值：h
	第3个参数的值：a
	第4个参数的值：8
	第5个参数的值：dongshaohan
	/*
	 * 匿名函数执行了两次，因为匹配到两次
	 * 第一个参数是通过reg匹配出来的结果，也就是表示匹配到的字符
	 * 第二个参数是reg中分组(h)所产生的结果
	 * 第三个参数是reg中分组(a)所产生的结果
	 * 第四个参数是匹配结果在str中的索引位置
	 * 第五个参数是str字符串本身
	 */

	/* 最终结论 
	 * 第一个参数是reg这个完整的正则匹配出来的结果
     * 倒数第二个参数是第一个参数（匹配结果）在目标字符串中的索引位置
     * 最后一个参数是目标字符串本身
     * 如果reg中存在分组，那么参数列表的长度N>3一定成立，且第2到N-2个参数，分别为reg中分组所产生的匹配结果
     * 然后我们经常用到的一般都是第一个和倒数第二个，有时候能把一些基础的东西琢磨透，更能在开发中提高效率
     */
	```

16. 简单template实现原理。

	模板写法:
	```javascript
	<ul>
    <% for ( var i in items ) { %>
        <li class='<%= items[i].status %>'><%= items[i].text %></li>
    <% } %>
	</ul>
	```
	实现原理：
	* 遇到普通的文本直接当成字符串拼接。
	* 遇到`interpolate`(即`<%= %>`)，将其中的内容当成变量拼接在字符串中。
	* 遇到`evaluate`(即`<% %>`)，直接当成代码。

17. 货币快速换算
	
	```javascript
	var s = '1234567.89';
	parseFloat(s).toLocaleString(); // 1,234,567.89
	s.replace(/(\d)(?=(?:\d{3})+(?:\.|$))/g, '$1,'); // 1,234,567.89
	```

18. 奇淫技巧

	```javascript
	向下取整Math.floor，可以用|0，可以用~~，也可以用右移符>>代替。
	var a= 1.2|0; // 1
	var a = ~~1.2; // 1 （比Math.floor快4倍左右）
	var a = 1.2>>0; // 1
	/* 但是两者最好都只用在正整数上，因为只是舍掉了小数部分。Math.floor(-1.2)应该为-2，这两种方法的结果为-1 */

	用( new Function(stringCode) )()比eval快50倍。

	转数字用+
	var a = +'1234'; // 1234

	合并数组
	var a = [1,2,3];
	var b = [4,5,6];
	Array.prototype.push.apply(a, b);
	console.log(a); // [1,2,3,4,5,6]

	交换值
	a= [b, b=a][0];

	快速取数组最大和最小值
	Math.max.apply(Math, [1,2,3]); // 3
	Math.min.apply(Math, [1,2,3]); // 1

	判断IE
	var ie = /*@cc_on !@*/false;

	运算符-->叫做趋向于，可以声明一个变量 然后让他 趋向于 另一个数。
	var x = 10; while (x --> 0)console.log(x); 
	// 9,8,7,6,5,4,3,2,1,0
	```

19. 高效复制对象
	
	```javascript
	// 浅复制
	var newObject = jQuery.extend({}, oldObject);

	// 深复制 较慢
	var newObject = jQuery.extend(true, {}, oldObject);

	// 比深复制快10% - 20% 但对Date对象不适用
	var newObject = JSON.parse(JSON.stringify(obj));
	```
	
20. 一道javascript笔试题
	
	```javascript
	var x = 20;
	var temp = {
	    x: 40,
	    foo: function () {
	        var x = 10;
	        return this.x;
	    }
	};

	console.log( temp.foo() );  			// 40
	console.log( (temp.foo)() ); 			// 40
	console.log( (temp.foo = temp.foo)() ); // 20
	console.log( (temp.foo, temp.foo)() );	// 20
	console.log( temp.foo.apply(window) );  // 20
	console.log( temp.foo.apply(temp) ); 	// 40
	```

21.YaHoo Web优化的14条原则
1.尽可能的减少HTTP的请求数
	```javascript
	http请求是要开销的，想办法减少请求数自然可以提高网页速度。常用的方法，合并css，js（将一个页面中的css和js文件分别合并）以及 Image maps和css sprites等。
	当然或许将css，js文件拆分多个是因为css结构，共用等方面的考虑。阿里巴巴中文站当时的做法是开发时依然分开开发，然后在后台 对js，css进行合并，
	这样对于浏览器来说依然是一个请求，但是开发时仍然能还原成多个，方便管理和重复引用。yahoo甚至建议将首页的css和js 直接写在页面文件里面，而不是外部引用。
	因为首页的访问量太大了，这么做也可以减少两个请求数。而事实上国内的很多门户都是这么做的。而css sprites是指只用将页面上的背景图合并成一张，
	然后通过css的background-position属性定义不过的值来取他的背景。
	```

2.使用CDN（内容分发网络）
	```javascript
	用户离web server的远近对响应时间也有很大影响。从用户角度看，把内容部署到多个地理位置分散的服务器上将有效提高页面装载速度。但是该从哪里开始呢？
	作为实现内容地理分布的第一步，不要试图重构web应用以适应分布架构。改变架构将导致多个周期性任务，如同步session状态，在多个server之间复制数据库交易。
	这样缩短用户与内容距离的尝试可能被应用架构改版所延迟，或阻止。
	我们还记得80-90%的最终用户响应时间花在下载页面中的各种元素上，如图像文件、样式表、脚本和Flash等。与其花在重构系统这个困难的任务上，还不如先分布静态内容。
	这不仅能大大减少响应时间，而且由于CDN的存在，分布静态内容非常容易实现。
	CDN是地理上分布的web server的集合，用于更高效地发布内容。通常基于网络远近来选择给具体用户服务的web server。
	一些大型网站拥有自己的CDN，但是使用如Akamai Technologies, Mirror Image Internet, 或 Limelight Networks等CDN服务提供商的服务将是划算的。在Yahoo!把静态内容分布到CDN减少了用户影响时间20%或更多。切换到CDN的代码修改工作是很容易的，但能达到提高网站的速度。
	```