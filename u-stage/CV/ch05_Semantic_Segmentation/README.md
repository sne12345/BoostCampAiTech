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


[ Semantic Segmentation architecture ]



## FCN(Fully Convolutional Networks)
- 첫 End-to-End sementic segmentation (End-to-End : 입력에서부터 출력까지 모두 미분가능한 neural network, 학습을 통해 문제를 풀 수 있음) : gpu로 병렬처리 가능 
- 임의의 사이즈의 영상을 넣어도 학습 가능한 호환성 높은 architecture
- Fully Connected layer => Fully Convolutional Networks 로 대체
![image](https://user-images.githubusercontent.com/51853700/132618059-5c266715-d916-4364-99cc-5616082f68b7.png)



#### Fully connected layer vs Fully convolutional layer 
- Fully connected layer : fixed dimensional vector가 주어지면 이를 앞에서 가져온 matrix와 multiplication을 해서 또다른 fixed dimenstional vector가 출력으로 나옴, spatial 정보를 버림 
- Fully convolutional layer : 입력도 activation map, 출력도 activation map, 1x1 convolution, spatial coordinates(? 공간 좌표) 보존  
![image](https://user-images.githubusercontent.com/51853700/132618167-5af31fed-7368-42ae-b4e7-db6fc280ac6e.png)



![image](https://user-images.githubusercontent.com/51853700/132629486-f4524217-cf49-4041-9a4c-ed38358a937b.png)
![image](https://user-images.githubusercontent.com/51853700/132629647-bac1ee59-22e2-4a10-b11d-d1080a623a81.png)
=> fully connected layer와 다르게 convolution을 하면 Sliding Window 방식으로 weight를 공유하기 때문에 spatial 한 정보가 보존됨

* Sliding Window란 무엇인가?  
sliding window는 사진을 윈도 사이즈에 맞춰 나눈 다음 매 윈도우로 잘린 이미지를
입력값으로 모델을 통과해서 결과를 얻는 방법입니다.


### Upsampling Layer  
stride와 pooling layer는 receptive field를 키워주지만, 해상도는 낮춤 => trade off
=> receptive field를 키워주고, 마지막에 upsampling해줌
![image](https://user-images.githubusercontent.com/51853700/132630550-d46c8a14-093f-4c73-af5d-4f355a4463d0.png)

 
 
 #### 1) Transposed Convolution
 
 ![image](https://user-images.githubusercontent.com/51853700/132630834-0107487d-f6e3-4884-9471-f0f7df577410.png)
 ![image](https://user-images.githubusercontent.com/51853700/132630861-a82aa697-dbc2-4623-9481-f965f23f12ba.png)


![image](https://user-images.githubusercontent.com/51853700/132631233-3ad8d627-bd12-4d53-b9b4-7dd332870bf7.png)

=> 낮은 layer에서 fine, 각각 픽셀의 세부 정보(국소적)
=> 깊은 Layer에서 coarse, 큰 그림

![image](https://user-images.githubusercontent.com/51853700/132631389-f8ece70f-0ab7-4991-a58a-9517587e0d8a.png)

* overlap issue ??


 #### 2) Upsample and Convolution
 : Convolution을 같이 이용 
 : overlap issue 해결, 골고루 영향받도록
 
 upsampling operation 을 두 개로 분리??
 ![image](https://user-images.githubusercontent.com/51853700/132631945-21ca24bb-c50e-436c-81b7-1f433864005c.png)

- 학습 가능한 upsampling을 만들기 위해 Convolution을 일괄적으로 적용



## HyperColumns for object segmentation
FCN과 비슷하지만, end-to-end가 아님


## U-Net
FCN을 base로 함

![image](https://user-images.githubusercontent.com/51853700/132632110-b056433b-e145-4451-8062-ff3b4804905f.png)

* contract path
: feature channel을 두 배씩 늘리면서 receptive field 키워줌

* expanding path
: contracth path layer와 대칭되게 동일하게 맞춤
: 해상도를 높이면서 contract path에서 대칭되는 특징을 skip connection으로 가지고 옴(concatenate 사용?) => localized 정보 제공
: feature channel을 두 배씩 줄면서 해상도를 늘림


#### 만약 feature map size가 홀수가 되면 어떡하지?
![image](https://user-images.githubusercontent.com/51853700/132634010-4fa58f31-9067-4734-a488-2c89395fb782.png)
=> 홀수가 나오지 않도록 해야 함


#### U-net in pytorch
![image](https://user-images.githubusercontent.com/51853700/132634109-1c60778a-8911-4f88-b85e-b323b444b18e.png)
![image](https://user-images.githubusercontent.com/51853700/132634273-fb16d8bd-e560-4899-a15a-ce71ff3697f0.png)
=> 중첩이 안 생김


## DeepLab
#### 1) CRF
![image](https://user-images.githubusercontent.com/51853700/132635508-e5ec3c52-356f-4aa8-93ee-e48d37073308.png)
물체의 안쪽과 바깥쪽으로 score map이 확산하게 만듦 

#### 2) Atrous Convolution(Dilated convolution)
convolution할 때에 일정한 공간을 넣어줌  
넓은 영역 고려할 수 있게 함 + 파라미터 수 늘어나지 않음
=> receptive field size가 크게 늚 ??
