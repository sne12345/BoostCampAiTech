## 1. Overview of Multi-modal Learning
서로 다른 데이터 형태를 사용하는 학습법 
![image](https://user-images.githubusercontent.com/51853700/133548786-4c2d03e3-8f54-4090-af7d-fbae95f2ad08.png)

* challenge (1)
-> 데이터마다 형태가 다르다는 점, 표현의 차이가 있다는 점

* challenge (2)
-> 정보의 양도 다르다. feature space 특징들도 unbalance하게 매치가 됨
![image](https://user-images.githubusercontent.com/51853700/133548930-6845ac6c-ba57-4492-a727-0139bbdc9c26.png)

* challenge (3)
-> model이 특정 modality에 biased 될 수 있다.
-> 대부분의 경우, visual data로 판단이 되기 때문에 visual 쪽으로 biased 
![image](https://user-images.githubusercontent.com/51853700/133549074-035db083-cba5-4b4f-81ce-ff0e5c881cc5.png)



* Multi-modal Learning의 패턴
![image](https://user-images.githubusercontent.com/51853700/133549166-ceb30071-9e0e-43f7-95aa-e48b867f80bb.png)




## 2. Multi-modal task (1) visual & text

안돼애애애애ㅠㅠㅠ



2.3 cross modal translation

![image](https://user-images.githubusercontent.com/51853700/133558625-c58ed378-ea2d-40ac-bcd2-e43c3744d84b.png)
![image](https://user-images.githubusercontent.com/51853700/133558658-74b47dee-2c6f-42d5-8260-6f545fd718c5.png)
generator network
: 가우시안 랜덤 코드 => 항상 같은 output이 나오지 않게 해줌, conditional gan
discriminal network
: 이전의 sentence를 줘서 이 sentence하에 이미지가 맞는지 확인

2.4 cross modal reasoning
* visual question answering
영상과 질문이 주어지면 답을 구하는 task

image stream : CNN  -> fixed dimensional vector 출력
question stream : RNN으로 encoding -> fixed dimensional vector 출력
-> point-wise multiplication
![image](https://user-images.githubusercontent.com/51853700/133558931-781731b7-918f-46fc-b073-a9e0d8d13b0e.png)


## 3. Multi-modal task (2) visual & audio

* sound representation
![image](https://user-images.githubusercontent.com/51853700/133559384-223193c1-9095-4a3c-9f30-4c931621f77c.png)

3.1 fourier transform : 음향 -> 주파수로 변환
* STFT(Short time Fourier transform)
: 짧은 구간에 대해서만 fourier transform을 적용함
hamming wave : 끝은 줄어주고, 가운데 부분에 초점을 맞출 수 있도록 
window를 적당히 overlap
![image](https://user-images.githubusercontent.com/51853700/133560654-e8c8c5fd-43e6-482b-a633-0c5d9d0e7513.png)


* fourier transform을 왜 하느냐?
: 어떤 주파수 성분이 들어있는지 분해 decompose
![image](https://user-images.githubusercontent.com/51853700/133560881-4b4f25fc-4fe4-4c16-aa93-8892a97469df.png)

* Spectrogram
: 시간에 따른 주파수 변화
![image](https://user-images.githubusercontent.com/51853700/133561114-d5b77445-ea9d-4dae-86ed-6b3312fcf3b3.png)



3.2 Joing embedding
* Sound net
object distribution : 객체 인식
scene distribution : 어떤 장면?

sound 쪽만 학습되고, visual 쪽은 fix되어 있음 
![image](https://user-images.githubusercontent.com/51853700/133562811-699fd82b-f1ba-4b45-a827-1cbf07ada453.png)

* waveform : spectrogram을 안씀, 특별한 사유 x

* 다른 target task를 할 때에는 pool5의 feature를 추출해서 사용(sound를 학습한 데이터이다) -> classifier -> target task 풀기
=> pool5가 더 generalized 한 sementic 정보를 가지고 있을 것이라 판단

3.3 cross modal translation

* Speech2Face
![image](https://user-images.githubusercontent.com/51853700/133563471-5597bbb2-fb05-4813-9156-91ea88bb2876.png)

=> Module network
: 미리 학습된 모델들을 잘 조합해서 사용 
![image](https://user-images.githubusercontent.com/51853700/133563558-d19d318c-a5fc-4f1a-b929-91783fcba3b1.png)

speech feature -> face feature을 따라하도록 학습
![image](https://user-images.githubusercontent.com/51853700/133563786-9c42d255-15e3-4ded-893e-640906a0d387.png)



* image to speech ????
: image captioning + speech synthesis(TTS)
: sub word unit을 추출
![image](https://user-images.githubusercontent.com/51853700/133564450-e29b4238-b937-4072-a327-2133d67e7435.png)




3.4 cross modal reasoning
* sound source localization
: 영상과 소리를 주면 영상의 어디에서 소리가 나는지 찾는 task
![image](https://user-images.githubusercontent.com/51853700/133564891-156a004a-a420-4da3-9c4e-78ea671f07dc.png)

![image](https://user-images.githubusercontent.com/51853700/133565095-8daa9889-157f-499c-a53d-e05277594f85.png)

* unsupervised
localization score를 weight로 visual net에서 나온 feature와 weighted sum pooling => attended visual feature
![image](https://user-images.githubusercontent.com/51853700/133565232-eaef2179-8a49-404e-ad6f-9a070ddc7578.png)
![image](https://user-images.githubusercontent.com/51853700/133565392-1e0b41e4-a975-4375-b542-aabb1142066b.png)
![image](https://user-images.githubusercontent.com/51853700/133565414-ac939796-d6ad-4240-af61-709056a101fd.png)



![image](https://user-images.githubusercontent.com/51853700/133564450-e29b4238-b937-4072-a327-2133d67e7435.png)



* visual 정보를 이용해서 speech separate
* ![image](https://user-images.githubusercontent.com/51853700/133565811-52118768-ba11-4eba-a7ae-3b5c2b33d526.png)

face embedding -> STFT(spectogram) -> 얼굴과 스피치 데이터를 concat, complex mask 형태로 출력 -> seperated speech spectogram 분리 -> waveform으로 구성
=> L2 loss (clean spectogram과 enhanced spectrogram 의 차이) but 분리가 불가능 -> GT를 만들때에는 두 개의 video를 합성

* synthesizing obama




## Image captioning
: CNN -> RNN
![image](https://user-images.githubusercontent.com/51853700/133566659-365ea4be-7bd5-44d3-b9db-209cfa7dcd19.png)

- CNN
cnn 전체에서 끝 두개를 잘라서 사용한다(classification을 하는게 아니기 때문에)  
 output 14 x 14로 나온다.
 
 
- RNN
: 중간중간 attention 정보도 함께 넣어준다.
![image](https://user-images.githubusercontent.com/51853700/133567115-2c136a51-4aea-46e9-8603-33d38d9e0c1c.png)


* Beam Search  
ex) k = 3
each decode 단계에서 best score 3개씩을 계속 남김 (계속해서 단어를 붙여서 누적으로 평가)
![image](https://user-images.githubusercontent.com/51853700/133568210-ada063f3-53bc-43b0-b4b4-afc7697a01e4.png)


 
 
