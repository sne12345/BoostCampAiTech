## CNN Visualization

#### 1. CNN Visualization이란? 
1) CNN black box 안에 무엇이 들어 있는지 확인하는 방법  
2) 왜 performance가 잘 나오는지를 분석  
3) 어떻게 개선할지를 분석  


- ZFNet 
: 낮은 계층에서는 방향성, 높은 계층에서는 의미있는
![image](https://user-images.githubusercontent.com/51853700/133009347-cb14e792-d140-4a49-9d5d-f831d1682e3b.png)

#### 2. filter visualization
: color, 모양을 학습한 각각의 pixel을 확인할 수 있음 
: 3채널 이상은 분석하기가 쉽지 않음, 추상적임 => 더 복잡한 방법 필요 
![image](https://user-images.githubusercontent.com/51853700/133021856-a39952e6-0e39-4f88-a1b2-d168f558f77e.png)



#### 3. Types of visualizing neural network
![image](https://user-images.githubusercontent.com/51853700/133030674-345763aa-6c27-4bf1-bce2-ee67787db2dc.png)





## Analysis of model behaviors

#### 1. embedding feature analysis

Nearest neighbors in Feature space
![image](https://user-images.githubusercontent.com/51853700/133031014-8273d739-5e18-4dfe-acbf-e90f0a7be4f0.png)
![image](https://user-images.githubusercontent.com/51853700/133031641-3a4fee13-5332-4854-8ef5-81849cf8e112.png)

Feature space
![image](https://user-images.githubusercontent.com/51853700/133031680-bcf23bbe-a343-4244-93ed-323721d724ef.png)



#### 2. embedding feature analysis
차원축소 방법을 통해 눈으로 쉽게 확인하기  

* t-SNE ?
![image](https://user-images.githubusercontent.com/51853700/133031865-7856ac92-7f90-4522-a4ae-727aca362b45.png)


* Layer activation 
: thresholding ?
: middle layer to high layer
![image](https://user-images.githubusercontent.com/51853700/133032008-06f068a2-a0d6-406d-a261-2acba7c8778d.png)


* Maximally activating patches
: hidden layer에서 가장 큰 값을 갖는 위치 근방의 패치를 가져옴 
: middle layer
![image](https://user-images.githubusercontent.com/51853700/133032252-3de6ae0a-c775-4711-a1c9-d862e8cc1510.png)


1) 특정 channel을 고르기 
2) 예제 데이터를 넣어서, 각 layer의 channel의 activation map을 저장
3) activation map 중에서 가장 큰 값 근방의 패치를 뜯어옴 


#### 3. activation investigation - 이미지 합성

* Gradient ascent
![image](https://user-images.githubusercontent.com/51853700/133033503-930c2ea0-6544-480d-9428-93280416c909.png)
I : 영상 입력값 

![image](https://user-images.githubusercontent.com/51853700/133033636-a87be7fe-425b-47f6-bb6a-81dd9a33a978.png)
L2

1) 랜덤한 입력 이미지에 대한 prediction score를 구함
2) backpropagation
3) 입력 데이터를 업데이트 



## model decision explanation

#### saliency test 1 : occlusion map
: mask 위치에 따른 score change를 분석 
![image](https://user-images.githubusercontent.com/51853700/133035260-083eb09b-29a4-404e-a0e5-0d174da1c4a0.png)


#### saliency test 2 : via Backpropagation
: 랜덤 x 특정 이미지를 입력값으로 넣고, backpropation을 해서 가장 영향을 많이 미치는 영역 히트맵으로 그려줌 
![image](https://user-images.githubusercontent.com/51853700/133035990-722db7db-8369-42b6-90b4-ee1030631b4a.png)

1) 특정 이미지를 입력값으로 넣어서 class score 를 구함
2) input domain까지 backpropagation
3) visualize the obtained gradient magnitude map


#### advanced saliency
:backward 연산을 할 때 activation (relu)를 적용 
![image](https://user-images.githubusercontent.com/51853700/133037468-43935113-aac1-4aa9-9ff3-ace90a38d077.png)


#### guided saliency
: forward 연산할 때에도 activation 적용 , backward 연산을 할 때도 activation 를 적용 
=> 양방향에서 모두 양수를 참조하는 방향으로 가기 때문
![image](https://user-images.githubusercontent.com/51853700/133037842-911b2c93-8683-4459-bd65-b97b6dd70e34.png)


#### CAM(Class activating map)
: global average pooling을 넣어주고, fc layer를 하나만
![image](https://user-images.githubusercontent.com/51853700/133038163-4e9f88ec-79bc-4b55-846a-8c60102b3b00.png)


[ 수식 ]
![image](https://user-images.githubusercontent.com/51853700/133038520-f291e8f8-b7cc-4ef5-8edc-1c30f27c8cdb.png)
![image](https://user-images.githubusercontent.com/51853700/133038630-12ad66ff-1c44-4d12-bf79-164ec27cd303.png)


* 위치도 찾아짐
![image](https://user-images.githubusercontent.com/51853700/133038708-28f51570-bb74-4948-bcbb-59b6c79619cd.png)
* but, 구조를 바꾸기 때문에 성능이 떨어질 수 있음 (ResNet, googleNet에서는 GAP들어가 있어서 성능 차이가 크지 않음)


#### Grad-CAM
: 기존 pretrained model 구조를 바꾸지 않고도 적용가능, CNN 필요 
![image](https://user-images.githubusercontent.com/51853700/133039007-eb5cd072-eb45-44e1-a460-334764db5cfc.png)


1) 각 채널의 gradient 성분의 크기 구함(weight) 
![image](https://user-images.githubusercontent.com/51853700/133039316-58e1bea1-4ed5-4657-b087-7967eb5da8b6.png)

2) CAM과 다르게 activation은 ReLU 사용 
![image](https://user-images.githubusercontent.com/51853700/133039227-2400e6e7-37dd-4627-a486-46e2bd4b575d.png)




* Grad-CAM은 여러가지 task 에 사용가능 
![image](https://user-images.githubusercontent.com/51853700/133039491-57251e41-58f0-46c6-8f24-f11d76f391af.png)


