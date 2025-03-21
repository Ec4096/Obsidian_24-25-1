**逻辑基础部分 chapter[1&2]**
# 命题-命题符号化
## 1. 命题-命题符号化-命题
- 命题的定义
	有**唯一真假值**的**陈述句**
## 2. 命题-命题符号化-符号化
- 命题的真值:
	每个具体命题的唯一真假值
- 原子
	不能再分的最小命题单元
	- 命题常元 
		真命题T 与 假命题F
	- 命题变元
		常用大写字母表示**PQR** 表示真命题和假命题
- 联结词 与 复合命题
	常用的逻辑联结词
		![[Pasted image 20241203130749.png]]
	**复合命题**是由**原子与联结词组成**
	复合命题也**具有真值**
		![[Pasted image 20241203130917.png]]
	Example:
		A 仅当 B 符号化得 A -> B
			B成立时A可以发生也可以不发生 即可以有 01与11
			而B不成立时 A一定不成立 即不能出现10 只能出现00
			所以翻译为 A -> B
## 3. 命题-命题符号化-合式公式
- 合式公式(Well-Formed Formulas **WFFs**)
	合式公式是命题符号化的结果 是命题”语言“中的”句子“
	- **Definition**
		1.Recursion **Base**
			命题常元和命题变元是WFFs
		2.Rule
			若A,B是WFFs 则 $(\lnot A),(A\lor B),(A\land B),(A\to B),(A\leftrightarrow B)$是WFFs
		3.极小性条款
			有以上规则在有限步生成的都是WFFs
- 合式公式的简化规则
	符号的优先级**由高到低**为:
	$\lnot ,\land ,\lor ,\to ,\leftrightarrow$
	**同等级**的一二元运算符号**从左到右进行结合**
		 ![[Pasted image 20241203133612.png]]
## 4. 命题-命题符号化-合式公式的语义
- 合式公式的记号
	记含n个原子P1,P2,...,Pn的公式G为: G(P1,P2,...,Pn)
- 指派assignment
	对G(P1,P2,...,Pn)中的原子Pn取值(0或1) 一共有$2^{n}$种不同的指派
	记为:I = x1x2,...,xn
- 解释Interpretation
	公式G在指派I下的值记为I(G)
	其递归定义为:
		I(T) = 1 , I(F) = 0
		I(G) = $x_{i}$ , if G=$P_{i}$
# 命题-永真公式
## 1. 命题-永真公式-公式的分类
- 永真式(重言式)
	对于G的任一个解释I 都有I(G) = 1
- 可满足式
	对于G存在一个解释I 有I(G) = 1
- 矛盾式
	对于G的任一个解释I 都有I(G) = 0
## 2. 命题-永真公式-逻辑等价
- 如果公式$(F)\leftrightarrow (G)$是重言式 那么F和G逻辑等价 记为$F\Leftrightarrow G$
	![[Pasted image 20241203200246.png]]
	PS:数电之前学过了,可以跳过
## 3. 命题-永真公式-永真蕴含关系
- $(F)\rightarrow (G)$是重言式 记为: $F\Rightarrow G$
	PS,对于F和G中原子的所有指派I,都有$I(F)\leq I(G)$ 即F为真时G一定为真
	![[Pasted image 20241203200825.png]]
## 4. 命题-永真公式-代入与替换规则
- 代入规则
	设G(P1,P2,...,Pn)是一个公式,F是另一公式, 设**Pi是G中的某一原子**,将G中出现的每个Pi都用F来替代 被称为**代入**
	G(P1,...,F/Pi,...,Pn)称为原公式的**代入实例**
	PS:形象地来看 F替换了Pi原来的位置 (骑在头上)
- 替换规则
	设G是一公式,A是在G的某个位置出现的子公式,将该**子公式A用公式B置换** 被称为**替换**
	PS:$A\Leftrightarrow B$ 则$G\Leftrightarrow G'$ 即替换规则
## 4. 命题-永真公式-对偶性
- 对偶公式
	假设G是一个仅含$\lnot ,\land ,\lor$ 运算符号的公式;
	G的对偶式就是将符号按照下表进行替换
	PS:要保持原有的运算关系(即先后顺序)

| 原符号     |  替换后的符号 |     |
| :------ | ------: | --- |
| $\land$ |  $\lor$ |     |
| $\lor$  | $\land$ |     |
| **T**   |   **F** |     |
| **F**   |   **T** |     |
- 广义DeMorgan定理
	![[Pasted image 20241204191446.png]]
	相关推论:
	- $F\Leftrightarrow G$ iff $F^{*}\Leftrightarrow G^{*}$
	- $F\Rightarrow G$ iff $G^{*}\Rightarrow F^{*}$ 
