- router
	- file based routing
		- nested routes with NuxtPage
		- dynamic routes: /course/chapter/[chapterSlug]/lesson/[lessonSlug].vue
- Universal Rendering: SSR
- Navigating with NuxtLink
	- open external link
		- noopener
		- noreferrer
- useHeader: bind meta data to the head of the page.
- ClientOnly
	- 解决ssr state和客户端state冲突的问题，如存在localstorage的state。
	- 当ssr给出的render和hydration得到的render不同时，会产生错误。
- Deploy app to Netlify
- how to organize code
	- pages, component,layout
	- composable
- Error
	- 404 page
	- handling client side errors with NuxtErrorBoundary
		- 依赖onErrorCaptured，本质是将处理error的func绑定到currentInstance上，当try-catch捕获到错误时，将error传给func
		- 处理error的方式
			- re-render
			- 跳转动无错误页面，再把error变为null
	- handling server side errors
		- 返回自定义的error页面
	- 使用route validation来提前抛出error
- Route Middleware
	- inline middleware
	- name middleware
	- global middleware
- set up superbase
	- create a superbase project
	- create github app
	- set the auth of superbase project
- add environment variables
	- add .env file
	- config netlify
- login
	- protecting routes with Auth
	- OAuth
		- stop sharing user login info, share tokens that represent permission
		- use a refresh token to "refresh" the access token
		- continue to have control over what applications can access our data without changing user info
- server route
	-