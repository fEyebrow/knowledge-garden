title:: ES6，node.js面试题

- ES6新的语法
	- const let
	- 模版字符串
	- 箭头函数
	- 函数参数默认值
	- spread操作符，将可迭代的对象分解成多个参数或数组里的多个元素
	- 对象和数组解构
	- for of, for in
- 变量提升
	- 仅提升声明，而不提升初始化。你可以在声明前使用变量
- new操作符干了什么事
	- 创建了新对象A，继承构造函数的原型
	- 以新对象A为this，调用构造函数，得到B
	- 如果b是对象就返回b，不然就返回a
- 0.2+0.1不等于0.3问题（浮点数精度）
	- 精度损失可能出现在进制转化和对阶运算过程中
	- float会丢失精度，转成整形
- node.js和js的区别
	- 语法一样，组成不一样
	- js：DOM，BOM
	- Node.js: file，os，net（网络系统），database
- 闭包
	- 函数和其所在词法作用域的引用绑在一起，就形成了闭包。闭包让开发者可以从内部函数访问外部函数。
- node.js中的event loop
	- 机制和browser一样，宏任务的种类更多：I/O file，Socket,
	- 微任务多了：process.nextTick()（不推荐使用）
	- 宏任务的callback Queue有多个
		- 1. timers
		  2. I/O： network, stream, tcp
		  3. idle
		  4. poll
		  5. check: setImmediate
		  6. close callback
		- 中间穿插微任务
- 异步取消
	- 使用AbortController
		- ```
		  let controller = new AbortController()
		  const signal = controller.signal
		  fetch(url, { signal }).then(() => {})
		  
		  // cancel
		  controller.abort()
		  ```
- 基本数据类型：number,undefined,string,object,boolean,symbol,biginit(2的53次方-1)
- 栈只存基本类型，来维护程序执行期间上下文状态,所有数据放到栈中，会影响上下文切换效率，进而影响整个程序的执行效率。
- 堆存对象类型。
- 垃圾回收：
	- 函数执行完，对应的栈就销毁了
	- 针对堆回收
		- 为什么限制内存大小，一次非增量的1.5gb的垃圾回收需要1s
		- 分新生区和老生区。
		- 新生区里又划分from和to两块空间，通过空间换时间的方法来提高效率，适合生命周期短的。
		- 新生区晋升到老生区的条件
			- 经历过回收，并存活下来
			- to空间的内存占用比超过25%
		- 老生区采用标记清除加标记整理的方法。当碎片空间不够分配大对象时，使用标记整理。
		- 使用增量标记，延迟清理，增量整理来减少停顿对执行的影响