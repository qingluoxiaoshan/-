# 关于闭包:

## 1.什么是闭包？

​	是指有权访问另一个函数作用域中的变量的函数。

## 2.常见创建闭包的方式

​	就是在一个函数内创建另一个函数，通过另一个函数访问这个函数的局部变量

## 3.闭包的特性

​	函数嵌套函数

​	函数内部可以引用外部的参数和变量

​	参数和变量不会被垃圾回收机制回收

## 4.闭包的好处

​	希望一个变量长期驻扎在内存中

```
function abc(){
  var a = 1;
  return function(){
    alert(a++)
  }
}
var fun = abc();
fun(); //1
fun(); //2
fun  = null; a被回收了

```

​	避免全局变量的污染  (沙箱模式)

```
var abc = (function(){
  var a = 1;
  return function(){
    a++;
    alert(a);
  }
})()
abc(); //2 调用abc就相当于调用的里面的函数的返回值
abc(); //3
```

​	私有成员的存在（实现数据私有）

```
var aaa = (function(){
  var a = 1;
  function bbb(){
    a++;
    alert(a);
  }
  function ccc(){
    a++;
    alert(a);
  }
  return {
    b:bbb,
    c:ccc
  }
})()
aaa.b() //2
aaa.c() //3  
```

## 5.利用闭包的在循环中找到对应元素的索引

```
<html>
	<head></head>
	<body>
		<ul>
			<li>1</li>
            <li>2</li>
            <li>3</li>
		</ul>
		<script>
			var lis = document.getElementByTagElement("li");
			for(var i = 0;i < lis.length;i++){
              (function(a){
                lis[a].onclick = function(){
                  alert(a);
                }
              })(i)
			}
		</script>
	</body>
</html>
```

## 6.内存泄漏的问题

​	内存泄漏：用动态存储分配函数动态开辟的空间，在使用完毕后未释放，结果导致一直占据该内存单元，直至程序结束。

​	在IE的js对象和DOM对象使用不同的垃圾收集方法，因此闭包在IE中会导致内存泄漏的问题，也就是无法销毁驻留在内存中的元素

```
 Function closuer(){  
 	dv = document.getElementById("dv"); 
    var test = dv.innerTest("123")  
    dv.onclick = function(){    
    	alert(test) 
        }  
    	dv = null; // 解除引用来避免内存泄漏
 	 }
```

