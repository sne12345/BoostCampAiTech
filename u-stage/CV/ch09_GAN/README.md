## 이미지 Notion 참조

#### https://harmonious-collar-f7c.notion.site/ch09_GAN-751b623055164b6bbbe99f7deae20657


# ch09_GAN

1.1  Conditional generative model

- generative model : random 하게 가방 이미지 생성
- conditional generative model : 가방 스케치를 참고해서 랜덤하게 가방 이미지 생성

![Untitled](https://user-images.githubusercontent.com/51853700/133376790-b506b2a3-f82f-4845-a292-e7838568c112.png)

- Generative Adversarial Network (적대적)

1) Generator : Fake Data를 만들고 Discriminator를 속이려고 학습 (생성 모델)

2) Discriminator : Fake / Real을 판단하려고 학습, Fake Data를 찾아내려고 학습 (판별 모델)

⇒ 상호작용을 통해 더 뛰어난 모델 성능

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f08d78ca-8a07-408b-a69f-0a05853906ea/Untitled.png)

- Conditional Generative Adversarial model

: C만 추가됨

![Untitled 2](https://user-images.githubusercontent.com/51853700/133376829-90cf2dd5-a1d2-4ea1-a69b-e32114236b86.png)


1.2 Conditional GAN and image translation

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a2f70095-b7ea-415d-aa43-1c85f69e0d6c/Untitled.png)

1.3 Example : Super resolution

입력 : 저해상도 → 출력 : 고해상도

Fake : 저해상도 → Real : 고해상도 ?

- Adversarial Network

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5722e973-c6db-4bfa-b8f7-ffb04d5303dd/Untitled.png)

- MAE, MSE

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/808cf4b1-0896-4712-ac23-de33ec437247/Untitled.png)
![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8097bea5-8706-4cf7-bd19-98b2e2b88d37/Untitled.png)
![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/63a49739-bf08-40ad-ad75-849404801fba/Untitled.png)
⇒ MSE-based solution은 loss를 줄이기 위해 애매한 중간의 blurry한 이미지를 나타내는 단점

⇒ GAN에서는 discriminator에 의해서 real data와 다른 data는 거르기 때문에 blurry한 이미지를 나타내는 단점이 없음

ex) 
![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/858e22f3-2f7e-453e-b399-d4fc82678167/Untitled.png)
![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5d51ae9e-1922-4b1f-9af4-54ed871d400c/Untitled.png)

2.1 Pix2Pix

: 이미지를 다른 스타일의 대응되는 다른 이미지로 변형시키는 task

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e8079896-b5dc-421e-ab31-7b92f68f1614/Untitled.png)

- Loss function of Pix2Pix

L1 loss function을 같이 쓰는 이유

이유1) L1 loss function이 GT와 직접 비교하기 때문에 

이유2) GAN의 학습이 불안정하고 어려웠기 때문에

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1c892c06-4f0d-460b-bdb6-98a6a478ec83/Untitled.png)

- 실험결과

GAN loss만 쓰면 스타일 반영 적음

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/18509a48-0aaa-4730-b013-8a45d19c3f44/Untitled.png)

2.2 Cycle GAN

Pix2Pix에서는 pairwise data를 필요로 함

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2fca4a29-31ea-4c7c-a4a4-38a561763a21/Untitled.png)

- CycleGAN

: non-pairwise dataset에서 translation

- Loss function of CycleGAN

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/be79b799-745f-4911-a157-fa66cedb6e13/Untitled.png)

1) GAN loss 

두 방향 모두 다른 GAN model 적용

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9dd96ce3-c4cc-42c9-bc1e-6a051ee842c5/Untitled.png)

- Mode Collapse : input과 관계없이 항상 같은 결과를 내면(style만 맞으면) 문제가 없다고 생각하는 문제

⇒ 이를 해결하기 위한 방법이 Cycle consistency loss

2) Cycle consistency loss

: 원본 이미지와의 차이를 줄이게 , 내부의 contents를 유지하도록

⇒ No supervision (self-supervision)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/00e09a61-0943-4292-bd49-0a632d3ea254/Untitled.png)

2.3 Perceptual loss

Q. supervised일 때 왜 GAN을 쓰는 것일까?

A. 퀄리티가 더 좋아서

but GAN은 학습시키기가 어려워서 Perceptual loss를 대신해서 사용한다.

- GAN loss : 상대적으로 코드를 짜고 학습시키기에 어렵다. 하지만 pre-trained network 필요하지 않다.
- Perceptual loss : 코드를 짜고 학습시키기 쉽다. 하지만 pre-trained network를 사용해야 한다.

### Perceptual loss

pre-trained network 의 초기 layer들의 필터는 사람의 시각 지각과 비슷하다? 에서 출발

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/feb014c9-4668-4e7a-9325-eee3ee0906c4/Untitled.png)

- image transform net : input에 따라 transform된 이미지를 도출 ?
- loss network : VGG-16 사용

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/36d25bc6-0f46-4b1d-b856-77c8f9fa0e35/Untitled.png)

- feature reconstruction loss : 원래 이미지의 content가 바뀌지 않았는지 확인

⇒ transform에서 가져온 feature map과의 compute loss 를 직접 구함

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/80e0dc9d-1b6a-4eb6-998e-84c2acc0360e/Untitled.png)

- Style reconstruction loss : 원래 이미지의 style을 유지하도록
- Gram matrics : feature map의 이미지 전반에 거친 통계를 반영 (by pooling feature map)

⇒ compute loss를 구하지 않고, gram matrics를 생성하여 gram matrics간의 차이를 구함 

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/38377c94-fa59-468d-8673-dbd8009581c8/Untitled.png)

1. Various GAN applications

3.1 Deepfake 

: 사람의 얼굴과 목소리를 다른 얼굴과 목소리로 변경

- Ethical concerns : 범죄

⇒ 범죄를 예방하기 위한 챌린지 

3.2 Face de-identification

: 원본 이미지를 변형시켜서 컴퓨터가 찾는 사람의 얼굴을 인식하지 못하도록
![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/49d8719d-1c4f-4aa4-aba1-49d515017e03/Untitled.png)

- Face anonymization with passcode

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/871a9a43-0bb4-4f0c-89b1-1ff2ee51bd66/Untitled.png)

3.3 Video translation
![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/03f30efd-05c8-4c44-be0e-c25bb0b1345c/Untitled.png)

References

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9be1ad93-6376-4576-b273-d435282f1a2a/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fde9289e-4543-45ae-9954-649b57002894/Untitled.png)
