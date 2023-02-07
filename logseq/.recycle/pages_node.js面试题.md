title:: node.js面试题

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
-