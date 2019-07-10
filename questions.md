## 一、页面布局
#### 1.假设高度已知，请写出三栏布局，其中左栏、右栏宽度各为300px，中间自适应
- float解决方案 

	脱离文档流，清除浮动，处理好周边关系后，兼容性较好

	**清除浮动：** 

	1. 父容器设置高度（扩展性不好）
	2. clear:both;
	3. 伪元素清除浮动 推荐使用
	4. 父容器设置overflow:hidden,触发BFC

	**BFC：** 

	1. 不会重叠浮动元素
	2. 可以包含浮动
	3. 可以阻止垂直外边距
	5. br标签清除浮动

- absolute解决方案

	方便快捷，子元素全部都脱离文档流，可使用性较差

- flexbox解决方案

	解决以上两个问题
- table解决方案 

	兼容性好，栅格布局，一格高度改变，其他跟着改变

- grid解决方案 

	代码简洁

	**深层问题：** 
	1. 每个解决方案的优缺点
	2. 高度不定
	3. 兼容性

#### 2.CSS盒模型 
- 基本概念： 标准模型+IE模型

- 标准模型和IE模型的区别 

	标准：
	height:content-height
	width:content-width

	IE:
	height:border+padding+content-height
	width:border+padding+content-height

- CSS如何设置这两种模型 

	标准（默认）：box-sizing:content-box;
	IE：box-sizing:border-box;

- JS如何设置获取盒模型对应宽和高  
	dom.style.width/height(只能取内联样式的宽和高)
	dom.currentStyle.width/height(仅IE支持)
	windom.getComputedStyle(dom).width/height(兼容)
	dom.getBoundingClientRect().width/height(以左上角为点，获取left，right，width，height)

- 实例题 （根据盒模型解释边距重叠）


- BFC（边距重叠解决方案）

基本概念：格式化上下文，一个独立的渲染容器

原理：

 **如何创建:**

1. 浮动元素，float除none以外的值
2. 定位元素，position（absolute，fixed）
3. display 值为：inline-block,table-cell,table-caption
4. overflow除了visible以外的值（hidden，auto，scroll）

特性：

1. 内部的Box会在垂直方向上一个接一个的放置。
2. 垂直方向上的距离由margin决定
3. bfc的区域不会与float的元素区域重叠。
4. 计算bfc的高度时，浮动元素也参与计算
5. bfc就是页面上的一个独立容器，容器里面的子元素不会影响外面元素。


## 二、DOM事件类 
#### 1.基本概念：  
#### 2.DOM事件的级别 

- DOM0	element.onclick=function(){} 
- DOM2	element.addEventListener('click',function(){},false)
- DOM3	element.addEventListener('keyup',function(){},false)

	注：true为捕获，false为冒泡

#### 3.DOM事件模型 

- 捕获		由上至下
- 冒泡		由下至上

#### 4.DOM事件流 
   捕获阶段	-> 目标阶段 -> 冒泡阶段

   阻止冒泡：

```
//阻止冒泡封装
function stopBub(event) {
    event = event || window.event;
    if(event && event.stopPropagation){
    	event.stopPropagation();
    	}else{
    		event.cancelBubble = true;
    }
```

#### 5.DOM事件捕获的具体流程 
    window -> document -> html -> body -> 父 -> 子
    例：
	    window.add...
	    document.add...   
	    [获取HTML"监听"]：
	    document.documentElement.addEventListener('click',function(){},true);
	    document.body.add...
	    父.add...



#### 6.Event对象的常见应用 

- event.preventDefault() [阻止默认事件]
- event.stopPropagation() [阻止冒泡]
- event.stopImmediatePropagation() [事件优先级]
- event.currentTarget() [事件流指到当前的事件元素]
- event.target() [事件发生的元素]

#### 7.自定义事件
```
var eve = new Event('custom');	//new CustomEvent('custom',{参数})
ev.addEventListener('custom',function(){
	console.log('custom');
	});
ev.dispatchEvent(eve);
```

