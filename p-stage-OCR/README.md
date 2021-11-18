# 대회개요 및 데이터소개, 평가방법

- 대회 개요
    - OCR task는 글자 검출 (text detection), 글자 인식 (text recognition), 정렬기 (Serializer) 등의 모듈로 이루어져 있습니다. 본 대회는 아래와 같은 특징과 제약 사항이 있습니다.
    - 본 대회에서는 '글자 검출' task 만을 해결하게 됩니다.
    - 예측 csv 파일 제출 (Evaluation) 방식이 아닌 **model checkpoint 와 inference.py 를 제출하여 채점**하는 방식입니다. (Inference) 상세 제출 방법은 AI Stages 가이드 문서를 참고해 주세요!
    - 대회 기간과 task 난이도를 고려하여 코드 작성에 제약사항이 있습니다. 상세 내용은 **베이스라인 코드 탭 하단의 설명을 참고**해주세요**.**
    - **Input** : 글자가 포함된 전체 이미지
    - **Output** : bbox 좌표가 포함된 UFO Format (상세 제출 포맷은 평가 방법 탭 및 강의 5강 참조)
- 데이터셋
    - 평가 데이터는 크롤링된 다양한 이미지 (손글씨, 간판, 책표지 등) 총 300장으로 구성되어 있습니다.
    - 이미지 내 단어 수의 평균이 17.5개, 최대 약 400개까지 있는 데이터로 다양하게 구성되어 있고, 가로쓰기 글자 외에도 세로쓰기, 진행방향이 불규칙한 글자, 휘어진 글자 등이 있습니다.
    - 언어는 주로 한국어이고, 영어, 그 외 다른 언어도 있습니다.
    - 한국어, 영어가 아닌 다른 언어는 don't care 처리하므로 검출하지 않아도 됩니다.
    
- 평가방법은 DetEval 방식
    
    **1) 모든 정답/예측박스들에 대해서 Area Recall, Area Precision을 미리 계산해냅니다.**
    
    여기서 Area Recall, Area Precision은 다음과 같습니다.
    
    Area Recall = 정답과 예측박스가 겹치는 영역 / 정답 박스의 영역
    
    Area Precision = 정답과 예측박스가 겹치는 영역 / 예측 박스의 영역
   

    **2) 모든 정답 박스와 예측 박스를 순회하면서, 매칭이 되었는지 판단하여 박스 레벨로 정답 여부를 측정합니다.**

    박스들이 매칭이 되는 조건은 박스들을 순회하면서, 위에서 계산한 Area Recall, Area Precision이 0 이상일 경우 매칭 여부를 판단하게 되며, 박스의 정답 여부는 Area Recall 0.8 이상, Area Precision 0.4 이상을 기준으로 하고 있습니다.

    매칭이 되었는가 대한 조건은 크게 3가지 조건이 있습니다.


    one-to-one match: 정답 박스 1개와 예측 박스 1개가 매칭 && 기본조건 성립

    one-to-many match: 정답 박스 1개와 예측 박스 여러개가 매칭되는 경우

    many-to-one match: 정답 박스 여러개와 예측박스 1개가 매칭되는 경우

# OCR 전체 과정

![image](https://user-images.githubusercontent.com/51853700/142345526-ef72eaf2-4c41-4f03-908b-739b5d7063b4.png)


1. Text Detector
- 다수 글자 검출
- 일반 객체 검출과 다른 글자 검출기의 차이점 1) 영역의 종횡비가 다르고 2) 객체 밀도도 다르다

1. Text Recognizer
- 하나의 글자 영역 이미지 입력에 해당 글자열이 출력인 모델
- 이미지 전체 입력이 아니라, 하나의 글자 영역에 해당하는 이미지의 일부가 인식기의 입력임을 유의
- CV와 NLP의 교집합 영역

1. Serializer
- 2D text → 1D text
- 기조 : "사람이 읽는 순서대로 정렬한다"
- 1) Grouping : 단락끼리 묶음  2)단락 간 정렬  3)단락 내 정렬(좌상단에서 우하단으로)

1. Text Parser
- BIO(Begin/Inside/Outside) 태깅

: 개체명 인식 - 문장에서 기 정의된 개체에 대한 값 추출

# 글자 검출 Text Detection

## 소개

1. 일반 객체 영역 검출과 글자 영역 검출의 차이
- 예측하고자 하는 정보
    - 일반 객체 검출 : 클래스 & 위치
    - 글자 검출 : text라는 단일 클래스 & 위치만 예측
- 글자 영역 검출 객체의 특징
    - 매우 높은 밀도
    - 극단적 종횡비
    - 특이 모양 ex) 구겨진 영역, 휘어진 영역, 세로쓰기

1. 글자 영역 표현법
- 사각형
    - 직사각형
    - 직사각형 + 각도
    - 사각형
- 다각형(Polygon)
    - 짝수개의 점, 상하 점들이 쌍을 이루도록 배치
    

## Taxonomy

1. Regression based vs Segmentation based
- Regression based
    - 이미지를 입력 받아 글자 영역 표현값들을 바로 출력
    - 단점
        - 주로 사각형 위주의 글자 표현만이 효과적 → 임의의 모양 텍스트에서는 성능 저하
        - 앵커박스보다 큰 종횡비를 갖는 경우 성능 저하
- Segmentation based
    - 이미지를 입력 받아 글자 영역 표현값들에 사용되는 화소 단위 정보를 뽑고 후처리를 통해서 최종 글자 영역 표현 값들을 확보
    - 각 화소별 글자 영역에 속할 확률 구함
    - 후처리
        - 이진화
        - 연결된 성분 분석(CCA)
        - RBOX 정합
    - 단점
        - 복잡하고 시간이 오래걸리는 후처리
        - 서로 간섭이 있거나 인접한 개체 간의 구분이 어려움
- Hybrid
    - Regression 방식으로 대략의 사각영역 추출 후
    - Segmentation 방식으로 해당 영역의 화소 정보 추출
    - 대표적인 방법 : MaskTextSpotter
    
1. Character based vs Word based
- Character based
    - character단위로 검출하고 이를 조합해서 word instance 예측
    - character-level GT 필요
    - 대표적 : CRAFT'19
        - character region과 affinity(연결성) 예측
        - 단어 단위 라벨링 → 글자 단위 라벨링 추정
- Workd based
    - word 단위로 예측
    - 대부분의 모델이 해당
    
1. Baseline Model - EAST

1) EAST : An Efficient and Accurate Scene Text detector

2) 특징

- Simple 2 stage system
- fast inference & end-to-end train
- segmentation based

3) Idea


네트워크가 2가지 정보를 pixel-wise로 출력

- 글자 영역 중심에 해당하는지 : score map
    
    ⇒ H/4 x W/4 x 1 binary map - 글자 영역의 중심이면 1, 배경이면 0 ⇒ 확률값을 0과 1사이의 값으로 inference
    
    ⇒ GT bounding box를 줄여서 생성 (글자 높이의 30%만큼 end points를 안쪽으로 이동)
    
- 어떤 화소가 글자 영역이라면 해당 bounding box의 위치는 어디인지 : geometry map
