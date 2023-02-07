- 学习资源：https://juejin.cn/book/7050063811973218341/section/7066617580068274207
- 前端构建工具，或者说前端工程化解决什么问题
	- 模块化方案：提供模块加载方案，兼容不同模块规范
	- 语法转移：支持高级语法转译；加载静态资源
	- 产物质量：压缩，混淆，Tree shaking， 语法降级
	- 开发效率：HMR、构建提速
- esm为什么是前端模块化的未来？
	- 模块化解决什么问题？
		- 问题1：变量名冲突；
		- 问题2：全局定义，不知道变量属于那个模块，调试困难；
		- 问题3：管理模块间的依赖关系和加载顺序
	- iife，没能解决问题3
	- CommonJS的模块自动加载方案是同步的，只适合服务端环境。
	- AMD：模块化代码标准使用复杂，代码不易阅读
	- ES Module能在Node.js和浏览器内运行，兼容90%以上的浏览器，是ECMAScript官方提出的规范。
	- 各模块兼容浏览器的形式
		- CommonJS：模块同步，如Browserify会对代码进行解析，整理出代码中的所有模块依赖关系，然后把nodejs的模块编译成浏览器可用的模块，相关的模块代码都打包在一起，形成一个完整的JS文件，这个文件中不会存在 require 这类的模块化语法，变成可以在浏览器中运行的普通JS，运行时加载
		- AMD：模块异步，依赖前置，是requireJS在推广过程中对模块定义的规范化产出，加载完依赖后立即执行依赖模块，依赖加载成功后执行回调
		- CMD：模块异步，延迟执行，是seaJS在推广过程中对模块定义的规范化产出，就近依赖，先加载所有依赖模块，运行时才执行require内容，按顺序执行
		- ESM的对外接口只是一种静态定义，为编译时加载，遇到模块加载命令import，就会生成一个只读引用。等脚本真正执行时，再根据这个只读引用，到被加载的那个模块内取值。由于ESM编译时就能确定模块的依赖关系，因此能够只包含要运行的代码，可以显著减少文件体积，降低浏览器压力。
- 接入css工程方案
	- 解决的问题：
		- 无法使用高级语法；
			- css预处理器
		- 无法使用css module，造成样式污染；
			- css类名处理成哈希值
		- 浏览器兼容问题；
			- postcss根据浏览器为一些css属性加前缀（autoprefixer），如transition。还有px转rem。
		- 打包后代码体积问题，提取css和css拆分。
			- vite默认进行css拆分，并保证css加载后，再执行对应的js
	- postcss：pxtorem, preset-env, cssnano(压缩)
	- css原子化框架：Windi CSS
- Lint
	- eslint核心配置
		- parser: @typescript-eslint/parser@latest
		- rules
		- plugins: @typescript-eslint/eslint-plugin
	- 配合编辑器插件接入eslint
	- stylelint
	- Husky + lint-staged(避免每次都全量检查)
	- commitlint：@commitlint/config-conventional
