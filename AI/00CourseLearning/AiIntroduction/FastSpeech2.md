# 难点分析
- 总述,
	本次实验可以按照README.md中的流程走
	以下下是我对于本次实验完成过程的记录
## 1. 环境的搭建
- python版本
	我使用的python版本为3.8.20,因为习惯使用3.8或者3.7的版本。
- pytorch的相关包版本如下
	torch == 2.4.1+cu118
	torchaudio == 2.4.1+cu118
	torchvision == 0.19.1+cu118
## 2. 数据&预训练模型的下载
- 下载aishell3并解压打开
- 下载AISHELL3_600000.pth.tar

## 3. 数据预处理
- 首先对齐语料库
```python
python3 prepare_align.py config/AISHELL3/preprocess.yaml
```
-  其次进行数据预处理，进行数据从原始语音数据和文本数据到模型训练所需格式转换
```python
python3 preprocess.py config/AISHELL3/preprocess.yaml
```
- PS
	在上述过程之前要核对检查，config文件夹下所有文件中有关路径的设置
	包括corpus_path，lexicon_path，raw_path，preprocessed_path等

## 4. 训练
- 接下来就是对于模型的训练
```python
python3 train.py -p config/AISHELL3/preprocess.yaml -m config/AISHELL3/model.yaml -t config/AISHELL3/train.yaml
```
- 
	我大概训练了38h左右
	跑完了90k的数据

## 5. 可视化&评估
- 使用Tensorboard来查看
```python
tensorboard --logdir output/log
```
- Loss曲线![[Pasted image 20241231234056.png]]
- Mel频谱![[Pasted image 20241231234127.png]]





## 6. 生成音频
- 首先要设置ID以及对应音频的文本
```python
python3 synthesize.py --text "大家好" --speaker_id SPEAKER_ID --restore_step 600000 --mode single -p config/AISHELL3/preprocess.yaml -m config/AISHELL3/model.yaml -t config/AISHELL3/train.yaml
```

# 相关收获
## 1. 对于.pth.tar文件的了解
之前对于模型的训练最后保存结果的后缀大多都为.pth
刚开始下载这个预训练模型时以为还需要解压来使用，发现解压后多出来很多文件
后面经过查找资料学习的过程明白了.pth.tar与.pth保存的区别
大体的来讲 .pth.tar可以包含更多的信息，比如 模型参数、优化器状态、训练轮数等，适合于训练中断后恢复的场景。相当于时.pth的升级版

## 2. 对于语音生成工程的实现流程的了解
学习到了语音生成与图像处理的区别
首先是数据类型
- 语音生成
	在本项目中语音生成输入数据为文本数据，这些文本数据需要经过规范化、分词等预处理步骤，转化为模型可以处理的格式。
	而输出数据是 Mel 频谱图&音频信号
- 图像处理
	输入数据就是矩阵只不过可能维度有差别
	输出数据可能是图像本身也可能是标签

其次对于数据的处理也大有不同
- 语音生成
	要将文本转化为音素，还要对文本进行分割
	原始音频也要转换为Mel频谱
	以及声码器的处理，训练过程中要将Mel频谱转换为原始波形
- 图像处理
	图像中的每个像素值要进行归一化处理
	尺寸可能不统一要将图像裁剪或者放缩让他尺寸统一
	有时候还要通过翻转，加噪声来增强数据

最后是特征提取
- 语音生成
	提取的是Mel频谱中的频谱信息
- 图像处理
	提取纹理边缘特征
- 当然以上是我个人的一些拙见，如果有理解有误的地方还请麻烦老师与助教指正。