# 命题-范式
## 1. 命题-范式-析取范式与合取范式
- 基本积与基本和
	- 基本积
		合式公式中的变元或变元否定的**合取$\land$
	- 基本和
		合式公式中的变元或变元否定的**析取$\lor$
- 析取范式与合取范式
	- 析取范式
		$A\Leftrightarrow A1\lor A2\lor ...\lor An$ (其中Ai是基本积)
	- 合取范式
		$A\Leftrightarrow A1\land A2\land ...\land An$ (其中Ai是基本和)
- 极小项与极大项
	- 极小项
		n个命题变元P1,P2,...,Pn的**基本积**,每个变元都要出现
		example:
			$\lnot P \land\lnot Q \land\lnot  R$ 标记为$m_{0}$ 全0才能为T
	- 极大项
		n个命题变元P1,P2,...,Pn的基本和,每个变元都要出现
		$\lnot P \lor\lnot Q \lor\lnot  R$ 标记为$M_{7}$ 全1才能为F
- 主析取范式
	- 定义
		与A逻辑等价,且有**极小项之和**组成
		主析取范式**唯一**
	- 数量关系
		已知对于**n个变元**,有$2^{n}$ 个**极小项**
		对于任一个主析取范式,可能含有某个极小项或者不含一共两种情况
		所以一共可以构造$2^{2^{n}}$ 个**主析取范式**
	- 主析取范式与主合取范式的转化
		$\sum m(1,3,5,6,7) \Leftrightarrow \prod M(0,2,4)$
# 命题-联结词的补充与归约
## 1. 命题-联结词的补充与归约-联结词的补充
- **n**个原子的公式共有$2^{2^{n}}$个不同的运算
	理解:每个主析取范式对应一种运算
	所以一元运算有4个:恒为1,恒为0,恒等,否定
	二元运算有16个
- 二元运算的补充
	- 与非(NAND)
		表示$P\uparrow Q \Leftrightarrow \lnot ( P\land Q)$
	- 或非(NOR)
		表示$P\downarrow Q \Leftrightarrow \lnot ( P\lor Q)$
	- 异或(XOR)
		表示$P\bigoplus Q \Leftrightarrow ( P\land \lnot Q) \lor (\lnot P\land Q)$
## 2. 命题-联结词的补充与归约-联结词的归约
- 全功能集
	所有运算均能用该集合中的联结词表示(主要是$\lnot , \land , \lor$)的组合
	- 极小全功能集
		在该集合中删除任意一个联结词后不再是全功能的
	- Example
		$\lnot , \land$ 和 $\lnot , \lor$ 是极小全功能集
		$\lnot ,\rightarrow$  是极小全功能集
		$\lnot ,\leftrightarrow$ 不是全功能的
		$\land ,\lor$ 不是全功能的
		$\downarrow$ 是全功能的,同理$\uparrow$ 也是全功能的
# 命题-推理和证明方法
## 1. 命题-推理和证明方法-有效结论
- 定义
	![[Pasted image 20241204204153.png]]
- 证明有效结论的方法
	- 恒等和不等变换
	- 真值表
	- 设结论为假,证明条件也是假
	- 设前提为真,证明结论也是真
	- **证明序列** (常用)
- 证明序列的使用
	- P(Premise) 引入前提
	- T(Tautolog) 恒等变换
	- MP(modus ponens) P,$P \rightarrow Q$ 那么Q
- 常用的推理规则
	![[Pasted image 20241204210315.png]]
	![[Pasted image 20241204210339.png]]
- 常用方法
	- CP规则-附加前提法
		要证明![[Pasted image 20241204211520.png]]
		那么先假设A成立
	- 反证法
		假设结论是假,即:
		![[Pasted image 20241204211733.png]]
	以及
	![[Pasted image 20241204211822.png]]
- 证明策略
	- 穷举法
	- 分情况证明
	- 正向推理,逆向推理

---


# 谓词-谓词与量词
## 1. 谓词-谓词与量词-基本概念
- 谓词公式形式系统中使用的符号
	- 常数符号(constant):3,John,Mary...
	- 变量符号(variables):x,y,z
	- 函数符号(functions):plus,father,f,g
	- 谓词符号(predicate):MAN,GREATER,LOVE,P,Q,R
	- 各种符号应和具体形式化对象相关
- 参数问题
	对于函数function,f的参数个数为n，那么称为n元函数
	对于谓词符号,P的参数是n，那么称为n元谓词
- 项的定义
	通过**常数,变元**和**函数符号**组成的对象
	- 递归定义
		1. 常数符号，变量符号是项
		2. if f是n元函数,t1,t1,...,tn是已经定义的项，则f(t1,t1,...,tn)也是项
		3. 所有的项都是由以上规则在有限步生成
