***chapter5 <大而快-层次化存储>***
# 5.1 Basis
## 5.1.0 标准术语
- 带宽 Bandwidth
	传输数据的字节数/周期数
- 总线 BUS
	总线（Bus）是计算机各种功能部件之间传送信息的公共通信干线
	**总线（Bus）是计算机各种功能部件之间传送信息的公共通信干线**，它是由导线组成的传输线束， 按照计算机所传输的信息种类，计算机的系统总线可以划分为 数据总线 、 地址总线 和 控制总线，分别用来传输数据、数据地址和 控制信号。

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
## 5.2.1 SRAM
- 易失性存储
	![[Pasted image 20241213113940.png]]
- 解释:
	M3-导通 M1-截止 表示1
	M3-截止 M1-导通 表示0
	wl(wordline) 字线
	bl(bitline) 位线
## 5.2.2 DRAM
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
	- 提高性能
		1. 突发方式(burst mode) -SDRAM 在总线上
			周期分为2T
			T1 给出地址数据
			T2 读写数据
			Example(读取四个连续地址的数据):
				读第一个地址中的数据需要完整的2T
				而之后的三个地址,只需要在原先的地址上逐级+4每个T 直接读取数据
				所以最后只需要5T
		2. 双倍的数据速率 -DDR
			在一个周期的上升沿和下降沿都读取数据
		3. 四倍速动态随机存储 -QDR
			在CPI总线上处理
			CPU不需要等待
			T的前半部分就用来给出地址
		4. 多体交叉存储器
			编址交叉
			Example(四体交叉):
				首体地址末尾为00,提供地址的高30位即可
				一个周期T就可以读取(对齐存储)
				小端存储 低位数据存在低地址位
				bank1(00) | bank2(01) | bank3(10) | bank4(11)
			![[Pasted image 20241106152034.png]]
			对于四个32位的连续地址
			- a = 4 * (1 +15 + 1)T
			- b = (1 + 15 + 1)T
			- c = (1 + 15 + 4 * 1)T
			
# 5.3 Flash&Disk storage
## 5.3.1 Flash storage 固态硬盘
均衡写
- 使用次数有限制 与二氧化硅的厚薄有关
- 分类
	- NOR flash--按照bit存储
	- NAND flash--以块位单位存储,先清空erase再写入数据
## 5.3.2 Disk storage 机械硬盘
- 又被称为温盘(温切斯特盘)
	- 根据磁性方向来判断1 or 0
	- 利用磁性材料存储信息
	- 磁道track -> 扇区sector ![[Pasted image 20241214195844.png]]
- 读取方式
	1. 定位磁道 寻道
		可以假象为小车
	2. 旋转
		将想要的扇区转到磁头下方
	3. 读数据(扇区)
- 数据的存储方式
	![[Pasted image 20241214200354.png]]
	ID在格式化的时候设置
	G(gap)方便磁头对准
	CRC为校验码
	G之后的是有效数据(以文件格式存储)
	校验码用于检测- 外设与主机间连接是否有错误 - 数据是否有错误
	多个任务(进程)请求数据时要以**队列形式**来等待硬盘响应
- 时间的分配
	0. 多个任务(进程)请求数据时要以队列来等待硬盘响应(不算在时间内)
	1. 寻道时间(average策略)
	2. 旋转时间(将**磁盘转半圈**作为average旋转时间)
	3. 传输数据的时间
	4. 控制延迟的时间

# 5.4 CPU&cache&Memory间的数据交换！！
## 5.4.1 CPU&cache&Memory间的数据交换-Block块-基本概念
- CPU与cache联动 求速度
	部分内容映像(内容一模一样)
	cache没有地址
	CPU访存使用的是主存Memory地址
	主存地址分为:字节地址 字地址 位地址
	
	可以将**主存地址拆分**为:
	字地址word address(memory的块号) | 块内地址
		块内地址在32位系统小用两位二进制数表示(因为有四个字节)
	直接映射
		![[Pasted image 20241216110106.png]]
		cache中的**实际存储位置**(cache中的索引**index**) =
		Block address(memory的块号) mod Blocks in cache(块数量)
	进而可以**继续细分主存地址**：
	tag(标签位) | index(cache块号) | offset(块内地址)
	在cache中一个基本块block需要存储的数据有：
	Valid(有效位) | Tag(标签) | Data(数据(块))
		PS: Index不在cache中存储,只是人为规定的序号?
	- cache的真实内存为:
		n为index的长度
		$2^{n} \times (Valid有效位大小 + Tag标签字段大小 + Data单个数据块容量)$ 
	- cache的命名规范：
		cache的命名规范中一般只考虑**Data数据**的大小
		Example:
			Data单个数据块容量为4KiB 那么就称cache为4KiB cache 
