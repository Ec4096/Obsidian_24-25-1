AiIntro
2024_12_17_Tue
19:27

# 课程名称
**讲师**: [讲师姓名]  
**日期**: [课程日期]  
**学期**: [学期]  
**课程代码**: [课程代码]  

---

## 1. 课程大纲
- **主题1**: [主题描述]
- **主题2**: [主题描述]
- **主题3**: [主题描述]

---

## 2. 课堂笔记

### 2.1 主题1
- **关键点**:
  - [关键点1]
  - [关键点2]
- **详细内容**:
  - [详细内容1]
  - [详细内容2]
- **例子**:
  - [例子1]
  - [例子2]

### 2.2 主题2
- **关键点**:
  - [关键点1]
  - [关键点2]
- **详细内容**:
  - [详细内容1]
  - [详细内容2]
- **例子**:
  - [例子1]
  - [例子2]

### 2.3 主题3
- **关键点**:
  - [关键点1]
  - [关键点2]
- **详细内容**:
  - [详细内容1]
  - [详细内容2]
- **例子**:
  - [例子1]
  - [例子2]

---

# 00. Extra
## 00.1 CNN卷积神经网络中的stride,padding,filter,channel
- stride-步幅
	卷积时的采样间隔
	- 目的
		减少输入参数的数量
- filter-过滤器
	filter本质上是卷积核kernel的集合
	总卷积和的数量 = $C_{in} \times C_{out}$ 
- padding-填充
	在输入特征图的边缘添加一定数目的像素
	- 目的
		1.如果没有padding,每次卷积之后图像的尺寸会越来越小,就**不足以支撑**足够多的 深度神经网络
		2.每个输入特征图的每一块都可以作为卷积窗口的中心,防止丢失边缘信息
	为了使输入输出图尺寸一致那么padding的计算公式为:
	$padding = (sizeFilter - 1)/2$
	所以size_filter通常为奇数
- channel通道数
	当 channel == 1时 filter可以被当作卷积核
	但一般情况下 有几个channel,then filter中就有多少个卷积核
- 计算输出特征图feature maps的大小
	- 设输入特征图为$a\times a$ | 卷积核为$b\times b$ | 步幅stride = c | 填充padding = d
	then size_output = $\frac{a+2d-b}{c} + 1$
- CSDN链接
	[CNN卷积神经网络中的stride、padding、channel以及特征图尺寸的计算_卷积神经网络stide-CSDN博客](https://blog.csdn.net/m0_54487331/article/details/112846015)
## 00.2 感受野
[什么是感受野？_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1tg411b7fn/?vd_source=049db469aa498a15142dda9a2e1eab05)

## 00.3 残差块

[经典网络结构 (五)：ResNet (残差网络)_residual block-CSDN博客](https://blog.csdn.net/weixin_42437114/article/details/106996019)

## 00.4 正则化regularization
[一篇文章完全搞懂正则化（Regularization）-CSDN博客](https://blog.csdn.net/weixin_41960890/article/details/104891561)

## 00.5 上采样，下采样，卷积，池化
[【深度学习基本概念】上采样、下采样、卷积、池化_上采样层-CSDN博客](https://blog.csdn.net/tingzhiyi/article/details/114368433)

## 00.6 目标检测评估指标
[目标检测---IOU计算详细解读(IoU、GIoU、DIoU、CIoU、EIOU、Focal-EIOU、SIOU、WIOU)_iou目标检测-CSDN博客](https://blog.csdn.net/J_oshua/article/details/136744345)

## 00.7 多头感知机multi-head-attention
[Multi-Head-Attention的作用到底是什么 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/626820422)

## 00.8 空洞卷积atrous convolution
[总结-空洞卷积(Dilated/Atrous Convolution) - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/50369448)

## 00.9 Dropout
[深度学习中Dropout的作用和原理_bayesian cnn dropout-CSDN博客](https://blog.csdn.net/shanshangyouzhiyangM/article/details/84848908)

## 00.10 K聚类
[聚类分析算法——K-means聚类 详解-CSDN博客](https://blog.csdn.net/goTsHgo/article/details/143231544)