- 谓词的定义
	- 递归定义
		if P是n元谓词,t1,t1,...,tn是已经定义的项，则P(t1,t1,...,tn)是原子
	- **谓词原子**和命题一样有**唯一的真假值**
	- **谓词公式**就是将命题公式中的原子换为**谓词原子**
		- 即谓词公式满足命题公式的所有运算规律
- 量词
	- 全称量词(Universal) $\forall$
	- 特称量词(Existential) $\exists$
- 符号的优先级**由高到低**为:
	$\forall ,\exists ,\lnot ,\land ,\lor ,\to ,\leftrightarrow$
- 符号化谓词公式-**唯一性**的表达
	Everyone has exactly one best friend
	B(x,y)：y是x最好的朋友
	Ineuqal(x,y)：x和y是不同的对象
	唯一性：“y是唯一的”$\Leftrightarrow$ “对任意的z，如果z不等于y，则x和z不是最好的朋友”
	$\forall x\exists y(B(x,y) \land \forall z(Ineauql(z,y)\rightarrow \lnot B(x,z)))$
	用数学符号$\exists!$表示唯一 即上式可以表示为
	$\forall x\exists! yB(x,y)$
- 辖域(Scope)&约束变量&自由变量
	$\forall x\exists yP(x,y) \land Q(x)$ 
	Scope of $\forall x$ is $\exists yP(x,y)$
	Scope of $\exists y$ is $P(x,y)$
	P中的x和y是约束变量
	Q中的x是自由变量