## 三、HTTP协议类
#### 1.HTTP协议的主要特点 
	简单		灵活		无连接		无状态
#### 2.HTTP报文的组成部分
	请求报文： 
		请求行
		请求头
		空行
		请求体

	响应报文： 
		状态行
		响应头
		空行
		响应体
#### 3.HTTP方法 
	get 获取资源
	post 传输资源
	put 更新资源
	delete 删除资源
	head 获取报文首部
#### 4.POST和GET的区别 
	1. get在浏览器回退时是无害的，而post会再次提交请求
	2. get产生的URL地址可以被收藏，而post不可以
	3. get请求会被浏览器主动缓存，而post不会，除非手动设置
	4. get请求只能进行URL编码，而post支持多种编码方式
	5. get请求参数会被完整保留在浏览器历史记录里，而post中的参数不会被保留
	6. get请求在URL中传送的参数是有长度限制的，而post没有限制 
	7. 对参数的数据类型，get只接受ASCII字符，而post没有限制 
	8. get比post更不安全，因为参数直接暴露在URL上，所以不能用来传递敏感信息

#### 5.HTTP状态码 
	1xx: 指示信息 - 表示已接收请求，继续处理

	2xx:成功 - 表示请求已被成功接收
		200： OK 客户端请求成功
		206： 客户发送了一个带有Range头的GET请求，服务器完成了它

	3xx: 重定向 - 
		301： 永久重定向
		302： 临时重定向
		304：客户端有缓存

	4xx: 客户端错误 
		400： 客户端有语法错误，不能被服务器所理解
		401： 请求未经授权，这个状态码必须和WWW-Authenticate报头域一起使用 
		403： 页面被禁止访问 
		404： 请求的资源不存在 

	5xx: 服务器端错误 
		500： 服务器发生不可预期的错误，原来缓存的文档还可以继续使用 
		503： 请求未完成，服务器临时过载或当机，一段时间后可能恢复正常

#### 6.什么是持久连接 
	即，当协议为http1.1版本时，设置为connection：keep-alive模式

#### 7.什么是管线化 
- 正常情况：请求1 -> 响应1 -> 请求2 -> 响应2 
- 管线化：请求1 -> 请求2 -> 请求3 -> 响应1 -> 响应2 -> 响应3
- 特性：

	1. 基于HTTP/1.1版本，可持久连接
	2. 只有get和head请求可以进行管线化，而post则有所限制
	3. 初次创建连接是不应启动管线机制，因为对方（服务器）不一定支持http/1.1版本的协议 
	4. 管线化不会影响响应到来的顺序
	5. http/1.1要求服务器端支持管线化，但并不要求服务器端也对响应进行管线化处理，只是要求对于管线化的请求不失败即可 
	6. 开启管线化很可能并不会带来大幅度的性能提升，而且很多服务器端和代理程序对管线化的支持不好，因此现代浏览器如Chrome和Firefox默认并未开启管线化 


## 原型链类
#### 创建对象的几种方法
	1. 字面量
		var o1={name:'o1'};
		var o11=new Object({name:'o11'})
	2. 构造函数 
		var M=function(){this.name='o2'};
		var o2=new M();
	3. 原型 
		var P={name:'o3'};
		var o3=Object.create(P);

#### 原型、构造函数、实例、原型链
1. 每一个函数（类）都有一个prototype（原型）属性，属性值是一个对象：这个对象中存储了当前类供实例调取使用的公有属性和方法
2. 在“浏览器默认”给原型开辟的堆内存中有一个属性constructor：存储的是当前类本身
3. 每一个对象（实例）都有一个__proto__(原型链)属性，这个属性指向当前实例所属类的原型（不确定所属的类，都指向Object.prototype）

