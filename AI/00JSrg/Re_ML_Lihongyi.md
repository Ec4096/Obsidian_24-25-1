# 课程名称
**讲师**: [Lihongyi]  
**日期**: [2024-11-19-22:00-->]  
**课程链接**:[DeepLearning](https://www.bilibili.com/video/BV1J94y1f7u5/)

---

## 1. 课程大纲
- **主题1**: [主题描述]
- **主题2**: [主题描述]
- **主题3**: [主题描述]

---

## 2. 课堂笔记


# 0.0 深度学习基本概念
- **Batch**与**Epoch**
	一个Batch计算一遍Loss并更新参数 为一次upgrade
	对所有的Batch处理完 为一次Epoch
	$upgrade times = Epoch \times (data \div Batch Size)$
	![[Pasted image 20240924141250.png]]
-  基本的**Optimization** (优化)
	演算法使用***Gradient Descent***[梯度下降]
	![[Pasted image 20240924135354.png]]
	其中η为个人设置的值,其原理便是不断逼近偏导为0的参数值
- **Hyper Parameter**
	人为设置的 超参数
	example：
		Learning rate 即η
		Batch size

# 0.1 Pytorch Tutorial

## 1 Training Neural Networks 训练流程
- Define Neural Network
- Loss Function
- Optimization Algorithm
- 数据集Dataset
	Training data 训练数据
	Validation data 验证数据
	Testing data 测试数据
## 2.流程的简单代码
- dataset & dataloader & modelDesign
```python
	dataset = MyDataset(file)
	tr_set = DataLoader(dataset,16,shuffle=True)
	model = MyModel().to(device)
	criterion = nn.MSELoss()  #损失函数 
	optimizer = torch.optim.SGD(model.parameters,lr)
	```
- Training Loop
```python
	
	for epoch in range(n_epochs):
	 model.train()
	 for x,y in tr_set():
	 	 optimizer.zero.grad()
	 	 x,y = x.to(device) , y.to(device)
	 	 pred = model(x)
	 	 loss = criterion(pred,y)
	 	 loss.backward()
	 	 optimizer.step()  #updata model with optimizer
	```
- Validation Loop
```python
	model.eval()
	for  x,y in dv_set():
		x,y = x.to(device) , y.to(device)
		with torch.no_grad():
			pred = model(x)
			loss = criter(pred,y)
		total_loss += loss.cpu().item() * len(x)
		avg_loss = total_loss / len(dv_set.dataset)
```
- Eval Loop
```python
	model.eval()
	preds = []
	for x in tt_set;
		x = x.to(device)
		with torch.no_grad():
			pred = model(x)
			preds.append(pred.cpu())
```
- save and load model
```python
	torch.save(model.state_dict(),path)
	###
	ckpt = torch.load(path)
	model.load_state_dict(ckpt)
```

# 1.0 Logistic Regression
- 似然概率
	![[Pasted image 20241125193935.png]]
	上下两处的L均是Likelihood的意思
- 最大似然概率
	![[Pasted image 20241125194632.png]]
- Classification
	![[Pasted image 20241125194850.png]]
- Logistic Regression & Linear Regression
	![[Pasted image 20241125200936.png]]
	PS：对于cross entropy解释
		![[Pasted image 20241125201401.png]]
		当PQ的class一样时，其值达到最小
	PS：此处的C为Cross Entropy，L为 Loss
		![[Pasted image 20241125201041.png]]
- Cross Entropy & Square Error
	黑色为Cross Entropy
	红色为Square Error
	![[Pasted image 20241125192047.png]]

	Cross Entropy 在距离目标点 梯度变化很大 容易到达目标点
	Square Error 在距离目标点 梯度变化小 容易卡住 不能到达目标点

# 2.0 机器学习任务攻略 2/25 -1
- General Guide
	![[Pasted image 20240925174933.png]]
	只有**Train Loss小**而**Test Loss大** 为overfitting
- 解决思路
	Q: 如果深层的model虽然比浅层的model弹性大,但是Loss却不能比浅层的model压得更低
	A:说明optimizer出现问题
## 1 Overfitting
- **Train Loss小**而**Test Loss大** 
	往往出现在model弹性较大
- How to resolve The Overfitting
	- Increase the DataSet
	- Data augmentation(数据增大)
		对于数据进行合理的(rationally)处理来增大DataSet
		example：
			for image we can flip left and right or zoom in
			and out(放大缩小)
	- constrain(约束) The model
		Less parameters or sharing parameters
# 2.1 Optimization 2/25 -[2,5]
## 1. local minima And saddle point
* Gradient is close to zero named **critical point**
	![[Pasted image 20241125210502.png]]
	Local minima is that there is **no way to go**
	saddle point is that there is **a way to escape**
- Taylor Series Approximation
	![[Pasted image 20240926203846.png]]
	g是一次求导的结果(vector向量)
	H是二次求导的结果(matrix矩阵)
	when the Gradient == 0,the second formula == 0
	so the judge depending on the third formula
		![[Pasted image 20240926204441.png]]
- Gradient Descent + Momentum(动量)可以帮助跳过critical point
	就像物理世界中一个滚石从山坡上滚下来 由于动量 会跳过凹坑
	It's like in the physical world, a rolling stone rolls down a hillside and ends up jumping over a crater because of momentum
	![[Pasted image 20240927201530.png]]
		$\lambda 和 \eta 都是hyper parameters$
## 2. Learning Rate
- 在陡峭的地方η减小
- 在平缓的地方η增大 
- Adagrad - Root Mean Square
	![[Pasted image 20240927160352.png]]
	可以累计之前的梯度g 达成想要的效果
- RMS Prop
	![[Pasted image 20241125215244.png]]
	可以 参考上一次计算得出的梯度来达成想要的效果
- Adam (in pytorch)
	![[Pasted image 20240927193639.png]]
- Learning Rate Scheduling(学习率调度)
	Learning Rate η 是随着时间变化而变化的
	![[Pasted image 20240927194750.png]]
- summary of optimization
	![[Pasted image 20240927200039.png]]
	Vanilla meaning 纯的最初的
	Momutum 是对于过去的梯度g相加 有考虑方向和大小
	RMS 只考虑过去梯度g 的大小
## 3. Loss Function
- Cross Entropy & Mean Square Error
	Cross Entropy在Loss比较大的时候可以比较顺畅快速的减小
	Mean Square Error会卡住
- 所以Loss的选择也会影响到optimization
- soft-max
	![[Pasted image 20240927203347.png]]
	When dealing with binary classification problems
	The softmax == sigmoid
- 在pytorch 中 调用 Cross Entropy时会自动添加一个softmax层
## 4.BatchNormalization
- 批量标准化
	![[Pasted image 20241127183708.png]]
	input vector中的每个元素 对于output vector都会产生影响

# 3.0 CNN(Convolutional Neural Networks) 3/04 image as input
## 1. Convolution
 - Convolution中的参数介绍
	 ![[Pasted image 20240929163505.png]]
	 stride步长 决定每次移动的步长
	 padding填充 可以被用来保持图片大小不变
	kernel size 核的大小 又可以被称为fillter？
- different Interpretive version(解释版本)
	![[Pasted image 20240929164345.png]]
- Whole CNN
	![[Pasted image 20240929164831.png]]
## 2. Pooling
- 最初被设计用来减少运算量 现在可以全使用Convolution
	![[Pasted image 20240929164636.png]]
	The pooling likes an actively function


# Inserting self-attention
- Vector set as input
	将向量作为输入
	主要有两种形式
	![[Pasted image 20241122204219.png]]
- input N个vector 经过model后 output N个vector
	例如 对于 一句话"I saw a saw" 辨别每个词的词性
- input N个vector 经过model后 output 一个label
	example：
	- 判断一句话(评价)是正面的还是负面的
	- 判读一段语音的主人
	- 判断一个有机聚合物是否有毒性
- input N个vector 经过model后 output N'个label
	即seq2seq 
	不知道要输出多少个
	例如真正的语音辨识 输入一段语音 返回一段文字
- sequence labeling
	比如判断一个句子中的词语的词性
	我们要考虑上下文的内容
	而move window 长度可能会对性能产生很大的影响
	所以我们使用self-attention
- self-attention 的realize
	tip：
		我们可以将多次经过self-attention处理后的数据再fully connected
	1. 首先计算向量之间(两个两个的计算)的关联性 为α(attention score)
		![[Pasted image 20241122205653.png]]
		左侧的是transformer中的方法也是最常用的方法
		![[Pasted image 20241122205935.png]]
	2. 计算b
		![[Pasted image 20241122210059.png]]
		哪个向量vector的α越大 最后得出的b就越接近哪个的V
	![[Pasted image 20241122210438.png]]
	I为input vector
	O为output
	**其中唯一的hyper parameter 是 Wq Wk Wv**
- advanced Multi-head-Self-attention
	![[Pasted image 20241122210715.png]]
	最后通过Wo 来整合Multi-head-b
	![[Pasted image 20241122210742.png]]
- 未知的资讯 例如位置资讯
	如果做self-attention时需要关注位置的咨询
	positional Encoding
	
- self-attention与CNN的关系
	CNN是有限制的self-attention
	CNN弹性比较小 所以所需数据量比较少
	self-attention 弹性比较大 所以所需的数据量比较大 才会有好一些的accuracy
	
- self-attention与RNN(Recurrent Neural Network)
	RNN可以单向也可以双向 
	在双向的情况下 每一个ouput也考虑了每一个input
	![[Pasted image 20241122212119.png]]
	selfAttention 可以平行处理所有的input
	而RNN对于并行处理方面略有缺陷


# 4.0 Transformer 3/18 Sequence2Sequence 
- 与上一节self attention关联性很强
-  专业词汇
	token 字符
	one-Hot vector其中一个是1 其他都是0
	error propagation(错误增殖) 一步错步步错
	query 查询向量
	key 键向量
	value 值向量
## 1. Encoder
- 整体架构 
	![[Pasted image 20241127201947.png]]
	![[Pasted image 20241127202044.png]]
	PS:这个设计不一定是最好的 layer Normalization的位置可以更改
- residual connection
	就是将处理过后的vector 再加上input 本身的vector
	被称为残差连接
- normalization
	Batch normalization
		不同的example 不同的feature 的同一个dimension 做normalization(计算mean & standard deviation)
	layer normalization
		同一个example 同一个feature 的不同的dimension做normalization(计算 mean & standard deviation)
## 2. Decoder
- Autoregressive (AT)


- output
	![[Pasted image 20241127212025.png]]
	每个output都是由output vector 在linear 和 softmax之后概率最大的结果
	此外，第一个的输出作为下一个的输入传入Decoder当中
- masked self-attention
	![[Pasted image 20241127212315.png]]
	每一个输出至于序列上之前的输入有关
	![[Pasted image 20241127212440.png]]
	此处计算b2的时候a3和a4的输入还没产生
	因为encoder 的输出是一个一个产生的
- AT & NAT
	![[Pasted image 20241127212720.png]]
	AT是先给出begin信号之后一个一个的输出
	而NAT是全部都给begin信号 结果一起输出
- Encoder 与 Decoder的连接 **Cross attention**
	![[Pasted image 20241127212834.png]]
	内部结构是下图这样的
	![[Pasted image 20241127212938.png]]
	query来自Masked self-attention
	key 与value来自 Encoder的输出
## 3. Training
- process
	![[Pasted image 20241127213236.png]]
	在每做一次文字输出的时候 相当于是一个分类的过程 并且将其与Ground truth 对比 进行计算其cross entropy
- Tearch forcing 
	![[Pasted image 20241127213410.png]]
	在decoder输入的时候会将正确答案作为输入
	那么会导致mismatch问题
		可以在训练的时候添加exposure bias
		即 输入正确答案时(加一些错误答案)
		**Scheduled Sampling** - 训练时加一些不相干的事情
- Training Tips
- Copy Mechanism
- Guided Attention
- Beam Search
- RL(reinforcement Learning) BLEU


# 5.0 GAN 3/25 Genaration
- Generative Adversarial Network

- generator
- discriminator
- distribution
- divergence


- **详细内容**:
  - [详细内容1]
  - [详细内容2]
- **例子**:
  - [例子1]
  - [例子2]


---

## 3. 讨论与思考
- **问题**:
  - [问题1]
  - [问题2]
- **个人见解**:
  - [见解1]
  - [见解2]

---

## 4. 作业与任务
- **作业1**: [作业描述]
  - **截止日期**: [截止日期]
  - **要求**: [作业要求]
  
- **作业2**: [作业描述]
  - **截止日期**: [截止日期]
  - **要求**: [作业要求]

---

## 5. 复习要点
- **复习主题1**: [复习内容]
- **复习主题2**: [复习内容]
- **复习主题3**: [复习内容]

---

## 6. 额外资源
- **书籍**:
  - [书名] - [作者]
- **网站**:
  - [网站名称] - [链接]
- **视频**:
  - [视频标题] - [链接]

---

## 7. 反馈与建议
- **课堂反馈**: [你的反馈]
- **改进建议**: [你的建议]

