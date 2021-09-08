## problems with deeper layers

#### 1. 더 깊은 네트워크가 더 좋은 성능을 냄을 알게 되었음
* larger receptive fields
* more capacity and non-linearity

[ 문제 발생 ]
#### Gradient vanishing / exploding 
#### Computationally complex
#### Degradatio nproblem


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
최종 출력은 두 개의 fc layer와 softmax Activation
![image](https://user-images.githubusercontent.com/51853700/132458256-482ce29b-1326-475e-9b44-483a7bc7965b.png)


* auxiliary classifier
: 테스트 때에는 사용하지 않음
![image](https://user-images.githubusercontent.com/51853700/132458727-4985e0e3-afbf-4787-95ba-46fcc7f091b6.png)
