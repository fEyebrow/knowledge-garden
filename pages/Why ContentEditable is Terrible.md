- [原文](https://medium.engineering/why-contenteditable-is-terrible-122d8a40e480)
- 一个好的所见即所得的编辑器，能直观地展示内容，并且可以用光表进行编辑。
	- The *mapping *between DOM content and Visible content should be *well-behaved.*
	- The *mapping *between DOM selection and Visible selection should be *well-behaved.*
	- All visible edits should map *onto *an algebraically closed and complete set of visible content.
	- well-behaved指的是：
		- ```
		  for all edit operations E, and DOM pages x and y
		  Render(x) = Render(y) 
		  implies Render(E(x)) = Render(E(y))
		  ```
- well-behaved content
	- 实际上，同样的展示往往map不同的dom content
- well-behaved selections
	- 光标展示的位置可以map到dom中不同的位置
- WYSIWYG Editor应该如何：
	- 依赖contentEditable来捕获操作，并对一些基本操作自定义
	- 将操作应用到model上。可以将操作合并来实现快速响应。