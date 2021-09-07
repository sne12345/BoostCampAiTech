## Data Augmentation
- 샘플 데이터와 실제 데이터의 차이를 줄여주기 위한 노력  
- 샘플 데이터에는 bias가 있을 수 있음  
- 이미지를 돌리거나 색을 바꾸는 등의 기법을 통해 train 데이터에서 미처 커버하지 못한 데이터를 만들어 준다.


## Leveraging pre-trained information
 
### 1) transfer learning : 기존에 미리 학습시켜놓은 모델을 가져옴  
한 데이터셋에서 가져온 데이터를 다른 데이터셋에 적용(공통된 것이 많지 않을까?)


1) FC layer만 새롭게 변경 (Freeze Weight, Update Weight)
- 적은 데이터로도 높은 정확도
![image](https://user-images.githubusercontent.com/51853700/132296417-e44fb47b-902b-40b5-8a9b-09f793798fcc.png)

2) 전체 모델을 Fine tuning 

![image](https://user-images.githubusercontent.com/51853700/132296322-a0f4bb79-bd14-44f9-945e-bbbe2394c8a8.png)



### 2) Knowledge distillation
: student model이 teacher model을 따라가도록 학습
![image](https://user-images.githubusercontent.com/51853700/132296921-ca31753f-effc-4252-8735-c3046f453a26.png)


* Hard label & Soft label
![image](https://user-images.githubusercontent.com/51853700/132296793-76c84434-b9b7-4485-a754-cae8c7257394.png)

* softmax with Temperature
![image](https://user-images.githubusercontent.com/51853700/132296865-554e6eee-7727-483a-a9d4-f0c90e6336e7.png)

* Distillation Loss
: KLdiv(Soft label, Soft prediction) - 두 개의 distribution 차이를 재는 방법 (teacher와 student prediction의 차이)

* Student Loss
: CrossEntropy(Hard label, Soft prediction) - student prediction 과 true label 간의 차이

=> Distillation Loss와 Student Loss의 Weighted Sum으로 학습 진행 
=> Student model만 back propagation


## Leveraging unlabeled dataset for training

#### 1) Semi-supervies learning
: Unsupervised + Fully Supervised
![image](https://user-images.githubusercontent.com/51853700/132298647-d9aeebe3-4653-4ba8-8e00-62e4a7f2ca1f.png)

#### with Pseudo Labeling
![image](https://user-images.githubusercontent.com/51853700/132298881-c5986305-bc20-487c-a2bd-393a47521adb.png)


#### 2) Self-training
: Noisy Student Training
- Augmentation + Teacher-Student Network + semi-supervised learning

![image](https://user-images.githubusercontent.com/51853700/132299088-cfcc8e7f-2444-44c3-a8c1-3d0ed0ab32df.png)
* 매 라운드마다 더 큰 student model을 학습시킴

![image](https://user-images.githubusercontent.com/51853700/132299278-41fa7565-7ba6-4673-9ffe-4c964e8f386a.png)

