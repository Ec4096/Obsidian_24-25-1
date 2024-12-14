
CPU与cache联动 求速度
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
