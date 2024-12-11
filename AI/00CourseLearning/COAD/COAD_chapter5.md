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
3. Disk Flash & Meganetic
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