
# JavaScript-Tips

偶然看到[JavaScript Tips](https://github.com/loverajoel/jstips)。这个项目是**每天分享一个JavaScript Tip**，受到这个项目的启发，我也打算每天更新一个小Tip，我会参考原文，如果觉得写的很不错，我也会仅仅充当翻译工，但是每一个Tip都会加入我自己的思考，思考如果是我来写这个Tip的话，我会怎么写。

经验不足，能力有限，还望轻拍。

2016-01-07 至 2016-01-16：

* <a href="#0116">2016-01-16: 注意不要破坏原型链</a>
* <a href="#0115">2016-01-15： 实现JS中的继承</a>
* <a href="#0114">2016-01-14: 测试JS代码块性能</a>
* <a href="#0113">2016-01-13：检测数据类型</a>
* <a href="#0112">2016-01-12: 变量和函数的提前声明</a>
* <a href="#0111">2016-01-11：判断对象中是否存在某个属性</a>
* <a href="#0110">2016-01-10：对象数组根据某个属性排序</a>
* <a href="#0109">2016-01-09: 为数组或者单个元素应用相同的函数</a>
* <a href="#0108">2016-01-08：null和undefined的区别</a>
* <a href="#0107">2016-01-07：往数组中插入一个元素</a>


####<a id="0122">2016-01-22: 清空一个数组</a>

清空数组通常的做法是直接使用`arr = []`的方式。不过这里想介绍另一种方式：使用`属性length`。

	var a = [1,2,3];
	a.length = 0;
	console.log(a);		// []
	
其实我在**2016-01-07：往数组中插入一个元素**就提到过，JS中直接操作数组的`length`属性，会影响到数组具体的内容。这里我们再来看看这两种清空数组的方式 有什么不同。

	var a = [1,2,3];
	var b  = a;
	a = [];
	console.log(a);		// []
	console.log(b);		// [1,2,3]

	var c = [1,2,3,4];
	var d = c;
	c.length = 0;
	console.log(c);		// []
	console.log(d);		// []
	
使用**将属性length设置为零**的方式会把从原数组复制的数组也清空，而使用`arr = []`的方式则不会。



####<a id="0121">2016-01-21：链式调用对象实例的方法</a>

实现**链式调用**通常是在`jQuery`或者其他一些框架里，那我们如何在原生的`JavaScript`里实现链式调用呢？

	function Person(name){
  		this.name = name;
  		this.setName = function(name){
    		this.name = name;
    		return this;
  		}
  		this.sayName = function(){
    		console.log(this.name);
    		return this;
  		}
	}
	var person = new Person("sunyuhui");
	person.sayName().setName("sunyuhui2").sayName();// "sunyuhui" "sunyuhui2"
	
关键词：**链式调用**、**this**

####<a id="0120">2016-01-20: 使用concat拼接字符串</a>

拼接字符串通常的做法是使用加号`+`，如`c = '1' + '2';`
	
使用加号`+`的前提是要确保参数都是字符串，万一有参数不是字符串，就得不到期望的结果：

	var a = 1;
	var b = 2;
	var c = '3';
	var d = a+b+c;
	console.log(d);		//"33"
	
这种时候可以使用`concat函数`.
	
	var a = 1;
	var b = 2;
	var c = '3';
	var d = ''.concat(a,b,c);
	console.log(d);			//"123"
	
注意：`Array`也有`concat`函数，要注意区分。

关键词： **concat**
	

####<a id="0119">2016-01-19: 函数嵌套时的作用域</a>

	var a = 1;
	function get(){
	  console.log(a);
	}
	get();    //1

	function get2(){
	  var a = 2;
	  console.log(a);
	}
	get2();   //2
	
上面代码的输出结果大家肯定明白，但是下面的代码呢？

	var a = 1;
	function get(){
	  console.log(a);
	}

	function get3(){
	  var a = 2;
	  get();
	}
	get3();	//1
	
输入结果为**1**的原因是由于函数嵌套时，作用域是在定义时决定的，而不是调用时决定的。`函数get`是在全局作用域下定义的，而运行是在`函数get3`的作用域下定义的，获取`变量a`的值是在全局作用域下获取的，而不是在`函数get`的作用域里获取的。

关键词： **嵌套**、**作用域**



####<a id="0118">2016-01-18: new这个关键字到底做了什么</a>

	function Person(name,age){
		this.name = name;
		this.age = age;
	}
	var person1 = new Person("sunyuhui", "22");

我们以上面的代码为例说明`new`这个关键字做了哪些事。

1. 创建一个新对象**person1**
2. 将构造函数`Person`的作用域赋给**person1**,这样**对象person1**就可以访问构造函数`Person`里的代码。this就指向**对象person1**。
3. 执行构造函数里的代码，**对象person**的`name`和`age`分别被赋值为**sunyuhui**、**22**。
4. 返回新对象，这样我们就得到了**对象person**。

关键词： **new**


####<a id="0117">2016-01-17: 递归函数的优化</a>

典型的递归函数是求一个数的阶乘：

	function getTotal(num){
		if (num<2){
			return 1;
		} else {
			return num * getTotal(num-1);
		}
	}
	console.log(getTotal(3)); 		// 6
	
这里说到的优化主要指的是函数在调用自身的时候可以不用指定函数名，防止函数被重新赋值导致调用出错。

	function getTotal(num){
		if (num<2){
			return 1;
		} else {
			return num * arguments.callee(num-1);
		}
	}
	console.log(getTotal(3)); 		// 6

其中`arguments`指向函数的参数，`arguments.callee`指向正在执行的函数，

关键词： **递归**、**arguments.callee**
	

####<a id="0116">2016-01-16: 注意不要破坏原型链</a>

在【2016-01-15：实现JS中的继承】中我提到`Man.prototype=`的形式会破坏构造函数的原型链。那在这种情况下我们有什么办法不破坏吗？

当然有

	function Person(name){
		this.name = name;
	}
	Person.prototype = {
		//constructor: Person,
		getName: function(){
			return this.name;
		}
	}
	var man = new Person("sunyuhui");
	console.log(man.constructor === Person);      // false
	
每一个构造函数的原型都有一个`construtor`属性，指向构造函数本身，上面这种设置原型的方式相当于重写了原型，却没有为原型的`constructor`属性赋值，就破坏了`Person`的原型链。将注释的部分取消注释就ok了。

关键词：**constructor**、**原型链**

####<a id="0115">2016-01-15： 实现JS中的继承</a>

	function Person (name,age,pro){
		this.name = name;
		this.age = age;
		this.pro = pro;
	}
	Person.prototype.getPro = function(){
		return this.pro;
	}
	function Man (name,age,pro){
		this.name = name;
		this.age = age;
	}
	Man.prototype = new Person("sunyuhui","22","fe");
	var man1 = new Man("sunyuhui2","23");
	console.log(man1.name);   			// sunyuhui2
	console.log(man1.age);				// 23
	console.log(man1.getPro());			// fe
	
将构造函数`Person`的实例赋予构造函数`Man`的原型,这样就实现了`Man`对`Person`的继承，从中可以看到`man1`的属性**name**和属性**age**覆盖了继承的属性，而属性**fe**被来自于继承。

注意：`Man.prototype = ***`的这种形式会修改原型链，使`Man`的实例对象的`constructor`属性不为`Man`.这个点以后单独说。

关键词： **继承**

####<a id="0114">2016-01-14: 测试JS代码块性能</a>

说到`console`我们通常使用`console.log()`来输出变量或者打印日志，这里介绍一个不太常用的`console.time()`和`console.timeEnd()`。

	console.time("time test");
	var arr = new Array(1000);
	for(var i=0;i<arr.length;i++){
		arr[i] = {};
	}
	console.timeEnd("time test");  // time test: 4.037ms
	
需要注意`console.time()`和`console.timeEnd()`的参数需要保持相同。

####<a id="0113">2016-01-13：检测数据类型</a>

我们通常会使用`typeof`、`instanceof`、`isArray`来检测数据的类型，这里我介绍一种万能型的：调用`Object`的`toString`方法。

	function a(){console.log("yes");}
	console.log(Object.prototype.toString.call(a)); // [object Function]
	var b = 123;
	console.log(Object.prototype.toString.call(b)); // [object Number]
	var c = "sunhui";
	console.log(Object.prototype.toString.call(c)); // [object String]
	var d = true;
	console.log(Object.prototype.toString.call(d)); // [object Boolean]
	var e = [1,2,3];
	console.log(Object.prototype.toString.call(e)); // [object Array]
	var f;
	console.log(Object.prototype.toString.call(f)); // [object Undefined]
	var g = null;
	console.log(Object.prototype.toString.call(g)); // [object Null]
	var h = {"name":"sunyuhui"};
	console.log(Object.prototype.toString.call(h)); // [object Object]

使用`call`调用`Object`的`toString`方法，将会返回一个遵循[object NativeConstructorName]格式的字符串。其中NativeConstructorName指的就是变量的构造函数名。

####<a id="0112">2016-01-12: 变量和函数的提前声明</a>

在JavaScript里 **变量声明** 是让系统知道这个变量存在，而**变量定义**是给这个变量赋值。**变量声明**会被提前到顶部，而 **变量定义** 不会。

	console.log(a);   // "ReferenceError: a is not defined"
	
上面的代码报变量没有定义的错误。

	console.log(a);   // undefined
	var a;

上面的代码提示`undefined`，这说明我们对`a`的声明被提前了。再来看：

	console.log(a);	// undefined
	var a = 3;

我们在这里对`a`同时做了**声明**和**定义**两个操作，从结果来看，只有**声明**被提前了，而**定义**却没有。

接下来看函数：

	getName();              	// "sunyuhui"
	function getName(){
		console.log("sunyuhui");
	}

结果说明我们对函数的声明被提前了。

	getAge();						// "TypeError: getAge is not a function"
	var getAge = function(){
		console.log(22);
	}

事实上，第一种方式叫**函数声明**，这种方式能被提前到顶部，第二种方式叫**函数表达式**，不会被提前。

关键词：**声明**、 **提前**
	

####<a id="0111">2016-01-11：判断对象中是否存在某个属性</a>

我们经常需要用到对象中的某个属性，为了保证程序的健壮性，在使用之前我们需要判断这个属性是否存在，可以用`if`来判断

	if(obj[property] !== undefined){
		// do something
	}
	
更方便的方式是使用这两个方法：`hasOwnProperty`和`in`。

	function Person (name) {
		this.name  = name;
	}
	Person.prototype.age = '22';

	var person = new Person("sunhui");
	console.log(person.hasOwnProperty('name'));    // true
	console.log("name" in person);	              // true
	
	console.log(person.hasOwnProperty('age'));    // false
	console.log("age" in person);	              // true
	

`hasOwnProperty`和`in`都可以用来判断某个属性是否存在于对象中，区别就是`hasOwnProperty`不能搜索到从原型链继承的属性，而`in`可以。

关键词：**hasOwnProperty**、**in**、**原型链**
	
	
####<a id="0110">2016-01-10：对象数组根据某个属性排序</a>

	function compare(prop){
		return function(obj1,obj2){
			var prop1 = obj[prop];
			var prop2 = obj[prop];
			if(prop1<prop2){
				return -1;
			} else if(prop1>prop2){
				return 1;
			} else {
				return 0;
			}
			
		}
	}
	var arr = [
		{"name":"bsunhui","age":"23"},
		{"name":"csunhui","age":"22"}
	];
	arr.sort(compare("age"));
	console.log(arr);					//[{"name":"csunhui","age":"22"},{"name":"bsunhui","age":"23"}]
	arr.sort(compare("name"));
	console.log(arr); 					//[{"name":"bsunhui","age":"23"},{"name":"csunhui","age":"22"}]
	
`compare`函数返回了一个**闭包**，这样就可以根据传入的属性进行排序。

关键词：**闭包**、**对象数组按属性排序**

####<a id="0109">2016-01-09: 为数组或者单个元素应用相同的函数</a>

举例：

	var a = "sun";
	var b = ["sun","hui"];

现在需要将`a`转换成大写，将`b`中的每个元素都转换成大写,通常的做法是：

	console.log(a.toUpperCase());     			// "SUN"
	for(var i=0;i<b.length;i++){
		console.log(b[i].toUpperCase());      // "SUN" "HUI"
	}

这种做法相当于写了两套代码，看看下面的方式。

	function toUpper(words){
		var elements = [].concat(words);
		for(var i=0;i<elements.length;i++){
			console.log(elements[i].toUpperCase());
		}
	}
	
	toUpper("sun");								// "SUN"
	toUpper(["sun","hui"]);						// "SUN" "HUI"
	
关键点是：`concat`的用法，`str.concat(arg)`的参数`arg`如果是一个数组会把数组里的（不是嵌套数组的）每一项都拿出来作为新数组的元素。

关键词：**concat**
	





####<a id="0108">2016-01-08：null和undefined的区别</a>

* `undefined`表示变量没有被声明或者声明了但是没有被赋值。
* `null`表示这个变量不是一个值。
* JavaScript里没有被赋值的变量默认为`undefined`。
* JavaScript里不会把一个值设置为`null`，只有程序员自己才能把一个值设置为`null`。
* 在JSON里`undefined`不可用，而`null`是可用的。
* `type of undefined`的值为`undefined`。
* `type of null`的值为`object`。
* `null`和`undefined`的布尔值都为false。
* `null == undefined`的值为**true**。
* `null === undefined`的值为**false**。

关键词：**null**、**undefined**


####<a id="0107">2016-01-07：往数组中插入一个元素</a>

一般而言，对数组元素的操作我们会用到这些函数：`push`，`pop`，`unshift`，`shift`，`splice`。

<pre>
	var a = [1, 2, 3];
	a.push(4);
	console.log(a);     //[1,2,3,4]
	a.pop();					
	console.log(a);		//[1,2,3]
	a.unshift(5);
	console.log(a);		//[5,1,2,3]
	a.shift();
	console.log(a);		//[1,2,3]
	a.splice(0,2,6);
	console.log(a)		//[6,3]
</pre>

我在这里想介绍的是利用`length`属性来进行数组元素的增减。

<pre>
	var a = [1, 2, 3];
	a.length = 1;
	console.log(a);		//[1]
	a.length = 5;
	console.log(a);		//[1,undefined,undefined, undefined, undefined]
	a = [1,2,3];
	a.splice(a.length/2,0,4,5,6);		//在数组的中间插入元素
	console.log(a);		//[1,4,5,6,2,3]
</pre>

值得注意的是以上所有方法都改变了原数组。我们再来一个：

<pre>
	var a = [1,2,3];
	var b = [4,5].concat(a);
	console.log(a);		//[1,2,3]
	console.log(b);		//[4,5,1,2,3]
</pre>

使用`concat`来增加数组元素就会返回一个新数组，不会改变原数组。

关键词：**数组**、**length**


