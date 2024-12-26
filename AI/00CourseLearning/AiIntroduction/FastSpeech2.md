# 难点分析
- 总述,
	本次实验可以按照README.md中的流程走
	一下是我对于本次实验完成过程的记录
## 1. 环境的搭建
- python版本
	我使用的python版本为3.8.20,因为习惯使用3.8或者3.7的版本。
- pytorch的相关包版本如下
	torch == 2.4.1+cu118
	torchaudio == 2.4.1+cu118
	torchvision == 0.19.1+cu118
## 2. 数据的下载
- 下载aishell3并解压打开
## 3. 数据预处理
- 首先对齐语料库
	```python
	python3 prepare_align.py config/AISHELL3/preprocess.yaml
```
-  其次进行数据预处理，进行数据从原始语音数据和文本数据到模型训练所需格式转换
	```python
	python3 preprocess.py config/LJSpeech/preprocess.yaml
```
## 4. 训练
- 接下来就是对于模型的训练
	```python
	python3 train.py -p config/LJSpeech/preprocess.yaml -m config/LJSpeech/model.yaml -t config/LJSpeech/train.yaml
	```
	我大概训练了38h左右
	跑完了90k的数据
## 5. 可视化&评估
- 使用Tensorboard来查看
	```python
	tensorboard --logdir output/log/LJSpeech
```

