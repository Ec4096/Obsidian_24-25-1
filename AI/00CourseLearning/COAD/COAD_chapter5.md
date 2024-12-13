***chapter5 <大而快-层次化存储>***
# 5.1 Basis
## 5.1.1 IEC Standard Prefixes

| Name  | Abbr | Factor   | SI size   |
| :---: | :--: | :------- | :-------- |
| Kilo  |  K   | $2^{10}$ | $10^{3}$  |
| Mega  |  M   | $2^{20}$ | $10^{6}$  |
| Giga  |  G   | $2^{30}$ | $10^{9}$  |
| Tera  |  T   | $2^{40}$ | $10^{12}$ |
| Peta  |  P   | $2^{50}$ | $10^{15}$ |
|  Exa  |  E   | $2^{60}$ | $10^{18}$ |
| Zetta |  Z   | $2^{70}$ | $10^{21}$ |
| Yotta |  Y   | $2^{80}$ | $10^{24}$ |
## 5.1.2 局部性
- 时间局部性 Temporal locality
	在**相邻时间**内 访问**同一个内存单元**
- 空间局部性
	访问存储单元 操作地址连续(同时被访问)
- 所以访问内存地址 局部、范围小

## 5.1.3  存储层次结构(memory hierarchy)
- 使用多级存储器，对着与CPU距离增加，存储器的容量和访问时间都会增加，但是每bit的成本降低，具体如下:
0. CPU
1. Cache-SRAM 易失性 只需要mos管的电平变化、所以数据交流快
2. Memory DRAM 易失性
3. Disk Flash(闪存一般应用在固态硬盘中) & Meganetic disk 机械硬盘 **非易失性**
- $Cache\Leftrightarrow Memory$  交换数据 以“块”为单位(cache行)
- $Memory \Leftrightarrow Disk$ 交换数据 以“块”为单位(页page)

## 5.1.4 temp
- CPU 给出Read/Write 控制信号
	- Read信号 CPU等待数据
	- Write信号 CPU给出数据 等待写数据操作完成

# 5.2 SRAM&DRAM
## 5.1.1 SRAM
- 易失性存储
	![[Pasted image 20241213113940.png]]
- 解释:
	M3-导通 M1-截止 表示1
	M3-截止 M1-导通 表示0
	wl(wordline) 字线
	bl(bitline) 位线
## 5.1.2 DRAM
- 非易失性存储
	![[Pasted image 20241213114240.png]]
- 解释:
	- Storage Capacitor 为1 即为有电荷存在 
	- 写操作 先将wl连通, 然后bl对Capacitor电容来充电或者放电
	- 读操作(具有破坏性)先连通wl, 然后查看bl线上是否有微弱电流流过(要经过放大器放大)因为**读取过程**中会**造成电荷流失**,所以还要进行充电操作
	- 速度慢 -因为需要进行充电放电操作
	- 缺点
		电容上的电荷会随着时间而丢失
		所以每隔2ms 要进行周期性的刷新(相当于读操作) 所以要增加刷新电路(计时以及其他功能) 刷新的优先级高 如果此时CPU要访问数据 那么需要等到刷新完成


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