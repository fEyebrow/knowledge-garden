title:: <<Effective TypeScript>>

- think of types as Sets of values
	- &指的是类型集合的交集，而非属性的交集
	- 将extends看成是子集
- Know how to tell whether a Symbol is in the type space or value space
	- ```
	  // 冒号前的部分是值，后半部分才是类型
	  //
	  function email( {person, subject, body}: {person: Person, subject: string, body: string} ) {
	  // ... }
	  // 这样写，只是创建了一个名为Person的变量，和两个名为string的
	  function email({ 
	  person: Person,
	  subject: string, 
	  body: string
	  }）{}
	  ```