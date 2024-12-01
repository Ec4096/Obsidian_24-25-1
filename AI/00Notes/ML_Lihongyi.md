ML_Lihongyi
2024_09_24_Tue
13:06






























# 2/18

## 1.0 Hyper Parameter <人为设置的> 超级参数
learning rate 即 η
sigmoid
Batch size

## 2.0 Function
### 1. LInear 即线性的函数
	
### 2. sigmoid 

![[Pasted image 20240924131207.png]]
**matrix notation**
![[Pasted image 20240924133224.png]]

### 3. ReLu
![[Pasted image 20240924142509.png]]





















# 4/22

## 1.0 Auto Encoder
### 1. Self-supervised Learning Framework
![[Pasted image 20240930121114.png]]

### 2. Auto-encoder for image
![[Pasted image 20240930121238.png]]
![[Pasted image 20240930121335.png]]
when the change is limit 
so we can decrease the type of changing

### 3. Add-noising (or mask)
![[Pasted image 20240930121430.png]]
![[Pasted image 20240930121505.png]]
The BERT frame can be seemed as the De-noising auto-encoder
### 4. Feature Disentanglement
![[Pasted image 20240930124612.png]]
Representation includes informatio of different aspects 
Making complexity undo(解开)
### 5. DisCrete Latent Representation
![[Pasted image 20240930125155.png]]

### 6. Anomaly(outlier \ novelty \ exceptions)  Detection
content1.2



# 4/29

## 1.0 Explainable Machine Learning
### 1. Explainable And Interpretable
Explainable ：本来是黑箱 人为想办法赋予它解释的能力
Interpretable ： 本来就不是黑箱，可以知道其中的内容
### 2. Saliency Map
![[Pasted image 20241003154654.png]]
The x is pixels
The e is Loss 
### 3. Smooth Grad(up from Saliency Map)
Randomly add noises to the input image,get saliencymaps of the noisy images,and average them
### 4. How a network precesses the imput data 
![[Pasted image 20241003160557.png]]
![[Pasted image 20241003160713.png]]
The probing is 探针
probing can be **classifier** or **not classifier**
such as the example below
![[Pasted image 20241003160917.png]]
### 1. Global explaination
Before this text are all the local explaination


ML_Lihongyi
2024_10_03_Thu
16:34


# 5/06
## 1.0 Adversarial Attack(part 1)
### 1.  Attck is classified into **Non-targeted** \ **Targeted**
	![[Pasted image 20241003172823.png]]
	Add some noisy
### 2. Attack exmaple
	![[Pasted image 20241003172947.png]]
	X0 is the original picture(Tensor) 
	X   is the target picture(Tensor)
	
### 3. Non-perceivale  (d(x0,x))
	![[Pasted image 20241003173259.png]]
	
### 4. Attack Approach
	![[Pasted image 20241003173810.png]]
### 5. FGSM
	![[Pasted image 20241003203555.png]]
	
## 2.0 Adversarial Attack(part 2)
### 1. White Box & Black Box 
	In the previous attack,we know the network parameters  Called **White Box Attack**
	
### 2. Black Box Attack
	![[Pasted image 20241003214829.png]]
	If you have the **training data** of the target network
	Train a proxy(代理) network yourself
	Using the proxy network to generate attacked objects
	Secondly 
	using the **Ensemble Attack**
	which means Train some proxy networks and create the attacking data
	
### 3. Universal Adversarial Attack 
	A attacking signal can make many detection functions invalid
	
### 4. Adversarial Reprogramming
	Like Parasitic(寄生)
	uisng other model to do the thing you want
## 3.0 Defense (Passive & Proactive)
### 1. Passive Denfense
![[Pasted image 20241003220959.png]]
The Filter such as Smoothing
Besides the **compression**(压缩) & **Generator**(生成器)
 
### 2. Passive Defense - **Randomization**
![[Pasted image 20241003221457.png]]

### 3. Proactive Defense
![[Pasted image 20241003221656.png]]
Adversarial Training(对抗性训练)
Training a model that is robust(强壮的) to adversarial attack









































