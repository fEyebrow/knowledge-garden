- 事件系统
  * 合成事件
      * 抹平各个浏览器的差异
  * 事件函数原先统一在document中处理
      * 优点
          * 减少内存开销
          * 统一管理
      * 缺点
          * 无法阻止原生的事件冒泡
  * 事件函数注册在各自的dom上
  * 事件池被删除
- fiber
	- 目的
		- 能够把可中断的任务切片处理。
		- 能够调整优先级，重置并复用任务。
		- 能够在父元素与子元素之间交错处理，以支持 React 中的布局。
		- 能够在 render() 中返回多个元素。
		- 更好地支持错误边界,并发
	- 实现方法： fiber针对react，重新实现了堆栈，来控制调用
	- React Components, Elements, and Instances
	  collapsed:: true
		- element 是virtual dom
		- component is class.
		- 能通过component创建instance
	- Reconciliation
	  collapsed:: true
	  如何高效的更新dom树
	  两个element类型不同，就删掉旧的，创建一个新的
	  两个element类型相同，则更新属性
	  两个component相同，则更新props
	  list间的比较，依靠key
	- the structure of fiber node
	  collapsed:: true
		- type and key
		- child and sibling
		- return
			- 下一个要处理的节点，child的returen是parent
		- pending props and memoizedProps
			- incoming pending props 等于memoizedProps时，可重用当前fiber
		- pendingWorkPriority
			- 除了nowork是0外，数字越大，优先级越低。
		- alternate
			- 当前fiber最多有两个相关的fiber，一个是work-in-progress,一个是flush
			- alternate 通过cloneFiber创建。
		- output
			- react中的叶子节点
		- element
			- render的返回值
		- 通过createFiberFromTypeAndProps初始化fiber node
		- fiber使用链表
		- effectTag, record associated effect
		- firstEffect和nextEffect连接带有effect的fiber node
		- updateQueue, a queue of state updates , callbacks and DOM updates
	- linked list
		- 记录parent的引用，就能stop 遍历，并在之后恢复
		- [[link List in Fiber]]
	- General algorithm
		- main phases
			- render, async所以不支持side effect
				- lifecycle method
					- getDerivedStateFromProps
					- shouldComponentUpdate
					- render
				- work loop，performUnitOfWork和completeUnitOfWork负责遍历节点，主要动作发生在begin work 和completeWork
					- [[work loop in Fiber]]
				- time-slicing
					- requestIdleCallback
						- 实际每秒调用次数达不到60，所以通过requestAnimationFrame和postMessage来polyfill
					-
			- commit, sync
				- lifecycle method
					- getSnapshotBeforeUpdate
					- componentDidMount
					- componentDidUpdate
					- componentWillUnmount
				- completeRoot, update DOM and call lifecycle methods
					- commitRoot
						- commitBeforeMutationLifecycles,
						- commitAllHostEffects
						- root.current = finishedWork
						- commitAllLifeCycles