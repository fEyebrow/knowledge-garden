- ```Typescript
  // infer只能用在extends. 可以通过infer来定义一个变量，用于引用和返回
  type Parameters<T extends (...args: any) => any> = T extends (...args: infer P) => any ? P : never;
  ```
- [[<<Effective TypeScript>>]]
- 尽量不要使用涉及值的Ts语法（enum，namespace），保证Ts转化成js后，不会有额外的代码。
- 第三方的构建工具如esbuild同一时间只能操作单个文件，将isolatedModules设为true，当出现不支持的语法时，获得warn
	- isolatedModules为true时，表明compiler不知道import的是type还是value，导入type时，得指明。