- 静态资源
	- 问题
		- 资源加载问题，将静态资源解析并加载为一个ES模块
		- 生产下，静态资源的部署，体积，网络性能
	- svg
		- vue3: [vite-svg-loader](https://github.com/jpkleemans/vite-svg-loader)
		- react:[vite-plugin-svgr](https://github.com/pd4d10/vite-plugin-svgr)
	- 自定义部署域名：通过base替换回cdn地址
	- 图片压缩：vite-plugin-imagemin
	- 雪碧图
		- 批量导入svg：import.meta.glob,import.meta.globEager,
		- 使用vite-plugin-svg-icons合成雪碧图
		- 创建icon component，内部通过use来引用
- 预构建
	- 解决的问题
		- 兼容各个规范的模块；将海量请求合并,即一个package请求一次，它的依赖包会被合并
	- optimizeDeps.include
	- optimizeDeps.esbuildOptions.plugins
	- 第三方库出现问题
		- patch-package来记录修改，再通过scripts.postinstall: 'patch-package'，这样每次instal时，都会进行修改
		- 写esbuild插件来解决
- esbuild和rollup双引擎
	- esbuild缺点
		- 不支持降级到es5
		- 不支持const enum
		- 不提供操作打包产物的接口
		- 不支持自定义code splitting
	- esbuild作用
		- 预构建
		- transformer ts/jsx 生产+开发， 通过vite plugin加入
		- minifier：生产中压缩文件，通过rollup plugin 加入
	- rollup
		- 基于rollup的优化
		- css代码分割，会抽出css，作为单个文件
		- 自动预加载入口chunk依赖的文件
		- 异步chunk加载优化
- esbuild插件开发指南
	- ```
	  const fs = require("fs/promises");
	  const path = require("path");
	  const { createScript, createLink, generateHTML } = require('./util');
	  
	  module.exports = () => {
	    return {
	      name: "esbuild:html",
	      setup(build) {
	        build.onEnd(async (buildResult) => {
	          if (buildResult.errors.length) {
	            return;
	          }
	          const { metafile } = buildResult;
	          // 1. 拿到 metafile 后获取所有的 js 和 css 产物路径
	          const scripts = [];
	          const links = [];
	          if (metafile) {
	            const { outputs } = metafile;
	            const assets = Object.keys(outputs);
	  
	            assets.forEach((asset) => {
	              if (asset.endsWith(".js")) {
	                scripts.push(createScript(asset));
	              } else if (asset.endsWith(".css")) {
	                links.push(createLink(asset));
	              }
	            });
	          }
	          // 2. 拼接 HTML 内容
	          const templateContent = generateHTML(scripts, links);
	          // 3. HTML 写入磁盘
	          const templatePath = path.join(process.cwd(), "index.html");
	          await fs.writeFile(templatePath, templateContent);
	        });
	      },
	    };
	  
	  ```
	-
- rollup基本概念及使用
- rollup插件机制
	- hook类型
		- build hook：模块代码转换，AST解析以及模块依赖的解析，操作粒度是模块
		- Output hook：代码打包，操作粒度是chunk
		- 执行方式： Async，Sync，Parallel，First（依次执行，直到返回一个非null，如resolveId，load），Sequential（串行执行，如transform）
- vite插件
	- 虚拟模块: load
	- svgr
- hmr
	- 当模块接受更新时，该模块会被认为hmr边界
- code splitting
	- 解决的问题
		- 无法按需加载
		- 线上缓存复用概率低
	- 自定义拆包策略
		- build.rollupOptions.output.manualChunks
		- vite-plugin-chunk-split
- 语法降级和Polyfill
	- 底层工具链
		- 编译时工具： @babe/preset-env, @babel/plugin-transform-runtime
		- 运行时即出库：core-js, regenerator-runtime
	- target最佳实践:
		- // 现代浏览器
		  last 2 versions and since 2018 and > 0.5%
		  // 兼容低版本 PC 浏览器
		  IE >= 11, > 0.5%, not dead
		  // 兼容低版本移动端浏览器
		  iOS >= 9, Android >= 4.4, last 2 versions, > 0.2%, not dead
	- preset-env在polyfill上缺点
		- 如果使用新特性，往往是通过基础库(如 core-js)往全局环境添加 Polyfill，如果是开发应用没有任何问题，如果是开发第三方工具库，则很可能会对全局空间造成污染。
		- 很多工具函数的实现代码(如上面示例中的_defineProperty方法)，会在许多文件中重现出现，造成文件体积冗余。
	- transform-runtime的优点：transform-runtime 一方面能够让我们在代码中使用非全局版本的 Polyfill，这样就**避免全局空间的污染，**这也得益于 core-js 的 pure 版本产物特性；另一方面对于asyncToGeneator这类的工具函数，它也将其转换成了一段引入语句，不再将完整的实现放到文件中，**节省了编译后文件的体积。**
	- 其实 transform-runtime 也不是完美的方案，在 polyfill 方面不会读取 targets，因此凡是项目用到的语法都会注入 polyfill，对于一些需要兼容低端浏览器的项目可能影响并不大，但如果是现代浏览器，会注入很多没有必要的 polyfill 代码，导致bundle体积明显增大。因此 transform-runtime 和 preset-env 两个方面没有绝对的好与坏，到底选择哪个看需求场景，Vite 也是出于项目打包场景和 Polyfill 体积的原因，选择了 polyfill 方案；如果是基础库打包，那么 transform-runtime 将更加合适。
	-
- SSR
	- ssr应用有两大生命周期：构建时和运行时
	- 借助vite实现客户端和ssr产物
	- 借助ssr中间接实现运行时逻辑： 加载服务端入口，数据预取，组件渲染和html拼接
- Module Federation
	- 一种导入远程模块或应用的方案
	- 优点：实现了任意粒度的模块共享、减少构建产物体积、运行时按需加载以及共享第三方依赖
- ESM：高级特性和Pure ESM时代
	- esm新特性
		- type="importmap"
		- dynamic import
		- import.meta, import.meta.url
		- modulepreload
		- JSON Modules和 CSS Modules
	- tsup: 基础库打包工具，能生成commonjs和esm两种格式的产物，且可以任意使用与模块强相关的一些全局变量和api
- 如何对vite项目进行性能优化
	- 网络优化： HTTP2， DNS预解析，Preload，Prefetch，利用缓存
	- 资源优化：构建产物分析，资源压缩，code splitting，按需加载
	- ssr和ssg
	- HTTP2
		- http1存在队头阻塞和请求排队的问题
		- 多路复用：将数据分为多个二进制帧，复用同一个tcp通道
		- server push：对一个html请求，http2可以将相应的js和css资源推送到浏览器
		- 使用vite-plugin-mkcert，在dev server上开启HTTP2
		- 线上使用[Nginx](http://nginx.org/en/docs/http/ngx_http_v2_module.html)
	- DNS预解析
		- ```
		  <link rel="preconnect" href="https://fonts.gstatic.com/" crossorigin>
		  <link rel="dns-prefetch" href="https://fonts.gstatic.com/">
		  ```
	- Preload/Prefetch
		- 通过分析manifest，获得当前请求依赖的文件，添加preload
		- Vite 中我们可以通过配置一键开启 modulepreload 的 Polyfill:
			- build.polyfillModulePrelooad: true
		- 案例
			- 提前加载字体文件。由于字体文件必须等到 CSSOM 构建完成并且作用到页面元素了才会开始加载，会导致页面字体样式闪动。所以要用 preload 显式告诉浏览器提前加载。假如字体文件在 CSS 生效之前下载完成，则可以完全消灭页面闪动效果。
			- 对于商品列表页面，在用户鼠标停留在某个商品的时候，可以去分析商品详情页所需要的资源并提前开启 preload 加载，跟第 3 点类似，都是用来预测用户的行为并且做出一些预加载的手段，区别在于当用户停留在商品上时，点击命中率更高，preload 可以立即加载资源，有效提升缓存命中率。
	- 产物分析报告：rollup-plugin-visualizer
	- 资源压缩
		- js：指定target，target会影响打包后的体积
		- css： 不过在需要兼容安卓端微信的 webview 时，我们需要将 build.cssTarget 设置为 chrome61，以防止 vite 将 rgba() 颜色转化为 #RGBA 十六进制符号的形式，出现样式问题。
		- 图片压缩：vite-plugin-imagemin
	- 按需加载
	- 预渲染优化
- 配置解析起：配置文件在Vite内部被转换成什么样子
	- 解析过程
		- 加载配置文件，处理四种类型的配置文件
		- 解析用户插件
		- 加载环境变量
		- 创建路径解析器工厂
		- 生成插件流水线
- 依赖预构建：vite如何使用esbuild的打包
-
-
- ---
- importer: 引用当前模块的模块路径
-