## 1. 谓词-谓词与量词-谓词的语义
- 论域D(Domain0
	某个问题中可能出现的所有问题
- 谓词公式F的取值
	设F=P(t1,t2,...,tn),设$t_{n}|_{I}$ 是$t_{n}$在I下的取值
	$F|_{I}=P|_{I}(t_{1},t_{2},...,t_{n})$
	- 对于含有自由变量的公式,只有在每个自由变量都取定D中的值时公式才有解释
	- 对于全称量词 只有P在**所有**解释下都成立 $(\forall xP(x))|_{I}$ 才等于1
	- 对于存在量词 只有P在**某个**解释下成立 $(\exists xP(x))|_{I}$ 才等于1
- Examples
	- ![[Pasted image 20241206204736.png]]
	- ![[Pasted image 20241206204748.png]]

# 谓词-公式间的关系式
## 1. 谓词-公式间的关系式-逻辑等价和永真蕴含关系
- 谓词公式的形态
	- G是可满足的(相容的) iff 存在解释I,使得$G|_{I}$ = 1,称I是G的模型
	- G是永假的(矛盾的) iff 对于所有解释I,$G|_{I}$ = 0
	- G是永真的 iff 对于所有的解释I,$G|_{I}$ = 1
- 逻辑等价
	$F\Leftrightarrow G$ iff $F\leftrightarrow G$是永真的
- 永真蕴含
	$F\Rightarrow G$ iff $F\rightarrow G$是永真的
- others
	- 所有有关恒等式和不等式的结构 谓词公式与命题公式通用
		- Example
			代入规则和替换规则
			恒等式的自反性,对称性和传递性,不等式的传递性
	- 而谓词公式特有的性质是关于量词的逻辑规律
## 2. 谓词-公式间的关系式-量词的逻辑关系
- 更名规则
	设F(x)表示含自由变量x的公式,设变量**y不出现在F(x)** 中 则：
	$\forall x F(x) \Leftrightarrow \forall y F(y)$
	$\exists x F(x) \Leftrightarrow \exists y F(y)$
- 量词的解消
	设F是**不含自由变量x**的公式 则：
	$\forall x F \Leftrightarrow F$
	$\exists x F \Leftrightarrow F$
	- 辖域的扩张
		$(\forall x F(x)) \land G \Leftrightarrow \forall x (F(x) \land G)$
		$(\forall x F(x)) \lor G \Leftrightarrow \forall x (F(x) \lor G)$
		$(\exists x F(x)) \land G \Leftrightarrow \exists x (F(x) \land G)$
		$(\exists x F(x)) \lor G \Leftrightarrow \exists x (F(x) \lor G)$
- 特例与量词的关系 (小范围到大范围)
	$\forall x F(x) \Rightarrow F(x)$ ????????????????????????????
	$F(a) \Rightarrow \exists x F(x)$
- 量词的否定
	$\lnot (\forall x F(x)) \Leftrightarrow \exists x \lnot F(x)$
	$\lnot (\exists x F(x)) \Leftrightarrow \forall x \lnot F(x)$
- 量词的分配形式
	设F(x)和G(x)是两谓词公式 则：
	$\forall x(F(x) \land G(x)) \Leftrightarrow \forall x F(x) \land \forall x G(x)$
	$\exists x(F(x) \lor G(x)) \Leftrightarrow \exists x F(x) \lor \exists x G(x)$
	$\forall x F(x) \lor \forall x G(x) \Rightarrow \forall x (F(x) \lor G(x))$ (大并 变 小并)
	$\exists x (F(x) \land G(x)) \Rightarrow \exists x F(x) \land \exists x G(x)$ (小交 变 大交)
- 多个量词的处理
	
	！！
	$\forall x \forall y F(x,y) \Leftrightarrow \forall y \forall x F(x,y)$
	$\exists x \exists y F(x,y) \Leftrightarrow \exists y \exists x F(x,y)$
	$\forall x \forall y F(x,y) \Rightarrow \exists y \forall x F(x,y) \Rightarrow \forall x \exists y F(x,y) \Rightarrow \exists y \exists x F(x,y)$ 
	
	
- 对偶原理
	假设G是一个仅含$\forall ,\exists ,\lnot ,\land ,\lor$ 运算符号的公式;
	G的对偶式就是将符号按照下表进行替换
	PS:要保持原有的运算关系(即先后顺序)
	- $F\Leftrightarrow G$ iff $F^{*}\Leftrightarrow G^{*}$
	- $F\Rightarrow G$ iff $G^{*}\Rightarrow F^{*}$ 

| 原符号       |    替换后的符号 |     |
| :-------- | --------: | --- |
| $\land$   |    $\lor$ |     |
| $\lor$    |   $\land$ |     |
| **T**     |     **F** |     |
| **F**     |     **T** |     |
| $\forall$ | $\exists$ |     |
| $\exists$ | $\forall$ |     |
- 前述范式
	简单来讲就是将**量词全部提前**
	求解步骤：
		1 消除$\rightarrow ,\leftrightarrow$
		2 用 De Morgan率消除对于非原子子公式的否定
		3 对约束变量 重新命名(换一个字母)
		4 量词的外提(吸收，扩张，分配)
	

# 谓词-谓词公式的自然推理
## 1. 谓词-谓词公式的自然推理-相关概念的复习
- 相关记号
	F(x1,x2,...,xn)表示x1,x2,...,xn在公式F中自由出现
## 2. 谓词-谓词公式的自然推理-量词的推理规则
！！
- 指定规则 **S(Specification)**
	- 全称指定规则 **US(Universal Specification)**
		$\forall x F(x) \Rightarrow F(y)$ 
			y一定不是公式F中出现的约束变量
			![[Pasted image 20241210133527.png]]
		$\forall x F(x) \Rightarrow F(c)$
			c为一个变元常量
		PS：
			US规则中的量词的**辖域是整个公式**
			![[Pasted image 20241210133811.png]]
		正确的使用
		![[Pasted image 20241210134015.png]]
	- 特称指定规则 **ES(Existential Specification)**
		$\exists x F(x) \Rightarrow F(c)$
			c是**新引入**的常元符号
		PS：
			ES规则中的量词的**辖域是整个公式**
			![[Pasted image 20241210134125.png]]
		正确的使用 
		![[Pasted image 20241210134140.png]]
- 推广规则 **G(Generalization)**
	- 特称推广规则 **EG(Existential Generalizatioin)**
		$F(c) \Rightarrow \exists y F(y)$
		$F(x) \Rightarrow \exists y F(y)$
			y是新引入的变元符号 
			c是新引入的常元符号
		PS：
			![[Pasted image 20241210134802.png]]
		正确的使用
		![[Pasted image 20241210134825.png]]
	- 全称推广规则 **UG(Universal Generalization)**
		$F(x) \Rightarrow \forall x F(x)$
			x不在F中约束出现
		PS：
			![[Pasted image 20241210135033.png]]
		正确的使用
		![[Pasted image 20241210135050.png]]
- 总结
	指定规则(US,ES)和推广规则(UG,EG)在**添加量词或者去量词**的时候要注意两点：
	1 注意**辖域是整个式子**
	2 注意有**变动的变元不能**在式子中**约束出现**
- Example
	- 
		![[Pasted image 20241210135608.png]]
		![[Pasted image 20241210135623.png]]
	- 
		![[Pasted image 20241210135758.png]]
		![[Pasted image 20241210135817.png]]
- 解题Tips
	一般先特殊后一般 即先ES 后 US
- 解题步骤
	- 选择证明方法：直接证明，间接证明(CP规则，反证法)
	- ES + US 去掉量词
	- 处理好特殊和一般的关系
	- 等同写命题公式的证明序列
	- UG和EG加量词(if 结论中有量词)，注意：避免上上上上图一样US + ES 后再UG



	

