# 持续更新ing
1. underfined不是关键字，要让一个变量指向未定义或删除该变量，`xxx = underfined`是错误的写法，因为
underfined可以当成一个变量来定义，就是说`var underfined = xxx`这样是合法的，
所以正确写法是使用void 加上任何数字，如`var xxx = void 0`，void加数字结果总是返回underfined。

2. 关于length
	1. 字符串的length值等于字符串个数.
	* 数组的length值等于数组长度.
	* 函数的length值等于形参个数.
	* arguments的length值等于实参个数.
	* object对象无length值.

3. 字符串API `charCodeAt`返回的unicode编码，通过toString(16)转成16进制，利用正则  
`/\u(00)“转换后的16位编码”/`可精确匹配。

4. 关于浮点数的运算误差
所有的支持二进制浮点数运算（绝大部分都是 IEEE 754[1] 的实现）都存在浮点数的运算误差。

	0.7 + 0.1  //  输出 0.7999999999
	0.2 + 0.6  //  输出 0.8

其原因就是，在有限的存储空间下，绝大部分的十进制小数都不能用二进制浮点数来精确表示。例如，0.1 这个简单的十进制小数就不能用二进制浮点数来表示。
你输入两个十进制数，转化为二进制运算过后再转化回来，在转化过程中自然会有损失了，
但一般的损失往往在乘除运算中比较多，而JS在简单的加减法里也会出现这类问题，你也看到了，这个误差也是非常小的，但是却是不该出现的。

	// 解决方法 
	// 通过toFixed方法来指定四舍五入的小数位数
	(0.2 + 0.1).toFixed(1)  //  0.3
	(0.2 * 0.1).toFixed(1)  //  0.2

