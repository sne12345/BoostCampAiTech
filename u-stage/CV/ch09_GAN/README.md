## Conditional generatvie model

#### 1.1 Conditional generatvie model

# ch09_GAN

1.1  Conditional generative model

- generative model : random 하게 가방 이미지 생성
- conditional generative model : 가방 스케치를 참고해서 랜덤하게 가방 이미지 생성

![Untitled](ch09_GAN%2094e72788f55246829286b4716976b22d/Untitled.png)

- Generative Adversarial Network (적대적)

1) Generator : Fake Data를 만들고 Discriminator를 속이려고 학습 (생성 모델)

2) Discriminator : Fake / Real을 판단하려고 학습, Fake Data를 찾아내려고 학습 (판별 모델)

⇒ 상호작용을 통해 더 뛰어난 모델 성능

![Untitled](ch09_GAN%2094e72788f55246829286b4716976b22d/Untitled%201.png)

- Conditional Generative Adversarial model

: C만 추가됨

![Untitled](ch09_GAN%2094e72788f55246829286b4716976b22d/Untitled%202.png)

1.2 Conditional GAN and image translation

![Untitled](ch09_GAN%2094e72788f55246829286b4716976b22d/Untitled%203.png)

1.3 Example : Super resolution

입력 : 저해상도 → 출력 : 고해상도

Fake : 저해상도 → Real : 고해상도 ?

- Adversarial Network

![Untitled](ch09_GAN%2094e72788f55246829286b4716976b22d/Untitled%204.png)

- MAE, MSE

![Untitled](ch09_GAN%2094e72788f55246829286b4716976b22d/Untitled%205.png)

![Untitled](ch09_GAN%2094e72788f55246829286b4716976b22d/Untitled%206.png)

![Untitled](ch09_GAN%2094e72788f55246829286b4716976b22d/Untitled%207.png)

⇒ MSE-based solution은 loss를 줄이기 위해 애매한 중간의 blurry한 이미지를 나타내는 단점

⇒ GAN에서는 discriminator에 의해서 real data와 다른 data는 거르기 때문에 blurry한 이미지를 나타내는 단점이 없음

ex) 

![Untitled](ch09_GAN%2094e72788f55246829286b4716976b22d/Untitled%208.png)

![Untitled](ch09_GAN%2094e72788f55246829286b4716976b22d/Untitled%209.png)

2.1 Pix2Pix

: 이미지를 다른 스타일의 대응되는 다른 이미지로 변형시키는 task

![Untitled](ch09_GAN%2094e72788f55246829286b4716976b22d/Untitled%2010.png)

- Loss function of Pix2Pix

L1 loss function을 같이 쓰는 이유

이유1) L1 loss function이 GT와 직접 비교하기 때문에 

이유2) GAN의 학습이 불안정하고 어려웠기 때문에

![Untitled](ch09_GAN%2094e72788f55246829286b4716976b22d/Untitled%2011.png)

- 실험결과

GAN loss만 쓰면 스타일 반영 적음

![Untitled](ch09_GAN%2094e72788f55246829286b4716976b22d/Untitled%2012.png)

2.2 Cycle GAN

Pix2Pix에서는 pairwise data를 필요로 함

![Untitled](ch09_GAN%2094e72788f55246829286b4716976b22d/Untitled%2013.png)

- CycleGAN

: non-pairwise dataset에서 translation

- Loss function of CycleGAN

![Untitled](ch09_GAN%2094e72788f55246829286b4716976b22d/Untitled%2014.png)

1) GAN loss 

두 방향 모두 다른 GAN model 적용

![Untitled](ch09_GAN%2094e72788f55246829286b4716976b22d/Untitled%2015.png)

- Mode Collapse : input과 관계없이 항상 같은 결과를 내면(style만 맞으면) 문제가 없다고 생각하는 문제

⇒ 이를 해결하기 위한 방법이 Cycle consistency loss

2) Cycle consistency loss

: 원본 이미지와의 차이를 줄이게 , 내부의 contents를 유지하도록

⇒ No supervision (self-supervision)

![Untitled](ch09_GAN%2094e72788f55246829286b4716976b22d/Untitled%2016.png)

2.3 Perceptual loss

Q. supervised일 때 왜 GAN을 쓰는 것일까?

A. 퀄리티가 더 좋아서

but GAN은 학습시키기가 어려워서 Perceptual loss를 대신해서 사용한다.

- GAN loss : 상대적으로 코드를 짜고 학습시키기에 어렵다. 하지만 pre-trained network 필요하지 않다.
- Perceptual loss : 코드를 짜고 학습시키기 쉽다. 하지만 pre-trained network를 사용해야 한다.

### Perceptual loss

pre-trained network 의 초기 layer들의 필터는 사람의 시각 지각과 비슷하다? 에서 출발

![Untitled](ch09_GAN%2094e72788f55246829286b4716976b22d/Untitled%2017.png)

- image transform net : input에 따라 transform된 이미지를 도출 ?
- loss network : VGG-16 사용

![Untitled](ch09_GAN%2094e72788f55246829286b4716976b22d/Untitled%2018.png)

- feature reconstruction loss : 원래 이미지의 content가 바뀌지 않았는지 확인

⇒ transform에서 가져온 feature map과의 compute loss 를 직접 구함

![Untitled](ch09_GAN%2094e72788f55246829286b4716976b22d/Untitled%2019.png)

- Style reconstruction loss : 원래 이미지의 style을 유지하도록
- Gram matrics : feature map의 이미지 전반에 거친 통계를 반영 (by pooling feature map)

⇒ compute loss를 구하지 않고, gram matrics를 생성하여 gram matrics간의 차이를 구함 

![Untitled](ch09_GAN%2094e72788f55246829286b4716976b22d/Untitled%2020.png)

1. Various GAN applications

3.1 Deepfake 

: 사람의 얼굴과 목소리를 다른 얼굴과 목소리로 변경

- Ethical concerns : 범죄

⇒ 범죄를 예방하기 위한 챌린지 

3.2 Face de-identification

: 원본 이미지를 변형시켜서 컴퓨터가 찾는 사람의 얼굴을 인식하지 못하도록

![Untitled](ch09_GAN%2094e72788f55246829286b4716976b22d/Untitled%2021.png)

- Face anonymization with passcode

![Untitled](ch09_GAN%2094e72788f55246829286b4716976b22d/Untitled%2022.png)

3.3 Video translation

![Untitled](ch09_GAN%2094e72788f55246829286b4716976b22d/Untitled%2023.png)

References

![Untitled](ch09_GAN%2094e72788f55246829286b4716976b22d/Untitled%2024.png)

![Untitled](ch09_GAN%2094e72788f55246829286b4716976b22d/Untitled%2025.png)
