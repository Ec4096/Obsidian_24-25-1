***chapter4 <处理器>***
![[Pasted image 20241105115718.png]]
# 4.0 ALU基础
- ALU used for
	- Load/Store：F = add
	- Branch：F = subtract
	- R-type：F depends on opcode

| ALU control | Function |
| :---------: | :------: |
|    0000     |   AND    |
|    0001     |    OR    |
|    0010     |   Add    |
|    0110     | Subtract |
- ALUOp 与 ALU operation
	ALUOp-2bits
	ALU operation-4bits
	由fun3与fun7处理而得(所以由fun3与fun7决定)
	![[Pasted image 20241023141927.png]]


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
## 4.2.1. Logic design
- 通过电压来确定电信号
	+(3~5V)为1
	-(3~5V)为0
## 4.2.2. 组合逻辑电路
- 由wire 与 门电路组成
	组合单元信号来自于状态单元 (为了保持操作的稳定性)
	组合单元结果输出给状态单元(保存输出结果)
## 4.2.3. 状态单元 时序单元
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
## 4.6.1. 结构冒险
- 举例：
	有的系统IM与DM为一个memory 在一个进行时另一个不能进行
- 解决方法：
	增加功能单元，例如加法器等
## 4.6.2. 数据冒险(数据相关)
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
- 冒险检测与前递单元
	- 前递单元 Forwarding Unit
		前递单元解决的是 DM WB阶段与EXE阶段的数据冒险
		比如：该阶段需要使用上一条指令EXE之后的结果
		那么在本阶段EXE时可以接受 DM传过来的结果 或者DM传过来的结果
	- 冒险检测
		冒险检测所做的事是 使用**nop**来解决需要**未来ld的数据**的问题
		比如：
		lw x2 , 20(x1)
		and x4 , x2 ,x5
		那么计算and时x2的数据还未读出，那么这个冒险检测单元就是要被输入一些中间寄存器有关ReadMem的指令 然后判断是否要插入nop解决数据冒险
## 4.6.3. 控制冒险
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
			原浪费了三个周期 **在DM阶段进行过程中才能判断是否要跳转**
			可以将EX末尾算出的Zero在MEM阶段提前到**ID阶段**
			但还是要浪费一个周期 即如果要跳转的话 那么是在ID计算完成的
			此时已经有一个冗杂指令到了IF阶段；那么接下来 beq进入EXE阶段 跳转之后的指令进入IF阶段 ID阶段会变成一个nop
			- 但是提前到ID阶段会出现两个问题：
				- 对于**原来对于分支指令的操作数进行前递操作是由ALU前递逻辑处理**的，此时如果提前到ID阶段就需要在ID阶段也引入新的前递逻辑，旁路获得的源操作数 可以从EX/MEM或MEM/WB获得
				- 在ID阶段分支比较的值可能在未来产生，可能会产生数据冒险 所以就需要nop来处理(相当于数据冒险中的冒险检测单元)。上一条指令数据在EXE阶段产生就需要nop一个周期，上一条指令数据在MEM阶段产生(load)就需要nop两个周期
		1. 动态预测
		- Branch prediction buffer(缓冲区) 使用**BHT(Branch history table)
		- BHT
			在运行刚开始时为空
			PC | 指令 | label | 跳转地址
		- 2-bit 预测
			![[Pasted image 20241211172610.png]]
			

# 4.7 流水线分析方法
- 单周期分析图
- 多周期分析图
	首先确定是否存在**数据冒险**
	其次是否存在**控制冒险**
# 4.8 异常与中断
## 4.8.1 什么是异常与中断
- 中断
	From an external I/O controller 由**硬件等引起**的异常
	主机、外设交互慢 因为主机一直在等外设提供数据
	硬件异常时 外设向主机提出**中断请求信号**
- 异常
	由**软件引起**的中断
	例如:
		overflowing、未定义的opcode、系统调用syscall
- 解决异常
	
## 4.8.2 如何解决异常
- SEPC (Supervisor-Exception-Program-Counter)
	save PC(**当前**的地址) of interrupted instruction
- SCAUSE (Supervisor-Exception-Cause-Register)
	save indiction of problem 保存产生异常的原因
	该寄存器中大多数位未被使用 其中：
	未定义指令 undefined code编码为(2)
	硬件故障 编码为(12)
- 处理异常程序的过程
	- 使用SEPC存储当前地址
	- 通过SCAUSE来检索异常处理程序的入口地址
		- 非法操作是直接退出
		- 解决异常长须后再将SEPC中存储的地址再还给PC
		- 停止系统运行 -死机
- Flush
	Flush为控制信号，可以对指令进行清除操作
- 流水线中如何处理例外(异常与中断)
	在流水线实现中，将例外处理堪称另一种控制冒险 清除掉异常程序，并从新地址开始取指
	- 将取指IF阶段的指令
		变成空操作nop，来消除影响
	- 对于进入译码ID阶段的指令
		增加新逻辑，控制译码阶段的多选器使输出为0，流水线停顿
		添加一个**新的控制信号ID.Flush** 与冒险检测单元的stall信号进行OR操作，来**对指令进行清除**
	- 对于进入执行阶段的指令
		添加一个**新的控制信号EX.Flush** 使多选器输出为0
	- 对于PC多选器新增一个输入,保证能将**例外入口**地址送给**PC寄存器**
	- 在**SEPC寄存器**中保存**引发例外的指令地址**
- jump to handler
	0000 0000 1C09 0000$_{hex}$ 地址固定 (RISC-V)

## 4.8.2 补充-处理异常与中断的另一种方法-向量式中断
- 中断描述符表IDT(Interrupt Descripter Table)
	有256条地址(中断服务(异常处理)程序的入口地址) 0-255 
	由操作系统初始化
	由中断向量号 来通过IDT来定位入口地址 以八位来表示
	