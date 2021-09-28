# 2. R-CNN
: 초기모델, 구조 직관적  
1) 객체가 있을 법한 후보 영역을 예측 : Sliding Window, Selective Search => Sementic feature vector를 고정된 크기로 와핑(resizing)     
2) 사전 학습된 ConvNet에서 학습 => 고정된 output vector => 분류   
![image](https://user-images.githubusercontent.com/51853700/135036279-8078d5e1-0a7a-4ff1-9b1e-378d93c53230.png)


# 2.1 - 1) Sliding Window 
: Window의 사이즈는 임의로    
But ! 무수히 많은 후보 영역이 있고, 객체를 포함할 가능성이 떨어짐 => R-CNN에서 사용x
![image](https://user-images.githubusercontent.com/51853700/135036829-8dab737d-98bb-4237-9f90-684b02dc026d.png)


# 2.1 - 2) Selective Search
: 무수히 많은 작은 영역으로 나누어 후보 영역 추출 => 몇 번의 iteration을 거치면서 작은 영역들을 병합  
* Selective Sarch에서 사용하는 segmentation 방법
* https://blog.naver.com/laonple/220925179894
* Greedy Algorithm을 사용한 영역 병합 방법
* https://blog.naver.com/laonple/220930954658


