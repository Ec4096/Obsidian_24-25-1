***chapter6 <并行处理 >***
# 7.0 前述
对于并行处理我们可以从两个层面考虑
- 硬件层面
	- 串行
		一条一条地处理指令
	- 并行
		同时处理指令
- 软件层面
	- 顺序
		指令一条一条地运行
	- 并发
		多条指令同时执行
	- PS
		单发射CPI = 1
- 问题
	- 负载均衡 <任务的划分到子任务>
	- 协调 <同步(任务之间)保证正确性>
	- 交流协调
# 7.1 Basis
# 7.1.0 标准术语
- 带宽 Bandwidth
	传输数据的字节数/周期数

- 总线连接串口-键盘鼠标
- 总线连接设备接口-声卡|显卡|网卡

# 7.2 Amdahl定律
- 最基本的公式
	$T_{improved} =  \frac{T_{affected}}{improved factor优化量} + T_{unaffected}$
- 公式推导
	$Speed = \frac{T_{old}}{T_{new}}$
	$Speed = \frac{T_{old}}{T_{old} - T_{affected} + \frac{T_{affected}}{n优化量}}$
	对于上式分子分母同时除$T_{old}$
	$Speed = \frac{1}{1 - F + \frac{F}{n优化量}}$
	其中$F = \frac{T_{affected}}{T_{old}}$
	通常优化量n与处理硬件数量有关
- 基本术语
	Scalars-标量-离散的数据
- 强缩放与弱缩放
	- 强比例缩放
		任务数量不变,处理单元(硬件)增加
	- 弱比例缩放
		任务与处理器同时增加(同比例)
- 流水线处理器 - 超标量处理器
# 7.3 指令-数据流
- 基础概念
	![[Pasted image 20241230170630.png]]
	- SISD
		单指令单数据流 例如add x5,x6,x6 一条指令处理一个数据流
	- SIMD
		单指令多数据流 例如处理声音和图像
		初衷是为了把控制单元的成本平摊到数个执行单元上
		减少了指令带宽和空间
	- MIMD
		SPMD(Single Program Mutiple Data)
		一个可以并行的处理器 在MIMD中
		对于不同处理器提供状态代码

- Vector Processors向量处理器架构
	- 向量存储器