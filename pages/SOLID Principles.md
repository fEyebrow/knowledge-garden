- S: Single Responsibilty Principle
	- class or function should only have one resaon to change.
	- 根据需求创建各自的类，当有新需求时，只需要修改相关的类。类只会因为一种原因被修改。
- O： Open-Closed Principle
	- 当你要添加新功能时，不需要修改旧代码
	- Higher level-components are protected from changes to lower level components
		- 抽象出公共的接口，接口不变，变的只是实现
- L: Liskov-Substition Principle
	- 要构建从可互换零件的软件系统，这些零件必须遵守允许这些零件彼此代替的合同。
	- 函数的参数接受接口，而不是类
- I:Interface Segregation Principle
	- Prevent classes from relying on things that they don't need
- D:Dependency Inversion Principle
	- Abstractions should not depend on details. Details should depend on abstractions.
	- interface不能依赖细节，如接受抽象类，而不是具体的类
	-