# 栈内存与堆内存
参考地址：https://www.jianshu.com/p/26c81fde22fb
1、栈内存（先进后出）
1）主要用于存储基本数据类型的变量。
2）有序存储，容量小，系统分配效率高。
3）释放：
A、函数执行形成的栈内存，函数执行完，内存自动释放。
B、若变量被外部占用，函数执行完成，无法释放。
C、全局作用域在加载页面时形成，在关闭页面时销毁（window）。
2、堆内存
1）主要用于存储Object类型的数据。
2）存储过程，需先在堆内存新分配存储区域，再把指针存到栈内存，效率低些。
3）释放：
A、当值为null时，堆内存才会自动释放。

# 执行环境和作用域
1、一些知识点：
1）某个执行环境中的所有代码执行完毕后，该环境被销毁，保存在其中的所有变量的函数定义也随之销毁。
2）作用域的搜索是由内及外的，前端是自身的作用域，末端是全局作用域。
3）函数参数也被当作变量来对待，因此其访问规则与执行环境中的其他变量相同。

# 延长作用域链
1）try-catch语句的catch块。
2）with语句
function buildUrl(){

	var qs = "?debug=true";
  // 可用于json对象的方便读取
	
with(location){
 
		var url = href + qs; // url的作用域被未被限制在with内

	}


	return url;

}


# 没有块级作用域
for(var i=0; i<10; i++){}
console.log(i); // 10


# 闭包
参考地址：https://www.jianshu.com/p/26c81fde22fb
1、概念
有权访问另一个函数作用域中的变量的函数。

2、缺陷
1）引用的变量发生改变
function createFunctions(){

	var result = new Array();

	for(var i=0; i < 10; i++){

		result[i] = function(){
 
   		return i;

		}

	}

	return result;

}


for(var i=0; i < 10; i++){

	console.log(createFunctions()[i]()); // 10

}

这里都引用了同一个变量i，而在循环结束后i=10，因此，每一次打印的结果都是10。可以通过创建一个匿名函数，在函数入口接收i，改变变量的作用域，来解决此问题。
function createFunctions(){

	var result = new Array();

	for(var i=0; i < 10; i++){

			result[i] = function(num){ // 这里将i赋值为num

			return num;

		}(i);

	}

	return result;

}


for(var i=0; i < 10; i++){

	console.log(createFunctions()[i])

}


3、内存泄漏
对于不再使用的对象数据，应该赋值为null，以便垃圾收集器下一次运行时将其回收，释放内存。