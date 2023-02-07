- react设计理念
	- react15的协调过程是同步的，这就造成耗时的任务阻塞线程，导致高优先级的任务无法及时响应
	- 解决办法：
	- 将任务分割，实现一套异步可中断的任务调度系统
	- Fiber就是为了这个目的设计的数据结构
	  collapsed:: true
		- ```
		  let firstFiber
		  let nextFiber = firstFiber
		  let shouldYield = false
		  //firstFiber->firstChild->sibling
		  function performUnitOfWork(nextFiber){
		    //...
		    return nextFiber.next
		  }
		     
		  function workLoop(deadline){
		    while(nextFiber && !shouldYield){
		            nextFiber = performUnitOfWork(nextFiber)
		            shouldYield = deadline.timeReaming < 1
		          }
		    requestIdleCallback(workLoop)
		  }
		     
		  requestIdleCallback(workLoop)
		  ```
	- Scheduler: 因为requestIdleCallback有兼容和出发不稳定的问题，react自己实现了一套时间片运行机制
	- Lane： 根据优先级，调度任务
	- 代数效应（Algebraic Effects）
		- 分离副作用的一种方法
		- async-await有传导性，没法分离副作用
		- useEffect就是这种方法的实现
- 源码架构
	- react17采用MessageChannel来代替requestIdleCallback