- 冷启动&热启动
	- 冷启动
		清理cache
	- 热启动
		不清理cache
- 读数据(笼统的表述一下)
	将memory主存中的数据以块为单位通过检索高位地址传送到cache中,此时流水线阻塞,然后CPU从cache中读取数据
- Hit&Miss-评测标准
	- Hit命中
		1.Valid = 1
		2.内存地址的高位与Tag相同
	Hit率越高 CPU访存数据越快
- elses
	- 空间局部性-块的大小
		**块的大小变大**,访问的**命中率会提高** cache的容量不固定的情况下??
		但是在缺失时花费的时间也会提高(阻塞等)
			解决方法:
			早启动 (所需的块的**内容来了**之后立马就往下流水)
	- 块污染-降低Hit率
		cache中的**块数较少**,**映射关系单一** 以及 太多个主存地址**映射到一个cache index** 
		会导致频繁的调度块,引起块污染
## 5.4.2 CPU&cache&Memory间的数据交换-读写操作
- CPU读操作
	- 根据主存address取得index 找到块号
	- 查看valid
		- valid == 0
			- 缺失  miss
		- valid == 1
			- 查看Tag
				- Tag == addrHigh
					命中 hit
				- Tag != addrHigh
					缺失 miss
	
	- hit OR miss
		- hit情况下
			- 写直达法(写穿透法)
				数据写入cache的同时也写入write buffer写缓冲(保存等待写入主存的数据)中
				如果**buffer满**了,CPU**必须停顿流水线**直到buffer中有**空白表项**
			- 写回法
				只写cache不写主存Mem
				那么就会出现可能cache中的内容正确 Mem中的内容错误.即主存中未更新
				所以在cache存储中需要添加**脏位Dirty**标记
				所以在cache中一个基本块block需要存储的数据更新为：
				**Dirty(脏位)** | Valid(有效位) | Tag(标签) | Data(数据(块))
					要覆盖数据时如果**Dirty为1**,那么要将cache数据写回,再调用cache数据
		- miss情况下(要写的块不在cache中)
			- 写分配法
				将块调入cache + 写回法写数据
			- 写不分配法
				不调用到cache,**直接写主存**
## 5.4.3 CPU&cache&Memory间的数据交换-CPU时间&AMAT
- CPU时间
	$CPU时间 = (CPU执行的时钟周期数 + 访存stall阻塞周期数) \times 时钟周期$
	- 访存stall阻塞周期数主要包括:
		- I-cache 指令访存
		- D-cache 数据访存
	- 访存stall阻塞周期数(Memory stall cycles)
		Memory stall cycles
		= $\frac{Memory accesses}{Program} \times Miss rate \times Miss penalty$
		= $\frac{instructions}{Program} \times \frac{Misses}{instroctions} \times Miss penalty$
- AMAT(Average Memory Access Time)平均存储访问时间
	AMAT = Hit time命中时间 + Miss rate失效率 * Miss penalty失效代价
- 降低缺失率
	- 增加块的大小 $Penalty\uparrow 缺失率\downarrow$
	- 增加相联度
	- 使用多级cache
## 5.4.4 CPU&cache&Memory间的数据交换-n相联
- 全相联
	主存块可以直接映射到cache的任何位置
	相联度为**cache中的块数**
	可以将**主存地址拆分**为:
	字地址word address(memory的块号) | 块内地址
	- 代价
		占用地址
		需要多添加比较器
- 组相联!!
	index组号 = 主存块号 mod cache中的组数
	相对于直接映射来说 相当于 将原index的前几位给了Tag
	- 替换算法
		- 最近最少使用LRU(Least Recently Used)
			**被替换**的数据块时**最长时间未被使用**的
		- 随机算法
	Example:
		![[Pasted image 20241216110148.png]]
- 全局缺失率
	多个cache加起来的缺失率
