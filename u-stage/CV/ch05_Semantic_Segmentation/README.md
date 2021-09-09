[ Semantic Segmentation ]

## Semantic Segmentation이란?
- 이미지 classification을 pixel 단위로 하는 것  
- 하나의 pixel이 사람인지, 말인지 구분  
- 같은 class이지만 서로 다른 물체(instances)를 구분하지는 않음  

#### 어디에 사용하는가?
- medical image
- 무인 자동차
- 영상 내 컨텐츠를 이해하는데 사용
- 영상 편집(computational photography)

## FCN(Fully Connected Networks)
- 첫 End-to-End sementic segmentation (End-to-End : 입력에서부터 출력까지 모두 미분가능한 neural network, 학습을 통해 문제를 풀 수 있음)
- 임의의 사이즈의 영상을 넣어도 학습 가능한 호환성 높은 architecture
![image](https://user-images.githubusercontent.com/51853700/132618059-5c266715-d916-4364-99cc-5616082f68b7.png)



#### Fully connected layer vs Fully convolutional layer 
- Fully connected layer : fixed dimensional vector가 주어지면 이를 앞에서 가져온 matrix와 multiplication을 해서 또다른 fixed dimenstional vector가 출력으로 나옴, spatial 정보를 버림  
- Fully convolutional layer : 입력도 activation map, 출력도 activation map, 1x1 convolution, spatial coordinates(? 공간 좌표) 보존  
![image](https://user-images.githubusercontent.com/51853700/132618167-5af31fed-7368-42ae-b4e7-db6fc280ac6e.png)



[ Semantic Segmentation architecture ]
