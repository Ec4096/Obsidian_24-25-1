x***chapter2 <指令-计算机的语言>***
# 2.1 引言

![[Pasted image 20241002162137.png]]

![[Pasted image 20241002162148.png]]

![[Pasted image 20241002162454.png]]

![[Pasted image 20241027163431.png]]

2. Register
	* x0 The constant value 0
	* x1 Return address
	* x2 Stack pointer
	* x3 Global pointer
	* x4 Thread pointer

![[Pasted image 20240927100441.png]]
PC 即sp寄存器 存储当前指令的地址
# 2-1 指令分类1
## 1. **运算指令**
- **算数运算**
- **逻辑运算**

## 2. 数据传送指令
- **load(w\h\b)**
	w-word-32bits
	h-half-16bits
	b-Byte-8bits
- **store(s\h\b)**

## 3. 控制指令
- **条件分支**
- **无条件分支(Go to)**

# 2-2 指令分类2
## 1. R型指令 - 一个目的reg、两个源reg
- 包含**运算指令**
	例如：
	运算指令 add x5,x6,x7 //x5 = x6 + x7
	逻辑指令 and x5,x6,x7 //x5 = x6 & x7

## 2. I型指令 - 一个目的reg、一个源reg、一个立即数
- 包含**运算指令** **load指令**
	例如：
	addi x5,x6,20   //x5 = x6 + 20
	slli    x5,x6,3     //x5 = x6<<3
	lw     x5,40(x6) //x5 = Mem[x6+40]
	**jalr** 也为I型 
	jalr x1,100(x5)  // x1 = PC+4; go to x5 +100
- 有关扩展的问题
	符号扩展按照读入数据的最高位(lw lh lb)
	其余位填充0 (lwu lhu lbu)

## 3. S型指令 - 两个源reg、一个立即数
- store指令
	例如：
	sw x5,40(x6)  //Mem[40+x6] = x5
	x6是rs1
	x5是rs2

## 4. SB型指令 - 两个源reg、一个立即数
- 跳转指令
	例如(beq | bne | blt | bge)
	blt x5,x6,100   //if(x5<x6) go to PC+100
	bge x5,x6,100 //if(x5>=x6) go to PC+100 
	跳转到imm左移一位的地址，因为地址均为偶数
	所以以上两个操作立即数都是50
	
## 5.U型指令 - 一个目的reg、一个立即数
- 例如**lui**与**auipc**
	lui将数据传入寄存器高20位，同时将低12位置零
	![[Pasted image 20240925144703.png]]
	auipc rd imm   // rd = PC + (imm<<12)

## 6. UJ型指令 - 一个目的reg、一个立即数
- 例如jal
	jal x1，10  //x1 = PC+4 ; go to PC + 10
	PC跳转到imm左移一位的地址，因为地址均为偶数
	所以以上操作的立即数是5
# 2-3Procedure Calling
- Place parameters in register x10 to x17
- Transfer control to precedure
- Acquire storage for procedure
- Perform procedure's operations
- Place result in register for caller
- Return to place of call(address in x1) using jalr

# 2-4 Addressing Summary
- Imediate addressing - 立即数寻址
	![[Pasted image 20240929103229.png]]
- Register - 寄存器寻址
	![[Pasted image 20240929103242.png]]
- Base - 基址寻址
	![[Pasted image 20240929103320.png]]
- PC-relative - PC相对寻址
	![[Pasted image 20240929103353.png]]

# 2-5 Atomic Read/Write memory operation
![[Pasted image 20240929105234.png]]
- 解释
	将RS1将RS1中的数据加载到rd寄存器中
	如果后续访存寄存器的值与RS1中的地址相等
	则将flag置为1 不能进行存取操作
	条件写指令 将RS2的值写入RS1寄存器地址
	如果flag为1 则写入失败 return 0
	如果flag为0 则可以写入 return 1 
- 举例1
	![[Pasted image 20240929110718.png]]
	x11为flag、x10为temp
- 举例2
	![[Pasted image 20240929110923.png]]
	x20寄存器所存储数据对应的内存的数据为锁
# 2-6 ASCII码表
![[Pasted image 20241030204222.png]]

# 2-7 链接器
- **Static Link**
	Inel 静态库 后缀*lib
	Linux 静态库 后缀*a
	在运行过程中copy code 快但是内存利用率低（因为会有多个副本code）
- **Dynamic Link**
	Intel 动态库 后缀*DLL 
	在代码段做标记---可以提高内存利用率
	- 编译时**打桩**
		需要能够访问包装函数的源代码
	- **链接时打桩**
		需要能够访问可重定位目标文件。 linux静态**链接**器支持用--wrap f标志进行**链接时打桩**
	- 运行时**打桩**
		只需要能够访问可执行目标文件。



# 12.21
- 希冀作业有关beq跳转范围的一道题