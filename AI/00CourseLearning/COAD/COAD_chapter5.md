***chapter5 <大而快-层次化存储>***
# 5.1 IEC Standard Prefixes

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
# 5.2

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