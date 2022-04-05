- ```Typescript
  // infer只能用在extends. 可以通过infer来定义一个变量，用于引用和返回
  type Parameters<T extends (...args: any) => any> = T extends (...args: infer P) => any ? P : never;
  ```
- [[<<Effective TypeScript>>]]