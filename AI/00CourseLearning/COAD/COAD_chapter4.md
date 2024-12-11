***chapter4 <处理器>***
![[Pasted image 20241105115718.png]]
# 4.1 Single core processer
-  **Datapath**
	执行操作
- **Control**
	控制数据通路
- **categories**
	单周期处理器
	多周期处理器
	流水线处理器
* **categories**
	loader(操作系统)传地址给PC
	PC传地址到Memory
- **指令执行过程**
	- 取址
		所需的硬件基础(PC IM(Instruction memory位于CPU当中不可写、只可读))
		PC将地址传到Memory取得指令
		改变PC的值
	- 译码与读操作数
		所需的硬件基础(Control、RF(Register File))
		PC 所取得的指令 传给Control
		Control解析指令的同时 Datapath读取操作数(寄存器的序号与立即数)
		RF是只读不写
- **执行**
	- **R型(例如add x10 x8 x9)**
		1. ALU计算x8+x9
		2. 写rd (RF硬件支持)
	- **load型(例如 lw x5，5(x6))** 
		1. 计算ALU与Immgen 需要做符号扩展
		2. 读数据:DM(Data Memory)
		3. 写rd (RF硬件支持)
	- **store(例如 sw x5 5(x6))**
		1. 计算地址ALU与Immgen 需要做符号扩展
		2. 写数据:DM
	- **beq()**
		1. 计算 将两个值相减
		2. 查看Zero的值
			当Zero为1时 PC = PC + imm * 2
			当Zero为0时 PC = PC + 4
# 4.2 Design
## 1. Logic design
- 通过电压来确定电信号
	+(3~5V)为1
	-(3~5V)为0
## 2. 组合逻辑电路
- 由wire 与 门电路组成
	组合单元信号来自于状态单元 (为了保持操作的稳定性)
	组合单元结果输出给状态单元(保存输出结果)
## 3. 状态单元 时序单元
- 由电平或电平变化来决定来决定
	1. 边沿触发(上升沿、下降沿)<主要使用>
	2. 电平触发(高电平、低电平)

# 4.3 单周期CPU
- 一个CPU只处理一条指令 800ps load指令
1. 组合逻辑 速度快 但控制信号多 导致设计复杂
2. 存储型控制器ROM慢 需要检索查找操作
	存储九位包括 Instruction [30fun7其中一位,14:12,6:2}
3. 数据通路datapath
# 4.4多周期CPU
- 分步完成各操作
	Instruction memory -> register file -> ALU ->
	data memory -> register file
- 其中Data memory需要200ps
# 4.5 流水线处理器 Pipeline
- 简介
	最常见的便是五级均匀流水线、加速比就是级数、倘若流水线**不均匀** 那么加速比就会下降
- 过程 ![[Pasted image 20241105183459.png]]
1. **IF**取指令: Instruction fetch from memory 200 PC IM
2. **ID**译码及读: Instruction decode & register read 100 RF
3. **EX**计算: Execute operation or calculate address 200 ALU
4. **MEM**访存: Access memory operand 200 DM(data memory)
5. **WB**写回: Write result back to register 100 RF(register file)
- 对于读写操作
	将一个周期分为两部分 规定**先写再读**可以避免浪费时间 
	对于WB阶段刚写入reg的值在 ID阶段便可取出使用
- 流水线寄存器
	为了防止已经读取的数据与PC 被 后来的数据"冲"掉，所以在每一级之间要加上流水线寄存器
# 4.6冒险
## 1. 结构冒险
- 举例：
	有的系统IM与DM为一个memory 在一个进行时另一个不能进行
- 解决方法：
	增加功能单元，例如加法器等
## 2. 数据冒险(数据相关)
- 举例：
	add x7,x5,x6
	sub x8,x7,x5
	在第一条指令EXE阶段第二条指令在ID阶段从register读取数据
- 解决方法：
	1. 阻塞Stall
		加bubble之后 bubble也会顺着数据通路流动
	2. 加旁路aka bypassing(forwarding)
		第二条语句ID照常进行、但是EXE时使用的是第一条语句旁路的数据(用于把第一条语句的结果直接传给EXE阶段ALU作为加数)
	3. code scheduling 编译器处理
- 阻塞条件ld指令
	ID/EX rd = IF/ID rs1 或 ID/EX rd = IF/ID rs2
	Memread = 1
- 操作:
	- 写ID/EX 控制信号为0 以致后续操作都不会进行
	- IF/ID不能写
	- PC也不能写
## 3. 控制冒险
- 举例：
	由分支指令、转移指令引起的
- 解决方法：
	1. 阻塞
	2. 预测
		1. 静态预测 
		- 开始时便预测
			EXE/MEM = 0
			ID/EMX flush
			IF/ID flush
			要增加功能部件
			原浪费了三个周期
			可以将EX末尾算出的Zero在MEM阶段提前到**ID阶段**
		1. 动态预测
		- Branch prediction buffer(缓冲区) 使用**BHT(Branch history table)
		- BHT
			在运行刚开始时为空
			PC | 指令 | label | 跳转地址

# 4.7 流水线分析方法
- 单周期分析图
- 多周期分析图
	首先确定是否存在**数据冒险**
	其次是否存在**控制冒险**