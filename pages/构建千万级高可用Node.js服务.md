title:: 构建千万级高可用Node.js服务

- APM
	- APM是什么
	  collapsed:: true
		- 是监控服务的一套技术手段，致力于监控和管理程序的性能和可用性
		- 应用
			- 服务器性能指标监控，如CPU，内存，硬盘
			- 服务监控，入服务来源，请求数及成功率等
			- 错误和一场监控
			- 日志收集
			- 依赖监控
			- 分布式事务追踪
			- 代码级监控分析（Profiling）
	- USE Method
	  collapsed:: true
		- Utilization（利用率，cpu利用率），Saturation（饱和度，排队情况），Errors（误差）
	- USE Method是方法论，APM根据USE Method去具体实践，
	- 如何发现Node.js应用的问题
	  collapsed:: true
		- 排查计算密集型的利器：火焰图
		- 排查内存泄露，查看内存快照，即Core Dump
			- 使用autocannon 压测
			- 使用clinic生成Core dump
		- QPS，TPS
		- RT， Concurrency
	- 使用Graphite来搭建APM
	  collapsed:: true
		- 架构
			- 指标收集器，Graphite-web。
			- 监听器，Carbon，接受指标采集器收集上来的数据，做聚合计算，之后进行缓存或存储。缓存用于给前端查看。
			- whisper，存储按照时间序列的值
		- Grafana, APM可视化工具
		- 统计指标类型
			- Gauges，不需要处理的数据，如cpu利用率
			- Counter，统计访问量，每分钟XXX。
			- Sets，需要再处理的指标，如统计地区数
		- stats是所有指标的集合
		- 使用lynx发送自定义指标
	- 使用prometheus来搭建APM
- 如何部署一个稳定的Node.js应用
	- 网络层负载均衡
		- browser -> dns -> lvs -> nginx -> apigateway
		- 集成TLS，减轻应用服务的压力
	- 应用层的负载均衡
		- 服务（进程）负载均衡
		- RPC负载均衡
			- 布隆过滤器取模运算
			- 一致性hash法取对应机器
				- 虚拟节点，让hash分布更均衡
	- 优雅退出
		- 监听信号
			- SIGTERM
			- SIGINT
			- SIGQUIT
	- 灰度发布
		- nginx负载均衡 + 金丝雀发布
	- pm2