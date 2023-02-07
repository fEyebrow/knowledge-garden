- 异步取消
	- 使用AbortController
		- ```
		  let controller = new AbortController()
		  const signal = controller.signal
		  fetch(url, { signal }).then(() => {})
		  
		  // cancel
		  controller.abort()
		  ```
-