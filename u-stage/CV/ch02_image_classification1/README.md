# 2강 Image Classification 1  
인공지능의 모티브는 인간이다.   
- 인간의 지각능력을 모방하는 것이 가장 중요   
- 그 중에서도 시각적 지각능력이 중요  

## 1.1 왜 visual perception이 중요한가?
- 시각적 정보들을 인식하여 컴퓨터와 사람이 알아들을 수 있도록 정보를 구성(Computer Vision)
- 정보를 바탕으로 시각적으로 보여줌(Computer Graphics)

![image](https://user-images.githubusercontent.com/51853700/132166241-8410753e-75e1-437c-a6a0-4576155953d5.png)


## 1.2 컴퓨터 비젼이란?
- 이미지, 비디오, 색깔 등을 input으로 받아 인식하고 사고하는 것
- 인간의 생물학적 특징을 반영하여 알고리즘 구현

#### 인간의 시각적 능력도 불완전하다.
- 어떻게 하면 이를 보완할 수 있을지 고민 


#### Machine Learning
- Input -> feature extraction(특징을 추출) : by 사람 -> classification -> Output

#### Deep Learning
- Input -> feature extraction(특징을 추출) + classification : by 컴퓨터  -> Output




## 1.3 어떤 것을 배울것인가?
-  Fundamental Image Tasks
-  Data Augmentation and knoledge distillation
-  Multi-modeling ( vision + {text,sound,etc} )
-  Conditional Generative model
-  visualization tools


## 2.1 Classification
input : 이미지 -> output : class

## 2.2 An ideal approach for image recognition
- k-NN(Nearest Neighbors) : 가까운 것을 기준으로 분류 
Time Complexity, Memory Complexity 문제 때문에 이 세상 모든 데이터를 가져올 수는 없음
유사도를 정해야 함 -> 어떻게 정의할지도 정해야 함
 
- CNN
ex) Single fully connected layer NN
: 모든 픽셀에 서로 다른 가중치를 주고 내적한 후, activation function을 통해서 분류 스코어로 출력
=> Single fully connected layer는 평균 이미지 밖에 표현이 안되고, 조금이라도 train 이미지와 달랐을 때 굉장히 다른 결과를 낼 가능성이 있다는 단점 !

ex) Locally connected layer
: 국소적인 영역만 뽑아서 고려
=> 파라미터수가 줆
=> 더 적은 파라미터로 더 효과적인 특징을 추출, 오버피팅 방지
=> 위치가 바뀌어도 인식 가능
![image](https://user-images.githubusercontent.com/51853700/132168531-cf5d8dba-ecc3-4a21-ac4f-86b9062f1e69.png)


## 2.3 Convolutional Neural Network
: CNN은 많은 CV task들의 backbone 으로 사용됨 


## 3.1 CNN의 역사
![image](https://user-images.githubusercontent.com/51853700/132171185-444c3443-9912-4d20-ad57-b0ebf10debb1.png)

1. LeNet
: 한 글자 단위로 인식, 우편물
![image](https://user-images.githubusercontent.com/51853700/132171140-dd153d90-d2c4-4450-a9b2-6bc127435435.png)

2. AlexNet
: 더 깊은 모델 => gpu 메모리가 모자라서 두 개로 나누어서 두 개의 gpu로 돌림
![image](https://user-images.githubusercontent.com/51853700/132171522-c6dfd34d-46bf-4f63-a698-867f468229a2.png)


- 벡터화(3d tensor -> 2d vector)
![image](https://user-images.githubusercontent.com/51853700/132171742-bc832c34-a9c5-4aad-ae0c-0dc794305c53.png)

- LRN : activation map에서 명암을 주는 역할, 현재는 BN이 쓰임
- filter size가 11 by 11 로 매우 컸음, 현재는 작음

**Receptive field
: 각 element에 영향을 미치는 입력 픽셀의 영역


3. VGGNet
: Deeper but Simple architecture + only 3x3 and 2x2 max pooling
: 일반화가 잘 됨
![image](https://user-images.githubusercontent.com/51853700/132172590-6258835d-0d2c-42c6-b4b3-6d175a6182ad.png)

- 224 x 224 RGB input images 
- RGB mean value를 train image 에서 빼줌(normalization)
- 3x3 and 2x2 pooling layer로만 구성 -> 3x3, 2x2를 깊게 쌓으면 큰 Receptive field size를 가질 수 있기 때문에 복잡한 이미지도 학습 가능
- 3 fully connected layer, ReLU

