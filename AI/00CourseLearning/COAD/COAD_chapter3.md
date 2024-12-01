***chapter3 <指令-计算机的语言>***
# 3.1 算术逻辑单元ALU
- ALU运算器主要完成算数与逻辑运算
# 3.2 数值操作
## 1. 整数
- 以二进制补码的形式存储数据、机器字长固定、使其可以循环
	分为有符号数与无符号数
## 2. 小数
- 遵循**IEEE754标准**
	**单精度float**
	**双精度double**
	可以由ALU运算器完成相对应运算
	**但是**效率低，所以使用**协处理器**完成浮点数运算
## 3. OverFlow
- Adding +ve and -ve operands
	**No OverFlow**
- Adding **two+ve** operands
	**OverFlow** if **result sign is 1**
- Adding **two -ve** operands
	**OverFlow** if **Result sign is 0**
- 因为
	[x]补 + [y]补 = {x+y}补
	[x]补 -  [y]补 = {x-y}补
## 4. Multiplication
- initial![[Pasted image 20241104203118.png]]

- optimized![[Pasted image 20241104203406.png]]
- multiplicand - 被乘数 | multiplier - 乘数 | product - 结果
	 乘数与部分积共用64位，当乘数右移高位补充的数值为
	 部分积右移溢出的低位的数值
	**但是**由于速度过慢 所以以**快速乘法器与阵列乘法器**代替
	- 1. 判断Multiplier的最后一位是否为1
		- 如果为0 那么进行下一步操作
		- 如果为1 那么将当前的被乘数multiplicand加临时积   作为新的临时积
	- 2. 将被乘数multiplicand 左移一位
	- 3. 将乘数multiplier 右移一位
	- 4. 判断是否重复32次
		- 如果没有那么继续上述过程
		- 如果有怎么退出循环得到结果
## 5. Four Multiply instructions
- **mul** : multiply
	Gives the lower 32 bits of the 64 bits product
- **mulh** : multiply high
	Gives the upper 32 bits of the 64 bits product
- **mulhu** : multiply high unsigned
	Gives the upper 32 bits of the 64 bits product
	assuming the operands are unsigned
- **mulhsu** : multiply high signed/unsigned
	Gives the upper 32 bits of the 64 bits product
	其中一个因子为有符号数，另一个因子为无符号数
## 6. Four instructions
- **div,rem** : signed divide and remainder
- **divu,remu** : unsigned divide and remainder
	参与运算的数值均是绝对值，符号位单另进行处理(通过因数与积的符号)
## 7. Mux(多路选择器)
- ![[Pasted image 20241009153839.png]]
	Operation为控制信号(由fun3与fun7产生)
	0：则进行AND指令
	1：则进行OR指令
	组合逻辑电路(输入改变则输出信号立即发生改变)
## 7.饱和算法(Saturating operation)
- 在overflowing的情况下
	- **无符号 8 位整数（范围 0 到 255）**
	    如果计算结果超过 255，则用 255 替代
	- **有符号 8 位整数（范围 -128 到 127）**
	    如果计算结果超过 127，则用 127 替代
	    如果小于 -128，则用 -128 替代
# 3.3.  IEEE_Floating_format
## 1. 规格化与非规格化
![[Pasted image 20241016151342.png]]
- 规格化
	- 定义
		- Exp不是全零也不是全一
		![[Pasted image 20241016151550.png]]
	- 范围
		![[Pasted image 20241104205857.png]]
	
- 非规格化
	- **Denormal number**
		![[Pasted image 20241016151453.png]]
		PS：此处的指数部分应该为**1 - Bias**
	- **Infinity & NaN**
		![[Pasted image 20241016151715.png]]
	
## 2. Bias的设计
- 加入Bias是为了方便进行指数Exp的比较，进而比较浮点数大小
## 3. Float Adding
- 对阶
	小阶变大阶
	计算机小数点始终处于最高位和次高位之间
	删去最小位 可以避免精度丢失严重
- 有效数字位相加
- 规格化
	最高位通过移位操作置1，同时指数位改变
- 判断溢出
- 舍入
	向偶数舍入 只需要判断被舍去位为10的情况
	且要注意**舍入后是否规格化**
	- 保护位、舍入位和粘贴位
		- 保护位
			最右边多保留的两位中的首位
		- 舍入位
			最右边多保留的两位中的第二位
		- 粘贴位
			是当舍入位右边有非零的数据时将其置为1
## 4. Float Multiplication
- 指数相加
	新Exp = 旧Exp1+旧Exp2 - Bias
- 有效数字位相乘
- 规格化
	最高位通过移位操作置1，同时指数位改变
- 判断溢出
- 舍入
	同上述规则 向偶数舍入
- 符号位
	判断最终结果的正负
## 5. FP(Float point) Instruction
- 指数相加
	新Exp = 旧Exp1+旧Exp2 - Bias
- 有效数字位相乘
- 规格化
	最高位通过移位操作置1，同时指数位改变
- 判断溢出
- 舍入
	同上述规则 向偶数舍入
- 符号位
	判断最终结果的正负