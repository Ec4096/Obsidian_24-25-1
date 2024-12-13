
1. CPU给出访存信号
2. CPU给出Read/Write 控制信号
 - 3.1 Write信号 CPU给出数据 等待写数据操作完成
 - 3.2 Read CPU等待数据


---

![[Pasted image 20241106142946.png]]
- M3的导通 M1的截止 表示1
- M3的截止 M1的导通 表示0

wl wordline 字线
bl bitline 位线

---

![[Pasted image 20241106144034.png]]
- 查看Storage Capacitor是否有电荷表示1 与0
- 写操作 wl先连通 bl对于Capacitor电容来充电或者放电
- 读操作（破坏性） wl连通 查看bl线上是否有微弱电流流过(经过放大器放大) 因为读取后电容上电荷流失所以还要进行充电操作
- 速度慢的原因是 需要进行充电放电操作
- 缺点 
	电容上的电荷会随着时间而丢失 2ms
	所以每隔2ms 要进行周期性的刷新 (相当于是读操作) 所以要增加刷新电路（计时以及其他功能）刷新的优先级高 如果此时CPU要访问数据 那么需要等到刷新完成
- 提高性能
	1. 突发方式 burst mode - SDRAM 在总线上
		周期分为2T
		T1用来 给出地址数据
		T2用来 来读写数据
		例如四个连续地址
			读第一个地址需要2T
			而之后的三个地址 只需要在原先的地址上逐级+4 每个1T 直接读取数据
			一共5T
		需要增加一个逻辑单元+4（在总线上）
	2. 双倍的数据速率DRAM   - DDR
		在一个周期的上升沿和下降沿都读写数据0
	3. 四倍速动态随机存储 -QDR
		CPI         总线 
		CPU不需要等待
		T的前部分就用来给出地址
		
	1. 多体交叉存储器
		编址交叉 
		例如 四体交叉
			首体地址末尾为(00) 提供地址的高三十位即可一个周期就可以读取(对齐存储) 
			小端存储 低位数据存在低地址位
			bank1(00)       bank2(01)      bank3(10)        bank4(11)
	![[Pasted image 20241106152034.png]]
	四个32位
	带宽 传输数据的字节数/周期数
	- a = 4 * (1 + 15 + 1)T = 
	- b = (1 + 15 + 1)T
	- c = (1 + 15 + 4)T
flash storage 固态硬盘
均衡写
- 使用次数有限制 与二氧化硅的厚薄有关
- 分类
	1. NOR flash 按bit存储
	2. NAND flash 以块为单位存储 先清空erase再写入数据
Disk storage 机械硬盘
又称为温盘(温切斯特盘)
	依据磁性方向来判断1 or 0
	利用磁性材料存储信息
	磁道track -> 扇区sector
	1.定位磁道  寻道
		假象的小车
	2.旋转
		将想要的扇区转到磁头下方 
	3.读数据(扇区)
	ID是格式化的时候设置
		CRC是校验 
	G (gap)方便磁头对准 
	G之后是有效数据(以文件格式存储)
	校验码用于检测(外设与主机间连接是否有错误 | 数据是否有错误)
	 多个任务(进程)请求数据时要以队列来等待硬盘响应
	时间分配
		0.多个任务(进程)请求数据时要以队列来等待硬盘响应 (不参与运算)
		1. 寻道时间(avergae)
		2. 旋转时间(将**磁盘转半圈**作为平均旋转时间)
		3. 传输数据的时间
		4. 控制延迟时间
CPU与cache联动 求速度
	部分内容映像(内容一模一样)
	cache没有地址
	CPU访存使用的是主存Memory地址
	主存地址分为：(字节地址)
	块号(字地址word address)            |              块内地址
	块内地址在32位系统下用两位二进制数表示(因为有四个字节)
	直接映射 
		cache 中的实际存储位置(cache中的索引index) = 
		Block address(memory的块号) mod Blocks in cache(块数量)
	进而可以继续细分主存地址：
	    tag +(有效位) | index(cache块号) | 块内地址
	冷启动(清理cache)与热启动(不清cache)
	读数据： 将memory主存中的数据以块为单位通过检索(高位地址)传送 到cache 此时流水线阻塞 然后CPU从cache中读取数据
	Hit与Miss
		Hit命中：
			1. value = 1
			2. 内存地址高位与Tag相同
	评测标准：命中率与缺失率
	Hint率越高 CPU的访存数据 
	由空间局部性得 块的大小变大 访问的命中率会提高 cache容量不固定的情况下 
	但是在缺失时花费的时间会提高(阻塞等)
		解决方法 ：
		早启动 所需的块内容来了后就立马往下流水
	cache中的块数减少 映射关系单一 及 太多个主存地址会映射到一个cache index 会导致频繁调度 引起块污染


the extra information
[小科普 | FAT32、NTFS、exFAT？格式化我该选谁？ - 哔哩哔哩 (bilibili.com)](https://www.bilibili.com/opus/134323427145258263)
[硬盘并行串行接口 - 搜索 (bing.com)](https://cn.bing.com/search?pglt=673&q=%E7%A1%AC%E7%9B%98%E5%B9%B6%E8%A1%8C%E4%B8%B2%E8%A1%8C%E6%8E%A5%E5%8F%A3&cvid=89ffeb91fa5d4280829c05dcaf802997&gs_lcrp=EgZjaHJvbWUyBggAEEUYOTIGCAEQABhAMgYIAhAAGEAyBggDEAAYQDIGCAQQABhAMgYIBRAAGEAyBggGEAAYQDIGCAcQABhAMgYICBAAGEDSAQkxMDg4N2owajGoAgiwAgE&FORM=ANNTA1&adppc=EdgeStart&PC=U531)
[PC 热启动和冷启动区别 - 搜索 (bing.com)](https://cn.bing.com/search?q=PC+%E7%83%AD%E5%90%AF%E5%8A%A8%E5%92%8C%E5%86%B7%E5%90%AF%E5%8A%A8%E5%8C%BA%E5%88%AB&qs=n&form=QBRE&sp=-1&lq=0&pq=pc%E7%83%AD%E5%90%AF%E5%8A%A8%E5%92%8C%E5%86%B7%E5%90%AF%E5%8A%A8%E5%8C%BA%E5%88%AB&sc=9-11&sk=&cvid=8C75A2481A914749BF02D40236C4AA54&ghsh=0&ghacc=0&ghpl=)
