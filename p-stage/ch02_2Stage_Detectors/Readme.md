# 1. 세가지 detectors 개요
![image](https://user-images.githubusercontent.com/51853700/135059283-6690682b-9d69-4678-936f-ff5dcd40470a.png)


# 2. R-CNN
: 초기모델, 구조 직관적  
1) 객체가 있을 법한 후보 영역을 예측 : Sliding Window, Selective Search => Sementic feature vector를 고정된 크기로 와핑(resizing)     
2) 사전 학습된 ConvNet에서 학습 => 고정된 output vector => 분류   
![image](https://user-images.githubusercontent.com/51853700/135036279-8078d5e1-0a7a-4ff1-9b1e-378d93c53230.png)


# 2.1 - 1) Sliding Window 
: Window의 사이즈는 임의로    
But ! 무수히 많은 후보 영역이 있고, 객체를 포함할 가능성이 떨어짐 => R-CNN에서 사용x
![image](https://user-images.githubusercontent.com/51853700/135058038-63f93011-b848-401c-ab6b-c98981e2c60d.png)


# 2.1 - 2) Selective Search
: 무수히 많은 작은 영역으로 나누어 후보 영역 추출 => 몇 번의 iteration을 거치면서 작은 영역들을 병합  
* Selective Sarch에서 사용하는 segmentation 방법
* https://blog.naver.com/laonple/220925179894
* Greedy Algorithm을 사용한 영역 병합 방법
* https://blog.naver.com/laonple/220930954658




# 2.2 Pipeline

### 1) 이미지 입력받기
![image](https://user-images.githubusercontent.com/51853700/135055337-d2ec6f1b-7cca-4da7-a974-7f7b31fe2625.png)


### 2) Selective Search를 통해서 약 2000개의 RoI(Region of Interest)를 추출 => 대략적인 bounding box
![image](https://user-images.githubusercontent.com/51853700/135056223-f8e022ea-6c6e-4727-bb0c-bdedca85e945.png)


### 3) RoI의 크기를 조절해 모두 동일한 사이즈로 변형
* FC layer의 입력 사이즈가 고정이기 때문에  
![image](https://user-images.githubusercontent.com/51853700/135056323-4e8f7084-92d2-4191-ad10-df5731681a70.png)
![image](https://user-images.githubusercontent.com/51853700/135056744-0ce04ee7-9a04-4d57-9ffa-46797bf20726.png)


### 4) RoI를 CNN에 넣어, feature를 추출
* 각 region마다 4096-dim feature vector 추출(2000 x 4096)
* pretrained AlexNet 구조 활용
* AlexNet 마지막에 FC layer 추가
* 필요에 따라 fine tuning 진행  
![image](https://user-images.githubusercontent.com/51853700/135056999-b9cd1390-910b-4144-b462-66959e88fb0d.png)


### 5) - 1 CNN을 통해 나온 feature를 *linearSVM에 넣어 분류 진행
* Input : 2000 x 4096 features
* Output : Class(클래스 개수) + Confidence scores(배경 여부)  
![image](https://user-images.githubusercontent.com/51853700/135057426-e80eaf8a-e836-4c58-83e0-883fa8c12099.png)


### 5) - 2 CNN을 통해 나온 feature를 linear regression을 통해 bounding box 예측 => bounding box 정확하게 학습
![image](https://user-images.githubusercontent.com/51853700/135059984-fc2d3e90-f747-4081-936e-11063a12f1d4.png)





# 2.3 Training
### 1) AlexNet
* Domain specific finetuning
* Dataset 구성
- *IoU > 0.5 : Positive samples 
- *IoU < 0.5 : Negative samples
- Positive samples 32, Negative samples 96


출처 : 네이버 부스트캠프
