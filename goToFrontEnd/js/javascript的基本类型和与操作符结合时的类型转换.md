## javascript 的基本类型
javascript的基本类型和类型转换系统相较于其他语言例如 Java 来说可以说是非常混乱的.这个是许多新手必定会遇到的坑.首先javascript 有五种简单的基本类型(undefined,null,Number,Boolean,String).和一种复杂的数据类型object.  
	   
类型检测有两种方式 typeof 和 instanceof . 

instanceof用来检测对象的原型链. 但有时候 instanceof 也会不好用比如不同window.frames[0]里的 Array检测,对于已经实现了 toString 方法的类型,我们可以用Object.prototype.toString.call(obj) 来检测,得到结果类似[object Array].

typeof : 因为NaN属于 number 的一种所以 typeof NaN === 'number' ; 在javascript 里 Object,String等都是一种构造函数,所以 typeof Object === 'function',typeof String === 'function'.
typeof 所有的检测结果如下:
![img](https://github.com/kukuv2/blog/raw/master/picture/屏幕快照 2016-02-26 上午11.11.45.png)

## 与操作符结合后的 javascript 类型转换
		弱类型的 javascript 的许多操作符会自动类型转换,很多时候转换后的
		结果会让人吃惊.下面我们来总结一下:
### 1. 尝试将两个变量转换为数值(调用 Number())的操作符 :一元操作符 ++,--;乘性操作符 *,
		Number的转换规则是 
		1.1 如果是string:
			1.1.1 判断是否能转换为数值含有字母和其他非.符号的直接返回 
	NaN ; 1.0 , .1 均可以转换为数值 ,.1.1含两个以上的不能转换为数值.
			1.1.2 能转换为数值的返回对应数值.
		1.2 如果是boolean:
			1.2.1 true 转换为0;
			1.2.2 false 转换为1;
		1.3 如果是 undefined: 转换为 NaN
		1.4 如果是 null 转换为 0
		1.5 如果是 Object var result = obj.valueof(); return Number(result) 如果得到 NaN,再调用 toString
### 2. 尝试将两个变量转换为布尔值: 布尔操作符 !;条件操作符 ? :
		Boolean的转换规则是:
		2.1 如果是string: 空字符串''返回 false ,其他返回 true.(注意 new String('') 属于对象)
		2.2 如果是number: 0返回 false,其他 true
		2.3 undefined 和 null 返回 false
		2.4 object : 返回true
### 3. 先判断是要转换为哪种基本类型,再做转换. 加性:+;条件>,<;非严格相等 ==
#### 3.1 加性 +,-
		3.1.1如果两个都是数值正常计算 infinity,-infinity 和 +0,-0 略过..
		3.1.2如果两个都是字符串拼接.
		3.1.3如果只有一个是字符串,将另一个转换为字符串.
		3.1.4如果有一个是对象,尝试转换为字符串
		3.1.5如果两个都不是字符串,且其中一个是数值,将另一个转换为数值.
		3.1.6其他情况都是 NaN
#### 3.2 条件 >,<
		3.2.1两个都是数值,数值比较
		3.2.2两个都是字符串,字符编码比较 (注意 A<a)
		3.2.3其中一个是 NaN ,false
		3.2.4其中一个是数值,转换另一个为数值
		3.2.5对象先尝试转换为数值,不行字符串再比较
		3.2.6布尔转换为数值.
#### 3.3 不严格相等 == 不同类型是
		3.3.1布尔值先转换为数值
		3.3.2一个是字符串,一个是数值.字符串转换为数值
		3.3.3对象调用 valueof(),再比较
		3.3.4 == 有 NaN,就为 fasle ;!= 有 NaN ,就为 true
		3.3.5 undefined == null ,undefined 和 null 不会转换为其他类型进行比较.
		
### 4.if 语句 使用 Boolean()



部分内容来源于 :
> Javascript 高级程序设计, MDN和网络
							 