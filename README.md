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
