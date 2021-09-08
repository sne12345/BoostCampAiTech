## problems with deeper layers

#### 1. 더 깊은 네트워크가 더 좋은 성능을 냄을 알게 되었음
* larger receptive fields
* more capacity and non-linearity

[ 문제 발생 ]
#### Gradient vanishing / exploding 
#### Computationally complex
#### Degradatio nproblem  
  
    
    
## CNN architectures for image classification 2

#### 2. GoogleNet
* Inception Module 구조
하나의 layer에서 다양한 크기의 convolution filter 사용 => 여러 측면으로 activation 관찰
=> 수평 확장

![image](https://user-images.githubusercontent.com/51853700/132457574-40695e5d-556c-4c4f-888d-3c560e49c458.png)  
=> 계산 복잡도와 용량 커짐 => 1x1 convolution 적용함으로써 해결(bottleneck layer?)

* 1x1 convolution 
ex) 필터 두 개 
=> 공간 크기는 변하지 않고, 각 픽셀 독립적으로 채널 수만 변경
![image](https://user-images.githubusercontent.com/51853700/132458058-cbc2c4de-e7b0-489d-b643-a25736f7b974.png)

* GoogleNet 전체구조
backpropagation gradient가 사라지는 문제 때문에 중간중간에 gradient를 주입해줌(auxiliary classifier?)  
최종 출력은 한 개의 fc layer  
![image](https://user-images.githubusercontent.com/51853700/132458256-482ce29b-1326-475e-9b44-483a7bc7965b.png)


* auxiliary classifier
: 중간중간 에 softmax를 두어 중간에서도 Backpropagation을 하게 함
: 학습에서만 사용하고, 테스트 때에는 사용하지 않음  
![image](https://user-images.githubusercontent.com/51853700/132458727-4985e0e3-afbf-4787-95ba-46fcc7f091b6.png)


#### 3. ResNet  
layer depth가 성능에 큰 영향이 있다 !

* 깊이가 깊을수록 overfitting이 일어난다? No ! 56 layer의 학습 최적화가 안 된 것이다. -> vanishing gradient 문제
![image](https://user-images.githubusercontent.com/51853700/132459740-dd7439d8-1880-4438-822b-5b490ffcf44e.png)

* vanishing gradient 문제 해결책 : Shortcut connection  
몇 개의 layer를 건너 뛰어 연결해 non-linearities를 추가하는 skip connections는 gradient가 layers를 건너뛰어 연결될 수 있는 shortcuts를 만들어 parameters를 network에 deep하게 업데이트 할 수 있다.  
![image](https://user-images.githubusercontent.com/51853700/132460476-e28e1c7f-8de8-456e-a793-9d338e148c2e.png)



* shortcut connection의 성능이 왜 좋을까?
: 2의 n승으로 layer를 거칠 수 있는 모든 경우의 수를 거침으로써 복잡한 매핑을 학습할 수 있음
![image](https://user-images.githubusercontent.com/51853700/132461000-07cd5d1c-f93e-4b86-97a8-9c6063626f31.png)


* ResNet 전체 구조
![image](https://user-images.githubusercontent.com/51853700/132461162-b88bcb65-8e48-4089-9717-6fe516042425.png)
채널은 높아지는 동시에, 공간축은 두 배씩 작아진다.

* Dense Net
: 모든 이전 layer들의 gradient 값을 앞으로 전달
: 더하기가 아닌 concatenate(기존 layer의 값을 그대로 보존)

* SENet
: 채널 간의 관계를 모델링
attention 생성하는 방법(뉴에이팅?, gating?)  
1) squeeze :
2) Excitation : 
scaling을 통해 중요한 것은 강조, 중요하지 않은 것은 값을 줄임


* EfficientNet 
: deeper, wider, high resolution networks(인풋 resolution?을 크게 넣어줌)  => compound scaling 
(saturation? 포화)
적은 FLOPS?에서도 좋은 성능
![image](https://user-images.githubusercontent.com/51853700/132463104-a52c8460-28d5-490c-ad88-2ba996551f16.png)



* deformable convolution
: irregular convolution 
: offset field에 따라 feature map을 irregular하게 넓혀줌

![image](https://user-images.githubusercontent.com/51853700/132463154-5f723349-b2b6-44d0-8ce8-6a27c17730e6.png)



## Summary of image classification
operations : coputational compelxity, 원의 크기 : memory size
![image](https://user-images.githubusercontent.com/51853700/132463393-fc64bbc3-9555-4cb2-b245-31488d2721ac.png)

* GoogleNet is the most efficient CNN model out of AlexNet, VGG, REsNet
* but, 구현 힘듦
* 일반적으로 많이 사용하는 backbone은 VGG, ResNet

feature map을 추출하는데 CNN 사용하고, classification할 때 다양한 target task에 응용



