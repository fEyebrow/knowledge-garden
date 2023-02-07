- Bugs and Errors
  collapsed:: true
	- 非use strict;
		- 没有事先声明的变量，使用时会自动绑定到全局作用域。
		- this在不以方法调用函数为window
		- 允许with
		- 允许函数参数名是一样的
	- types
	- testing
		- 减少对外部可变变量的依赖
	- debugging
		- console.log
		- debugger
	- exceptions
	- clean up after exceptions
		- finally
	- selective scathing
		- 判断捕获的error是不是想要的
		- 通过定义一种新的错误类型，并用instanceof来识别
- Regexp
  collapsed:: true
	- Testing for mathes
		- /abc/.test('abcde') // true
	- Sets of Characters
		- /[0-9]/.test('in 1992') // true
		- ```
		  \d \w \s
		  \D \W \S
		  [^01]
		  ```
	- Repeating parts of a pattern
		- ```
		  + * ?
		  /\d{1,2}-\d{1,2}-\d{4} \d{1,2}:\d{2}/
		  {5,}
		  ```
	- Grouping Subexpressions
		- ```
		  /boo+(hoo+)+/i;
		  ```
	- Matches and Groups
		- ```
		  let match = /\d+/.exec("one two 100");
		  console.log(match);
		  // → ["100"]
		  console.log(match.index);
		  // → 8
		  
		  let quotedText = /'([^']*)'/;
		  console.log(quotedText.exec("she said 'hello'"));
		  // → ["'hello'", "hello"]
		  
		  console.log(/bad(ly)?/.exec("bad"));
		  // → ["bad", undefined]
		  console.log(/(\d)+/.exec("123"));
		  // → ["123", "3"]
		  ```
		- \\n可用于正则表达式的匹配环节， 比如  `/apple(,)\sorange\1/`  匹配"apple, orange, cherry, peach."中的'apple, orange,' 。
		- $&  表示整个用于匹配的原字符串。
	- word and string Boundaries
		- ^ $ \b
	- choice patterns
		- ```
		  let animalCount = /\b\d+ (pig|cow|chicken)s?\b/;
		  console.log(animalCount.test("15 pigs"));
		  ```
	- matching的机制
		- 从左到右
		- 贪心匹配，当下一个模式找不到匹配项时，就回溯
	- String.replace
		- 按group修改，适合用String.replace
		- ```
		  console.log(
		    "Liskov, Barbara\nMcCarthy, John\nWadler, Philip"
		      .replace(/(\w+), (\w+)/g, "$2 $1"));
		  // → Barbara Liskov
		  //   John McCarthy
		  //   Philip Wadler
		  
		  let stock = "1 lemon, 2 cabbages, and 101 eggs";
		  function minusOne(match, amount, unit) {
		    amount = Number(amount) - 1;
		    if (amount == 1) { // only one left, remove the 's'
		      unit = unit.slice(0, unit.length - 1);
		    } else if (amount == 0) {
		      amount = "no";
		    }
		    return amount + " " + unit;
		  }
		  console.log(stock.replace(/(\d+) (\w+)/g, minusOne));
		  // → no lemon, 1 cabbage, and 100 eggs
		  ```
	- Greed
		- 加问号变得不贪婪：`+?` ,  `*?` ,  `??` ,  `{}?`
		- ```
		  function stripComments(code) {
		    return code.replace(/\/\/.*|\/\*[^]*?\*\//g, "");
		  }
		  ```
	- Dynamically creating Regexp Objects
		- 当正则表达式的部分是变量时，可以使用动态创建
		- ```
		  let name = "dea+hl[]rd";
		  let text = "This dea+hl[]rd guy is super annoying.";
		  let escaped = name.replace(/[\\[.+*?(){|^$]/g, "\\$&");
		  let regexp = new RegExp("\\b" + escaped + "\\b", "gi");
		  console.log(text.replace(regexp, "_$&_"));
		  // → This _dea+hl[]rd_ guy is super annoying.
		  ```
	- String.search(/\S/)
		- 能使用正则的String.indexOf
	- Last index
		- 通过设置lastIndex来指定开始匹配的位置
		- g 会改变lastIndex的值，在执行一次exec后
	- Looping over matches
		- 使用lastIndex和exec，来实现
	- Parsing an INI FIle
		- `^`  and  `$`使expression matches the whole line
	- International characters
		- js的正则需要带上u，来支持匹配两个code unit的表情符号，以及将一些非26字母，数字的特殊符号当成\W
		- 可以使用 \p 来匹配Unicode标准定义的字符
- module
	- CommonJS
		- 因为require是一个函数，所以需要运行后才知道他的依赖
		- export出的是一个值
	- ES module
		- export出的是binding
		- import发生在module script执行前
	-