# 5.5 虚拟Virtual
## 5.4.1 虚拟机-virtual machine
- 硬件
	- Host
		主机Host模仿客户端Guest 操作系统和机器资源
		- 改进了多个客户端guests的隔离
		- 避免了安全性和可靠性问题
		- 辅助资源共享
	地址转换
	存储介质
- 软件
	- Virtual Machine Monitor虚拟机监视器(管理程序)
		- 将虚拟地址映射为物理地址(主存 | I/O devices | CPUs)
		- 客户端Guest的代码在本地机上以用户模式运行
			根据特权指令和对受保护资源的访问捕获到VMM
		- 客户端的操作系统 Guest OS 可能与主机操作系统 Host OS不同
		- VMM 操作真实的I/O设备
			模拟客户端Guest的通用虚拟I/O设备
- summary
	虚拟化会对性能有一定的影响
	但是对于现代高性能计算机来说可行
- Memory Layout 内存布局
	机器会为每个进程(正在运行的程序)分配相应内存
	![[Pasted image 20241216121415.png]]
	static data在内存中有分配空间
	Text中是 程序代码
## 5.4.1 虚拟地址-virtual memory
- Virtual address 与 Physical address转换
	![[Pasted image 20241216121913.png]]
	Physical address是物理地址(主存Mem地址)
	Page offset就是页内地址
	该转换过程由CPU完成
# 5.6 cache(TLB)&Memory(PT)&Disk间的数据交换-Page
## 5.6.1 页表PT
- Page Table(在Memory中)!!
	{VPN}不占用空间存储
	存储的内容有:
	Valid(有效位) | PPN 
	Valid为0 说明该页Page不在内存Memory中
	![[Pasted image 20241216122624.png]]
## 5.6.2 更新后的页表PT&快表TLB
- TLB (Translation Look-aside Buffer)(在cache中)!!
	![[Pasted image 20241216161012.png]]
	内存**Mem中的页的大小**与**硬盘Disk**中的大小**相同**
	PS:
		在**TLB是CPU中的一个cache** 必须要包括**Tag标签字段**(保存虚拟页号的一部分)
		其中**页大小** == $2^{offset}$ !!
	- 在更新之后的**页表Page Table**!!
		- 页表在进程周期中是常驻内存
		- VPN不需要占存储空间 仅作为index来搜索PTE页表项
		所以页表项PTE中真正所需要存储的内容有:
		Valid(有效位) | Dirty(脏位) | Reference(引用位) | PPN(物理页号)
		- 解释:
			- valid为0 那么发生缺页故障异常
				调用异常处理软件 将硬盘Disk中的内容调到主存Mem中 使用全相联映射 再更改页表中的内容
			- Reference应用位 又可以称为use使用位
				定期将应用位清零,然后再重新记录,用于替换策略
			- Dirty
				写数据时 只写cache 置脏位Dirty为1
- CPU内有一个寄存器指向正在进行的进程的页表的起始地址
- 表项太多-解决方案
	- 反置页表
	- 多级页表
		用的多的页表放在主存Mem中,用的少的放在硬盘Disk中
		比如0级页表放在主存Mem中,其他级的页表放在硬盘Disk中
	- TLB快表
		快表找不到的,去页表中查找,并将其表项放在TLB中
		只允许一个进程占用处理器CPU 使用TLB
## 5.6.3 cache(TLB)&Memory(PT)&Disk间的数据交换-Page-Summary
- 基本概念
	- PT页表
	- PTE页表项
- 主存Memory的保护
	- 不同的任务(进程)可以共享其部分虚拟任务空间
		需要操作系统OS的协助
	- 操作系统OS保护的硬件支持
		- Privileged supervisor mode特权监督模式(aka kernel mode)
	- 操作系统(0级栈)和用户使用的栈(3级栈)不允许交叉使用
	- 只有在内核态下页表和其他的状态信息才能被访问
	- 页表寄存器(特殊状态的寄存器)
	- 内核态--操作系统运行线程
		对于CPU来说 存在内核态和用户态
	- CPU调用内核态(系统代码)--系统调用--产生异常
- Write操作
	0. CPU
	1. Cache
	2. Memory
	3. Disk
	* $1Cache\Leftrightarrow 2Memory$
		可以使用写直达法和写回法
		可以使用直接映射 | 组相联 | 全相联
	- $2Memory\Leftrightarrow 3Disk$ 
		只能使用写回法 写(因为 stall代价太大)
		使用全相联 虚拟表号页表
		