![原型链图](https://images2015.cnblogs.com/blog/1030655/201610/1030655-20161031041705971-1605480841.jpg)

#### instanceof原理
	通过实例的__proto__与类的prototype对比是否相等，判断该实例是否属于当前类，相等返回true，否则__proto__向上查找   

#### new运算符
1. 一个新对象被建立，它继承自foo.prototype 
2. 构造函数foo被执行，执行的时候，响应的传参会被传入，同时上下文（this）会被指定为这个新实例，new foo等同于new foo（），只能用在不传递参数的情况 
3. 如果构造函数返回了一个对象，那么这个对象会取代整个new出来的结果，如果构造函数没有返回对象，那么new出来的结果为步骤1创建的对象 
```
function myNew(fn){
	let res = Object.create(null);

	if(fn.prototype) res.__proto__ = fn.prototype;

	let ret = fn.apply(res,Array.prototype.slice.call(arguments,1));

	if((typeof ret === "Object" || typeof ret === "function") && ret !== null) return ret
	return res
}
```

#### 继承 

## 通信类 
####  同源策略及限制 
	即协议、域名、端口均相同则为同源，是一个用于隔离潜在恶意文件的关键的安全机制
	特点：

1. Cookie、LocalStorage和IndexDB无法读取
2. DOM无法获得 
3. AJAX请求不能发送

####  如何创建ajax

1. XMLHttpRequest对象的工作流程
2. 兼容性处理 
3. 事件的触发条件 
4. 事件的触发顺序

####  跨域通信的几种方式 

		1. jsonp 
		2. hash 
		3. postMessage 
		4. WebSocket 
		5. CORS 

## 安全类 
#### CSRF 
- 基本概念和缩写

	cross-site request forgery--跨站请求伪造

- 攻击原理

	用户登录过网站A，网站A下发cookie

	用户访问网站B，网站B引诱用户点击访问网站A

- 防御措施 
	
	1. token验证
	2. referer验证 
	3. 隐藏令牌

#### XSS 
- 基本概念和缩写 
	
	cross-site scripting 跨站脚本攻击

- [攻击原理](http://www.imooc.com/learn/812)
- [防御措施](http://www.imooc.com/learn/812)
 	
## 算法类 
- 排序

	1.[快速排序](https://segmentfault.com/a/1190000009426421)

	2.[选择排序](https://segmentfault.com/a/1190000009366805)

	3.[希尔排序](https://segmentfault.com/a/1190000009461832)

- 堆栈、队列、链表 

	1.[堆栈](https://juejin.im/entry/58759e79128fe1006b48cdfd)

	2.[队列](https://juejin.im/entry/58759e79128fe1006b48cdfd) 

	3.[堆栈](https://juejin.im/entry/58759e79128fe1006b48cdfd)

- 递归 

	[递归](https://segmentfault.com/a/1190000009857470)

- 波兰式、逆波兰式 
	
	1.[理论](http://www.cnblogs.com/chenying99/p/3675876.html)

	2.[源码](https://github.com/tairraos/rpn.js/blob/master/rpn.js)


## 渲染机制类 
#### 什么是DOCTYPE及作用 
	DTD：文档类型定义。浏览器使用它类判断文档类型，决定使用何种协议来解析，以及切换浏览器模式

	DOCTYPE是用来声明文档类型和DTD规范的，一个主要的用途便是文件的合法性验证。如果文件代码不合法，那么浏览器解析时便会出错

		HTML 5 

			!DOCTYPE html

		HTML 4.01 Strict 

			该DTD包含所有HTML元素和苏醒，但不包括展示性的和弃用的元素（比如font）

		HTML 4.01 Transitional
			该DTD包含所有HTML元素和属性，包括展示性的和弃用的元素

#### 浏览器的渲染过程
	
	HTML->	HTML parser-> ** DOM  tree ** |

										Attachment->**render tree**->Paiting->Display

	Style Sheet->CSS parser->**Style Rules** |

	根据RenderTree计算节点在页面上的大小和位置，然后把节点绘制在页面上 


#### 重排（回流）reflow 
	操作整体

	回流必定会发生重绘，重绘不一定引发回流

#### 重绘repaint 
	不影响布局，操作部分

#### 减少重绘和回流 
##### CSS
1. 使用 transform 替代 top 
2. 使用visibility 替代display：none （前者只会引起重绘，后者会引发回流）
3. 避免使用table布局，可能很小的一个小改动会造成整个table的重新布局
4. 尽可能在DOM树的最末端改变class，回流是不可避免的，但可以减少其影响。 
5. 避免设置多层内联样式，CSS选择符从右往左匹配查找，避免节点层级过多 
6. 将动画效果应用到 position 属性为absolute或fixed元素上，避免影响其他元素的布局，这样只是一个重绘，而不是回流。同时，控制动画速度可以选择 requestAnimationFrame  
7. 避免使用CSS表达式，可能会引发回流 
8. 将频繁重绘或回流的节点设置为图层，图层能够阻止该节点的渲染行为影响别的节点，例如 will-change、video、iframe等标签，浏览器会自动将该节点变为图层 
9. CSS3硬件加速（GPU加速），可以让transform、opacity、filters这些动画不会引起回流重绘。但是对于动画的其他属性，如background-color这些，还是会引起回流重绘 

#### JS 
1. 避免频繁操作样式，最好一次性重写style属性，或者将样式列表定义为class 并一次性更改class属性 
2. 避免频繁操作DOM，创建一个documentFlagment，在它上面应用所有DOM操作，最后再把它添加到文档中。 
3. 避免频繁读取会引发回流重绘的属性，如果需要多次使用个，就用一个变量缓存起来 
4. 对具有复杂动画的元素使用绝对定位，使他脱离文档流，否则会引起父元素及后续元素频繁回流


#### 布局layout 

## 运行类机制 
#### 任务队列和Event Loop事件循环 
![事件循环](https://pic2.zhimg.com/80/v2-da078fa3eadf3db4bf455904ae06f84b_hd.jpg)

- 宏任务 
	script（整体代码）、setInterval、setTimeout、setImmediate、I/O、UI rendering
- 微任务
	Promise、process.nextTick、Object.observe、MutationObserver

## 页面性能类
#### 提升页面性能的方法有哪些？
	1.资源压缩合并，减少HTTP请求 

	2.非核心代码异步加载 
		异步加载的方式 
			1.动态脚本加载 
			2.defer 
			3.async 

		异步加载的区别
			1.defer是在HTML解析完之后才会执行，如果是多个，按照加载的顺序依次执行 
			2.async是在文件加载完之后立即执行，如果是多个，执行顺序和加载顺序无关
![defer/async](http://segmentfault.com/img/bVcQV0)

	3.利用浏览器缓存 

		缓存的分类 
		缓存的原理

	4.使用CDN 

	5.预解析DNS 
		<meta http-equiv="x-dnsprefetch-control" content="on">
		<link rel="dns-prefetch" href="//host_name_to_prefetch.com" >

#### 性能优化之JS 
一、加载和运行 

	脚本位置 ： 将所有script标签放在页面的底部，紧靠</body>上方，以保证页面脚本运行之前完成解析 

	将脚本成组打包，页面script标签越少加载越快，响应也就更迅速。不论外部脚本文件或者内联代码都是如此。

	defer & async ： 异步加载使用 

	动态脚本 ： 动态添加script标签并插入进页面 

	监听加载函数 

	XHR 注入 : 前提条件是同域，此处与异步加载一样，只不过使用的是XMLHTTPRequest

二、数据访问 

	直接量与局部变量访问速度非常快，数据项和对象成员需要更长时间 

	局部变量比域外变量访问速度快，因为它位于作用于的第一个对象中。变量在作用域链的位置越深，访问所需时间越长。全局变量总是最慢的，因为它位于作用于链的最后一环 

	嵌套对象成员会造成重大性能影响，尽量少用 

	属性在原型链中的位置越深，访问速度越慢 

	将对象成员、数组项、域外变量存入局部变量能提高js代码的性能 

	注：
	直接量：仅代表自己，不存储于特定位置。包括：字符串、数字、布尔值、对象、数组、函数、正则表达式、具有特定意义的空置、未定义 
	局部变量：使用var/let关键字创建用于存储数据值 
	数组项：具有数字索引，存储一个js数组对象 
	对象成员：具有字符串索引，存储一个JS对象 

三、dom编程 

	最小化dom访问，在JS端做尽可能多的事

	在反复访问的地方使用局部变量存放dom引用 

	谨慎处理HTML集合，因为它们表现‘存在性’，总对底层文档重新查询。将length属性缓存到一个变量中，在迭代中使用这个变量。如果经常操作这个集合，可以将集合拷贝到数组中 

	使用速度更快的API，如：document.querySelectorAll()和firstElementChild() 

	注意重绘和重排，批量修改分隔，离线操作DOM，缓存或减少对布局信息的访问 

	使用时间托管技术中的最小化时间句柄数量

四、算法与流程控制 

	for,while,do while循环的性能特性相似 

	除非要迭代遍历一个属性位置的对象，否则不要使用for-in循环 

	改善循环的最佳方式减少每次迭代中的运算量，并减少循环迭代次数 

	一般来说，switch总比if-else更快，但总不是最好的解决方式 
	当判断条件加多，查表法忧郁if-else和switch 

	浏览器的调用栈大小限制了递归算法在JS中的应用，栈溢出导致其他代码不能正常执行 

	如果遇到栈溢出，将方法修改为制表法，可以避免重复工作 

五、字符串和正则表达式 

六、响应接口 

	JS运行时间不应该超过100毫秒。过长的运行时间导致UI更新出现可察觉的延迟，从而对整体用户体验产生负面影响 

	JS运行期间，浏览器响应用户交互的行为存在差异。不论如何，JS长时间运行将导致用户体验混乱和脱节 

	同一时间只有一个定时器存在，只有当这个定时器结束时才创建一个新的定时器 

	定时器可用于安排代码推迟执行，它使得你可以将长运行脚本分解成一系列较小的任务 

七、Ajax 

	请求数据
		5种常用技术用于向服务器请求数据 
			XMLHTTPRequest(XHR)
			Dynamic script tag insertion 动态脚本标签插入 
			iframes 
			Comet 
			Multipart XHR 多部分的XHR 

八、编程实战 

	通过避免使用eval()和Function()构造器避免二次评估。setTimeout()和setInterval()传递函数参数而不是字符串参数

	避免重复进行相同工作。当需要检测浏览器时，使用延迟加载或条件预加载 

	原生方法总比JS写的东西要快 

	当执行数学运算时，考虑使用位操作，它直接在数字底层进行操作 

九、创建并部署高性能JS应用程序 
	
	合并js文件，减少http请求的数量 

	以压缩形式提供js文件(gzip编码) 

	通过设置http响应报文头使js文件可缓存，通过向文件名附加时间戳解决缓存问题 

	使用CDN提供js文件，CDN不仅可以提高性能，它还可以为你管理压缩和缓存


#### 浏览器缓存 
##### 缓存的分类 
- 强缓存  

	Expires Expires:Thu,21 May 2019 23:39:02 GMT  
	Cache-Control Cache-Control:max-age=3600 (秒) 

- 协商缓存 
	
	Last-Modified  
	If-Modified-Since  
	Etag  
	If-None-Match 

#### 错误监控类 
##### 前端错误的分类

1. 即时运行错误 ：代码错误

		捕获方式：

			1.try...catch 
			2.window.onerror 


2. 资源加载错误 

		捕获捕获方式：

			1.object.onerror
			2.performance.getEntries() 
			3.Error事件捕获

3. 延伸：跨域的js运行错误可以捕获吗，错误提示什么，应该怎么处理？

		可以捕获，提示:Script error.

		处理：

			1.在script标签上增加crossorigin属性 
			2.设置js资源响应头 Access-Control-Allow-Origin:*  

##### 上报错误的基本原理

		1.采用Ajax通信的方式上报 
		2.利用Image对象上报 
			script中：(new Image()).src='http://baidu.com/tfah?r=fdg'; 



## 常见问题

#### 1.JS获取和设置盒模型对应的宽和高的四种方法 
	
- dom.style.width/height

 	这种方法只能获取到内联样式的宽和高，而样式一般不回以内联方式写，所以这种方式不适用现在大多数的开发；

- dom.currentStyle.width/height

	这种方式只适用于IE，直接PASS；

- window.getComputedStyle(dom).width/height

	getComputedStyle()是一个可以获取当前元素所有最终（划重点）使用的CSS属性值的方法；兼容性很好。

- dom.getBoundingClientRect().width/height

	getBoundingClientRect用于获取某个元素相对于视窗的位置集合。集合中有top, right, bottom, left,height,width,x,y共8个属性；兼容性很好。

　　rectObject = dom.getBoundingClientRect()

　　rectObject.top：元素上边到视窗上边的距离;

　　rectObject.right：元素右边到视窗左边的距离;

　　rectObject.bottom：元素下边到视窗上边的距离;

　　rectObject.left：元素左边到视窗左边的距离;

　　rectObject.width：元素宽度;

　　rectObject.height：元素高度;

　　rectObject.x：元素内容与视口的水平距离;

　　rectObject.y：元素内容与视口的垂直距离;

#### 2.停止冒泡和阻止默认行为
- 停止冒泡
```
function stopBubble(e){
	if(e && e.stopPropagation)
		e.stopPropagation(); 	//非IE浏览器
	else
		window.event.cancelBubble = true;
}
```
- 阻止默认行为 
```
fucntion stopDefault(e){
	if(e && e.preventDefault)
		e.preventDefault(); 	//非IE 
	else	
		window.event.returnValue = false;

	return false;
}
```
- 注：return false  	
JS 中只会阻止默认行为，JQuery中能阻止冒泡和阻止默认行为

#### 3.Promise 
new Promise(function1（resolve,reject）{})
.then(function2(resolve))
.catch(function3(reject))

Promise.all([function1,function2])

发展进程：
callback => promise => async/await 



#### 4.for in 和for of 
- for of ：

		适用于遍历数组的值

		遍历普通对象会报错，补救方法：

			1.Object.keys(obj)  遍历键名 

			2.Object.values(obj)  遍历键值 

			3.Object.entries(obj)  遍历对象（键名：键值）

			如：for(let i of Object.keys(obj)){}		遍历obj键名

- for in ：

		适用于遍历对象或json，遍历的是对象的键名 

		遍历数组时，遍历的是索引 

		使用for…in遍历时，原型链上的所有属性都将被访问

- forEach：

		遍历数组，但不能使用break、continue和return语句 


#### 5.hasOwnProperty()方法 
检测当前属性是否为对象的私有属性。可以检测一个属性是存在于实例中，还是存在于原型中。

这个方法只在给定属性存在于对象实例中时，才会返回true。

注：in -> 检测当前对象是否存在某个属性（不管当前属性是对象的私有属性还是公有属性，只要有结果就是true）


#### 6.Vue的跳转方法 
- router-link 

![router-link](https://github.com/ImStruggler/-/blob/master/Imgs/router-link1.png)

- this.$router.push() 

	1. 不带参数
	 
		this.$router.push('/home')

		this.$router.push({name:'home'})

		this.$router.push({path:'/home'})	 
	 
	2. query传参 
	 
		this.$router.push({name:'home',query: {id:'1'}})

		this.$router.push({path:'/home',query: {id:'1'}})
	 
		// html 取参  $route.query.id

		// script 取参  this.$route.query.id	 
		 
	3. params传参
	 
		this.$router.push({name:'home',params: {id:'1'}})  // 只能用 name
		 
		// 路由配置 path: "/home/:id" 或者 path: "/home:id" ,

		// 不配置path ,第一次可请求,刷新页面id会消失

		// 配置path,刷新页面id会保留
		 
		// html 取参  $route.params.id

		// script 取参  this.$route.params.id	 
	 
	4. query和params区别

		query类似 get, 跳转之后页面 url后面会拼接参数,类似?id=1, 非重要性的可以这样传, 密码之类还是用params刷新页面id还在
	 
		params类似 post, 跳转之后页面 url后面不会拼接参数 , 但是刷新页面id 会消失

- this.$router.replace() (用法同上,push)

- this.$router.go(n) ()
向前或者向后跳转n个页面，n可为正整数(后退)或负整数（前进）

- ps : 区别

		1.this.$router.push
		跳转到指定url路径，并想history栈中添加一个记录，点击后退会返回到上一个页面

		2.this.$router.replace
		跳转到指定url路径，但是history栈中不会有记录，点击返回会跳转到上上个页面 (就是直接替换了当前页面)

		3.this.$router.go(n)
		向前或者向后跳转n个页面，n可为正整数或负整数


#### 7.数组的常用方法

1. toString() - 将数组转换为以逗号分隔的字符串。
2. concat() - 将两个数组组合在一起，或者将更多项添加到数组中，然后返回一个新数组。
3. reverse() - 方法将数组中元素的位置颠倒,并返回该数组。该方法会改变原数组
4. join() - 将所有数组元素组合成一个字符串。
5. split() - 用于字符串。将一个字符串分成子串并将它们作为数组返回。
6. splice() - 通过添加，删除和插入元素修改一个数组。
7. slice() - 截取。复制数组的给定部分，并将复制的部分作为新数组返回。 它不会改变原始数组。

8. push() - 将项目添加到数组的末尾，改变原始数组。
9. pop() - 删除数组的最后一项并返回
10. shift() - 删除数组的第一项并返回
11. unshift() - 将一个项添加到数组的开头，改变原始数组

12. indexOf() - 查找数组中的项目并返回其索引，如果没找到则返回-1
13. lastIndexOf() - 从右到左查找项目并返回找到的最后一个索引

14. filter() - 如果数组的项目符合某个条件，则创建一个新数组
15. map() - 通过操纵数组中的值来创建一个新数组
16. reduce()  - 根据数组中的单个值进行计算
17. forEach() - 遍历数组，将函数作用于数组中的所有项

18. every() - 检查数组中的所有项是否都符合指定的条件，如果符合则返回 true，否则返回 
false
19. some() - 检查数组中的项（一个或多个）是否符合指定的条件，如果符合则返回 true，否则返回 false。
20. includes() - 检查数组是否包含某个项目。

21. sort() - 排序
22. Array.from() - 类数组使用数组方法时进行转换 
23. Array.isArray() - 判断是否为数组 
24. Array.of() - 创建一个具有可变数量参数的新数组实例，而不考虑参数的数量或类型
25. fill(value,firstIndex,lastIndex) - 用一个固定值填充一个数组中从起始索引到终止索引内的全部元素。不包括终止索引


#### 8.普通函数和构造函数（new）的执行过程 

1. 形成一个私有的作用域（栈内存）
	形参赋值和变量提升都是私有变量 
2. 【构造函数独有】在JS代码自上而下执行之前，首先在当前形成的私有栈中创建一个对象（创建一个堆内存：暂时不存储任何东西），并且让函数中的执行主体（this）指向这个新的堆内存（this === 创建的对象） 
3. 代码自上而下执行 
4. 【构造函数独有】代码执行完成，把之前创建的堆内存地址返回（浏览器默认返回）

** 实现new方法 ** 
```
function _new(fn,...arg){
	const obj = Object.create(fn.prototype);
	const ret = fn.apply(obj,arg);
	return ret instanceof Object ? ret:obj;
}

过程：
1.首先创建一个空对象，空对象的__proto__属性指向构造函数的原型对象 
2.把上面创建的空对象赋值构造函数内部的this，用构造函数内部的方法修改空对象 
3.如果构造函数返回一个非基本类型的值，则返回这个值，否则返回上面创建的对象
```

#### 9.JS中的6个假值 

1. undefined 
2. null 
3. NaN 
4. 0 
5. ' ' 
6. false 

#### 10.JS判断数据类型总结 

1. typeof 判断基础数据类型 
识别：number、Boolean、string、undefined、null、symbol、function其中数据类型

		注：	

			1.对于基本类型，除null以外，均返回正确结果 
			2.对于引用类型，除function之外，均返回object  
			3.对于null，返回object 
			4.对于function，返回function 

2. instanceof 判断一个【对象】是否是另一个对象的实例 
A instanceof B  => A.__proto__ === B.prototype ? true;false 

		注：

			1.不能准确判断实例具体是哪个对象的实例(XX instanceof Object => true )
			2.不能判断iframe是否为数组 （可用Array.isArray()进行判断）
			3.只能判断对象类型

3. constructor 判断实例的确切数据类型，但可更改 
f.constructor == F 

		注：

			1.创建实例（new）时，函数的原型中的constructor会传递给实例对象
			2.constructor可重写，所以不稳定 
			3.null和undefined是无效的对象，因此没有constructor，无法以此方法进行判断 

4. toString() 能判断所有数据类型
	- 对于Object对象，直接调用通toString()就能返回[object Object] 
	- 对于其他对象，则需要通过调用call/apply才能返回正确的类型 
	- Array.isArray()是由toString封装的，只能判断数组 
	- 这种方法对于所有基本的数据类型都能进行判断，即使是 null和defined,且和Array.isArray方法一样都检测出 iframes 
	- 常用于判断浏览器内置对象
	- 不能精准判断自定义对象，对于自定义对象只会返回[object Object]【instanceof可判断】

	Object.prototype.toString.call(undefined) => [object undefined] 


#### 11.封装Ajax 
```
function ajax(url,type,datas,async,callback){
	let xhr = null;
	
	let params = dealDatas(datas);

	//考虑兼容性
	if(window.XMLHttpRequest){
		xhr = new XMLHttpRequest();
	}else{
		xhr = new ActiveXObject('Microsoft.XMLHTTP');
	}

	//get 和 post请求方式不同
	if(type == 'get'){
		xhr.open(type,url + '?' + params,async);
		xhr.send(null);
	}else if(type == 'post'){
		xhr.open(type,url,async);
		xhr.setRequestHeader('Content-type','application/x-www-form-urlencoded');
		xhr.send(params);
	}
	
	xhr.onreadystatechange = function(){
		if(xhr.readyState === 4){
			if(xhr.status >= 200 && xhr.status < 300 || xhr.status == 304){
				let data = xhr.responseText;
				callback(data);
			}
		}
	}
}

//处理传入的参数
function dealDatas(data){
	var arr = [];
	for(var item in data){
		arr.push(item + '=' +data[item]);
	}
	return arr.join('&');
}
```

#### this指向问题 
- 对于直接调用foo()来说，不管foo函数被放在上面位置，this一定是window 
- 对于obj.foo()来说，谁调用了函数，this就指向谁（即obj）
- 在构造函数模式中，类中（函数体中）出现的this.xxx=xxx中的this是当前类的一个实例
- call、apply、bind：this是第一个参数 
- 箭头函数没有自己的this，看其外层是否有函数，如有，外层函数的this就是内部箭头函数的this，如没有，则this就是window
![this指向](F:/InterviewQuestions/this.jpg) 

#### 13.String()和toString()的区别 
- String():可以将null和undefined转换为'null'和'undefined'字符串
			不能转进制字符串 

- toString():不能将null和undefined转为字符串
			可以toString(2)，括号内写数字，表示几进制 

#### 14.vue中不使用箭头函数的地方 
- 定义一个生命周期 
- 定义method方法 
- 定义计算属性函数 
- data属性 
- 定义watcher函数 

#### 15.手写深拷贝 
```
function deepClone(obj){
	//let res;
	if(typeof obj == 'object'){
		let res = Array.isArray(obj) ? [] : {};
		for(let i in obj){
			res[i] = typeof obj[i] =='object' ? deepClone(obj[i]) : obj[i];
		}
	}else{
		let res = obj;
	}
	return res
}
``` 

#### 17.手写防抖和节流 
- 防抖 
```

```

- 节流 
```
``` 

#### 18.手写Promise 
```

```
